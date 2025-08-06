_Intrusion Detection Systems (IDS)_ help protect networks by analyzing traffic and performing actions (such as generating real-time alerts) when suspicious activity is detected based on predefined signatures.

_Snort_ is an open-source IDS/IPS widely used for network security monitoring. It analyzes network traffic in real-time to identify potentially malicious activities. Snort consists of two main components: a detection engine (_the Snort Engine_) with a modular plug-in architecture, and a flexible rule language for defining traffic to be monitored (_the Snort Rules_).

_Digital Bond_ created a set of Snort rules for several control system protocols, known as the Quickdraw SCADA IDS signatures. The snort.conf1 configuration file has only Quickdraw rules enabled.

To start Snort on the loopback interface, use the following command:
`sudo snort -A console -i lo -c /etc/snort/snort.conf1`

The _-A_ option sets the alert output mode, _-i_ specifies the network interface to monitor, and _-c_ defines the path to the Snort configuration file.
# Replaying Network Traffic

_Digital Bond_ created network capture files for testing Snort configuration, located in the _/quickdrawPCAPs_ directory. We will use the _tcpreplay_ utility to resend traffic from one of these files.

_Tcpreplay_ is a tool for replaying network packet captures (PCAP files) onto a network interface, allowing you to reproduce captured network traffic by "replaying" the packets.

While Snort is running, open a _new terminal_ to generate traffic for processing.
To replay traffic from the _modbus_test_data_part1.pcap_ PCAP file to the loopback interface at the highest possible speed, run the following command:
`sudo tcpreplay --topspeed -i lo quickdrawPCAPs/modbus_test_data_part1.pcap

The traffic was quickly replayed in under a second, and the output confirmed the successful transmission of packets.

What utility is used to resend traffic from the capture files? **tcpreplay**
# Alerts displayed

Switch back to _terminal 1_, where several alerts are displayed.

Snort has detected various types of suspicious activities, such as unauthorized write and read requests, along with potential denial of service (DOS) attacks.

Next, we will examine the actual rules and try to understand how they work, considering that Snort rules files are text files.

Press _Ctrl+Z_ to stop Snort and return to the command prompt.
### Looking at Rules

Run the following command to read the Quickdraw rules file and use _more_ command to jump directly to rules related to Modbus PLC protocol:
`cat /etc/snort/rules/all-quickdraw.rules | more +/MODBUS

At this point, let's explore the explanation of the following rule:
> Unauthorized Read Request to a PLC

The rule begins with the Rule Action, which in this case is _alert_. Snort can perform several actions, including log (to log matching packets) and drop (to block packets for active defense).

Next is the protocol (tcp), followed by the source of the packet. Since all traffic comes from/to the same machine, we modified the original rule, which had !$MODBUS_CLIENT* to exclude a specific IP. In our configuration, we replaced it with any any, meaning the rule applies to traffic from any source. The targeted traffic goes to the Modbus server on port 502.

The section in parentheses includes Rule Options. The flow option checks for client-to-server traffic in established sessions. The content option looks for "00 00" in packet data, with offset specifying where to start and depth telling Snort how far to scan. This helps detect zero values for the Protocol Identifier. To refine this, the pcre option is used, allowing Snort to search for patterns in packet data. Specifically, it looks for any 3 characters `([\S\s]{3})` followed by values separated by pipe (|), which represent Modbus Function Codes for various read requests. Together, content and pcre narrow the match to only Modbus read requests.

Next, we'll simulate an attack to trigger this rule.
Press _enter_ to navigate through the output or press _q_ to exit and return to the command prompt.
# Logging Snort Alerts

Logging Snort alerts refers to recording and storing information about potential security events detected by Snort.

To restart Snort and direct detailed alerts to a file instead of displaying them on the console, use the following command:
`sudo snort -A full -i lo -k none -c /etc/snort/snort.conf1

The _-k_ none option disables packet checksum verification, which is necessary for locally generated traffic.
### Starting Modbus Server

A _Modbus slave_ is a device that responds to Modbus master commands and requests using an address for identification and communication. A Modbus slave contains registers, which can be read or written by the Modbus master based on the communication setup.

Diagslave is a free, open-source Modbus slave simulator and diagnostic tool used to simulate Modbus slaves, test the Modbus master device, troubleshoot, and provide detailed communication settings.

Switch to _second terminal_, and change the working directory to the location of the Modbus slave device simulator:
`cd /modbus/diagslave/x86_64-linux-gnu

Utilize the following command to employ the diagslave tool to initiate a Modbus slave device simulator:
`sudo ./diagslave -m tcp

The _-m_ option specifies the Modbus protocol to use for communication.

What tool is used to simulate a Modbus slave for testing and troubleshooting?
**diagslave**
### Starting TShark

Let's begin capturing traffic for a more detailed examination of the packets.

TShark is the command-line version of Wireshark, a widely used open-source protocol analyzer and packet capture tool. It allows capturing and analyzing network traffic directly from the command line, without a GUI.

Now, let's start sniffing network traffic on port 502, the default Modbus communications port.

Open a _new terminal_ and use TShark to capture TCP traffic on port 502 and save it to the /tmp/cap file:
`sudo tshark -i lo -w /tmp/cap port 502
# Starting Metasploit Framework

> To recap: Snort is running on Terminal 1, a Modbus server on Terminal 2, and TShark is capturing traffic on Terminal 3.

_MSFconsole_ is the command-line interface for the Metasploit Framework, an open-source security toolset for penetration testing and vulnerability assessment. It provides a collection of modules and libraries to test system and application security.

Open a _new terminal_ and start Metasploit with the following command:
`msfconsole -L

When prompted to set up a new database, type _no_.
### Selecting Module

Generate suspicious traffic using the Metasploit modbusdetect auxiliary module.

First, select the modbusdetect module:
`use auxiliary/scanner/scada/modbusdetect

To display the module options, run:
`options

The output shows five required options for the scan, with the RHOSTS option unconfigured.

Configure the module by setting the RHOSTS option to the 192.168.1.102 for a local environment scan:
`set RHOSTS 192.168.1.102

Then, enter _run_ to launch the scan.
Metasploit successfully identifies the Modbus server.

What is the unit ID displayed in the output? **1**
# Examining Alerts

To check if Snort has detected any activities, switch to _Terminal 1_ and press _Ctrl+Z_ to stop Snort.

By default, Snort saves alerts to the _/var/log/snort/alert_ file. To view the alert file, run:

sudo cat /var/log/snort/alert

The Unauthorized Read Request rule has been triggered, and Snort provides detailed information, including the timestamp, alert message, classification, priority, source/destination IPs and ports, TCP flags, and more about the packet that caused the alert.

### Examining Packets

Switch to _terminal 3_ and press _Ctrl+C_ to stop TShark.

Once again, use tshark to analyze captured network traffic from the/tmp/cap file and then pipes the output to more to start viewing from the line containing Modbus.

sudo tshark -r /tmp/cap -V -x | more +/Modbus

The _-r_ option specifies the input file for captured packets, the _-V_ (verbose) option shows packet details, and the _-x_ option displays packet data in hex format.

The output shows the presence of "00 00" content, and then, towards the end, there is the "04" value representing the Modbus Function Code 04. As a result, it becomes apparent that the modbusdetect module utilizes this specific request type to identify Modbus servers.

Hit the _q_ key to exit and return to the command prompt.

What does Modbus Function Code 04 mean?
**Read Input Register**

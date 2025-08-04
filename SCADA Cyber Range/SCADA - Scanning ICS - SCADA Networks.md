_ICS SCADA_ systems are part of critical infrastructure in power plants, chemical factories, oil refineries, and large complexes. As monitoring technology matured, networking capabilities aimed at improving connectivity among components introduced a new risk: network attacks. Worse, systems previously believed to be isolated have been found connected to the Internet and accessible remotely. The number of critical systems online has been steadily increasing, and many organizations now have _interconnected_ networks that allow access to network segments where the ICS SCADA systems are.

Security researchers have shown that many protocols and products are highly vulnerable, as they were often built without security in mind. Nmap should be used cautiously when scanning critical infrastructure, as many network stacks are fragile and susceptible to denial-of-service attacks from certain scanning techniques. Therefore, it is recommended to use a TCP connect scan, properly close each port probe connection, and limit the list of scanned ports.
# Starting Modbus Slave

_Modbus PLC slave_ device emulators are used in SCADA systems to simulate devices that the system can control. This helps test and develop the SCADA system without requiring the actual physical device. The Modbus PLC allows developers to ensure the system functions as expected before deploying it in a real environment.

To start the Modbus PLC slave device emulator, run the following command:
`sudo /./modbus/diagslave/x86_64-linux-gnu/diagslave -m tcp -a 123`

This command initiates the diagslave program with TCP/IP communication.

The _-m_ tcp option specifies the Modbus/TCP protocol, and the _-a_ option sets the slave address.
### Using PLCScan

Navigate to the _Kali Student machine_.

PLCScan is a Python script released by SCADA. The script is used to scan the provided IP range for the open 102 and 502 TCP ports. Moreover, the script calls for other functions if the specified ports are found. PLCScan is already pre-installed on the Student machine. To use PLCScan, navigate to the _/home/PLCScan/_ directory:
`cd /home/PLCScan`

To discover the slave device on the network and brute-force the slave ID, use the following command:
`python2 plcscan.py 192.168.1.102 --brute-uid`
# Enumerating Modbus Slave

_Nmap (Network Mapper)_ is a popular open-source network exploration, management, and security auditing tool commonly used to map out large networks, identify hosts and services on a computer network, and detect security vulnerabilities.

In ICS/SCADA pentesting scenarios, running UDP scans or using fingerprinting options are not recommended. Hence, the easiest way to discover Modbus devices on a network would be by scanning for the open 502 TCP port.

Scan the Target machine using the following command and save the output to the file open_modbus:
`sudo nmap -sT 192.168.1.102 -p 502 -oG open_modbus`

To read the content saved in the open_modbus file, use the following command:
`cat open_modbus`

The output displays that the Target machine is up and that **port 502** is open.
### Nmap Modbus Discover via NSE

One of the most powerful features of Nmap is the Nmap Scripting Engine (NSE). Many scripts, including those used in this lab, are included with the standard Nmap installation. However, custom scripts can also be created or found online.

The command below uses the modbus-discover script, which enumerates SCADA Modbus slave IDs and attempts to gather vendor and firmware information:

`sudo nmap --script modbus-discover.nse --script-args='modbus-discover.aggressive=true' -p 502 -Pn -sT 192.168.1.102`

Nmap successfully determined the slave ID and provided additional vendor and firmware information.
![[Pasted image 20250803235425.png]]
# Stopping Modbus Slave Simulator

Navigate to the _Ubuntu student machine_ running the Modbus slave, and enter _Ctrl+C_ to stop the Modbus slave.

Given that the next steps provide a walkthrough on using Conpot, which has its own SNMP server, use the following command to stop the current running SNMP server:
`sudo service snmpd stop`
### Starting Conpot

_Conpot_ is an open-source honeypot software that emulates various industrial control system (ICS) protocols, including Modbus, DNP3, and S7comm. Honeypots mimic real systems to detect and analyze attacks.

In the context of ICS, Conpot can create a realistic ICS environment to monitor and study attacks targeting ICS devices and networks. By deploying Conpot, organizations can gather intelligence on the tactics, techniques, and procedures used by threat actors.

Conpot supports multiple ICS protocols and can be customized to emulate specific devices and systems. It also includes features like logging, alerting, and plugins to enhance functionality.

The default Conpot template includes a simulated SNMP server, which will be used to demonstrate retrieving basic system information.

Log in as the conpot user:
`su conpot`

When prompted for the password, enter:
`conpot`

Finally, initiate Conpot with the following command:
`/home/conpot/.local/bin/conpot --template default --logfile /var/log/conpot/conpot.log -f --temp_dir /tmp`
# Nmap Scan S7comm

Navigate to the _Kali Linux machine_.

_Siemens S7 PLC_ devices from the S7 300/400 family use the S7comm protocol for PLC programming, diagnostics, and data exchange between PLCs and SCADA. These devices normally listen on port 102 (iso-tap). Using Nmap's scripting engine (NSE), adversaries can obtain information from the device.

The scan detects PLC devices over s7comm, a protocol used by the Siemens S7 300/400 family. Additionally, the scan gathers information about the device, such as the type, system name, serial number, and version.
`sudo nmap -Pn -sT -p102 192.168.1.102`
### Enumerating BACnet

_BACnet_ devices are commonly used to interconnect and control HVAC, power, ventilation, and other components in building automation systems. Nmap scans can gather various information from these devices, such as vendor, device name, serial number, description, location, and firmware version. Typically, the available information includes the device's location, name, description, vendor ID, and firmware version.
`sudo nmap -Pn -sU -p47808 192.168.1.102`
# Enumerating Ethernet/IP devices

Ethernet/IP is a very popular protocol used in industrial systems that employs Ethernet as the transport layer and CIP for providing services and profiles needed for applications. Ethernet/IP devices from various vendors usually operate on UDP port 44818. Using Nmap's scripting engine (NSE), we can gather information such as vendor name, product name, serial number, device type, product code, internal IP address, and version.
`sudo nmap -Pn -sU -p44818 --script enip-info 192.168.1.102`
### Msfconsole

In addition, to exploit modules, Metasploit includes hundreds of auxiliary modules that perform scanning, fuzzing, sniffing, and other essential tasks. Issue the following command to start Metasploit Framework:
`msfconsole`

Metasploit includes several SCADA-specific auxiliary modules. Use the modbusdetect, which scans the provided IP range for Modbus devices.
`use auxiliary/scanner/scada/modbusdetect`

Set the RHOST of the target to check for the Modbus device.
`set RHOST 192.168.1.102`

Now enter run to start the scan.
`run`

The scan successfully found the simulated Modbus device.
**4**
# Identifying Protocols

Using the discovered port numbers, identify the associated protocols.

There are two protocols. Protocol 1, running on port 502, is very common and often used throughout the labs.

Protocol 2 is new for these labs but is also very common. This protocol uses two of the three ports discovered. This protocol may be difficult to identify by scanning or googling, so you may need to try an actually connect to one of the ports on 192.168.1.101.

After you find the port, use the following command to view the service menu:
`H`

To exit the telnet session, enter:
`Q`
### Enumerating Devices

Now that you know what devices you are working with it is time to find some more specific information about the target.

For the device associated with Protocol 1 (port 502), find its unit ID using one of the tools you've used in the labs.
`cd /home/PLCScan`
`python2 plcscan.py 172.17.0.2 --brute-uid`

A useful tool for completing this lab is pre-installed on the system and can be found in the /home/PLCscan directory.

Run the script to read the value of Energy in hi-res.
`python2 kamstrup.py 172.17.0.2`

The script output will showcase the measured data values obtained.

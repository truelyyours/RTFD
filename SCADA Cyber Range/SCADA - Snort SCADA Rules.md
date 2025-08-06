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


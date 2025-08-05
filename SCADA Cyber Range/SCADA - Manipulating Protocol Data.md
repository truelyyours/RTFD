The purpose of this lab is to demonstrate the process of creating and injecting packets from an unauthorized computer into the process control network, thereby simulating Modbus PLC attacks. This process involves sniffing existing Modbus communications, extracting protocol decodes, loading them into a hex-edited binary file, and using a packet crafting tool to send them.

TShark is a command-line version of Wireshark, a popular open-source protocol analyzer and packet capture tool. The tool enables capturing and analyzing network traffic directly from the command line as opposed to using a GUI.

Let's start sniffing network traffic on port 502, the default Modbus communications port.

Open a new terminal, and use tshark to capture TCP traffic on port 502 and save it to the /tmp/capture file:
`sudo tshark -i lo -V -x tcp port 502 > /tmp/capture`

The -i option specifies the network interface, such as the loopback interface (lo), known as localhost.

The -V (verbose) option is used to show packet details.
The -x option prints a hex and ASCII dump of the packet data.

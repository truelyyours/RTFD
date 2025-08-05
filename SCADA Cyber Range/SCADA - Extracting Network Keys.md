Extracting network keys involves obtaining sensitive authentication details or cryptographic keys from a network or system. These keys are pivotal for ensuring secure communication and data protection within a networked environment.

Extraction can occur through methods like cryptanalysis, reverse engineering, or exploiting software/hardware vulnerabilities.

Motivations range from malicious intent to legitimate security assessments. However, unauthorized key extraction is typically illegal and unethical. To safeguard networks, robust security measures such as key rotation, encryption protocols, and vulnerability management are imperative.
### Changing Directories

_ZigBee_ is a low data rate, low power wireless network standard used to connect embedded devices like smart thermostats and medical monitors. From a security standpoint, ZigBee builds on the security framework defined in IEEE 802.15.4, which relies on proper management of symmetric keys and the correct implementation of security methods and policies.

In this lab, we'll use tools specifically developed for pentesting ZigBee communications.

First, open a _new terminal_ and change the working directory to one containing a sample ZigBee traffic capture:

`cd /killerbee-2.7.1/sample/
# Starting scapy

_Scapy_ is a powerful interactive network manipulation tool that, when used with the KillerBee toolkit, allows us to interact with and analyze ZigBee and IEEE 802.15.4 networks. In this lab, we'll use Scapy to perform tasks like packet crafting and analysis within a ZigBee network environment.
`scapy

### Importing Capture File

A sample capture file named control4-sample.pcap is present in the current directory, containing pre-captured traffic.
`scapy can be used to examine network traffic capture files as well as live traffic.

To import a sample ZigBee capture file, issue the following commands:
`inf = 'control4-sample.pcap'
`packets = rdpcap(inf)

### Looking at Packets Summary

With scapy, it's easy to examine ZigBee packets with various levels of detail.To see the summary of all captured packets, use the following command:
`packets.display()

### Examining Single Packet

To look closely at a single packet, we can use the following command (with the packet number in square brackets):

`packets[0]

We can also view the summary for a specific packet, by adding the _.summary()_ option:

`packets[0].summary()



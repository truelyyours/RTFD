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
# Extracting Network Key

Data in ZigBee protocol is encrypted, however, if a device that does not have a stored symmetric key wants to join the network, the key is transmitted to this device in cleartext and may be captured and easily extracted.

Exit _scapy_ by entering _enter()_.
Run the following command to extract the network key for the capture file:
`sudo wireshark control4-sample.pcap

To apply a filter in Wireshark, use the following command:
`zbee_nwk

To analyze further, apply another filter using the following command:
`zbee_aps

Click on one of the packets, specifically the _Transport Key_, and in the _Command Frame_, you will find the key, which is reversed.
# Examining Zigbee Packet Layers

_ZigBee Tools_ is another toolkit that was designed to expand the KillerBee functionality. It includes several Python scripts for interacting with different packet layers.

_LAYER_identifier.py_ determines the different layers found in ZigBee packets and displays the summary. Let's run it on our capture file:
`python2.7 /home/kali/LAYER_identifier.py -f control4-sample.pcap`
#### Applying Network Key

You can see that there are encrypted packets in the capture. To get more visibility into layers, we can apply the network key we extracted earlier. Re-issue the command with the -k option followed by the formatted key value:
`python2.7 /home/kali/LAYER_identifier.py -f control4-sample.pcap -k 2f397d5171525d7b726a393b726b5426
#### Extracting Network Key - KEY_identifier

The _KEY_identifier.py_ script will also quickly extract the key:
`python2.7 /home/kali/KEY_identifier.py -f control4-sample.pcap
#### Extracting Network Key - zbdsniff

Finally, zbdsniff (also part of the KillerBee suite) can also extract network keys and fromat them to be used with Wireshark.
Run it on our capture file as follows:
`python2.7 /home/kali/zbdsniff.py control4-sample.pcap

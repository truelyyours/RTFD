In this lab session, participants will explore in-depth, utilizing various tools and techniques to analyze packets. The primary objective is to proficiently identify SCADA protocols, port numbers, and unique device identifiers (UIDs) that facilitate SCADA communications.
The core task involves identifying two SCADA devices actively engaged in network communication. Participants will thoroughly analyze their IP addresses, understand the client device involved, and successfully ascertain its UID. These activities will demonstrate the participants' network analysis and cyber forensics expertise.
Additionally, the exercise will comprehensively examine function codes used in SCADA communication. Through this process, participants will enhance their ability to decipher packet headers and accurately interpret protocol-specific intricacies with precision.
# Starting Conpot

_Conpot_ is an open-source honeypot software that emulates several protocols of industrial control systems (ICS), including Modbus, DNP3, and S7comm. Honeypots are designed to resemble actual systems; however, they are set up to detect and analyze attacks.

With regard to industrial control systems (ICS), Conpot can be used to create a realistic-looking ICS environment for detecting and analyzing attacks against ICS devices and networks. By deploying Conpot on a network, organizations can gather intelligence on the tactics, techniques, and procedures employed by threat actors targeting their ICS systems or devices.

Conpot supports a range of ICS protocols and can be tailored to emulate specific devices and systems. Moreover, Conpot includes several features, such as logging, alerting, and plugins, that further enhance its functionality. The default template of Conpot incorporates a simulated SNMP server, which will be used to demonstrate the process of retrieving fundamental system information.

To begin, log in as the user conpot:
`su conpot`

When prompted for the password, enter conpot:
`conpot`

Lastly, to initiate Conpot, enter the following command:
`/home/conpot/.local/bin/conpot --template default --logfile /var/log/conpot/conpot.log -f --temp_dir /tmp`
# Identifying Protocols

The role of the root user involves creating a directory owned by the root and closely overseeing network activity across all interfaces.
Open a new terminal and enter the following command to log in as a root user:
`sudo su`

When prompted for the password, type:
`passw0rd!`

Commence the packet sniffing process to conduct a comprehensive analysis of network traffic.

Navigate to the _files_ directory and use the following command:
`cd files`
`tshark -r capture1.pcap`

### Enumerating Hosts

Based on the results of the analysis, it has been determined that two SCADA devices are actively communicating within the network. The IP addresses of these devices are _192.168.6.133_ and _192.168.6.134_.

---

What specific ICS/SCADA protocol and associated port number are used to communicate within the captured network?
 **Modbus on port 502**

NOTE: The portal does not accpet this answer, idk what the format is. SO this remains incomplete!




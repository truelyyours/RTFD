Adversaries perform SNMP reconnaissance to gather information about a target network that they can use to plan and execute future attacks. SNMP allows access to a wide range of system and network information, including details about network devices, hosts, operating systems, and applications. Adversaries can use this information to identify vulnerabilities and weaknesses that they can exploit and gain a better understanding of the topology and configuration of the target network.

SNMP reconnaissance can also help adversaries identify potential attack targets, such as devices with outdated firmware or unpatched software. Additionally, the information gathered during SNMP reconnaissance can be used to craft convincing phishing emails or other social engineering attacks that appear legitimate, in turn increasing the likelihood of success.
# Simple Network Management Protocol

SNMP, an acronym for Simple Network Management Protocol, is a widely used protocol for monitoring and managing devices on a network. The protocol is designed to facilitate the exchange of information between a management system, typically a network administrator's computer, and devices such as servers, routers, and switches.

SNMP enables network administrators to gather information about the status and performance of devices on the network. This information can include data on configuration settings, traffic flow, and system utilization, among other metrics. By collecting this data, administrators can identify issues and take corrective measures to keep the network running optimally.

The protocol uses a client-server model, where the management system acts as the client and the monitored devices act as servers. SNMP messages containing information about the devices being monitored are sent between the management system and the servers. The servers respond to these messages with current status and performance data.

To ensure that the SNMP service is not running, enter the following command:
`sudo service snmpd status`

If it is running run the following command to stop it:
`sudo service snmpd stop`
# Starting Conpot

Conpot is an open-source honeypot software that emulates several industrial control systems (ICS) protocols, including Modbus, DNP3, and S7comm. Honeypots are designed to resemble actual systems but are set up to detect and analyze attacks.

With regard to industrial control systems (ICS), Conpot can be utilized to create a realistic-looking ICS environment to detect and analyze attacks against ICS devices and networks. By deploying Conpot on a network, organizations can gather intelligence on the tactics, techniques, and procedures employed by threat actors targeting ICS systems or devices.

Conpot supports a range of ICS protocols and can be tailored to emulate specific devices and systems. Moreover, Conpot entails several features, such as logging and alerting, as well as plugins that enhance its functionality.

The default template of Conpot incorporates a simulated SNMP server, which will be utilized to demonstrate the process of retrieving fundamental system information.

First, log in as the user conpot:
`su conpot`

When prompted for the password, enter conpot:
`conpot`

Lastly, to initiate Conpot, enter the following command:
`/home/conpot/.local/bin/conpot --template default --logfile /var/log/conpot/conpot.log -f --temp_dir /tmp`
# Walking SNMP

The **snmpwalk** command is used to obtain information regarding target devices. The command is a tool used for querying SNMP enabled devices such as routers, switches, and servers. Its purpose is to provide a hierarchical list of all available data present in the Management Information Base (MIB) of an SNMP device.

The MIB serves as a database of information that stores details regarding a device's performance, configuration, and status. The **snmpwalk** command performs a comprehensive scan of this database and returns a comprehensive list of all available information. Its usage can be particularly beneficial when troubleshooting network issues, assessing a device's health, or monitoring its performance.

Open a new terminal, and run the following command:
`snmpwalk -v 1 -c public student`

The "-v" flag specifies the SNMP version. Community strings function as passwords, securing the data accessible through SNMP. It is worth noting that most SNMP installations have the read-only community string set to "public" by default.
# Gathering System Information - System Description (OID)

The output from the previous command can be overwhelming due to the significant amount of information that it provides. To narrow down the search to specific information, administrators can use the snmpwalk command.

In order to determine the type of system they are working with, administrators can query the system description using a specific code known as an Object Identifier (OID). The OID for the system description is 1.3.6.1.2.1.1.1. This unique code represents a branch in the hierarchical tree structure of OIDs and corresponds to the system description.

To query the system description using snmpwalk, enter the following command:

snmpwalk -v 1 -c public student 1.3.6.1.2.1.1.1

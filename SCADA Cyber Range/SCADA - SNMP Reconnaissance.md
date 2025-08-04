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

To query the system description using **snmpwalk**, enter the following command:
`snmpwalk -v 1 -c public student 1.3.6.1.2.1.1.1`
# Management Information Bases (MIBs)

Management Information Bases (MIBs) are an essential part of SNMP that help define the structure of the management information exchanged between SNMP agents and managers. MIBs provide a way to describe data that can be managed by SNMP and how that data is organized.

MIBs are like a tree structure that consists of a series of objects, each with its own object identifier (OID) that uniquely identifies them. Each object represents a piece of data that can be managed, like the status of a network interface, CPU utilization, or the amount of available memory.

SNMP agents use MIBs to store and organize the management information that they provide to SNMP managers. SNMP managers use MIBs to identify and retrieve the management information they need from SNMP agents. Using MIBs, agents and managers can communicate in a standardized way, ensuring they understand the structure and meaning of the exchanged data.

On the other hand, adversaries can use SNMP queries to access the MIB content directly from SNMP-managed devices. By collecting network information, adversaries can map networks and plan future targeted attacks.
# Gathering System Information - System Description (MIB)

The output of the previous command indicates that MIB resolution is enabled on the target system, simplifying the process by using text-based MIB names instead of typing in lengthy numeric OID values.

To demonstrate this functionality, enter the following command:
`snmpwalk -v 1 -c public student sysDescr`

The results should be identical to those in the fifth step.
### Gathering System Information - Hostname

The hostname of a target system can be retrieved by using the MIB "sysName" name. To do so, enter the following command:
`snmpwalk -v 1 -c public student sysName`

Once the command is executed, an SNMP query is sent to the device to retrieve its hostname, and the result is displayed in the output. In the example provided, the result of the query shows that the system is a CP 443-1 EX40 communication processor.
### Getting System Uptime

To obtain dynamic information, such as the system uptime of a target device, users can utilize the sysUpTime MIB.

The sysUpTime MIB retrieves the amount of time that has passed since the device was last booted or restarted.
`snmpwalk -v 1 -c public student sysUpTime`

The value returned by the sysUpTime MIB is expressed in hundredths of a second and provides a way to track the availability of network devices. This information aids network administrators in determining the amount of time a device has run without interruption and troubleshooting issues related to the device's uptime.
# Stopping Conpot

By default, Conpot provides limited information. In order to obtain more comprehensive data, it may be necessary to analyze an actual system, such as our own machine. To stop Conpot, access the terminal where it is currently running and press Ctrl+C. Close the terminal window Conpot had been running in.

To proceed with the subsequent steps, starting the SNMP server is necessary. To do so, enter the following command:

sudo service snmpd start

### Starting Modbus Slave Simulator

The current task involves generating SCADA activity on the system and subsequently examining whether it can be identified via snmpwalk. To achieve this, the Modbus PLC slave device emulator can be utilized.

Modbus PLC slave device emulators are used in SCADA systems to simulate a device that the system can control. This helps in testing and developing the SCADA system without having the actual physical device. Modbus PLC allows the developer to ensure that the system is working as expected before deploying it to the real environment.

To start the Modbus PLC slave device emulator, run the following command:
`sudo /modbus/diagslave/x86_64-linux-gnu/diagslave -m tcp`
# Walking SNMP

To extract additional information from the target system, rerunning the snmp walk command with the "public" community string is **necessary**. The SNMP version used by the server must be specified. In this case, the SNMP version is 2c. Consequently, open a new terminal and enter the following command:
`snmpwalk -v 2c -c public student`

The output produced by the command would include important data. However, it is important to note that most systems typically provide only fundamental system information to external hosts employing the "public" community string.
## Hydra

Hydra is a network login cracking tool that can be used to brute-force the login credentials (username and password) of a remote server or network service.

Hydra is a command-line tool that is run on a local machine and used to remotely connect to a target system. To brute-force login credentials, Hydra attempts a vast number of login combinations until the correct one is discovered. The tool is capable of running on multiple platforms, including Linux, macOS, and Windows, and can use a variety of protocols and methods to perform the attack.

Adversaries may use Hydra to bruteforce SNMP community profiles by attempting to log in to an SNMP device using a list of known or common SNMP community strings. Hydra is a popular password-cracking tool that supports various protocols, including SNMP, and can automate the process of attempting different community strings until a successful login is achieved

To list the available options for Hydra, run the following command:
`hydra -h`
# Cracking Community String

A specific community string is required to access SNMP from external hosts; however, this specific string is unknown. As with passwords, finding the community string through various cracking techniques is necessary. Fortunately, various tools are available to assist in this endeavor, and one of them is Hydra.

hydra -P /opt/conpot/snmp_default_pass.txt student snmp

The "-p" option specifies the path of the wordlist that Hydra must utilize to attempt to guess the correct community string for the SNMP device.

In this scenario, it did not take long for the THC Hydra to discover the "public" community string. Additionally, the tool also revealed another community within seconds. Given that the correct community string has been found, SNMP can now be accessed from external hosts.

We find another key "secret" and we can again do SNMP walk:
`snmpwalk -v 2c -c secret student | more`
### List Running Processes

The **hrSWRunName** MIB is a standard Management Information Base (MIB) that allows network administrators to monitor the resources of a host computer using SNMP. This MIB is used to retrieve the names of running processes on a host computer along with their Process IDs. The term **hrSWRunName** is formed by combining "Host Resources" (hr) and "Software Running" (SWRun), and the "Name" suffix indicates that this MIB returns the names of the running software on the target device.
`snmpwalk -v 2c -c secret student hrSWRunName`
# Network Information

SNMP is a protocol that offers extensive information, and it is worthwhile for network administrators to invest time in exploring the available OIDs/MIBs to understand the information they can provide. In this step, network information related to the target device that was not previously accessible using the "public" community string will be retrieved.

The **tcpConnLocalPort** MIB is a specific OID that enables network administrators to obtain information about open TCP ports on a network device. This MIB retrieves the local port numbers associated with active TCP connections on the target device.

To retrieve a list of open TCP ports on the device, execute the following command:
`snmpwalk -v 2c -c secret student tcpConnLocalPort`
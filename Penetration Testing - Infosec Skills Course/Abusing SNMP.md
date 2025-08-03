SNMP (Simple Network Management Protocol) is widely used for managing network devices such as *routers, switches, servers, and workstations*. It allows administrators to monitor network performance, track devices, and configure settings remotely. SNMP uses "community strings" as a simple form of authentication to control access, commonly set as public (read-only) or private (read-write).

`sudo nmap 192.168.1.101 -Pn -sU`
# Scanning Modules
Start by launching the Metasploit console with the following command:
`msfconsole`

Next, search for SNMP-related modules by running:
`search snmp`

This command will display over 30 modules, but since our focus is on information gathering and not exploitation, we don't need any exploit modules.
To narrow down the results to modules specifically for enumeration, run:
`search snmp enumeration`

This refined search provides a list of 11 modules dedicated solely to information gathering, making the process more efficient and targeted.
# Execution: SNMP System Enumeration
In this step, we will use the snmp_enum module to gather detailed information about the target system. This module retrieves general SNMP data, such as system details and network configurations.

To select the module, you can use either of the following commands:
`use 7`
Or
`use auxiliary/scanner/snmp/snmp_enum`

Once selected, check the module's requirements by running:
`show options`

From the output, you'll see that the only mandatory option is RHOSTS, which specifies the target IP address. Set the target by running:
`set RHOSTS 192.168.1.101`

With the target configured, the module is ready to run. Execute it with the following command:
`run`

After running the module, review the output to analyze the retrieved information, which may include system uptime, hostname, network interfaces, and more.




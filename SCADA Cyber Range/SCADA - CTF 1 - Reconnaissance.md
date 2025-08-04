Reconnaissance is the process of gathering information about a target system or network to identify potential vulnerabilities or attack vectors. This process can be conducted through both passive and active methods, and it can be utilized by both attackers and defenders. Security measures, such as firewalls and regular audits, can help protect against reconnaissance attacks.

_Passive_ reconnaissance involves gathering information without directly interacting with the target system or network. This can be achieved by searching publicly available information sources, such as social media or corporate websites. _Active_ reconnaissance, on the other hand, involves direct interaction and is achieved through various methods, such as scanning for open ports and attempting to exploit known vulnerabilities.
### SSH Connection

_Secure Shell (SSH)_ is a cryptographic network protocol used to secure connections between a client and a server. It is often used to remotely access and manage servers, providing a secure connection and allowing authorized users to log in to a server and execute commands remotely.

On the _Student machine_, use the following command to connect to the _target machine_ via ssh:
`ssh admin@172.17.0.2`

Type _yes_ when prompted to continue.

Enter _admin_ when prompted for the password.

Which reconnaissance type involves direct interaction, like scanning for open ports or exploiting vulnerabilities?
*Active*
# Starting Conpot

_Conpot_ is an open-source honeypot software that emulates several protocols used in industrial control systems (ICS), including Modbus, DNP3, and S7comm. Honeypots are designed to resemble real systems, but they are configured to detect and analyze attacks.

In the context of ICS, Conpot can be used to create a realistic ICS environment for detecting and analyzing attacks against ICS devices and networks. By deploying Conpot on a network, organizations can gather intelligence on the tactics, techniques, and procedures employed by threat actors targeting their ICS systems or devices.

Conpot supports a range of ICS protocols and can be customized to emulate specific devices and systems. Additionally, Conpot includes several features, such as logging, alerting, and plugins, that further enhance its functionality. The default Conpot template incorporates a simulated SNMP server, which will be used to demonstrate the process of retrieving basic system information.

To start Conpot, enter the following command:
`sudo /usr/local/bin/conpot -t kamstrup_382 --logfile /var/log/conpot/conpot.log -f &`

After 10-15 seconds press enter to proceed with the next command.
### Starting Modbus Slave Simulator

_Modbus PLC_ slave device emulators are used in SCADA systems to simulate devices that the system can control. This allows testing and development of the SCADA system without the need for the actual physical device. Modbus PLC helps developers ensure the system works as expected before deployment in a real environment.

To start the Modbus PLC slave device emulator, run the following command:
`sudo /./modbus/diagslave/x86_64-linux-gnu/diagslave -m tcp -a 248`

The -m tcp option specifies the Modbus/TCP protocol.
The -a option sets the slave address (or unit identifier).
# Port Scanning

Now that the initial setup of the lab is completed, this CTF is all about reconnaissance.

To complete this step you will need to open a _new terminal_ and scan open ports on the target to discover open TCP ports commonly used by ICS/SCADA protocols.


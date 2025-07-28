_Snort_ is software used for real-time monitoring of network traffic. It is an open-source _Intrusion Detection System_ and _Intrusion Prevention System_.

- IDS (Intrusion Detection System): An IDS monitors network traffic for suspicious activity. When it detects something unusual, it raises an alert but does not block the traffic.
- IPS (Intrusion Prevention System): An IPS monitors network traffic for suspicious activity and can actively block or intercept malicious traffic to prevent threats.

Snort by defualt has 5 rules :

- Alert
- Dynamic
- Pass
- Log
- Activate

The first thing we do every time we have a target is to enumerate open ports. We can do this using nmap (Network Mapper):
`nmap 192.168.1.100`

This will reveal the open ports on the target. Using the _Ubuntu machine with Snort_, we will create rules for these services to trigger an alert whenever someone attempts to connect to them.

First open a terminal and navigate to the _snort directory_ on the _ubuntu machine_ using the following command:
`cd /etc/snort/rules`

To begin adding rules to the configuration file, enter the following command:
`sudo nano /etc/snort/rules/local.rules`

When propted for a password, enter:
*passw0rd!*

The syntax for implementing a rule is:

> alert icmp any any -> any any (msg:"Message"; sid:1000001;)

Note: _alert_ is the type of rule, _icmp_ is the protocol, _any_ is the source IP, _any_ is the source PORT, _"->"_ is the direction, _any_ is the destination IP, _any_ is the destination PORT, _msg_ is the message displayed for this alert and _sid_ a unique number identifier for the rule.
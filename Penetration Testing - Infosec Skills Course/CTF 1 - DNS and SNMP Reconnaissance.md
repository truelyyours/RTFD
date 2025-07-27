# Enumeration
Scan: `nmap 192.168.1.100`
DNS IP lookup: `nslookup 192.168.1.100`
Or can use dig: `dig @192.168.1.100 infolab.com`

![[Pasted image 20250726205447.png]]

Some of the most commonly used records are A (IPv4), AAAA (IPv6), CNAME(Canonical Name), MX(Mail exchange), NS(Name server) etc.
# Enumeration SNMP
_NMP (Simple Network Management Protocol)_ is a protocol used to manage network devices. By default, it uses _UDP_ for transmission. To check the status of this service, execute the following command:

- _MIB ( Management Information Base)_ is a database which contains information about the device, configuration settings and more.
    

_MIB_ contain structured data in the form of _OIDs (Object Identifier)_, which are hiearchical paths that lead to specific peaces of information.
`sudo nmap -sU -p 161 192.168.1.100`

More info using snmp

`snmp-check -c public 192.168.1.100`
The _-c_ flag used for community strings, which in this case is public.

`The _-v_ flag is used for version, _-c_ used for community string and the last number is _oid_`
The _-v_ flag is used for version, _-c_ used for community string and the last number is _oid_

Then we will have to brute force password on these services and access files.
# Mitigation

Some best practices which we can use for those services are as following:

DNS Best Practices:

- Restrict Zone Transfers: Limit zone transfers (AXFR) to trusted IP addresses only.
- Hide Internal Information: Avoid exposing sensitive internal hostnames in public DNS. Use separate internal and external zones.
- Monitor DNS Logs: Regularly review DNS logs for suspicious or abnormal query patterns.

SNMP Best Practices:

- Change Default Community Strings: Replace default strings (public, private) with strong, unique ones. Use SNMP v3 for enhanced security authentication and encryption.
- Use SNMP v3: Upgrade to SNMP v3 if possible. Disable SNMP v1/v2 if not needed.
- Monitor SNMP Requests: Regularly check SNMP logs for unusual or unauthorized access attempts.
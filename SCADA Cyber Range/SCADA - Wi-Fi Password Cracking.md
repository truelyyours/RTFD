Wi-Fi password cracking refers to the process of accessing a wireless network by breaking its security protocols. This can be achieved through methods like brute-force attacks, dictionary attacks, and exploiting vulnerabilities in the network's security.

_Brute-force_ attacks involve systematically trying every possible combination of characters until the correct password is found. _Dictionary attacks_ use a pre-compiled list of common passwords and attempt to match them against the network. Additionally, Wi-Fi vulnerabilities, such as the WPA2 vulnerability (KRACK), can be exploited to intercept and read Wi-Fi traffic.
# Wi-Fi Pentesting with Aircrack-ng
Hackers can exploit wireless communication to breach ICS/SCADA networks, often targeting the WEP and WPA2 security protocols commonly used in Wi-Fi networks to infiltrate such systems.

_Aircrack-ng_ lists the following focus areas for its product:

- **Monitoring**: Packet capture and export to text files for processing by third-party tools.
- **Attacking**: Replay attacks, de-authentication, fake access points, and others via packet injection.
- **Testing**: Checking Wi-Fi cards and driver capabilities (capture and injection).
- **Cracking**: WEP and WPA PSK (WPA 1 and 2).
- The main focus here is on cracking.

Some preparatory work has already been done using Aircrack-ng tools (airmon-ng, airodump-ng, and aireplay-ng) to discover wireless networks and capture authentication packets containing the key.

The next step is to extract the key from the captured traffic. Analyze the captured files and list the contents of the wifihacking directory.
`cd /
`ls wifihacking
# TShark

_TShark_ is a command-line network protocol analyzer and packet sniffer that captures and analyzes network traffic in real time. Part of the Wireshark toolset, it works on Linux, Windows, and macOS.

TShark supports various formats (pcap, pcapng, tcpdump) and protocols (TCP, UDP, DNS, HTTP, SMTP). It allows filtering based on criteria like protocol, IP addresses, and packet contents. Common uses include network troubleshooting, performance analysis, and security monitoring.

To list available options for TShark, run:
`tshark -h`
### Analyzing Capture Files With TShark

TShark can be used to explore the contents of files in the wifihacking directory.

To open the wepcapture.cap file, use the following command in the terminal, and pipe the output into more to avoid flooding the terminal with data:

`tshark -r wifihacking/wepcapture.cap | more

It may take a moment for the packet data to appear. Press Enter or Space to scroll through the file, or press Q to quit.

Finding the key among all the data can be difficult. This is where Aircrack-ng simplifies the process.
# WEP and WPA2

_WEP (Wired Equivalent Privacy)_ and _WPA2 (Wi-Fi Protected Access 2)_ are two wireless network security protocols.

_WEP_, introduced in the late 1990s, was a basic encryption standard for Wi-Fi networks. However, it is now considered insecure due to vulnerabilities like packet injection and brute-force attacks, which can intercept and decrypt Wi-Fi traffic. As a result, WEP is no longer recommended for securing networks.

_WPA2_, the current recommended standard, uses the stronger _Advanced Encryption Standard (AES)_ and includes additional security features such as key management. WPA2 also requires stronger passwords, making it more difficult for attackers to crack network credentials.

In summary, WEP is no longer reliable for securing Wi-Fi networks, while WPA2 is a much stronger and more secure option.
# Cracking WEP

To obtain the password of the access point from the WEP capture file, run the following command:
`aircrack-ng wifihacking/wepcapture.cap

Allow the command to run for some time as **aircrack-ng** tries to calculate the key using the provided .cap file.

### Cracking WPA2

_Airmon-ng_, _airodump-ng_, and _aireplay-ng_ were used to capture the 4-way handshake packets containing the WPA2 PSK. The captured traffic is stored in a file named wpa2capture.cap. To crack the key using Aircrack-ng, a wordlist is required. Kali Linux includes several wordlists, and one will be used for this example.

aircrack-ng -w /usr/share/wordlists/rockyou.txt wifihacking/wpa2capture.cap

_-w_ specifies the dictionary file path

> Note that if the password isn't in the dictionary file, Aircrack-ng cannot crack it and may display a message like "pass-phrase is not present in the dictionary." To mitigate such attacks, it's recommended to use strong passwords and WPA2 encryption. Additional countermeasures include enabling MAC address filtering and hiding the network SSID.

What is the password obtained after cracking the WPA2 key?
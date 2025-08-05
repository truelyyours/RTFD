Wi-Fi password cracking refers to the process of accessing a wireless network by breaking its security protocols. This can be achieved through methods like brute-force attacks, dictionary attacks, and exploiting vulnerabilities in the network's security.

_Brute-force_ attacks involve systematically trying every possible combination of characters until the correct password is found. _Dictionary attacks_ use a pre-compiled list of common passwords and attempt to match them against the network. Additionally, Wi-Fi vulnerabilities, such as the WPA2 vulnerability (KRACK), can be exploited to intercept and read Wi-Fi traffic.
# Wi-Fi Pentesting with Aircrack-ng
Hackers can exploit wireless communication to breach ICS/SCADA networks, often targeting the WEP and WPA2 security protocols commonly used in Wi-Fi networks to infiltrate such systems.

_Aircrack-ng_ lists the following focus areas for its product:

- Monitoring: Packet capture and export to text files for processing by third-party tools.
- Attacking: Replay attacks, de-authentication, fake access points, and others via packet injection.
- Testing: Checking Wi-Fi cards and driver capabilities (capture and injection).
- Cracking: WEP and WPA PSK (WPA 1 and 2).
- The main focus here is on cracking.

Some preparatory work has already been done using Aircrack-ng tools (airmon-ng, airodump-ng, and aireplay-ng) to discover wireless networks and capture authentication packets containing the key.

The next step is to extract the key from the captured traffic. Analyze the captured files and list the contents of the wifihacking directory.

cd /

ls wifihacking
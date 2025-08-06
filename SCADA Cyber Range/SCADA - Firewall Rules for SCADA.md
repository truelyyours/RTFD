Previous laboratory exercises have highlighted that many SCADA (Supervisory Control and Data Acquisition) protocols suffer from significant security deficiencies. This vulnerability underscores the critical need for robust protective measures, with _firewalls_ being a primary defense. While commercial firewalls tailored for SCADA environments are available, open-source firewall applications can also play a vital role in enhancing SCADA security.

This lab's objective is to provide hands-on experience with iptables, a powerful command-line firewall tool designed for Linux environments. Through this exercise, you will gain a comprehensive understanding of how iptables can regulate network traffic and strengthen security.
# Viewing rules

To begin, let's determine whether any rules have been configured already. Run the following command In the Student machine.
`sudo iptables -L

The output will confirm the absence of any configured rule.
### Creating Rule Chain

In this step, a new rule chain will be created to selectively focus on specific packets in the network traffic. These packets will be logged with a unique identifier before being removed. The new chain will be named _LOGGING_.

Enter the following command to create the rule chain:
`sudo iptables -N LOGGING

### Selecting Packets: Incoming Traffic

First, opting to capture all incoming TCP traffic destined for Modbus.
`sudo iptables -A INPUT -p tcp --dport 502 -j LOGGING

The _-A_ option adds a rule to the INPUT chain, the _-p_ option specifies the protocol, _--dport_ identifies the destination port, and _-j_ sets the action, directing the packet to the LOGGING chain instead of the standard actions (ACCEPT, REJECT, DROP, LOG).
# Selecting Packets: Outgoing Traffic

Next, create a rule for the outbound traffic. This rule involves including packets originating from source port _--sport 502_ into the OUTPUT chain and subsequently routing them through the LOGGING chain:
`sudo iptables -A OUTPUT -p tcp --sport 502 -j LOGGING

### Logging and Dropping Packets

First, log the selected packets with the following rule, which records all packets passing through the LOGGING chain:
`sudo iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped-Modbus: "

The _--log-prefix_ option adds a label to the logged packets, making them easier to identify later.

Next, drop all logged packets with this rule:
`sudo iptables -A LOGGING -j DROP

This rule ensures that after logging, any packet reaching the LOGGING chain is dropped and not further processed.

Finally, list all the newly created rules:
`sudo iptables -L
# Starting Modbus Slave Simulator

Modbus PLC slave device emulators are used in SCADA systems to simulate devices that the system can control. This enables testing and development of the SCADA system without needing the physical device, allowing developers to verify system functionality before deployment.

To start the Modbus PLC slave device emulator, run the following command:
`sudo /modbus/diagslave/x86_64-linux-gnu/diagslave -m tcp -a 123
### Reading Registers

Open a new terminal and initiate a Read Registers request via Modpoll, which simulates a Modbus master:
`sudo /modbus/modpoll/x86_64-linux-gnu/modpoll -m tcp -a 123 -r 100 -c 5 -1 192.168.1.100

The request should fail, confirming the effectiveness of the firewall rule in blocking the connection.

Next, stop the Modbus slave simulator. In the other terminal, terminate Diagslave by pressing _Ctrl+C_, which will halt the process and return you to the command prompt.
### Targeting Specific Queries

Flush the previously added rules with the following command:
`sudo iptables -F

To create a new rule, use the u32 match feature, which extracts 32 bits from a packet at a specific location and compares it to a given value. In the command below, u32 locates the Modbus message's byte 7, searching for function code 43 (Read device ID), where 7>>24&0xff=0x2b.

Enter this command in the terminal:
`sudo iptables -A INPUT -p tcp -s 192.168.1.101 --dport 502 ! -f -m u32 --u32 "0>>22&0x3c @12>>26&0x3c@ 7>>24&0xff=0x2b" -j DROP

>Here’s an explanation of each part:
>  - `-A INPUT`: Append this rule to the INPUT chain (it will check incoming packets).
>- `-p tcp`: Match TCP protocol packets.
>- `-s 192.168.1.101`: Match packets coming from source IP 192.168.1.101.
>- `--dport 502`: Match packets destined to TCP port 502, typically used for Modbus protocol (an ICS/SCADA protocol).
>- `! -f`: Match packets that are **not** fragments (the `-f` matches fragmented packets beyond the first fragment, so `! -f` means this is the first fragment or non-fragmented packet).
>- `-m u32`: Use the "u32" match module, which allows matching based on specific bits or bytes at arbitrary locations within the packet payload.
>- `--u32 "0>>22&0x3c @12>>26&0x3c@ 7>>24&0xff=0x2b"`: This is the key complex u32 expression. Breaking it down:
    - `0>>22&0x3c`:  
        The expression `0>>22` means right-shift 0 bits by 22 (effectively extracting bits), mask it with `0x3c` (binary mask). In u32 module syntax, the number before `>>` refers to the offset (byte position 0 in the packet).  
        This expression calculates the IP header length—it extracts the lower nibble (4 bits) of the first byte of the IP header (byte 0), then multiplies by 4 (shift right 22 bits, mask 0x3c). The result is the size of the IP header in bytes.
    - `@12>>26&0x3c@`:  
        The `@` symbol means "jump" — it uses the IP header length from the prior expression to jump into the packet payload dynamically. Then at (IP header length + 12 bytes), it performs a right shift and mask again (similar operation) to go deeper.
    - `7>>24&0xff=0x2b`:  
        This checks byte 7 in the TCP header (right shifted 24 bits and masked with 0xff to extract a byte) and compares it to the value `0x2b` (which is decimal 43).

>Putting it in more accessible terms:
>- The rule dynamically calculates the IP header length.
>- Using that, it jumps to TCP header data.
>- It examines the 8th byte (offset 7) of the TCP header for the specific value `0x2b`.
>- If the value does not match, and other conditions are met, it will DROP the packet.
# Testing Rules - Allowed Traffic

_PLCScan_ is a Python script released by SCADA. The script is used to scan the provided IP range for the open 102 and 502 TCP ports. Moreover, the script calls for other functions if the specified ports are found. PLCScan is already pre-installed. To use PLCScan, on the other terminal navigate to the /PLCScan/ directory:
`cd PLCScan/

Initially, examine whether the rule isn't discarding packets that should be permitted. Execute the command with the --brute-uid option, designed to send Read device ID requests to all identified devices:
`python2 plcscan.py 192.168.1.100 --brute-uid

### Testing Rules - Blocked Traffic

On the Target machine, open a new terminal and navigate to the /PLCScan/ directory:
`cd /home/PLCScan/

Enter the following command to start scanning:
`python2 plcscan.py 192.168.1.100 --brute-uid

What output would be displayed in the terminal?



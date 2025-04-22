As usual, before starting remember to add `10.10.11.47 linkvortex.htb` entry in your `/etc/hosts` file.

# Task 1
We first of course check for open ports using `nmap -A -sC 10.10.11.47` 
```sh
┌──(truelyyours㉿kali)-[~/Documents/kali_vpn]
└─$ nmap -A -sC 10.10.11.47 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-21 16:51 EDT
Nmap scan report for linkvortex.htb (10.10.11.47)
Host is up (0.16s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:f8:b9:68:c8:eb:57:0f:cb:0b:47:b9:86:50:83:eb (ECDSA)
|_  256 a2:ea:6e:e1:b6:d7:e7:c5:86:69:ce:ba:05:9e:38:13 (ED25519)
80/tcp open  http    Apache httpd
|_http-generator: Ghost 5.58
|_http-title: BitByBit Hardware
|_http-server-header: Apache
| http-robots.txt: 4 disallowed entries 
|_/ghost/ /p/ /email/ /r/
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 53/tcp)
HOP RTT      ADDRESS
1   80.69 ms 10.10.16.1
2   40.17 ms linkvortex.htb (10.10.11.47)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.76 seconds

```
We can see that it has **2 tcp ports open** hosting an Apache Server. The domain name is `http://linkvortex.htb`.  This site uses4 Ghost 5.58 and looking up for its vulnerabilities, I found (https://cve.mitre.org/cgi-bin/cvename.cgi?name=2023-40028)[CVE-2023-40028]. This can be exploited to perform an arbitrary file read of any file on the host operating system.
![[Pasted image 20250421175302.png]]
# Task 2
When we visit the URL, we simply see it is a blog by someone (BitByBit Hardware) and all posts are by `admin`. There is a **Sign Up** button at the end of page but it does nothing👍! This tasks asks for any subdomain that is valid for `linkvortex.htb`. I use `fuff` to find `vhost` that may be available
```bash
┌──(truelyyours㉿kali)-[~/Documents/kali_vpn]
└─$ ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt -u http://linkvortex.htb -H "Host: FUZZ.linkvortex.htb" -fw 14   

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://linkvortex.htb
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt
 :: Header           : Host: FUZZ.linkvortex.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
 :: Filter           : Response words: 14
________________________________________________

dev                     [Status: 200, Size: 2538, Words: 670, Lines: 116, Duration: 53ms]
:: Progress: [20478/20478] :: Job [1/1] :: 862 req/sec :: Duration: [0:00:24] :: Errors: 0 ::
```
Looks like we have `dev.linkvortex.htb` available. So, I add it to `/etc/hosts` and checkout the URL.
It simply shows a "Launching Soon" banner. Looks link someone has been procrastinating 🫣.
![[Pasted image 20250421175247.png]]
# Task 3
Now, we are looking for any `directory` exposed by the `dev` domain. So, I use `gobuster` for directory traversal.

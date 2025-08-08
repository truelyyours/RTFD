Connect to THM vpn and do an `nmap` scan:
`nmap -A -sV -sC 10.201.28.2`
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-08-07 20:09 EDT
Nmap scan report for 10.201.28.2
Host is up (0.17s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.2.0.140
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-title: Apache2 Ubuntu Default Page: It works
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)

```
TODO: -- How to enumerate Anonymous FTP

First step for HTTP service is to open in browser and do a directory traversal. The site is just tha landing page of apache server!
![[Pasted image 20250807211833.png]]
However, `gobuster` yields interesting results:
```
┌──(truelyyours㉿kali)-[~/tryhackme/SimpleCTF]
└─$ gobuster dir -w /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt -u http://10.201.28.2 -e 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.201.28.2
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
http://10.201.28.2/.htpasswd            (Status: 403) [Size: 295]
http://10.201.28.2/.htaccess            (Status: 403) [Size: 295]
Progress: 1417 / 20479 (6.92%)[ERROR] Get "http://10.201.28.2/_sitemap": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
[ERROR] Get "http://10.201.28.2/_styles": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
http://10.201.28.2/robots.txt           (Status: 200) [Size: 929]
http://10.201.28.2/server-status        (Status: 403) [Size: 299]
http://10.201.28.2/simple               (Status: 301) [Size: 311] [--> http://10.201.28.2/simple/]
Progress: 20478 / 20479 (100.00%)
===============================================================
Finished
===============================================================
```

I see a `/simple` endpoint that can be accessed. It is a "CMS Made Simple" home page. At the bottom of the page I can see the version `2.2.8`. So, I reach for it using `searchsploit`.
```shell                              
┌──(truelyyours㉿kali)-[~/tryhackme/SimpleCTF]
└─$ searchsploit cms 2.2.8                                                                              
---------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                |  Path
---------------------------------------------------------------------------------------------- ---------------------------------
Bolt CMS < 3.6.2 - Cross-Site Scripting                                                       | php/webapps/46014.txt
CMS Made Simple < 2.2.10 - SQL Injection                                                      | php/webapps/46635.py
Composr-CMS Version <=10.0.39 - Authenticated Remote Code Execution                           | php/webapps/51060.txt
Concrete CMS < 5.5.21 - Multiple Vulnerabilities                                              | php/webapps/37225.pl
Concrete5 CMS < 5.4.2.1 - Multiple Vulnerabilities                                            | php/webapps/17925.txt
Concrete5 CMS < 8.3.0 - Username / Comments Enumeration                                       | php/webapps/44194.py
DeDeCMS < 5.7-sp1 - Remote File Inclusion                                                     | php/webapps/37423.txt
Drake CMS < 0.2.3 ALPHA rev.916 - Remote File Inclusion                                       | php/webapps/2713.txt
Kirby CMS < 2.5.7 - Cross-Site Scripting                                                      | php/webapps/43140.txt
Monstra CMS < 3.0.4 - Cross-Site Scripting (1)                                                | php/webapps/44855.py
Monstra CMS < 3.0.4 - Cross-Site Scripting (2)                                                | php/webapps/44646.txt
Mura CMS < 6.2 - Server-Side Request Forgery / XML External Entity Injection                  | cfm/webapps/43045.txt
Redaxo CMS Mediapool Addon < 5.5.1 - Arbitrary File Upload                                    | php/webapps/44891.txt
zKup CMS 2.0 < 2.3 - Arbitrary File Upload                                                    | php/webapps/5220.php
zKup CMS 2.0 < 2.3 - Remote Add Admin                                                         | php/webapps/5219.php
---------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

There is `CMS Made Simple < 2.2.10 - SQL Injection` . Submitting the CVE (**CVE-2019-9053**) for this in the TryHackMe shell, it give a green flag  so its-a-go! 

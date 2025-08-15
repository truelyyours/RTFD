The ip I have is: `10.201.71.36`
First, I do a simple nmap scan:
```                                    
┌──(truelyyours㉿kali)-[~/tryhackme/AllInOne]
└─$ nmap -sC -sV 10.201.71.36
Starting Nmap 7.95 ( https://nmap.org ) at 2025-08-14 20:33 EDT
Nmap scan report for 10.201.71.36
Host is up (0.49s latency).
Not shown: 861 closed tcp ports (reset), 136 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.2.0.159
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 9c:33:20:84:72:51:81:df:4e:64:9b:3f:47:f7:e6:84 (RSA)
|   256 3b:e1:2e:63:94:07:c2:00:41:df:fb:80:67:a3:55:91 (ECDSA)
|_  256 f5:3f:70:df:12:bf:28:d9:25:92:36:f0:04:0d:f3:e3 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.12 seconds
```

Then we do directory traversal:
```
┌──(truelyyours㉿kali)-[~/tryhackme/AllInOne]
└─$ gobuster dir -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt -u http://10.201.71.36 -e 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.201.71.36
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Expanded:                true
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
http://10.201.71.36/.hta                 (Status: 403) [Size: 277]
http://10.201.71.36/.htaccess            (Status: 403) [Size: 277]
http://10.201.71.36/.htpasswd            (Status: 403) [Size: 277]
http://10.201.71.36/index.html           (Status: 200) [Size: 10918]
http://10.201.71.36/server-status        (Status: 403) [Size: 277]
http://10.201.71.36/wordpress            (Status: 301) [Size: 316] [--> http://10.201.71.36/wordpress/]
Progress: 4744 / 4745 (99.98%)
===============================================================
Finished
===============================================================
```

So, I can see a WordPress file so I visit that endpoint and explore a bit.
We have a login page `http://10.201.71.36/wordpress/wp-login.php`

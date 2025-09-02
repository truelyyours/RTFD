This is a Medium Linux machine. My first solo Medium! Let's go!
# Init
Starting with `nmap` scan:
```
┌──(truelyyours㉿kali)-[~/htb/machines/era]
└─$ sudo nmap -sC -sV -oA nmap/era 10.10.11.79
[sudo] password for truelyyours:
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-02 01:34 EDT
Nmap scan report for 10.10.11.79
Host is up (0.14s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://era.htb/
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.03 seconds
```

So, we have ftp (no anonymous login) and http. The website is pretty slick with following web stack:
![[Pasted image 20250902014209.png]]

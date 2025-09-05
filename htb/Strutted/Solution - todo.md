This is a retired Medium machine. Doing for practice!
# Init
Starting with `nmap`:
```
┌──(truelyyours㉿kali)-[~/htb/machines/strutted]
└─$ sudo nmap -sC -sV -oA nmap/strutted 10.10.11.59
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-02 04:18 EDT
Nmap scan report for 10.10.11.59
Host is up (0.13s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://strutted.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.62 seconds
```

So, `ssh` and `http` are running.  The website is some image upload and sharing site. You can download source code too!
![[Pasted image 20250902042657.png]]

Although, after uploading the image, you cannot "Copy Shareable Link"! The README in source code does instruct that you can access the image via `http:localhost:8080/s/{id}` where `id` is some unique identifier

We do get a id and password:
```
┌──(truelyyours㉿kali)-[~/htb/machines/strutted/source_code]
└─$ cat tomcat-users.xml
<?xml version='1.0' encoding='utf-8'?>

<tomcat-users>
    <role rolename="manager-gui"/>
    <role rolename="admin-gui"/>
    <user username="admin" password="skqKY6360z!Y" roles="manager-gui,admin-gui"/>
</tomcat-users>
```

Looking at the source code, the MVC is (Apache) `Struts2` with version `6.3.0.1`.

As we can upload file, googling CVE for this version, there is [CVE-2024-53677](https://www.cve.org/CVERecord?id=CVE-2024-53677) which allows arbitrary path traversal and RCE. [POC](https://github.com/TAM-K592/CVE-2024-53677-S2-067).


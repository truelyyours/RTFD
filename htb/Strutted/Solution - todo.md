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

Although, after uploading the image, you cannot "Copy Shareable Link"! The README in source code does 
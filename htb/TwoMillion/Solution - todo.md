Because this is still one of my initial boxes, I am going to do this in guided mode (It's available for retired machines).
Starting with `nmap`:
```
# Nmap 7.94SVN scan initiated Thu Aug 28 17:03:04 2025 as: nmap -sC -sV -oA nmap/twomillion 10.10.11.221
Nmap scan report for 10.10.11.221
Host is up (0.11s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp open  http    nginx
|_http-title: Did not follow redirect to http://2million.htb/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Aug 28 17:03:15 2025 -- 1 IP address (1 host up) scanned in 11.83 seconds
```

So, we have 2 TCP ports open. As seen, we have `2million.htb` on port 80, so I put this in `/etc/hosts` and open the site in browser.
This is the classic HTB site. Back then we had to find an invite code to join the HTB. I remember I prolly got the code from somewhere else during my college days. But let's do this properly this time!
![[Pasted image 20250828181708.png]]

Upon clicking "Join HTB", we are taken to the page `/invite`:
![[Pasted image 20250828181750.png]]

The `/invite page`

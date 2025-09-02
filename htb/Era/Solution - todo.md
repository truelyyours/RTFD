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

The site has no links and no results in gobuster. However `fuff` does give something interesting - `ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt -u http://era.htb -H "Host: FUZZ.era.htb"` - returns
![[Pasted image 20250902020158.png]]

So, add this in `/etc/hosts` and visiting the site lands us to "Era Storage"
![[Pasted image 20250902020234.png]]

So, we have sign in page, but no creds. Just a blind guess (after 30 mins of poking around with different tools), I try `http://file.era.htb/register.php` and I land on registration page and you can now create an account!
Better way to achieve this is:
```
┌──(truelyyours㉿kali)-[~/htb/machines/era]
└─$ gobuster dir -u http://file.era.htb/ -w /usr/share/wordlists/dirb/common.txt -t 50 --exclude-length 6765 -x php
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://file.era.htb/
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] Exclude Length:          6765
[+] User Agent:              gobuster/3.8
[+] Extensions:              php
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.htpasswd            (Status: 403) [Size: 162]
/.hta                 (Status: 403) [Size: 162]
/.htaccess            (Status: 403) [Size: 162]
/assets               (Status: 301) [Size: 178] [--> http://file.era.htb/assets/]
/download.php         (Status: 302) [Size: 0] [--> login.php]
/files                (Status: 301) [Size: 178] [--> http://file.era.htb/files/]
/images               (Status: 301) [Size: 178] [--> http://file.era.htb/images/]
/layout.php           (Status: 200) [Size: 0]
/LICENSE              (Status: 200) [Size: 34524]
/login.php            (Status: 200) [Size: 9214]
/logout.php           (Status: 200) [Size: 70]
/manage.php           (Status: 302) [Size: 0] [--> login.php]
/register.php         (Status: 200) [Size: 3205]
/upload.php           (Status: 302) [Size: 0] [--> login.php]
Progress: 9226 / 9226 (100.00%)
===============================================================
Finished
===============================================================
```

But after creating the account and trying to login, you get the same error that "Invalid username or password"!!

I don't know what is wrong! I am just not able to login! Maybe some played a mischief on this machine!??
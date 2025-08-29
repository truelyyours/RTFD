`TheFrizz` is a medium-difficulty Windows machine featuring a web application showcasing Walkerville Elementary School and a Gibbon CMS instance.
# Init

Starting with the `nmap` scan, we have SSH, LDAP and Kerberos ports open along with `frizzdc.firzz.htb` and `firzz.htb` on port `80`.
```
┌─[htb_lab_truelyyours]─[10.10.16.82]─[truelyyours@parrot]─[~/htb/thefrizz]
└──╼ [★]$ sudo nmap -sC -sV -oA nmap/thefrizz 10.10.11.60
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-08-29 16:19 EDT
Stats: 0:01:12 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.94% done; ETC: 16:20 (0:00:00 remaining)
Nmap scan report for 10.10.11.60
Host is up (0.20s latency).
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
22/tcp   open  ssh           OpenSSH for_Windows_9.5 (protocol 2.0)
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Apache httpd 2.4.58 (OpenSSL/3.1.3 PHP/8.2.12)
|_http-server-header: Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.2.12
|_http-title: Did not follow redirect to http://frizzdc.frizz.htb/home/
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-08-30 03:20:14Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: frizz.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: frizz.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Hosts: localhost, FRIZZDC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-08-30T03:20:28
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: 7h00m00s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.65 seconds
```

The port 80 takes us to a WES School website hosted on `apache` server. There is a peculiar text under "Hacking & The Law" course name:
`V2FudCB0byBsZWFybiBoYWNraW5n IGJ1dCBkb24ndCB3YW50IHRvIGdv IHRvIGphaWw/IFlvdSdsbCBsZWFy biB0aGUgaW4ncyBhbmQgb3V0cyBv ZiBTeXNjYWxscyBhbmQgWFNTIGZy b20gdGhlIHNhZmV0eSBvZiBpbnRl cm5hdGlvbmFsIHdhdGVycyBhbmQg aXJvbiBjbGFkIGNvbnRyYWN0cyBm cm9tIHlvdXIgY3VzdG9tZXJzLCBy ZXZpZXdlZCBieSBXYWxrZXJ2aWxs ZSdzIGZpbmVzdCBhdHRvcm5leXMu`
Each string is 28 bytes but is a simple base64 encoding. Lols.
```
[b'Want to learn hacking',
 b" but don't want to go",
 b" to jail? You'll lear",
 b"n the in's and outs o",
 b'f Syscalls and XSS fr',
 b'om the safety of inte',
 b'rnational waters and ',
 b'iron clad contracts f',
 b'rom your customers, r',
 b'eviewed by Walkervill',
 b"e's finest attorneys."]
```

Next, we have "Staff Login" button which takes us to Gibbon LMS login (This site is based on Gibbon framework) page asking for username password and an option to start Student and Staff applications!
![[Pasted image 20250829171201.png]]

Trying with random email and password gives "Incorrect username **and** password" strange. Anyways, there is Forgot password button, which take us to `http://frizzdc.frizz.htb/Gibbon-LMS/index.php?q=passwordReset.php`. Seems like passing a file in the query param. So, maybe we can do arbitrary read? 
A quick search lands me at [CVE-2023-45878](https://nvd.nist.gov/vuln/detail/CVE-2023-45878). A public exploit is available at https://herolab.usd.de/security-advisories/usd-2023-0025/.
Basically we can access a particular path (`/modules/Rubrics/rubrics_visualise_saveAjax.php`) which expects an 'img' tag and we can make it write arbitrary data under the pretext of bsae64 encoded image.

So, I head over to Burpsuite and send a POST request to above file and ask it to write `php` code into `yuyu.php` file.
![[Pasted image 20250829184850.png]]



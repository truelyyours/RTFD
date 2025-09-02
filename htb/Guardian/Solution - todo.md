This is one of the machine from Release Arena.
# Init
Starting with `namp` scan:
```
┌─[htb_lab_truelyyours]─[10.10.16.89]─[truelyyours@parrot]─[~/htb/machines/guardian]
└──╼ [★]$ nmap -sC -sV  -oA nmap/guardian 10.10.11.84
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-01 21:13 UTC
Nmap scan report for 10.10.11.84
Host is up (0.092s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 9c:69:53:e1:38:3b:de:cd:42:0a:c8:6b:f8:95:b3:62 (ECDSA)
|_  256 3c:aa:b9:be:17:2d:5e:99:cc:ff:e1:91:90:38:b7:39 (ED25519)
80/tcp open  http    Apache httpd 2.4.52
|_http-title: Did not follow redirect to http://guardian.htb/
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: Host: _default_; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.34 seconds
```

We have `ssh` and port 80 open redirecting to `guardian.htb`. So, add this to `/etc/hosts` and the link open a "Guardian University" website. There are some student's review with their email id, so let's note that.
```
GU0142023@guardian.htb
GU6262023@guardian.htb
GU0702025@guardian.htb
admissions@guardian.htb
```
We also have have subdomain or `portal.guardian.htb` for student login. So, add that to hosts too.

Looking at the guide link `http://portal.guardian.htb/static/downloads/Guardian_University_Student_Portal_Guide.pdf`, we find the default password of `GU1234`. Trying this for the known students, it works on **GU0142023**.

Looking at the chats, the URL is like: `/student/chat.php?chat_users[0]=13&chat_users[1]=14`, where 13 and 14 are the user's ids. So, trying out to manipulate the  ids, it turns out you can read chat between any two users!

Using this vulnerability, you can read chat between admin and user `jamin.enockson`
```
gitea:jamil.enockson:DHsNnk3V503
```


Among the assignments, we can upload .docx and .xlsx file at `http://portal.guardian.htb/student/submission.php?assignment_id=15`. 
`guardiandb` is the database name because I somehow found an error file uploading a file!

Given there is `gitea` stuff, I decided to fuzz it:
```
ffuf -c -u "http://guardian.htb" -H "HOST: FUZZ.guardian.htb" -w ~/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -t 100 -fc 301
```
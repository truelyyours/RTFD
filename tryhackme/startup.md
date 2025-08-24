This is an easy `linux` machine. 
I start by doing an `nmap` scan.
```

```
Here we have Anonymous ftp access. However the interesting part is the `/ftp` director has `rwx` permission for everyone! So that means, if we can find a way to execute files from this directory, we can get a reverse shell!

Doing a simple `gobuster` search, I find that there is `/files` endpoint that shows all the files/directories of the ftp root folder in the browser. So, this means we can "execute" or "access" a file via browser. 


# Privilege Escalation
I put `linpeas.sh` in `/tmp` path and have some interesting findings.

1.
```
╔══════════╣ Unexpected in root
/vagrant
/recipe.txt
/vmlinuz.old
/vmlinuz
/incidents
/initrd.img
/initrd.img.old
```

2.
travers `/root` and `/home`

3.

```
╔══════════╣ SGID
╚ https://book.hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html#sudo-and-suid
-rwxr-sr-x 1 root shadow 35K Apr  9  2018 /sbin/unix_chkpwd
-rwxr-sr-x 1 root shadow 35K Apr  9  2018 /sbin/pam_extrausers_chkpwd
-rwxr-sr-x 1 root utmp 425K Feb  7  2016 /usr/bin/screen  --->  GNU_Screen_4.5.0
-rwxr-sr-x 1 root shadow 61K Mar 26  2019 /usr/bin/chage
-rwsr-sr-x 1 daemon daemon 51K Jan 14  2016 /usr/bin/at  --->  RTru64_UNIX_4.0g(CVE-2002-1614)
-rwxr-sr-x 1 root mlocate 39K Nov 18  2014 /usr/bin/mlocate
```

4.
```
══╣ Last time logon each user
Username         Port     From             Latest
vagrant          pts/0    10.0.2.2         Thu Nov 12 04:50:52 +0000 2020
lennie           pts/0    10.2.0.159       Sun Aug 24 01:59:17 +0000 2025
```
5.
root      1041  0.0  0.2  24040  2304 ?        Ss   01:56   0:00 /usr/sbin/vsftpd /etc/vsftpd.conf

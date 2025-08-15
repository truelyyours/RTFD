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


What is the username found in the leaked file?
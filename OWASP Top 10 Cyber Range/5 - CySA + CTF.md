# Log Analysis

Any machine exposed to the Internet is at risk of getting compromised. Detecting these attack attempts, even if they are not successful, is crucial for securing servers, hosts or any other asset. When access is gained into a potentially compromised machine, the first thing that should be done is to figure out the attack vectors that could have been used to compromise the device. Since the machine is running a web server, checking logs related to HTTP requests could disclose any malicious activity.

To check the website open a browser window and search for http://hdmovies.ca

The website is being hosted in an Apache web server, and the best place to check the web traffic is the Apache access log, which contains all recorded data for requests processed by the Apache server. To do that, go to _/var/log/apache2_ and read the access.log.

sudo cat /var/log/apache2/access.log.1

Sort the logs and look for anything suspicious. Focus on uncommon User Agents or files that do not appear to be web resources.

---

What is the name of suspicious file?
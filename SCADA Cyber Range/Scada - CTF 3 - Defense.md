Defending against cyber threats is of utmost importance in today's digital landscape. To secure an organization's online assets, robust security measures such as firewalls, intrusion detection systems, and encryption play a crucial role in strengthening defenses. Regular security audits and vulnerability assessments help identify and address potential weaknesses, thereby enhancing system resilience.

In addition to technological defenses, having an educated workforce is vital. Training employees in cybersecurity best practices equips them to recognize and thwart social engineering attacks, which are common tactics used by cybercriminals.

Continuous monitoring and a well-prepared incident response plan enable quick detection and containment of potential breaches, reducing the impact of any security incidents.
# Reviewing files

In this _Capture The Flag_ challenge, you'll be playing the role of an incident responder tasked with investigating a potential attack on a SCADA system. You've received two important files: a log file taken from a _Conpot honeypot_ (which simulates the target) and a _PCAP file_ that records all the actions carried out by the attacker. These files are located in the files folder named _conpot.log_ and _capture2.pcap_. Your task is to formally analyze these files, aiming to identify and appropriately address any security breaches that might have occurred.

Navigate to the _/home/ubuntu-user/files_ directory:
`cd /home/ubuntu-user/files

Utilize an editor to open and review the files:
`nano conpot.log capture2.pcap
Or simply just use `cat/more/less` etc
### Identifying Attacker

Upon completing the review of the files, open a new terminal and input _attack_quiz_ to answer the question in this CTF.
`attack_quiz`
# Identifying Target Device

Next, try to find out which specific device the attacker was trying to take advantage of. Once you've conducted a proper investigation, share the answer to Question 2 from the quiz.
### Investigating Attacker's Actions - Function Codes

The attacker deployed various Modbus function codes against the target system. Identify these specific codes to respond to Question 3 of the quiz.
### Investigating Attacker's Actions - Attacking Host System

Lastly, the attacker made an attempt to target the host system. Provide the response to Question 4 of the quiz to successfully conclude this Capture The Flag (CTF) challenge.
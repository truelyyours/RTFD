# Code Execution

To see how pickle can be exploited, open a browser window and go to this url:
`http://192.168.1.101:5000

To view the source code of web, enter the following commands:
`cd Documents/lab/python-Code/vulnerabilities/
`nano pickle_vuln.py

The script below handles _base64-encoded_ pickled input. _Base64_ is an encoding scheme that represents binary data in an ASCII format. The if conditions check whether the user has entered any data in the input field on the website. The _OutputGrabber()_ is a custom class designed for displaying output appropriately.

The script demonstrates that the data is first decoded from _Base64_ and then unpickled using _pickle.loads()_. To exploit this implementation of the pickle module, the payload must first be pickled with _pickle.dump()_ and then encoded to Base64.

A script that serves this purpose can be found in the _/Documents_ directory. To view the contents of this script, please use the following commands:
`cd /home/kali/Documents/lab/
`cat exploit.py

What is the value of the DEFAULT_COMMAND variable?
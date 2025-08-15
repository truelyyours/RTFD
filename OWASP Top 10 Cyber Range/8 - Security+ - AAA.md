# Authentication Management - Password Keys

_Secure Shell (SSH)_ is a cryptographic network protocol that uses public and private keys for authentication. Users can generate key pairs and log into servers that support key-based authentication. The server uses the correct public key to verify the client's private key, and if they match, the user is authenticated.

The command below generates RSA key pairs with a 4096-bit length. By default, the keys are stored in the user's home directory, but another directory can be specified.

If a passphrase is added, the private key is encrypted. The user must decrypt it with the passphrase when authenticating. The encrypted key can only be brute-forced if an attacker obtains the key itself.
`ssh-keygen -t rsa -b 4096
### Generate keys

An SSH connection can be established between the host machine and the container. To enable communication using this protocol, both devices must authenticate each other using key pairs.

To begin, switch to the _Student machine_ and open a new terminal and repeat the steps for creating key pairs:
`ssh-keygen -t rsa -b 4096

On your device, the path _/home/kali/.ssh_ will be set to the _.ssh_ folder on your home directory.

What does the `4096` mean in the `ssh-keygen` command? **The bit length**
# Copying Keys and Connecting using Keys

First, make sure `ssh` is enabled and running on the host. Then copy the public key to the second machine using the `ssh-copy-id` command. When the host tries to connect to the machine, the host's private key is matched against the public key in the machine, authenticating the host and allowing it access.

Run the following commands on your Student machine.
`ssh-copy-id root@192.168.1.100
`yes

When prompted for the password, enter:
`passw0rd!

The terminal prompts to confirm if it's safe to connect to the Docker container. This occurs when two devices connect for the first time or their key pairs change. The `ssh-copy-id` command saves the public key in the user's path and requests validation by asking for the root password. Authorization requires both key pairs and valid credentials.

Connect to the `ssh` server using the private key (run the command on the Student machine):
`ssh root@192.168.1.100

To move onto the next step, execute the _whoami_ command:
`whoami

What output will be displayed? `root`
# Password Vaults

When using password authentication, users often reuse credentials or choose weak passwords for ease of remembrance. Strong, random passwords are hard to brute-force but easy to forget, which led to the creation of password vaults. A password vault is a program that stores, generates, and manages passwords. By encrypting the storage, it allows users to access multiple passwords with a single master password. These vaults are often called password managers.

Many password managers exist, with _KeePass_ being one of the most well-known. _KeePass_ is a free, open-source tool that stores passwords in an encrypted database, unlocked with a master key. The _kpcli_ tool is a command-line utility for managing KeePass databases. In the previous challenge, the private key was encrypted with a passphrase for added security. It's best practice to add a passphrase whenever generating a new key pair, so storing these values securely is essential.

To store this in a KeePass database, disconnect from the SSH server and open _kpcli_ with the following command:
`exit
`kpcli

Which of the folowing best describes the function of the _master password_ in a password vault like KeePass? **It is the only password a user needs to remeber to unlock the entire password database**
# Kpcli

To list the existing directories, type the following command:
`ls

Since SSH is a protocol on its own, create another directory as shown below:
`mkdir ssh
`cd ssh

To move onto the next step lets create a new entry with the following command:
`new
### New SSH entry

To add a new entry in the ssh directory, type new. Then fill in the information required. To follow with the challenge, type the same commands as listed below:
`ssh_passphrase
`admin
`secret
`secret
`root@192.168.1.100

How many directories are listed in kpcli? **2**
# Saving SSH Credentials

Everything typed after _Password:_ will not be shown. This is a countermeasure to prevent accidental password disclosure in case the device's screen is visible to other people.

to exit the editor put a single `.` on a line and press _enter_

To store the credentials just entered, write them to a KeePass database:
`saveas ssh_credentials.kdbx

When prompted for the master password, enter the following password:
`secret

The master password is used to encrypt and decrypt the database. When choosing a master password, make sure to select a secure and robust password.

While not necessary since we haven't left `kpcli`, if you were to exit and try to get back in you would have to enter the password. If that were the case you would start `kpcli` again, load the database and type the new password:
`open ssh_credentials.kdbx
`secret
### Viewing SSH Credentials

Change directory to ssh if you are not already there and list the content to see the password entries:
`cd ssh/
`ls

The show command lists all the details.
`show 0
`show 0 -f

Which flag is used to see the password in plaintext? `-f`
# KeePass database

There is another KeePass database located on the admin's Desktop. Running the following commands, displays the password.

`open /home/kali/Desktop/Challenges/Password_vaults/test_credentials.kdbx
`admin
`ls
`cd test/
`ls
`show 0 -f

What is the password displayed in the output? `secure_password`
### Mandatory Access Control (MAC)

_Mandatory Access Control (MAC)_ is a security mechanism where the operating system limits users' ability to access or perform operations on system constructs such as files, directories, TCP/UDP ports, shared memory, and I/O devices. Each user and construct has security attributes, and when a user attempts access, the OS kernel checks these attributes against authorization rules to determine if access is granted. A database management system can also enforce MAC, with objects like tables, views, and procedures. MAC is controlled by a security policy administrator, meaning users cannot override the system.

For network access control, the most common security mechanism is the firewall, which controls data transmission to and from a device. A traditional firewall in Linux is iptables, a utility for configuring IP packet filter rules. These rules are organized in tables containing chains that define how network traffic should be handled. Iptables requires root privileges to function.

Since this lab only requires access from the ssh_connection container, access can be restricted to a specific IP range. The host machine and containers are assigned IPs from the range 192.168.1.0. A rule can be added in iptables to block connections from other IP ranges. Initially, iptables should have no rules. To list the current rules, run the following commands:
`exit
`sudo su root
`iptables -L
### Iptables

To see which networks the container is part of, run the ifconfig command:
`ifconfig

The output of `ifconfig` shows that the _IP_ assigned to the challenges container as well as the subnet mask under the _eth0_ entry. Based on this information the range that needs to be allowed is 192.168.0.0/16. In iptables, the command would look like the following:
`iptables -I INPUT ! -s 192.168.0.0/16 -j DROP

To see the updated `iptable`, list the entries again using iptables -L.
`iptables -L

Who controls the security policy in a MAC system? **The system administrator or security policy administrator**
# Changing file permissions

To ensure proper inspections, reset the iptables policy with the following command as the root user:
`iptables -F INPUT

Most file systems assign permissions to users and groups, controlling access to files and directories. In Linux, each file and directory has three permission groups:
`-owner`: Permissions apply only to the file's owner. `-group`: Permissions apply to the group assigned to the file. `-all` users: Permissions apply to all other users on the system.

Each file or directory has three basic permission types:

`-read`: Allows viewing the file's contents. `-write`: Allows modifying the file or directory. `-execute`: Allows executing the file or listing directory contents.

To test file permissions, create a file and view its permissions with ls -al:
`touch test.txt
`ls -al test.txt

The file's permissions will be shown as `-rw-r--r-- 1`.

-The first dash indicates it's a file (a directory would show d).

-The next set of characters (`rw-r--r--`) shows the owner's permissions: read and write, but not execute.

-The second group (`rw-r--r--`) shows the group's permissions: read-only.

-The final group (`rw-r--r--`) shows the permissions for all users: read-only.

`-root` root shows the owner and group names.
# Admin file privileges

On the terminal, change user to _kali_, switch directory as shown below and list the content. Two directories will be listed.
`su kali
`cd /home/kali/Desktop/Challenges/Filesystem_permissions/
`ls

Go to _Guest_ and list all the files.
`cd Guest
`ls -al

There is a file in this directory, owned by user guest. The permissions show that the owner and group can read the file, but only the owner can modify it. Other users cannot read, write, or execute it. If the cat command is run as user admin, the "Permission denied" error is shown.
`cat guest_notes.txt
### Explicit Annotation

File or directory permissions can be changed using either explicit annotation or octal notation. The explicit method uses the following modifiers:

`-u`: owner `-g`: group `-o`: other users `-a`: all users

To allow the user admin to read the guest_notes.txt file, use o+r to modify the permissions. The command must be run with sudo because the file's permissions are being changed by a non-owner user:
`sudo chmod o+r guest_notes.txt

Then, verify the changes:
`ls -al
`cat guest_notes.txt

What output will be displayed? **Guest permissions**
# Octal notations

Another way of setting permissions is by using the octal notation method. Setting permissions boils down to changing bits. A file that has read, write, and execute permissions for all users has all the bits set:
`rwx rwx rwx = 111 111 111
`rwx rw- r-- = 111 110 100

The value 111 in binary represents 7, 110 represents 6, 100 represents 4 and so on, so the permissions above can be written as:
`rwx rwx rwx = 111 111 111 = 7 7 7
`rwx rw- r-- = 111 110 000 = 7 6 4

Listing the files in the _/Development_ directory shows that all users can read _developer_note.txt_. In a real-world development environment, files like this would be restricted, as they are considered intellectual property. The same applies to any sensitive or proprietary files. If such files are accessible to unauthorized users (e.g., guest users), it constitutes a security incident.

The commands below switch from admin to guest and reveal the content of _developer_notes.txt_.
`cd ..
`cd Development/
`su guest
`guest
`cat developer_notes.txt
### Revoking permissions

To revoke the permissions of _developer_notes.txt_, revoke read rights for other users:
`rw- r-- --- = 110 100 000
`su developer
`developer
`chmod 640 developer_notes.txt
`ls -al

### Changed permissions

When a guest user tries to read a file owned by the developer, they will receive a _Permission denied_ error. This indicates that the file's permissions are correctly set, allowing only the developer to view or modify it. To prevent information disclosure, ensure that users can only access files they own or those necessary for normal workflow.
`su guest

When prompted for the password, enter:
`guest
`ls -al
`cat developer_notes.txt

The same output would be achieved with the `chmod o-r developer_notes.txt` command.

What numerical value corresponds to the permissions rw-r-----? `640`

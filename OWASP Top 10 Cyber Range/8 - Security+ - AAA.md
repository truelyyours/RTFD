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

open /home/kali/Desktop/Challenges/Password_vaults/test_credentials.kdbx

admin

ls

cd test/

ls

show 0 -f

---

What is the password displayed in the output?
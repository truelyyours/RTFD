# Steganography

_Steganography_ is the practice of hiding confidential data within seemingly ordinary files, images, or videos. The term is derived from the Greek words _steganos_ (hidden) and _graphein_ (writing). Unlike cryptography, which only obfuscates the content, steganography also hides the fact that a secret message is being transmitted.

Both attackers and penetration testers use steganography to bypass security measures by concealing malware or command-and-control (C2) channels. Since files with embedded content look like regular files, they can evade detection mechanisms on networks and endpoints. An attacker might send multiple images containing payloads, such as PowerShell scripts or malware, to maintain remote access. Another method is tunneling C2 communication within existing protocols by exploiting unused or uncommon fields.

_Steghide_ is a tool that allows users to hide and extract data from images and audio files. In the _Challenges/Steganography_ directory, there is a PNG file named _clue.png_. To view it, either double-click on the image or use the command eog in the terminal.
`cd Desktop/Challenges/Steganography
`eog clue.jpg
### Steghide

_Steghide_ can be used to test if there is any covert information in the image. This tool secures the hidden messages with passphrases.
`steghide extract -sf clue.jpg

When prompted for the passphrase, enter:
`secure

The output of the command shows that the hidden message was written in the _hidden.txt_ file. Use _cat_ to view the content of the file:
`cat hidden.txt

What output is displayed in the terminal? `Steganography`
# Symmetric-key algorithm comparison

In the terminal, navigate to _Cryptography_ directory and list the file. Each of the files is the ciphertext of the word _Cryptography_ encrypted using different symmetric algorithms and modes of operations. The command below displays the ciphertext in each file.
`cd ../Cryptography/Symmetric
`awk '{print}' * > salted.txt`

The output of the command shows the difference in encryption.
`cat salted.txt
### OpenSSL

_OpenSSL_ is a command-line utility that can be used to generate private keys, encrypt, decrypt, create CSRs, generate certificates, and identify certificate information. To decrypt the DES ECB ciphertext, use openssl.
`openssl enc -d -des-ecb -in des-ecb.enc -out des-ecb.txt

When prompted for the password, type:
`secure

To view the content of this file, use the following command:
`cat des-ecb.txt

What message will be displayed after decrypting the ciphertext? `Cryptography`
# Decrypt DES CBC Cipher text

Follow the same instructions to decrypt the DES CBC ciphertext.
`openssl enc -d -des-cbc -in des-cbc.enc -out des-cbc.txt

When prompted for the password, enter:
`secure

Therefore, to view the content, use the following command:
`cat des-cbc.txt
### Decrypt 3DES ECB Cipher text

The command below decrypts the 3DES ECB ciphertext:
`openssl enc -d -des-ede3-ecb -in 3des-ecb.enc -out 3des-ecb.txt
`secure
`cat 3des-ecb.txt
### Decrypt 3DES CBC Cipher text

To decrypt the 3DES CBC ciphertext, use the following command:
`openssl enc -d -des-ede3-cbc -in 3des-cbc.enc -out 3des-cbc.txt

When prompted for the passphrase, enter:
`secure
`cat 3des-cbc.txt

What type of encryption is the DES algorithm? **Symmetric**
# Decrypt AES ECB Cipher text

The command below decrypts the AES ECB ciphertext:
`openssl enc -d -aes-256-ecb -in aes-ecb.enc -out aes-ecb.txt

When prompted for the passphrase, enter:
`secure

To view the content, use the following command:
`cat aes-ecb.txt

### Decrypt AES CBC Cipher text

The same procedure will apply in the next step of decrypting the AES CBC ciphertext, with only a few differences.Use the following command:
`openssl enc -d -aes-256-cbc -in aes-cbc.enc -out aes-cbc.txt
`secure
`cat aes-cbc.txt

Which option in the openssl enc command specifies decrytion? `-d`
# Digital Signatures

Public-key cryptography enables authentication through digital signatures. The sender encrypts a message with their private key to create a digital signature, then transmits both the plaintext message and the encrypted one. To validate authenticity, the recipient uses the sender's public key to decrypt the message and compare it to the plaintext. If they match, the message is authentic and untampered. The benefits of digital signatures include:

- Authentication: The receiver can use the sender's public key to confirm the sender's identity. If the keys don't match, the message is not authentic.
- Non-repudiation: The sender cannot deny sending a message signed with their private key, as only they have access to it.
- Integrity: Verifying the signature ensures the document or message remains unchanged. Any alteration would cause the check to fail.

The following command creates a certificate based on the _X.509_ standard with a _4096-bit RSA key_:

`openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365

When prompted for the password, enter:
`secure

What key does the sender use to create a digital signature? **The sender's private key**
# Elliptic-curve cryptography

_Elliptic Curve Cryptography (ECC)_ uses the algebraic structure of elliptic curves over finite fields to encrypt data efficiently with shorter keys. Its main advantages include smaller ciphertexts, shorter key and signature lengths, and faster key and signature generation. The signature process occurs in two stages, reducing latency.

Although ECC is vulnerable to _side-channel_ and _twist-security_ attacks, these risks can be mitigated by implementing proper security measures, such as careful curve selection. Side-channel attacks, like differential power analysis, fault injection, simple power, and timing attacks, often lead to information leaks. Twist-security attacks, including invalid-curve and small-subgroup attacks, can result in private key exposure.

To create an ECC key pair with OpenSSL, use the _-list_curves_ option to list available ECC algorithms.
`openssl ecparam -list_curves

_Secp256k1_ is an elliptic curve parameter defined in Standards for Efficient Cryptography (SEC). To use it for key generation, run the following command:
`openssl ecparam -name prime256v1 -genkey -noout -out private_ecc.pem
`cat private_ecc.pem

What can twist-security attacks expose? `private key`
# ECC Key Generation

To generate a public key from the private ECC key, run the following command:
`openssl ec -in private_ecc.pem -pubout -out public_ecc.pem
`cat public_ecc.pem

The commands below display the RSA and ECC private keys. The difference in lengths is easily noticeable.
`cat private_ecc.pem
`cat private_key.pem
### ECC Digital Certificate

_The ECC_ private key can be used to create a digital certificate as shown below:
`openssl req -new -x509 -key private_ecc.pem -out cert_ecc.pem -days 365
`cat cert_ecc.pem

Why does an ECC private key appear shorter in length compared to an RSA private key? **ECC provides the same level of security with shorter key sizes**
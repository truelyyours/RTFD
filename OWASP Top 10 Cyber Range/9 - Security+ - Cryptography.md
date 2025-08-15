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

What message will be displayed after decrypting the ciphertext? `Cryptograph`
---
title: JDSD 2 - SSH
created: '2020-02-19T06:52:59.095Z'
modified: '2020-02-19T07:51:23.781Z'
---

# JDSD 2 - SSH

* SSH - Secure Shell
* It's a protocol like FTP, SFTP, HTTP etc. It's an agreement between computers on how to transfer files and other things.
* With SSH, it's encrypted but you can transfer files and control computers.
  * You can use the terminal to talk to and control another computer.
* The host - the remote server you are trying to access.
* The client - the computer you are using to access the server.

## SSH Command

* `ssh {user}@{host}`
* Mac is easy, Windows need an SSH client.

## Saving the Day Through SSH

* Log on to the server with `ssh {user}@{host}`
* Install git with `sudo apt-get install git`
  * This is kind of like using npm.
* `git clone git@github.com:url`
  * However, you need to set SSH up with Github.
* `rsync -av . root@167.99.146.57:~/superawesome.com`
  * This is how you would upload something from your own pc.

## How SSH Works

* Symmetrical Encryption, Asymmetrical Encryption, Hashing

### Symmetrical Encryption

* Encryption is a way to jumble up a piece of text that is impossible for a third party to read.
* Symmetrical Encryption uses one secret key for both parties.
  * Hello --> Key --> ndfdsfsf --> Key --> Hello
* Anyone who possesses the key can read the info.
  * If the bad guy has the key, they could decrypt the message.
* Key Exchange Algorithm
  * How to share the key, without sharing the key.
  * They send public data that is then transformed into the key.

### Asymmetrical Encryption

* The key exchange algorithm needs asymmetrical encryption.
  * Public-Private Key pair: - We can share the public key with anyone, but the private key should never be shared.
    * They are linked in terms of functionality.
    * The private key cannot mathematically compute from the public key.
    * Hello --> Blue Public Key --> Blue Private Key --> Hellooo
    * The public key can't decrypt the message, it can only encrypt.
    * It can also only be decrypted by the Private key.
* This form of encryption is only used with SSH in generating the key exchange algorithm.
  * The Difiie Hellman Key Exchange.

### Hashing

* Another form of cryptography.
* They are never meant to decrypt anything. Just encrypt with a replaced fixed variable.
* Using a hash, each message must be transmitted with a:
  * MAC - a hash generated from the symmetric key, the packet sequence number and the message contents that we're sending.
* To check the legitimacy of the message they can use the private key they both have, and the hash function that was sent outside of the SSH.

### Passwords or SSH?

* You need to make sure that whoever runs the command has the password, otherwise anyone could just do it.
* A better alternative, however, is a second way to get authenticated.

## SSH Into a Server

* How to prove you are the user without a password.
* Create `.ssh` on the computer if it doesn't exist.
  * If we don't have one we need to generate: `id_rsa` and `id_rsa.pub`
    * Share the .pub with anyone you want. Never share the other one.
* `ssh-keygen -C "jon.lemarquand@gmail.com"` - To create a new key
* `ssh pbcopy < ~/.ssh/id_rsa_digitalocean.pub` - To copy the public key to the clipboard.
*  When you find the SSH on the server you'll have: `authorized_keys`, `known hosts`.
  * Go to authorised keys and then paste it in this folder
* When you try and log on, if it doesn't work you'll need to add your private key using:
  * `ssh-add ~/.ssh/id_rsa_digitalocean` (In this instance.)



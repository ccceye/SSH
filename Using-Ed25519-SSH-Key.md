# Using Ed25519 for OpenSSH keys (instead of DSA/RSA/ECDSA)


## Introduction into Ed25519

OpenSSH 6.5 added support for **Ed25519** as a public key type. It is using an elliptic curve signature scheme, which offers better security than ECDSA and DSA. At the same time it also has good performance. This type of keys may be used for user and host keys. With this in mind, it is great to be used together with OpenSSH. In this article we have a look at this new key type.

## DSA or RSA

Many forum threads have been created regarding the choice between DSA or RSA. DSA is being limited to 1024 bits, as specified by FIPS 186-2. This is also the default length of ssh-keygen. While the length can be increased, it may not be compatible with all clients. So it is common to see RSA keys, which are often also used for signing. With Ed25519 now available, the usage for both will slowly decrease.

## Configuring the server

The first thing to check is if your current OpenSSH package is up-to-date. You will need at least **version 5.6 of OpenSSH**.

## Create SSH host keys

```
[root@arch ~]# cd /etc/ssh
[root@arch ssh]# mkdir backup
[root@arch ssh]# mv ssh_host_* ./backup/
[root@arch ssh]# ssh-keygen -f /etc/ssh/ssh_host_ed25519_key -N '' -t ed25519
Generating public/private ed25519 key pair.
Your identification has been saved in etc/ssh/ssh_host_ed25519_key.
Your public key has been saved in etc/ssh/ssh_host_ed25519_key.pub.
The key fingerprint is:
96:67:0f:50:8d:16:51:c2:47:9c:4e:85:b4:79:bd:6b root@arch
The key's randomart image is:
+--[ED25519 256]--+
|         .=X++.  |
|         .+.Bo . |
|        .. +o . .|
|         o  ..  .|
|        S +    . |
|       . o o    .|
|            .  E |
|              .  |
|                 |
+-----------------+
```

## Change SSH configuration (server)

Next step is changing the **sshd_config file**. Add the new host key type:

`HostKey /etc/ssh/ssh_host_ed25519_key`

**Remove** any of the other HostKey settings that are defined.

## Client Configuration

After configuring the server, it is time to do the client. We have to create a new key first. Make sure that your ssh-keygen is also up-to-date, to support the new key type. 

*Note: the tilde (~) is an alias for your home directory and expanded by your shell.*

```
$ ssh-keygen -t ed25519 -C "michael@linux-audit.com"
 Generating public/private ed25519 key pair.
 Enter file in which to save the key (/home/michael/.ssh/id_ed25519):
 Enter passphrase (empty for no passphrase):
 Enter same passphrase again:
 Your identification has been saved in /home/michael/.ssh/id_ed25519.
 Your public key has been saved in /home/michael/.ssh/id_ed25519.pub.
 The key fingerprint is:
 a0:b4:7a:e5:7e:85:45:ff:12:df:ef:aa:12:e4:ad:e0 michael@linux-audit.com
 The key's randomart image is:
 +--[ED25519 256]--+
 |                 |
 |          .      |
 |    . .  . .     |
 |   . o .  o o    |
 |    o . S= . + . |
 |   . o  o + o o .|
 |  . . .. o o .  .|
 |   . .  E o     .|
 |      ..   ....o.|
 +-----------------+
 ```
 
 **Optional step**: Check the key before copying it.
 
 `ssh-keygen -l -f ~/.ssh/id_ed25519`
 
 If that looks good, **copy it to the destination host**.
 
 `ssh-copy-id -i ~/.ssh/id_ed25519.pub michael@192.168.1.251`
 
 Then determine if we can log in with it.
 
 `$ ssh -i ~/.ssh/id_ed25519 michael@192.168.1.251 Enter passphrase for key ‘~/.ssh/id_ed25519’:`
 
 When using this newer type of key, you can configure to use it in your **local SSH configuration file (~/.ssh/config)**. Defining the key file is done with the IdentityFile option.
 
```
Host [name]
HostName [hostname]
User [your-username]
IdentityFile ~/.ssh/id_ed25519
IdentitiesOnly yes
```

### Insight: using -o

Normally you can use the -o option to save SSH private keys using the new OpenSSH format. It uses bcrypt/pbkdf2 to hash the private key, which makes it more resilient against brute-force attempts to crack the password. Only newer versions (OpenSSH 6.5+) support it though. For this key type, the -o option is implied and does not have to be provided. Also a bit size is not needed, as it is always 256 bits for this key type.

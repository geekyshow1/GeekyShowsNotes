### How to access server via ssh and setup ssh key for passwordless authentication
Without setting up SSH Key It will ask Password each time we try to get access

- To Get Access via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 21350 root@216.32.44.12
```

#### Note:- Run Below Commands on Your Local Machine, Not on Your VPS or Remote Server
- Generate SSH Key
```sh
ssh-keygen -f C:\Users\R/.ssh/my_key -t rsa -b 4096
```
- Copy SSH Public Key to Remote Server (Not for Windows)
```sh
Syntax:- ssh-copy-id -p PORT -i ~/.ssh/keyname.pub USERNAME@HOSTIP
Example:- ssh-copy-id -p 21350 -i ~/.ssh/id_rsa.pub u27653@216.32.44.12
```

- Copy Public Key to Remote Server (for Windows)
```sh
1. Make sure you have .ssh folder in remote server
Syntax:- ssh -P PORT USERNAME@HOSTIP "mkdir /home/USERNAME/.ssh"
Example:- ssh -P 21350 u27653@216.32.44.12 "mkdir /home/u27653/.ssh"

2. Copy Public Key
Syntax:- scp -P PORT SSH_PUBLIC_KEY_PATH USERNAME@HOSTIP:/home/USERNAME/.ssh/authorized_keys
Example:- scp -P 21350 C:\Users\R/.ssh\id_rsa.pub u27653@216.32.44.12:/home/u27653/.ssh/authorized_keys
```

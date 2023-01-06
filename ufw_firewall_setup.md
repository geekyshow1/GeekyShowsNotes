### How to Setup and Config UFW on VPS
#### What is Firewall ?
##### A firewall is computer hardware or software that controls inbound and outbound traffic of a machine.

#### What is UFW ?
##### UFW (Uncomplicated Firewall) is presented as a front-end of Iptables. By default, UFW denies all incoming connections and allows all outgoing connections.
#

- To Get Access via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```
- Install UFW
```sh
apt install ufw
```
- Enable UFW
```sh
ufw enable
```
- Check UFW Status
```sh
To Check Normal Mode: ufw status 
To Check in more Comprehensive: ufw status verbose
To Check with Number: ufw status numbered
```
- Configure to support IPv6:
```sh
Open Config File: nano /etc/default/ufw
then Change: IPV6=yes
```
- To Restart Firewall Disable it then Enable it:
```sh
ufw disable
ufw enable
```
- To Check Open Port, It will show only those which are currently running:
```sh
netstat -tulpn
```
- To Open Port:
```sh
Syntax:- ufw allow port/protocol
Example:- ufw allow 21/tcp
```
- To Close Port:
```sh
Syntax:- ufw deny port/protocol
Example:- ufw deny 21/tcp
```
- To Open a Range of Ports:
```sh
Syntax:- ufw allow [Starting_port:Ending_port]/protocol
Example:- ufw allow 300:310/tcp
```
- To Close a Range of Ports:
```sh
Syntax:- ufw deny [Starting_port:Ending_port]/protocol
Example:- ufw deny 300:310/tcp
```
- To Allow Service:
```sh
Syntax:- ufw allow service_name
Example:- ufw allow http
```
- To Deny Service:
```sh
Syntax:- ufw deny service_name
Example:- ufw deny http
```
- To Allow Access to IP Address:
```sh
Syntax:- ufw allow from IPAddress
Example:- ufw allow from 192.168.1.4
```
- To Deny Access for IP Address:
```sh
Syntax:- ufw deny from IPAddress
Example:- ufw deny from 192.168.1.5
```
- To Allow IP to connect only specific Port:
```sh
Syntax:- ufw allow from IPAdress to any port Port
Example:- ufw allow from 192.168.1.4 to any port 45
```
- To Delete a Specific Rule:
```sh
1. Check Status with Number: ufw status numbered
2. Delete with Number 
   Syntax:- ufw delete number
   Example:- ufw delete 3
```
- To Reset to Default Setting:
```sh
ufw reset
```
- Some usefull connection which You may want to allow
```sh
To Allow SSH Connection: ufw allow ssh or ufw allow 22/tcp
To Secure Web Server: ufw allow 80/tcp
To Allow FTP Connection: ufw allow ftp or ufw allow 21/tcp and 20/ftp
To Allow Web Server Profile: ufw allow www
```

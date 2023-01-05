### How to Enable HTTPS in Your Domain Hosted on Linux Remote Server or VPS
#### Let's Encrypt is a non-profit certificate authority run by Internet Security Research Group that provides X.509 certificates for Transport Layer Security encryption at no charge.

- To Access Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```

- Install Certbot and python3-certbot-apache
```sh
apt install certbot python3-certbot-apache
- Certbot is a free, open source software tool for automatically using Let’s Encrypt certificates on manually-administrated websites to enable HTTPS.
- python3-certbot-apache is a Apache plugin for Certbot. 
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
ufw status verbose
```
- Obtain an SSL certificate
```sh
certbot --apache
```
- Check Status of Certbot
```sh
systemctl status certbot.timer
```
- Dry Run SSL Renewal
```sh
certbot renew --dry-run
```

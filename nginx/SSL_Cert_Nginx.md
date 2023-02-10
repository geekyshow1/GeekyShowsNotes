### How to Enable HTTPS in Your Domain Hosted on Linux Remote Server or VPS
#### Let's Encrypt is a non-profit certificate authority run by Internet Security Research Group that provides X.509 certificates for Transport Layer Security encryption at no charge.
#### Certbot is a free, open source software tool for automatically using Let’s Encrypt certificates on manually-administrated websites to enable HTTPS.

- To Access Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 1034 raj@216.32.44.12
```
- Install Certbot and it’s Nginx plugin
```sh
sudo apt install certbot python3-certbot-nginx
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
sudo ufw status verbose
```
- Obtain an SSL certificate
```sh
sudo certbot --nginx -d your_domain.com -d www.your_domain.com
```
- Check Status of Certbot
```sh
sudo systemctl status certbot.timer
```
- Dry Run SSL Renewal
```sh
sudo certbot renew --dry-run
```

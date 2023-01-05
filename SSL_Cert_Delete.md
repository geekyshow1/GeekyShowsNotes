### How to Remove HTTPS from Your Domain Hosted on Linux Remote Server or VPS
#### Let's Encrypt is a non-profit certificate authority run by Internet Security Research Group that provides X.509 certificates for Transport Layer Security encryption at no charge.

- To Access Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```

- Show Installed Certificates
```sh
certbot certificates 
```
- Delete Certificate by Domain Name
```sh
certbot delete --cert-name www.example.com
```
- Delete Certificate by Index Number
```sh
certbot delete
```

### How to Install LAMP (Linux Apache MySQL PHP) Stack

- To Get Access via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```

- Updates the package lists for Upgrades then Upgrade
```sh
apt update
apt upgrade
```
- Install Apache
```sh
apt install apache2
```
- (Optional) If Get Error While Installation "cannot stat '/usr/share/apache2/default-site/index.html': No such file or directory
dpkg: error processing package apache2 (--configure)" then Try to Install Again
```sh
apt purge apache2-data
```
- Allow Apache through Firewall
```sh
ufw allow "Apache Full"
```
- Check Server IP on Web Browser You will see Apache Default Page
- Install MySQL
```sh
apt install mysql-server
```
- Install PHP
```sh
apt install php libapache2-mod-php php-mysql
The above command includes three packages:-
php -  To Install PHP
libapache2-mod-php - It is Used by apache to handle PHP files
php-mysql - It is a PHP module that allows PHP to connect to MySQL 
```
- To Restart Apache
```sh
service apache2 restart
```

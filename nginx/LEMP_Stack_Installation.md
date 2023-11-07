### How to Install LEMP (Linux Nginx MySQL PHP) Stack

- To Get Access via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 raj@216.32.44.12
```

- Updates the package lists for Upgrades then Upgrade
```sh
sudo apt update
sudo apt upgrade
```
- Install Nginx
```sh
sudo apt install nginx
```
- Allow Apache through Firewall
```sh
sudo ufw allow "Nginx Full"
```
- Check Server IP on Web Browser You will see Nginx Default Page
- Install MySQL
```sh
sudo apt install mysql-server
```
- Install PHP
```sh
sudo apt install php-fpm php-mysql
The above command includes three packages:-
php-fpm -  To Install PHP and php-fpm
php-mysql - It is a PHP module that allows PHP to connect to MySQL 
```
- Check php-fpm is running. Note- Write your own php version e.g. php8.2-fpm
```sh
sudo service php8.2-fpm status
OR
sudo systemctl status php8.2-fpm
```
- Check Nginx Status
```sh
sudo service nginx status
OR
sudo systemctl status nginx
```
- Start Nginx
```sh
sudo service nginx start
OR
sudo systemctl start nginx
```
- Stop Nginx
```sh
sudo service nginx stop
OR
sudo systemctl stop nginx
```
- Test Nginx Config
```sh
sudo nginx -t
OR
sudo service nginx configtest
```
- Reload Nginx
```sh
sudo service nginx reload
OR
sudo systemctl reload nginx
```
- Restart Nginx
```sh
sudo service nginx restart
OR
sudo systemctl restart nginx
```

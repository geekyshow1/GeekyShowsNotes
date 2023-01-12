### How to Install Composer on Remote Server or VPS

- Install Dependencies
```sh
apt update
apt upgrade
apt install php-cli unzip
```
- Download and Install Composer
```sh
cd ~
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

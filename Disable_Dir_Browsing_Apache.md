### How to Disable Directory Browsing or Index of on Apache Linux Remote Server or VPS

#### Method 1: Disable in Apache Configuration
- Open the /etc/apache2/apache2.conf file.
```sh
nano /etc/apache2/apache2.conf
```
- Find below content
```sh
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```
- Change Above content to below Content
```sh
<Directory /var/www/>
        Options FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```
- Restart Apache
```sh
service apache2 restart
```

#### Method 2: Disable in Virtual Hosts File
- Go to /etc/apache2/sites-available
```sh
cd /etc/apache2/sites-available
```
- Open the Required Virtual Host File
```sh
nano 000-default.conf
```
- Add below Content
```sh
<Directory /var/www/html/>
        Options FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```
- Restart Apache
```sh
service apache2 restart
```

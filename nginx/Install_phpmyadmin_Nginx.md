### How to Install phpmyadmin with Nginx Web Server
- Install phpMyAdmin and Other Plugins
```sh
sudo apt update
sudo apt upgrade
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```
```sh
- php-mbstring: A module for managing non-ASCII strings with different encodings
- php-zip: An extension that facilitates uploading .zip files to phpMyAdmin
- php-gd: Enables support for GD graphics library
- php-json: Provides support for JSON serialization
- php-curl: Allows PHP to communicate with other servers
```
- Create symlink
```sh
sudo ln -s /usr/share/phpmyadmin /var/www/project_folder_name/phpmyadmin
```
- Login to phpmyadmin using web browser
- If get warning e.g. "The $cfg['TempDir'] (/var/lib/phpmyadmin/tmp/) is not accessible." then run below commands
```sh
cd /var/lib/phpmyadmin
sudo chmod -R 775 tmp
sudo service nginx restart
```

### How to Point Domain and Host Laravel Project using Github on Apache Web Server VPS Hosting
##### Note - If you get Permission Denied Please use sudo
- Login to Your Domain Provider Website
- Navigate to Manage DNS
- Add Following Records:

| Type | Host/Name | Value |
| :---: | :---: | :--- |
| A     | @     | Your Remote Server IP |
| A     | www   | Your Remote Server IP |
| AAAA  | @     | Your Remote Server IPv6 |
| AAAA  | www   | Your Remote Server IPv6 |

- Important: If your project have user uploaded files and you have create symbolic link then Delete the storage folder which is inside your-project-folder/public/ Because It will not work on Live Server. You will have to create symbolic link again on live server.
- Push your Poject to You Github Account as Private Repo
- To Access Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```
#### Note:- Run Below Commands on Your Remote Server Linux Machine or VPS, Not on Your Local Windows Machine
- Verify that all required softwares are installed
```sh
apache2 -v
mysql
php -v
composer -v
git --version
```
- Verify Apache2 is Active and Running
```sh
service apache2 status
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
ufw status verbose
```
- Make Connection between Remote Server and Github via SSH Key
- Generate SSH Keys
```sh
ssh-keygen -t rsa -b 4096 -C "miniblog_github"
```
- Open Public SSH Keys then copy the key
```sh
cat ~/.ssh/id_rsa.pub
```
- Go to Your Github Repo
- Click on Settings Tab
- Click on Deploy Keys option from sidebar
- Click on Add Deploy Key Button and Paste Remote Server's Copied SSH Public Key then Click on Add Key
- Verify the Connection, Go to Your Server Terminal then run below
```sh
ssh -T git@github.com
```
- Go to the Web Server public directory
```sh
cd /var/www
```
- Clone Project from your github account
```sh
- Using HTTPS Path It doesnt require to setup SSH Key on Github
Syntax:- git clone https_repo_path
Example:- git clone https://github.com/geekyshow1/miniblog.git

- Using SSH Path It requires to setup SSH Key on Github
Syntax:- git clone ssh_repo_path
Example:- git@github.com:geekyshow1/miniblog.git
```
- Run ls command to verify that the project is present
```sh
ls
```
- Create Virtual Host File
```sh
nano /etc/apache2/sites-available/your_domain.conf
```
- Add Following Code in Virtual Host File
```sh
<VirtualHost *:80>
    ServerName www.example.com
    ServerAdmin contact@example.com
    DocumentRoot /var/www/project_folder_name/public
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    <Directory /var/www/project_folder_nam/public/>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
- Enable Virtual Host
```sh
cd /etc/apache2/sites-available/
a2ensite your_domain.conf
```
- Enable the Apache mod_rewrite module
```sh
a2enmod rewrite
```
- Restart Apache2
```sh
service apache2 restart
```
#### Go to Your Project Directory then follow below instructions:
##### Note - If you get Permission Denied Please use sudo
- Copy .env.example to .env
```sh
cp .env.example .env
```
- Install Dependencies
```sh
composer install --optimize-autoloader --no-dev
```
- Generate Application Key
```sh
php artisan key:generate
```
- Set Permission for storage and bootstrap/cache Folder
- Make Webserver as owner for storage and bootstrap/cache. Our Webserver is running as www-data and group is also www-data.
```sh
sudo chown -R www-data:www-data storage
sudo chown -R www-data:www-data bootstrap/cache
```
- You may face problem if you work with FTP so to fix this add your user to webserver user group following below instruction:
- Check Your User Group
```sh
sudo groups raj
```
- Add your User to webserver group
```sh
sudo usermod -a -G www-data raj
```
- Verify Your User is in Webserver Group
```sh
sudo groups raj
```
- Set storage's File Permission to 644
```sh
sudo find storage -type f -exec chmod 644 {} \;
```
- Set storage's Folder Permission to 755
```sh
sudo find storage -type d -exec chmod 755 {} \;
```
- Done
- Open Mysql
- Create Database User (Optional)
- Create Database
- Open .env File
```sh
sudo nano .env
```
- Make below changes, Important: Write DB User, DB User Password and DB Name in .env File, You may have to write DB User and DB Password enclosed with single quote
```sh
APP_NAME=Laravel
APP_ENV=production
APP_KEY=you_can_generate_it_php_artisan_key:generate
APP_DEBUG=false
APP_URL=http://your-domain.com/

LOG_CHANNEL=stack
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=database_name
DB_USERNAME='db_user_name'
DB_PASSWORD='db_user_password'
```
- Create Database Tables
```sh
php artisan migrate
```
- Create Symbolic Link at public/storage which points to the storage/app/public directory.
```sh
php artisan storage:link
```
- Clearing cache - You may need to run with sudo
```sh
php artisan cache:clear
php artisan config:clear
```
- If you make any changes in your project then you need to pull the new changes from github repo. It will update your website with latest changes.
```sh
git pull
```
- Cache routes, config and views (Optional)
```sh
php artisan route:cache
php artisan config:cache
php artisan view:cache
```
- If you ever need to work on your project you can switch ON/OFF Maintenance mode 
```sh
php artisan down
php artisan up
```


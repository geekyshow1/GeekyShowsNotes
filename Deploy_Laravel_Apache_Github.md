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
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- If Permission Denied then Own .ssh then try again to Generate SSH Keys
```sh
Syntax:- sudo chown -R user_name .ssh
Example:- sudo chown -R raj .ssh
```
- Open Public SSH Keys then copy the key
```sh
cat ~/.ssh/id_ed25519.pub
```
- Go to Your Github Repo
- Click on Settings Tab
- Click on Deploy Keys option from sidebar
- Click on Add Deploy Key Button and Paste Remote Server's Copied SSH Public Key then Click on Add Key
- Verify the Connection, Go to Your Server Terminal then run below
```sh
ssh -T git@github.com
// OR
ssh -vT git@github.com
```
- You may get an error git @ github.com: Permission denied (publickey) If you will try to clone it directly on Web Server Public Folder /var/www So we will clone github repo in User's Home Directory then Move it to Web server Public Directory
- Clone Project from your github account
```sh
- Using HTTPS Path It doesnt require to setup SSH Key on Github
Syntax:- git clone https_repo_path
Example:- git clone https://github.com/geekyshow1/miniblog.git

- Using SSH Path It requires to setup SSH Key on Github
Syntax:- git clone ssh_repo_path
Example:- git clone git@github.com:geekyshow1/miniblog.git
```
- Run ls command to verify that the project is present
```sh
ls
```
- Move Project Folder to Web Server public directory
```sh
Syntax:- sudo mv project_folder_name /var/www
Example:- sudo mv miniblog /var/www
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
- If you ever need to work on your project you can switch ON/OFF Maintenance mode 
```sh
php artisan down
php artisan up
```

### How to Automate Laravel Deployment using Github Action
- On Your Local Machine, Open Your Project using VS Code or any Editor
- Create A Folder named .scripts inside your root project folder e.g. miniblog/.scripts
- Inside .scripts folder Create A file with .sh extension e.g. miniblog/.scripts/deploy.sh
- Write below script inside the created .sh file
```sh
#!/bin/bash
set -e

echo "Deployment started ..."

# Turn ON Maintenance Mode or return true
# if already is in maintenance mode
(php artisan down) || true

# Pull the latest version of the app
git pull origin master

# Install composer dependencies
composer install --optimize-autoloader --no-dev --no-interaction

# Clearing Cache
php artisan cache:clear
php artisan config:clear

# Recreate cache
php artisan optimize

# Run database migrations
php artisan migrate --force

# Turn OFF Maintenance mode
php artisan up

echo "Deployment finished!"
```
- Go inside .scripts Folder then Set File Permission for .sh File
```sh
git update-index --add --chmod=+x deploy.sh
```
- Create Directory Path named .github/workflows inside your root project folder e.g. miniblog/.github/workflows
- Inside workflows folder Create A file with .yml extension e.g. miniblog/.github/workflows/deploy.yml
- Write below script inside the created .yml file
```sh
name: Deploy

# Trigger the workflow on push and
# pull request events on the master branch
on:
  push:
    branches: ["master"]
  pull_request:
    branches: ["master"]

# Authenticate to the the server via ssh
# and run our deployment script
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          port: ${{ secrets.PORT }}
          key: ${{ secrets.SSHKEY }}
          script: "cd /var/www/miniblog && ./.scripts/deploy.sh"
```
- Go to Your Github Repo Click on Settings
- Click on Secrets and Variables from the Sidebar then choose Actions
- On Secret Tab, Click on New Repository Secret
- Add Four Secrets HOST, PORT, USERNAME and SSHKEY as below
```sh
Name: HOST
Secret: Your_Server_IP
```
```sh
Name: PORT
Secret: Your_Server_PORT
```
```sh
Name: USERNAME
Secret: Your_Server_User_Name
```
- You can get Server User Name by loging into your server via ssh then run below command
```sh
whoami
```
- Generate SSH Key for Github Action by Login into Remote Server then run below Command OR You can use old SSH Key But I am creating New one for Github Action
```sh
Syntax:- ssh-keygen -f key_path -t ed25519 -C "your_email@example.com"
Example:- ssh-keygen -f /home/raj/.ssh/gitaction_ed25519 -t ed25519 -C "gitactionautodep"
```
- Open Newly Created Public SSH Keys then copy the key
```sh
cat ~/.ssh/gitaction_ed25519.pub
```
- Open authorized_keys File which is inside .ssh/authroized_keys then paste the copied key in a new line
```sh
cd .ssh
nano authorized_keys
```
- Open Newly Created Private SSH Keys then copy the key, we will use this key to add New Repository Secret On Github Repo
```sh
cat ~/.ssh/gitaction_ed25519
```
```sh
Name: SSHKEY
Secret: Private_SSH_KEY_Generated_On_Server
```
- Commit and Push the change to Your Github Repo
- Get Access to Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```
- Go to Your Project Directory
```sh
Syntax:- cd /var/www/project_folder_name
Example:- cd /var/www/miniblog
```
- Pull the changes from github just once this time.
```sh
git pull
```
- Your Deployment should become automate.
- On Local Machine make some changes in Your Project then Commit and Push to Github Repo It will automatically deployed on Live Server
- You can track your action from Github Actions Tab
- If you get any File Permission error in the action then you have to change file permission accordingly e.g. Maintenance mode ON/OFF may need File Permission so you can set it to 775
- Changing storage/framework File Permission (This is just for example)
```sh
cd /var/www/miniblog/storage
sudo chmod -R 775 framework
```
- All Done

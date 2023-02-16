### How to Point Domain and Deploy Laravel Project using Github on Nginx Remote Server or VPS
- Get Access to Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 1034 raj@216.32.44.12
```
- Verify that all required softwares are installed
```sh
nginx -v
php -v
mysql
composer -v
node -v
npm -v
git --version
```
- Install Nginx, PHP and MySQL https://github.com/geekyshow1/GeekyShowsNotes/blob/main/nginx/LEMP_Stack_Installation.md
- Install Composer https://github.com/geekyshow1/GeekyShowsNotes/blob/main/Install_Composer.md
- Install Node and NPM https://github.com/geekyshow1/GeekyShowsNotes/blob/main/Install_Node_NPM.md
- Install Git
 ```sh
sudo apt install git
```
- Verify Nginx is Active and Running
```sh
sudo service nginx status
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
sudo ufw status verbose
```
- Exit from Remote Server
```sh
exit
```
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
- Copy Project from Local Machine to Remote Server or VPS. There are two ways to do it:-
  1. Using Command Prompt
      - On Local Windows Machine Make Your Project Folder a Zip File using any of the software e.g. winzip
      - Open Command Prompt
      - Copy Zip File from Local Windows Machine to Linux Remote Server
      ```sh
      Syntax:- scp -P Remote_Server_Port Source_File_Path Destination_Path
      Example:- scp -P 1034 miniblog.zip raj@216.32.44.12:
      ```
      - Copied Successfully
      - Get Access to Remote Server via SSH
      ```sh
      Syntax:- ssh -p PORT USERNAME@HOSTIP
      Example:- ssh -p 1034 raj@216.32.44.12
      ```
      - Unzip the Copied Project Zip File
      ```sh
      Syntax:- unzip zip_file_name
      Example:- unzip miniblog.zip
      ```
      
  2. Using Github
      - Open Project on VS Code then add .gitignore file (If needed)
      - Push your Poject to Your Github Account as Private Repo
      - Make Connection between Remote Server and Github Repo via SSH Key
      - Generate SSH Keys
      ```sh
      Syntax:- ssh-keygen -t ed25519 -C "your_email@example.com"
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
      - Clone Project from your github Repo using SSH Path It requires to setup SSH Key on Github
      ```sh
      Syntax:- git clone ssh_repo_path
      Example:- git clone git@github.com:geekyshow1/miniblog.git
      ```
- Move Project Folder to Web Server public directory
```sh
Syntax:- sudo mv project_folder_name /var/www
Example:- sudo mv miniblog /var/www
```
- Create Virtual Host File
```sh
Syntax:- sudo nano /etc/nginx/sites-available/your_domain
Example:- sudo nano /etc/nginx/sites-available/sonamkumari.com
```
- Write following Code in Virtual Host File
```sh
server{
    listen 80;
    listen [::]:80;
    server_name your_domain www.your_domain;
    root /var/www/project_folder_name/public;
    index index.php index.html;
    location / {
         try_files $uri $uri/ /index.php$is_args$args;
    }
    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
    }
}
```
- Enable Virtual Host or Create Symbolic Link of Virtual Host File
```sh
Syntax:- sudo ln -s /etc/nginx/sites-available/virtual_host_file /etc/nginx/sites-enabled/virtual_host_file
Example:- sudo ln -s /etc/nginx/sites-available/sonamkumari.com /etc/nginx/sites-enabled/sonamkumari.com
```
- Check Configuration is Correct or Not
```sh
sudo nginx -t
```
- Restart Nginx
```sh
sudo service nginx restart
```
- Copy .env.example to .env
```sh
cd /var/www/project_folder_name
cp .env.example .env
```
- Install Dependencies
```sh
composer install --optimize-autoloader --no-dev
npm install
```
- Generate Application Key
```sh
php artisan key:generate
```
- Set Permission for storage and bootstrap/cache Folder
```sh
sudo chown -R www-data:www-data storage
sudo chown -R www-data:www-data bootstrap/cache
```
- Add your User and web server user to www-data group
```sh
sudo usermod -a -G www-data raj
sudo usermod -a -G www-data nginx
```
- Verify Your User is in Webserver Group
```sh
sudo groups raj
sudo groups nginx
```
- Set storage's File Permission to 644
```sh
sudo find storage -type f -exec chmod 644 {} \;
```
- Set storage's Folder Permission to 775
```sh
sudo find storage -type d -exec chmod 775 {} \;
```
- Open Mysql
- Create Database User https://github.com/geekyshow1/GeekyShowsNotes/blob/main/AssignMySQL_DB_To_User.md
- Create Database
- Open .env File
```sh
cd /var/www/project_folder
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
- Create phpmyadmin Symlink to open phpmyadmin on web browser
```sh
sudo ln -s /usr/share/phpmyadmin /var/www/project_folder_name/public/phpmyadmin
```
- If you make any changes in your project then you need to pull the new changes from github repo. It will update your website with latest changes. (Only if you have used Github)
```sh
git pull
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

# Install npm dependencies
npm install --yes

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
          script: "cd /var/www/project_folder_name && ./.scripts/deploy.sh"
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


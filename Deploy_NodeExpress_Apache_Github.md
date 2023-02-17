### How to Point Domain and Host Node Express Project using Github on Apache Web Server VPS Hosting
#### What is PM2 ?
PM2 is a powerful, widely-used, and feature-rich, production-ready process manager for Node.js. Restarting PM2 with the processes it manages every time your server boots/reboots is critical. One of PM2’s key features is support for a startup script (generated dynamically based on the default init system on your server), that restarts PM2 and your processes at every server restart.

- Login to Your Domain Provider Website
- Navigate to Manage DNS
- Add Following Records:

| Type | Host/Name | Value |
| :---: | :---: | :--- |
| A     | @     | Your Remote Server IP |
| A     | www   | Your Remote Server IP |
| AAAA  | @     | Your Remote Server IPv6 |
| AAAA  | www   | Your Remote Server IPv6 |

- Open Project on VS Code then add .gitignore file also create .env.example file
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
node -v
npm -v
pm2 --version
mongod --version
git --version
```
- Install Apache
```sh
sudo apt install apache2
```
- Install Node and npm
```sh
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```
- Install PM2
```sh
sudo npm install -g pm2@latest
```
- Add PM2 Process on Startup
```sh
sudo pm2 startup
```
- Install MongoDB 6.0.3 on Ubuntu 22.04
  Follow this https://github.com/geekyshow1/GeekyShowsNotes/blob/main/InstallConfigMongoDB.md
- Install Git
```sh
sudo apt install git
```
- Verify Apache2 is Active and Running
```sh
sudo service apache2 status
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
sudo ufw status verbose
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
- Go to Your Project Directory
```sh
Syntax:- cd /var/www/project_folder_name
Example:- cd /var/www/miniblog
```
- Install Dependencies
```sh
npm install
```
- Install Node JS MongoDB Driver (If needed)
```sh
npm install mongodb
```
- Create Virtual Host File
```sh
sudo nano /etc/apache2/sites-available/your_domain.conf
```
- Add Following Code in Virtual Host File
- Change the IP and Port According to your Node Application Code
```sh
<VirtualHost *:80>
        ServerName www.example.com
        ServerAdmin contact@example.com
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:8000/
        ProxyPassReverse / http://127.0.0.1:8000/
        <Directory "/var/www/project_folder_name">
                AllowOverride All
        </Directory>
</VirtualHost>
```
- Check Configuration is correct or not
```sh
sudo apache2ctl configtest
```
- Enable the Proxy module with Apache
```sh
sudo a2enmod proxy
sudo a2enmod proxy_http
```
- Enable Virtual Host
```sh
cd /etc/apache2/sites-available/
sudo a2ensite your_domain.conf
```
- Restart Apache2
```sh
sudo service apache2 restart
```
- Start Node Express Application using pm2
```sh
cd /var/www/miniblog
sudo NODE_ENV=production pm2 start app.js --watch --update-env --ignore-watch="user_upload_path"
```
- Save PM2 Process
```sh
sudo pm2 save
```
- Check PM2 Status
```sh
sudo pm2 status
```
- Connect to MongoDB shell with Super User,
  To Create Super User Follow https://github.com/geekyshow1/GeekyShowsNotes/blob/main/CreateMongoDBUser.md
```sh
Syntax:- mongosh --port 27017 --authenticationDatabase "database_name_where_user_stored" -u "username" -p "password"
Example:- mongosh --port 27017 --authenticationDatabase "admin" -u "superuser" -p "Hello123456"
```
- Show database
```sh
show dbs
```
- Create New Database
```sh
Syntax:- use database_name
Example:- use blogdb
```
- Create New User
```sh
Syntax:- db.createUser({user:"username", pwd:passwordPrompt(), roles:[{role:"readWrite", db:"database_name"}]})
Example:- db.createUser({user:"rahul", pwd:passwordPrompt(), roles:[{role:"readWrite", db:"blogdb"}]})
```
- Verify Users
```sh
show users
```
- Exit Mongo Shell
```sh
quit()
```
- Access Mongo Shell as User to Verify the User Credentials
```sh
Syntax:- mongosh --port 27017 --authenticationDatabase "database_name_where_user_stored" -u "username" -p "password"
Example:- mongosh --port 27017 --authenticationDatabase "blogdb" -u "rahul" -p "Hello123456"
```
- Exit Mongo Shell
```sh
quit()
```
- Restart MongoDB
```sh
sudo service mongod restart
```
- Open .env
```sh
cd /var/www/miniblog
sudo nano .env
```
- Make below changes
```sh
HOST = 127.0.0.1
PORT = 8000

DATABASE_URL = "mongodb://127.0.0.1:27017"
DBNAME = "Your_Database_Name"
DBUSERNAME = "Your_Database_Username"
DBPASSWORD = "Your_Database_Password"
DBAUTHSOURCE = "database_name_where_user_stored"
```
- Restart Apache2
```sh
sudo service apache2 restart
```
- Start Node Express Application using pm2
```sh
cd /var/www/miniblog
sudo NODE_ENV=production pm2 start app.js --watch --update-env --ignore-watch="user_upload_path"
```
- Save PM2 Process
```sh
sudo pm2 save
```
- You can check error logs If you get any error:
```sh
cd /var/log
su
cd apache2
cat error.log
```
- You can Clear Error Logs (Optional)
```sh
sudo bash -c 'echo > /var/log/apache2/error.log'
```
##
### How to Automate Node Express Deployment using Github Action
- On Your Local Machine, Open Your Project using VS Code or any Editor
- Create A Folder named .scripts inside your root project folder e.g. miniblog/.scripts
- Inside .scripts folder Create A file with .sh extension e.g. miniblog/.scripts/deploy.sh
- Write below script inside the created .sh file
```sh
#!/bin/bash
set -e

echo "Deployment started ..."

# Pull the latest version of the app
git pull origin master
echo "New changes copied to server !"

echo "Installing Dependencies..."
npm install --yes

echo "Deployment Finished!"
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
- If you get any File Permission error in the action then you have to change file permission accordingly.
- All Done

### Extra PM2 Information:
- Add PM2 Process on Startup
```sh
sudo pm2 startup
```
- List All PM2 Process
```sh
sudo pm2 list
```
- Kill PM2 Process
```sh
sudo pm2 kill
```
- Delete App from PM2 Process
```sh
sudo pm2 delete app_name
```
- Save PM2 Process
```sh
sudo pm2 save
```
- Save PM2 Process with --force flag
```sh
sudo pm2 save --force
```
- Restore Last Saved PM2 Process
```sh
sudo pm2 resurrect
```
- Clear PM2 Dump File
```sh
sudo pm2 cleardump
```
- Remove PM2 Process from Startup
```sh
sudo pm2 unstartup
```

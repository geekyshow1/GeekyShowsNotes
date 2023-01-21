### How to Point Domain and Host Node Express Project on Apache Web Server VPS Hosting
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

- On Local Windows Machine Make Your Project Folder a Zip File using any of the software e.g. winzip
- Open Command Prompt
- Copy Zip File from Local Windows Machine to Linux Remote Server
```sh
Syntax:- scp -P Port_number Source_File_Path Destination_Path
Example:- scp -P 22 miniblog.zip raj@216.32.44.12:
```
- Get Access to Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 raj@216.32.44.12
```
#### Note:- Run Below Commands on Your Remote Server Linux Machine or VPS, Not on Your Local Windows Machine
- Verify that all required softwares are installed
```sh
apache2 -v
node -v
npm -v
pm2 --version
mongod --version  
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

- Verify Apache2 is Active and Running
```sh
sudo service apache2 status
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
sudo ufw status verbose
```
- Run ls command to verify that the zip file is present
```sh
ls
```
- Unzip the Copied Zip File
```sh
Syntax:- unzip zip_file_name
Example:- unzip miniblog.zip
```
- Move Project Folder from User Home to Web Server Public Directory
```sh
Syntax:- sudo mv project_folder_name Destination_Path
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
        <Directory "/var/www/miniblog">
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
sudo NODE_ENV=production pm2 start app.js --update-env
```
- Save PM2 Process
```sh
sudo pm2 save
```
- Check PM2 Status
```sh
sudo pm2 status
```
- Connect to MongoDB shell with Super User
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
sudo NODE_ENV=production pm2 start app.js --update-env
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


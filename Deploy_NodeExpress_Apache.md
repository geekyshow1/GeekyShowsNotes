### How to Point Domain and Host Node Express Project on Apache Web Server VPS Hosting
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
- Add PM2 to the startup script
```sh
sudo pm2 startup systemd
```
- Install MongoDB 6.0.3 on Ubuntu 22.04
- Installation Process depends on MongoDB Version and Ubuntu Version
```sh
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update

sudo apt install mongodb-org
```
- Enable MongoDB to start at system startup
```sh
sudo systemctl enable mongod
```
- Start MongoDB
```sh
sudo service mongod start
```
- Check MongoDB Status
```sh
sudo service mongod status 
```
- Open and Edit MongoDB config file
```sh
sudo nano /etc/mongod.conf
```
- Make below changes in config file
```sh
security:
  authorization: enabled
net:
  port: 27017
  bindIp: 127.0.0.1,Your_IP_Address
```
- Check Port is Allowed through Firewall
```sh
sudo ufw status verbose
```
- If Port is not Allowed then Allow it through Firewall
```sh
sudo ufw allow 27017
```
- Restart MongoDB
```sh
sudo service mongod restart
```
- Confirm if MongoDB is allowing remote connections
```sh
sudo lsof -i | grep mongo
```
- Install Node JS MongoDB Driver (If needed)
```sh
sudo npm install -P mongodb
```
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
- Create Virtual Host File
```sh
sudo nano /etc/apache2/sites-available/your_domain.conf
```
- Add Following Code in Virtual Host File
```sh
<VirtualHost *:80>
        ServerName www.example.com
        ServerAdmin contact@example.com
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:3000/
        ProxyPassReverse / http://127.0.0.1:3000/
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
sudo a2emmod proxy
sudo a2emmod proxy_http
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
- Connect to MongoDB shell
```sh
mongosh
```
- Show database
```sh
show dbs
```
- Create New Database
```sh
Syntax:- use database_name
Example:- use miniblog
```
- Create New User
```sh
Syntax:- db.createUser({user:"username", pwd:passwordPrompt(), roles:[{role:"readWrite", db:"database_name"}]})
Example:- db.createUser({user:"rahul", pwd:passwordPrompt(), roles:[{role:"readWrite", db:"miniblog"}]})
```
- Verify Users
```sh
show users
```
- Access Mongo Shell as User:
```sh
Syntax:- mongosh --port 27017 --authenticationDatabase "database_name_where_user_stored" -u "username" -p "password"
mongosh --port 27017 --authenticationDatabase "miniblog" -u "rahul" -p "Hello123456#"
```
- Open .env
```sh
cd /var/www/miniblog
sudo nano .env
```
- Make below changes
```sh
HOST
DBNAME
DBUSERNAME
DBPASSOWRD
DBPORT
```
- Restart Apache2
```sh
sudo service apache2 restart
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


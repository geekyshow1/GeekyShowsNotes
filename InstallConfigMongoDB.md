### How to Install MongoDB 6.0.3 on Ubuntu 22.04
- Verify MongoDb Installed or Not
```sh
mongod --version  
```
- Install MongoDB 6.0.3 on Ubuntu 22.04
- Installation Process depends on MongoDB Version and Ubuntu Version
```sh
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update

sudo apt upgrade

sudo apt install mongodb-org
```
- Verify MongoDb Installed or Not
```sh
mongod --version  
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

### How to Configure MongoDB
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
  bindIp: 127.0.0.1
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

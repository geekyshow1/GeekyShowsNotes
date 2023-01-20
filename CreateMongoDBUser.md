### How to Create MongoDB Super User
- If Authorization is Enable then Disable it by commenting below code
```sh
sudo nano /etc/mongod.conf
#security:
  #authorization: enabled
```
- Restart MongoDB
```sh
sudo service mongod restart
```
- Connect to MongoDB shell
```sh
mongosh
```
- Show database
```sh
show dbs
```
- Change to admin database
```sh
use admin
```
- Create superuser with all privileges
```sh
Syntax:- db.createUser({user: "username" , pwd: passwordPrompt() , roles: ["root"]})
Example:- db.createUser({user: "superuser" , pwd: passwordPrompt() , roles: ["root"]})
```
- Verify Users
```sh
show users
```
- If Authorization is Disable then Enable it by removing comments from below code
```sh
sudo nano /etc/mongod.conf
security:
  authorization: enabled
```
- Restart MongoDB
```sh
sudo service mongod restart
```
- Access Mongo Shell as Super User:
```sh
Syntax:- mongosh --port 27017 --authenticationDatabase "database_name_where_user_stored" -u "username" -p "password"
Example:- mongosh --port 27017 --authenticationDatabase "admin" -u "superuser" -p "Hello123456"
```
- To Make Connection use:
```sh
Syntax:- mongodb://username:password@IP_Address:27017/database_name
Example:- mongodb://superuser:Hello123456@216.32.44.12:27017/blogdb
```

### How to Create MongoDB User and Assign to a Database
- Connect to MongoDB shell
```sh
- If Authorization Disable
    mongosh

- If Authorization Enable and have Superuser (Recommended)
    mongosh --port 27017 --authenticationDatabase "admin" -u "superuser" -p "Hello123456"
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
- Access Mongo Shell as User:
```sh
Syntax:- mongosh --port 27017 --authenticationDatabase "database_name_where_user_stored" -u "username" -p "password"
Example:- mongosh --port 27017 --authenticationDatabase "blogdb" -u "rahul" -p "Hello123456"
```
- To Make Connection use:
```sh
Syntax:- mongodb://username:password@IP_Address:27017/database_name
Example:- mongodb://rahul:Hello123456@216.32.44.12:27017/blogdb
```

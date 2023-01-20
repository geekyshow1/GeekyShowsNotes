### How to Create MongoDB Admin User
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
- Create boss aka admin user with all privileges and setup password
```sh
Syntax:- db.createUser({user: "username" , pwd: passwordPrompt() , roles: [{ role: "userAdminAnyDatabase" , db: "admin"}]})
Example:- db.createUser({user: "boss" , pwd: passwordPrompt() , roles: [{ role: "userAdminAnyDatabase" , db: "admin"}]})
```
- Verify Users
```sh
show users
```
- Access Mongo Shell as Boss:
```sh
Syntax:- mongosh --port 27017 --authenticationDatabase "database_name_where_user_stored" -u "username" -p "password"
Example:- mongosh --port 27017 --authenticationDatabase "admin" -u "boss" -p "Hello123456#"
```
- To Make Connection use:
```sh
Syntax:- mongodb://username:password@IP_Address:27017/database_name
Example:- mongodb://boss:Hello123456#@216.32.44.12:27017/miniblog
```

### How to Create MongoDB User and Assign to a Database
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
- To Make Connection use:
```sh
Syntax:- mongodb://username:password@IP_Address:27017/database_name
Example:- mongodb://rahul:Hello123456#@216.32.44.12:27017/miniblog
```

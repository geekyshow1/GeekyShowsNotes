### How to Assign Specific Database to Specific User in MySQL

- Login to MySQL with Root User
```sh
mysql -u root -p
```
- Create New User
```sh
Syntax:- CREATE USER 'user_name'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'user_password';
Example:- CREATE USER 'rahul'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'Hello123456#';
```
- Create New Database
```sh
Syntax:- CREATE DATABASE database_name;
Example:- CREATE DATABASE osmsdb;
```
- Verify Created New Database and New User
```sh
show databases;
use mysql;
select user from user;
```
- Assign Database to Specific user
```sh
Syntax:- GRANT ALL PRIVILEGES ON database_name.* TO 'user_name'@'localhost';
Example:- GRANT ALL PRIVILEGES ON osmsdb.* TO 'rahul'@'localhost';
```
- Exit from MySQL
```sh
exit
```
- Restart Apache
```sh
service apache2 restart
```

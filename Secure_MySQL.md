### How to Set Password for MySQL Root User and Create a New User with Root Privileges
#
#### Note - You must login on your operating system or VPS Remote server as root user (Not Sudo User). You can switch from sudo to root. 
#### What is mysql_secure_installation ?
This program enables you to improve the security of your MySQL installation in the following ways:
- You can set a password for root accounts
- You can remove root accounts that are accessible from outside the local host.
- You can remove anonymous-user accounts.
- You can remove the test database (which by default can be accessed by all users, even anonymous users), and privileges that permit anyone to access databases with names that start with test_.

#### What is caching_sha2_password ?
It is a Plugins that perform authentication using SHA-256 password hashing. This is stronger encryption than that available with native authentication. There are some other plugins e.g. mysql_native_password.

- Install MySQL If not Installed
```sh
apt update
apt upgrade
apt install mysql-server
```
- Login to MySQL
```sh
mysql
```
- Set Password for Root
```sh
Syntax:- ALTER USER 'user_name'@'localhost' IDENTIFIED WITH caching_sha2_password by 'user_password';
Example:- ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password by 'Hello123456#';
```
- Exit from MySQL
```sh
exit
```
- Run the mysql_secure_installation Program
```sh
mysql_secure_installation
```
- Login to MySQL as Root User
```sh
mysql -u root -p
```
- Create New User
```sh
Syntax:- CREATE USER 'user_name'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'user_password';
Example:- CREATE USER 'kumar'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'Hello123456#';
```
- Grant New User All Privileges
```sh
Syntax:- GRANT ALL PRIVILEGES ON *.* TO 'user_name'@'localhost' WITH GRANT OPTION;
Syntax:- GRANT ALL PRIVILEGES ON *.* TO 'kumar'@'localhost' WITH GRANT OPTION;
```
- Exit from MySQL
```sh
exit
```
- Restart Web Server
```sh
- If Apache
service apache2 restart

- If Nginx
service nginx restart
```

### How to Set Password for MySQL Root User and Create a New User with Root Privileges
#### What is mysql_secure_installation ?
This program enables you to improve the security of your MySQL installation in the following ways:
- You can set a password for root accounts
- You can remove root accounts that are accessible from outside the local host.
- You can remove anonymous-user accounts.
- You can remove the test database (which by default can be accessed by all users, even anonymous users), and privileges that permit anyone to access databases with names that start with test_.

#### What is caching_sha2_password ?
It is a Plugins that perform authentication using SHA-256 password hashing. This is stronger encryption than that available with native authentication.

- Install MySQL If not Installed
```sh
apt install mysql-server
```
- Set Secure Password for Root
```sh
mysql_secure_installation
```
- Login to MySQL with as Root User
```sh
mysql -u root -p
```
- Create New User
```sh
CREATE USER 'user_name'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'user_password';
```
- Grant New User All Privileges
```sh
GRANT ALL PRIVILEGES ON *.* TO 'user_name'@'localhost' WITH GRANT OPTION;
```
- Exit from MySQL
```sh
exit
```
- Restart Apache
```sh
service apache2 restart
```

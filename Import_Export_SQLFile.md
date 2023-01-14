### How to Import MySQL Database or SQL File
- Access MySQL with Root User
```sh
mysql -u root -p
```
- Create New Database
```sh
Syntax:- CREATE DATABASE Database_name;
Example:- CREATE DATABASE osmsdb;
```
- Access Newly Created Database
```sh
Syntax:- USE Database_name;
Example:- USE osmsdb;
```
- Import Database or SQL File
```sh
Syntax:- source sql_file_path
Example:- source /var/myimporteddb/osmsdb.sql
```
#
### How to Export MySQL Database as SQL File
- Export MySQL Database as SQL File
```sh
Syntax:- mysqldump -u db_user_name -p Database_Name > File_Path/File_Name.sql
Example:- mysqldump -u root -p dps > /var/myexporteddb/dps.sql
```

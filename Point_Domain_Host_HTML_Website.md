### How to Point Domain and Host HTML Website on Remote Server or VPS

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
Example:- scp -P 22 osmsProject.zip root@216.32.44.12:/var/www
```
- Copied Successfully

- To Access Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```
#### Note:- Run Below Commands on Your Remote Server Linux Machine or VPS, Not on Your Local Windows Machine
- Go to the Destination Path Where you copied the zip file
```sh
Syntax:- cd Destination_Path
Example:- cd /var/www
```
- Run ls command to verify that the zip file is present
```sh
ls
```
- Unzip the Copied Zip File
```sh
Syntax:- unzip zip_file_name
Example:- unzip osmsProject.zip
```
- Verify that all required softwares are installed
```sh
apache2 -v
mysql
php -v
```
- Verify Apache2 is Active and Running
```sh
service apache2 status
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
ufw status verbose
```
- Create Virtual Host File
```sh
nano /etc/apache2/sites-available/your_domain.conf
```
- Add Following Code in Virtual Host File
```sh
<VirtualHost *:80>
    ServerName www.example.com
    ServerAdmin contact@example.com
    DocumentRoot /var/www/project_folder_name
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Enable Virtual Host
```sh
cd /etc/apache2/sites-available/
a2ensite your_domain.conf
```
- You can Disable Default Virtual Host
```sh
cd /etc/apache2/sites-available/
a2dissite 000-default.conf
```
- Restart Apache2
```sh
service apache2 restart
```

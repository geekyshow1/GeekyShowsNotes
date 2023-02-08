### How to Point Domain and Host HTML Website on Nginx Remote Server or VPS
#
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
Syntax:- scp -P Remote_Server_Port Source_File_Path Destination_Path
Example:- scp -P 1034 myproject.zip raj@216.32.44.12:
```
- Copied Successfully

- To Access Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 1034 raj@216.32.44.12
```
#### Note:- Run Below Commands on Your Remote Server Linux Machine or VPS, Not on Your Local Windows Machine
- Unzip the Copied Project Zip File
```sh
Syntax:- unzip zip_file_name
Example:- unzip myproject.zip
```
- Move Project Folder to Web server Public Directory
```sh
Syntax:- sudo mv Project_Folder Destination_Path
Example:- sudo mv myproject /var/www
```
- Verify Nginx is Installed
```sh
nginx -v
```
- Verify Nginx is Active and Running
```sh
sudo service nginx status
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
sudo ufw status verbose
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
- Restart Nginx
```sh
sudo service nginx restart
```

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
Syntax:- sudo mv project_folder_name /var/www
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
Syntax:- sudo nano /etc/nginx/sites-available/your_domain
Example:- sudo nano /etc/nginx/sites-available/sonamkumari.com
```
- Add Following Code in Virtual Host File
```sh
Syntax:-
server{
    listen 80;
    listen [::]:80;
    server_name your_domain www.your_domain;
    root /var/www/project_folder_name;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}

Example:- 
server{
    listen 80;
    listen [::]:80;
    server_name sonamkumari.com www.sonamkumari.com;
    root /var/www/sonamkumari;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```
- Enable Virtual Host or Create Symbolic Link of Virtual Host File
```sh
cd /etc/nginx/sites-available/
Syntax:- sudo ln -s /etc/nginx/sites-available/virtual_host_file /etc/nginx/sites-enabled/virtual_host_file
Example:- sudo ln -s /etc/nginx/sites-available/sonamkumari.com /etc/nginx/sites-enabled/sonamkumari.com
```
- Check Configuration is Correct or Not
```sh
sudo nginx -t
```
- Restart Nginx
```sh
sudo service nginx restart
```

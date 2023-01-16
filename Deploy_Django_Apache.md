### How to Point Domain and Host Django Project on Apache Web Server VPS Hosting
- Login to Your Domain Provider Website
- Navigate to Manage DNS
- Add Following Records:

| Type | Host/Name | Value |
| :---: | :---: | :--- |
| A     | @     | Your Remote Server IP |
| A     | www   | Your Remote Server IP |
| AAAA  | @     | Your Remote Server IPv6 |
| AAAA  | www   | Your Remote Server IPv6 |

- On Local Windows Machine, Goto Your Project Folder then create follow below instruction:
```sh
- Open Terminal
- Activate Your virtual Env
- Create requirements.txt File
  pip freeze > requirements.txt
- Deactivate Virtual Env
```
- On Local Windows Machine Make Your Project Folder a Zip File using any of the software e.g. winzip
- Open Command Prompt
- Copy Zip File from Local Windows Machine to Linux Remote Server
```sh
Syntax:- scp -P Port_number Source_File_Path Destination_Path
Example:- scp -P 22 miniblog.zip raj@216.32.44.12:
```
- Copied Successfully

- Get Access to Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 raj@216.32.44.12
```
#### Note:- Run Below Commands on Your Remote Server Linux Machine or VPS, Not on Your Local Windows Machine
- Verify that all required softwares are installed
```sh
  apache2 -v
  python --version
  apache2ctl -M
  pip --version
- SQLite is Included with Python
  python -c "import sqlite3; print(sqlite3.sqlite_version)"
```
- If Required Softwares and Modules are not Installed then Install them:
```sh
sudo apt install apache2
sudo apt install python
sudo apt install libapache2-mod-wsgi-py3
sudo apt install python3-pip
```
- Install virtualenv
```sh
pip list
sudo pip install virtualenv
```
- Verify Apache2 is Active and Running
```sh
sudo service apache2 status
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
sudo ufw status verbose
```
- Run ls command to verify that the zip file is present
```sh
ls
```
- Unzip the Copied Zip File
```sh
Syntax:- unzip zip_file_name
Example:- unzip miniblog.zip
```
- Move Project Folder from User Home to Web Server Public Directory
```sh
Syntax:- sudo mv project_folder_name Destination_Path
Example:- sudo mv miniblog /var/www
```
- Go to Your Project Directory
```sh
Syntax:- cd /var/www/project_folder_name
Example:- cd /var/www/miniblog
```
- Create Virtual env
```sh
Syntax:- virtualenv env_name
Example:- virtualenv mb
```
- Activate Virtual env
```sh
Syntax:- source virtualenv_name/bin/activate
Example:- source mb/bin/activate
```
- Install Dependencies
```sh
pip install -r requirements.txt
```
- Create Virtual Host File
```sh
sudo nano /etc/apache2/sites-available/your_domain.conf
```
- Add Following Code in Virtual Host File
```sh
<VirtualHost *:80>
    ServerName www.example.com
    ServerAdmin contact@example.com
    DocumentRoot /var/www/project_folder_name
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    
    Alias /static /var/www/project_folder_name/static
    <Directory /var/www/project_folder_name/static>
        Require all granted
    </Directory>
    
    Alias /media /var/www/project_folder_name/media
    <Directory /var/www/project_folder_name/media>
        Require all granted
    </Directory>
    
    <Directory /var/www/project_folder_name/Inner_project_folder_name>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>
    
    WSGIDaemonProcess project_folder_name python-home=/var/www/project_folder_name/myprojectenv python-path=/var/www/project_folder_name
    WSGIProcessGroup project_folder_name
    WSGIScriptAlias /  /var/www/project_folder_name/inner_project_folder_name/wsgi.py
</VirtualHost>
```
- Check Configuration is correct or not
```sh
sudo apache2ctl configtest
```
- Enable Virtual Host
```sh
cd /etc/apache2/sites-available/
sudo a2ensite your_domain.conf
```
- Restart Apache2
```sh
sudo service apache2 restart
```
- Open Django Project settings.py
```sh
cd /var/www/miniblog/miniblog
sudo nano settings.py
```
- Make below changes
```sh
ALLOWED_HOST = ["your_domain"]
DEBUG = False
STATIC_URL = 'static/'
STATIC_ROOT = BASE_DIR / 'static'
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```
- Restart Apache2
```sh
sudo service apache2 restart
```
- You can check error logs If you get any error:
```sh
cd /var/log
su
cd apache2
cat error.log
```
- You can Clear Error Logs (Optional)
```sh
sudo bash -c 'echo > /var/log/apache2/error.log'
```
- If get Error mod_wsgi (pid=1234): Failed to proxy response from daemon then follow below instructions:
- Open apache2.conf
```sh
cd /etc/apache2
sudo nano apache2.conf
```
- Write below code in the bottom of apache2.conf file
```sh
WSGIApplicationGroup %{GLOBAL}
```
- To Know more about %{GLOBAL} follow this link: https://modwsgi.readthedocs.io/en/develop/configuration-directives/WSGIApplicationGroup.html

- Serve Static Files
```sh
python manage.py collectstatic
```
- Create Database Tables
```sh
python manage.py makemigrations
python manage.py migrate
```
- Set Permission for database file, media file and bootstrap/cache Folder
- Make Webserver as owner for database file, media and bootstrap/cache. Our Webserver is running as www-data and group is also www-data.
```sh
Syntax:- 
sudo chown -R www-data:www-data project_folder_name
sudo chown -R www-data:www-data database_file_name
sudo chown -R www-data:www-data media_folder_name

Example:-
sudo chown -R www-data:www-data miniblog
sudo chown -R www-data:www-data db.sqlite3
sudo chown -R www-data:www-data media
```
- You may face problem if you work with FTP so to fix this add your user to webserver user group following below instruction:
- Check Your User Group
```sh
sudo groups raj
```
- Add your User to webserver group
```sh
sudo usermod -a -G www-data raj
```
- Verify Your User is in Webserver Group
```sh
sudo groups raj
```
- Set SQLite DB File Permission to 664
```sh
sudo chmod 664 db.sqlite3
```
- Done
- If needed Deactivate Virtual env
```sh
deactivate
```


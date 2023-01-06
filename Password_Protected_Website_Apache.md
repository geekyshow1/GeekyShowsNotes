### Passowrd Protection in Website running on Apache Hosted on Linux Remote Server or VPS

- Create Login Credentials
```sh
htpasswd -c /etc/apache2/.htpasswd user_name
```
- Verify that Credential has been Created
```sh
cat /etc/apache2/.htpasswd
```
- Go to /etc/apache2/sites-available
```sh
cd /etc/apache2/sites-available
```
- Open the Required Virtual Host File
```sh
nano 000-default.conf
```
- Add below Content
```sh
<Directory /var/www/html/admin>
        AuthType Basic
        AuthName "admin area"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
</Directory>
```
- Restart Apache
```sh
service apache2 restart
```

### Passowrd Protection in Website running on Nginx Hosted on Linux Remote Server or VPS

- Install Util
```sh
sudo apt install apache2-utils
```
- Create Login Credentials
```sh
sudo htpasswd -c /etc/nginx/.htpasswd user_name
```
- Verify that Credential has been Created
```sh
cat /etc/nginx/.htpasswd
```
- Go to /etc/nginx/sites-available
```sh
cd /etc/nginx/sites-available
```
- Open the Required Virtual Host File
```sh
sudo nano default
```
- Add below Content
```sh
location /admin {
  try_files $uri $uri/ =404;
  auth_basic "admin area";
  auth_basic_user_file /etc/nginx/.htpasswd;
}
```
- Restart Nginx
```sh
sudo service nginx restart
```

### How to Configure PHP Nginx
- Create/Open Virtual Host File
```sh
Syntax:- sudo nano /etc/nginx/sites-available/your_domain
Example:- sudo nano /etc/nginx/sites-available/sonamkumari.com
```
- Add Following Code in Virtual Host File
```sh
- Method 1
server{
    listen 80;
    listen [::]:80;
    server_name your_domain www.your_domain;
    root /var/www/project_folder_name;
    index index.php;
    location / {
        try_files $uri $uri/ =404;
    }
    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
    }
}

- Method 2
server{
    listen 80;
    listen [::]:80;
    server_name your_domain www.your_domain;
    index index.php;
    location / {
        root your_project_root_directory_path;
        try_files $uri $uri/ =404;
        index index.php;
        location ~ \.php$ {
            fastcgi_pass unix:/run/php/php8.1-fpm.sock;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        }
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
- Fix Error: connect() to unix:/run/php/php8.1-fpm.sock failed (13: Permission denied) while connecting to upstream
- The problem is php user is www-data but nginx user is nginx so it doesnt have permission.
- There are multiple ways to resolve this: 
```sh
- Open file /etc/php/8.1/fpm/pool.d/www.conf then change the below details
  user = nginx
  group = nginx
  listen.owner = nginx
  listen.group = nginx
  
// OR
- By adding nginx user to www-data group
  sudo usermod -a -G www-data nginx

// OR

- Open /etc/nginx/nginx.conf then change the below detail
  user  www-data;
```
- Restart PHP-FPM and Nginx
```sh
sudo service php8.1-fpm restart
sudo service nginx restart
```

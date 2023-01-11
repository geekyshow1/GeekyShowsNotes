### How to Remove Apache2 from Remote Server or VPS having Ubuntu OS

- Stop Apache Service
```sh
service apache2 stop
```
- Remove and Cleanup all apache2 packages
```sh
apt purge apache2 apache2-utils apache2.2-bin apache2-common
//or 
apt purge apache2 apache2-utils apache2-bin apache2.2-common
```
- Remove other dependencies
```sh
apt autoremove
```
- Check if There is any Folder or File
```sh
whereis apache2
```
- Remove if found any
```sh
Syntax:- rm -r File_or_Folder_Path
Syntax:- rm -r /etc/apache2
```

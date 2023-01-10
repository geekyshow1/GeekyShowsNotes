### How to Copy File from Windows to Linux Remote Server or VPS
Note:- You can Copy Complete Project Folder by making it a Zip File then Unzip it on Server.

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
#
#
### How to Copy File from Linux Remote Server or VPS to Windows Local Machine
Note:- You can Copy Complete Project Folder by making it a Zip File then Unzip it on Local Machine.
- To Access Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```
- Install zip
```sh
apt install zip
```
- On VPS or Linux Remote Server Make Your Project Folder a Zip File
- Zip the Folders and Files
```sh
Syntax:- zip -r zip_filename.zip target_folder_name
Example:- zip -r mynewproject.zip mynewproject
```

#### Note:- Run Below Commands on Your Local Windows Machine, Not on Your Remote Server Linux Machine or VPS 
- Open Command Prompt
- Copy Zip File from Linux Remote Server to Local Windows Machine
```sh
Syntax:- scp -P Port_number Source_File_Path Destination_Path
Example:- scp -P 22 root@216.32.44.12:/var/www/mynewproject.zip D:\new\
```
- Copied Successfully


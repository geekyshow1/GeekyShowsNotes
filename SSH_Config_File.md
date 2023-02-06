### How to create and use SSH Config File 
#
##### To Ease the Remote Server Login from Window Machine
- Open Notepad then save file with filename 'config' on path C:\Users\YourUser\\.ssh
- We will convert newly created config text file to config file, To do this Open cmd then
```sh
cd c:\users\yourUser\.ssh
copy config.txt config
del config.txt
```
- Edit config using notepad or notepad++
- Write below credentials then save it
```sh
Syntax:- 
Host ANY_NAME_1
	HostName SERVER_IP or SERVER_DOMAIN
	User SERVER_USERNAME
	Port SERVER_PORT
	IdentityFile ~/.ssh/YOUR_PVT_SSH_KEY_NAME
	IdentitiesOnly yes

Example:-
Host geek-vps-ubuntu
	HostName 181.219.74.228
	User rajesh
	Port 1037
	IdentityFile ~/.ssh/raj_ed25519
	IdentitiesOnly yes
```
- You can add as many as Host you need. This way you can manage multiple users and their respective ssh keys.
- Now you can use Host to login into Remote Server via cmd or terminal
```sh
Syntax:- ssh HOST
Example:- ssh geek-vps-ubuntu
```

##### To Ease the Github Clone Pull from Remote Server Linux Machine
- Open CMD or Terminal
- To Get Access to Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 raj@216.32.44.12
OR
Syntax:- ssh HOST
Example:- ssh geek-vps-ubuntu 
```
- Create config file
```sh
touch ~/.ssh/config
```
- Edit config using nano
```sh
nano ~/.ssh/config
```
- Write below credentials then save it
```sh
Syntax:- 
Host ANY_NAME_1
	HostName GITHUB_IP or GITHUB_DOMAIN
	User GITHUB_USERNAME
	IdentityFile ~/.ssh/YOUR_PVT_SSH_KEY_NAME
	IdentitiesOnly yes

Example:-
Host react_project
	HostName github.com
	User geekyshow1
	IdentityFile ~/.ssh/react_ed25519
	IdentitiesOnly yes
```
- You can add as many as Host you need. This way you can manage multiple github repo and their respective ssh keys.
- Now you can use git command as follow
```sh
- Verify Connection
  Syntax:- ssh -T git@HOST
  Example:- ssh -T git@react_project

- Clone Git Repo
  Syntax:- git clone git@HOST:GITHUB_USERNAME/REPO_NAME
  Example:- git clone git@react_project:geekyshow1/ecomm.git
  
- Git Pull will work as normal
  git pull
  
```

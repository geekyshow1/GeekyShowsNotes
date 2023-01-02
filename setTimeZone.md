### How to Set TimeZone

- To Get Access via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 21350 u27653@216.32.44.12
```

- Check Current System Time Zone
```sh
timedatectl
```
- Check All TimeZone
```sh
timedatectl list-timezones
```
- Filter TimeZone by City
```sh
Syntax:- timedatectl list-timezones | grep City_Name
Example:- timedatectl list-timezones | grep Kolkata
```
- Set TimeZone
```sh
Syntax:- timedatectl set-timezone [timezone]
Example:- timedatectl set-timezone Asia/Kolkata
```

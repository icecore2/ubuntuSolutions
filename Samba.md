# Samba installation Ubuntu 18.04 to Windows 10

# Menu
- [Menu](#menu)
    - [OS Details:](#os-details)
    - [Procedure:](#procedure)

## OS Details:
---
> * Ubuntu 18.04 LTS
> * Ubuntu Mate(16.04 LTS)

## Procedure:
---

1. update and upgrade if possible:
```shell
$ sudo apt-get update & apt-get upgrade
```
2. Install samba service:
```shell
$ sudo apt-get install samba
```
1. Setup your samba password:
```shell
$ sudo smbpasswd -a YOUR-PASSWORD
```
4. Create a sharing folder you want to
```shell
$ mkdir ~/Desktop/Share
```
5. Edit smb.conf file:
```shell  
$ sudo nano /etc/samba/smb.conf
```
6. At the end of the file add this lines:
```ini
[<folder_name>] path = /home/<user_name>/<folder_name> available = yes valid users = <user_name> read only = no browsable = yes public = yes writable = yes
```
7. Restart samba service:
```shell
$ sudo service smbd restart
```
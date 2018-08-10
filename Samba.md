# Samba installation Ubuntu 18.04 to Windows 10

## OS Details:
---
> * Ubuntu 18.04 LTS
> * Ubuntu Mate(16.04 LTS)

## Procedure:
---

1. update and upgrade if possible:
```console
$ sudo apt-get update & apt-get upgrade
```
2. Install samba service:
```console
$ sudo apt-get install samba
```
1. Setup your samba password:
```console
$ sudo smbpasswd -a YOUR-PASSWORD
```
4. Create a sharing folder you want to
```console
$ mkdir ~/Desktop/Share
```
5. Edit smb.conf file:
```console  
$ sudo nano /etc/samba/smb.conf
```
6. At the end of the file add this lines:
```ini
[<folder_name>] path = /home/<user_name>/<folder_name> available = yes valid users = <user_name> read only = no browsable = yes public = yes writable = yes
```
7. Restart samba service:
```console
$ sudo service smbd restart
```
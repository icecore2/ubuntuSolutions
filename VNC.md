# VNC Installation

## OS Details:
---
> * Ubuntu 18.04 LTS
> * Ubuntu Mate (16.04 LTS)

## Prerequisites:
---
1. A local computer with a VNC client installed
2. Ubuntu installed with SSH connection

## Procedure:
---

### Step 1 - Installing Desktop and VNC Server:
1. 
```shell
$ sudo apt-get update
$ sudo apt install xfce4 xfce4-goodies tightvncserver
```
2.
```shell
$ vncserver
```

### Step 2 - Configuring VNC Server:

1. 
```shell
$ vncserver -kill :1
```
2. 
```shell
$ mv ~/.vnc/xstartup ~/.vnc/xstartup.bak
```
3. 
```shell
$ nano ~/.vnc/xstartup
```
Paste these commands into the file so that they are performed automatically whenever you start or restart the VNC server, then save and close the file:
|Command|Description|
|-------|-----------|
|xrdb $HOME/.Xresources|VNC's GUI framework to read the server user's|
|.Xresources|file name - is where a user can make changes to certain settings of the graphical desktop|
|startxfce4 &|Tells the server to launch Xfce, which is where you will find all of the graphical software that you need to comfortably manage your server.|

```ini
~/.vnc/xstartup
#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &
```
4. To ensure that the VNC server will be able to use this new startup file properly, we'll need to grant executable privileges to it:
```shell
$ sudo chmod +x ~/.vnc/xstartup
```
5. Restart of the VNC server:
```shell
$ vncserver
```
6. The server should be started with an output similar to this:
```shell
New 'X' desktop is your_server_name.com:1

Starting applications specified in /home/sammy/.vnc/xstartup
Log file is /home/sammy/.vnc/liniverse.com:1.log
```

### Step 3 - Testing VNC:

**Remember to replace:**
|Variable|Description|
|-------|-----------|
|user |sudo non-root username |
|server_ip_address |IP address of your server|

```shell
$ ssh -L 5901:127.0.0.1:5901 -N -f -l username server_ip_address
```

VNC client to attempt a connection to the VNC server at:
>localhost:5901

**Once you are connected, you should see the default Xfce desktop.**

### Step 3 - Creating a VNC Service File:
1. create a new unit file: 
> vncserver@.service
```
$ sudo nano /etc/systemd/system/vncserver@.service
```
**Copy and paste the following into it:**

```ini
[Unit]
Description=Start TightVNC server at startup
After=syslog.target network.target

[Service]
Type=forking
User=sammy
PAMName=login
PIDFile=/home/sammy/.vnc/%H:%i.pid
ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/usr/bin/vncserver -depth 24 -geometry 1280x800 :%i
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```
Save and close the file.

2. Reload system deamon:
```shell 
$ sudo systemctl daemon-reload
```
3. Enable the unit file:
```shell
$ sudo systemctl enable vncserver@1.service
```
4. Stop the current instance of the VNC server if it's still running:
```shell
$ vncserver -kill :1
```
5.Then start it as you would start any other systemd service:
```shell
$ sudo systemctl start vncserver@1
```
6.You can verify that it started with this command:
```shell
sudo systemctl status vncserver@1
```

[Tutorial link @ digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-16-04)
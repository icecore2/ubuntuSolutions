# Gogs Installation



## OS Details:
---
> * Ubuntu 18.04 LTS
> * Ubuntu Server 14.04 LTS (HVM)
> * Ubuntu Mate 16.04 LTS

## Prerequisites:
---
1. Dependencies:

```bash
$ sudo apt-get update
$ sudo apt-get install -y wget unzip git
```

## Procedure:
---

### Step 1 - creating user and home directory:
1. create a new user called `git`:

```bash
$ sudo useradd --system --create-home git
```
This will create a system user for Gogs and a home directory in `/home/git`.

1. Switch the shell over to the `git` user:
   
```bash
$ sudo su git
$ cd ~
```

### Step 2 - Download Gogs:

1. Download later version of Gogs as `zip` archive:
Find latest version [**HERE.**](https://gogs.io/docs/installation/install_from_binary)

2. Use this commands for downloading latest version:

```bash
$ wget PUT_YOUR_LINK_HERE
$ unzip gogs*.zip
$ rm -rf gogs*.zip
```
**NOTE:** Change link after `wget` command to latest version link.

### Step 3 - Run Gogs:

1. Go to `Gogs` folder

```bash
$ cd gogs
$ ./gogs web --port=1337
```
2. Go to web browser and surf to `http://0.0.0.0:1337` address,\
   You'll reach to `Installation page`.

### Step 4 - Configure Gogs:

1. Database type: 
   >`SQLite3`

**NOTES:** 

- Update the `Domain` and `Application URL` in the `Application General Settings`.
- Edit `Optional Settings`.
- Set up you Account under `Admin Account Settings`.

2. Press `Install Gogs`.
3. That's all!

# Additionals

### Set up auto-run after boot:

**NOTE:** Find `Deamon script` file, usually lays at:

> \home\git\gogs\gogs\scripts\init\[YOUR OS]\/**gogs**

 The file called `gogs`, contains a bash script for run `Gogs` right after boot.

1. Log out of the `git` user shell.
2. Copy `gogs` script to `init.d` folder:

```bash
$ sudo cp /home/git/gogs/gogs/scripts/init/[YOUR OS]/gogs /etc/init.d
$ cd /etc/init.d
$ sudo chmod +x gogs
```

3. Using sed we can automatically find and replace within the gogs daemon file without opening it, using the following command:

```shell
$ sudo sed -i -e 's/DAEMON_ARGS="web"/DAEMON_ARGS="web --port 1337"/g' gogs
```
4. Start service by command:

```bash
$ sudo service gogs start
```

5. configure Ubuntu to start Gogs automatically after booting:

```
$ sudo update-rc.d gogs defaults
$ sudo update-rc.d gogs enable
```

6. Reboot your system:

```
$ sudo reboot
```

[Tutorial link @ eladnava.com](https://eladnava.com/host-your-own-private-github-with-gogs-io/)

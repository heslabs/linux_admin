# Linux Desktop Setup Guide

### Ubuntu initial setup
```
$ sudo -i passwd root
$ sudo apt-get update
$ sudo apt install -y net-tools
$ sudo apt install -y vim konsole wget p7zip-full curl lsb tree subversion xpdf
```

---
### Configure SSH
```
$ sudo apt install -y openssh-server
$ sudo vim /etc/ssh/sshd_config 
AllowUsers <user>
TCPKeepAlive yes
```
```
$ sudo systemctl restart ssh
$ rm -f /home/<user>/.ssh/known_hosts
$ sudo shutdown -r / -h now
```

---
### Add new user
```
$ sudo useradd -s /bin/bash -d /home/<user> -m <user>
$ sudo usermod -aG sudo <user>
$ sudo passwd <user>
```
```
$ sudo vim /etc/ssh/sshd_config
## Add the line at the end
AllowUsers <user>
```
```
$ sudo systemctl restart ssh
```

### Generate random password
```
$ openssl rand -base64 6
$ sudo userdel -r <user>
```
---
### Configure network and wifi
```
$ sudo vim /etc/hosts 
$ sudo vim /etc/hostname
$ sudo nmcli dev wifi connect <SSID> password <PASSWD>
$ sudo nmcli dev wifi connect SOCLABS password Call6576886
$ sudo nmcli dev wifi connect LABS password arm12345
$ sudo apt-get -y install --reinstall systemd (Optional)
$ sudo apt install nfs-common nfs-kernel-server
$ sudo systemctl restart nfs-kernel-server
$ sudo apt-get update
$ sudo apt-get install portmap
```
---
### Add new FTP user
```
$ sudo groupadd sftpusers
$ sudo useradd -s /bin/bash -d /home/<user> -m -G sftpusers <user>
$ sudo mkdir -p /home/<user>/uploads
$ sudo chown -R root:sftpusers /home/<user>
$ sudo chown -R <user>:sftpusers /home/<user>/uploads
$ sudo chmod -R g-rwx /home/<user>
$ sudo chmod -R g+rx /home/<user>
$ sudo chmod -R o-rwx /home/<user>
```

---
### nfs server
```
$ sudo apt install nfs-kernel-server
$ mkdir /home/nfs
$ sudo vim /etc/exports
/home/nfs      *(rw,sync,no_subtree_check)
$ sudo chmod 777 /home/nfs
$ sudo exportfs -rav
exporting *:/home/nfs
$ sudo systemctl restart nfs-kernel-server
```

### nfs client
```
$ sudo apt install nfs-common
$ mkdir /home/nfs
$ sudo mount 192.168.50.9:/home/nfs /home/demo/nfs
$ sudo showmount -e 192.168.50.9
$ systemctl daemon-reload
```

---
### Install samba
```
$ sudo apt-get -y install samba
$ sudo vim /etc/samba/smb.conf
$ sudo smbpasswd -a username
$ sudo systemctl restart smbd
$ sudo /etc/init.d/network-manager restart
```

```
## /etc/samba/smb.conf
[demo]
comment = needs username and password to access
path = /home/demo
browseable = yes
guest ok = no
#valid users = @samba
valid users = demo
force users = demo
force group = demo
public = yes
available = yes
writable = yes
```

---
### Ubuntu Utilities
* Kazam
```
$ sudo apt install kazam
```
* Filezilla
```
$ sudo apt-get install filezilla
```
* Sensors
```
$ sudo apt install -y gnome-screenshot
$ sendors
```
* Screenshot
```
$ sudo apt install -y gnome-screenshot
```
* Ubuntu（22.04 LTS）如何开启中文输入法
```
https://blog.csdn.net/windson_f/article/details/124932523 
```



# Changes IP address
```bash
# checks ip address
sudo ip addr

# refresh DHCP client(kills and restart)
sudo dhclient -r
sudo dhclient
```

# Installs SSH server
```bash
# install ssh-server
sudo apt-get install openssh-server

# enable connectivity remotely
sudo vim /etc/ssh/sshd_config
...
PasswordAuthentication yes
...

# restarts ssh-server
sudo service ssh restart
```

# Adds disk(replace home directory)
```bash
# backup home directory
sudo mv /home /home_bak

# list block
sudo lsblk

# creates partition
fdisk /dev/sda

# format partition
sudo mkfs.ext4 /dev/sda

# checks block id
sudo blkid

# edits tstab
sudo vim /etc/fstab
...
UUID=xxx-xxx-xxx    /home   ext4    nodev,nosuid    0   2
...

# creates mount point
sudo mkdir /home

# updates fstab
sudo mount -a

# restores home directory
sudo mv /home_bak/* /home

```



# How to autostart service

## update-rc.d
```bash
sudo update-rc.d apache2 defaults
```


# Adds disk
```bash
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

# updates fstab
sudo mount -a

```

# Enable SSH


# How to autostart service

## update-rc.d
```bash
sudo update-rc.d apache2 defaults
```
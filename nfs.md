# NFS Server

## Install nfs
```shell
sudo apt install nfs-common nfs-server
```

## Start service
```shell
sudo systemctl start nfs-server
sudo systemctl enable nfs-server
```

## Config export file
```shell
sudo vim /etc/exports
...
/home/test *(rw)
...

# adjust
exportfs -r

# confirm
showmount -e
exportfs -v
```




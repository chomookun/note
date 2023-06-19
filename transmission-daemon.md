# Transmission Daemon (Torrent)

```shell
sudo apt install transmission-daemon
sudo vim /etc/transmission-daemon/settings.json
```

# sysconf

```shell
sudo vim /etc/sysctl.conf
...
net.core.rmem_max = 16777216
net.core.wmem_max = 4194304
...
sudo sysctl -p
```

# change to root

```shell
sudo vim /etc/init.d/transmission-daemon
...
USER=root
...
sudo service transmission-daemon restart
```

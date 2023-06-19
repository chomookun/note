# Transmission Daemon (Torrent)

sudo apt install transmission-daemon

sudo vim /etc/transmission-daemon/settings.json


# sysconf
sudo vim /etc/sysctl.conf
...
net.core.rmem_max = 16777216
net.core.wmem_max = 4194304
...
sudo sysctl -p

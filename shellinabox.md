# Installation of shellinabox
```bash
# install
sudo apt install shellinabox

```

# Configuraiton
```bash
# sets configuration
sudo vim /etc/default/shellinabox
...
# sets port
SHELLINABOX_PORT=4200

# sets color theme
SHELLINABOX_ARGS="--no-beep --css='/etc/shellinabox/options-available/00_White On Black.css'"
...
```

# Starts and Stop
```bash
# start
sudo service shellinabox start
# stop
sudo service shellinabox stop
```
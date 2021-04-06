# WSL Installation

```powershell
# window subsystem
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# virtual machine
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

```

# Updates WSL2

<https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi>

```powershell
# sets defualt version
wsl --set-default-version 2

# list
wsl -l -v
-- result --
  NAME            STATE           VERSION
* Ubuntu-20.04    Running         1

# changes WSL version
wsl --set-version Unbuntu-20.04 2

```

# Port forwarding
```powershell
# checks WSL network
ifconfig /all

# checks WSL ip
wsl
ip addr | grep etho
-- result --
4: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000 inet 172.24.117.69/20 brd 172.24.127.255 scope global eth0

# set portproxy
netsh interface portproxy add v4tov4 listenport=80 listenaddress=0.0.0.0 connectport=80 connectaddress=172.24.117.69
netsh interface portproxy add v4tov4 listenport=443 listenaddress=0.0.0.0 connectport=443 connectaddress=172.24.117.69

# show portproxy
netsh interface portproxy show all

# delete portproxy
netsh interface portproxy delete v4tov4 listenport=8080 listenaddress=0.0.0.0
```
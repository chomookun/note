# WSL Installation

## BIOS configuration
Advanced Frequency Settings > Advanced CPU Core Settings > SVM Mode > enable


## Runs powershell as administrator
```powershell
# window subsystem
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# virtual machine
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# restart
Restart-Computer
```

# Updates WSL2

<https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi>

```powershell
# install 
.\wsl_update_x64.msi

# sets defualt version
wsl --set-default-version 2

# [ONLINE Market] install ubuntu from Microsoft Store
https://aka.ms/wslstore

# [OFFLINE Manual]
# 1. download Ubuntu distro (PC connected to Internet)
Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1604 -OutFile Ubuntu.appx -UseBasicParsing

# 2. Installs Ubuntu distro
Add-AppxPackage .\Ubuntu.appx

# list
wsl -l -v
-- result --
  NAME            STATE           VERSION
* Ubuntu-20.04    Running         1

# changes WSL version
wsl --set-version Unbuntu-20.04 2

```

# Port forwarding(manual)
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

# Port forwarding(script)

## creates script
```powershell
# wsl_port_forward.ps1
$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  echo "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,8080,4200);


#[Static ip]
#You can change the addr to your ip config to listen to a specific address
$addr='0.0.0.0';
$ports_a = $ports -join ",";


#Remove Firewall Exception Rules
iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

#adding Exception Rules for inbound and outbound Rules
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
```
## Executes script
```powershell
powershell -ExecutionPolicy Bypass -Command .\wsl_port_forward.ps1
```


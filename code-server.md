# Code-Server(Web based VSCode)

## Installation
```bash
wget https://github.com/cdr/code-server/releases/download/v3.9.2/code-server-3.9.2-linux-amd64.tar.gz
tar -xvf ./code-server-*
ln -s ./code-server-* ./code-server
```
## Configuration
```bash
vim ~/.config/code-server/config.yaml
...
bind-addr: 0.0.0.0:8080
auth: password
password:${password} 
cert: true
...
```


## Start and shutdown script
```bash
vim ./code-server.sh
```
```bash
#!/bin/bash
APP_FILE=$(basename ${0})
APP_NAME=${APP_FILE%.*}
PID_FILE=${APP_NAME}.pid
touch ${PID_FILE}
PID=$(cat ${PID_FILE})
LOG_FILE=${APP_NAME}.log

# start
function start() {
	if [ -f /proc/${PID}/status ]; then
		echo "${APP_NAME} is alreay running."
		status
		exit -1
	fi
    ./code-server >> ${LOG_FILE} 2>&1 &
	echo $! > ${PID_FILE}
}

# status 
function status() {
    if [ -f /proc/${PID}/status ]; then
	    ps -f ${PID}
    fi
}

# stop
function stop() {
	if [ ! -f /proc/${PID}/status ]; then
		echo "${APP_NAME} is not running."
		exit -1
	fi
	kill ${PID}
}

# log
function log() {
    tail -F ${LOG_FILE}
}

# main
case ${1} in 
	start)
		start
		;;
	status)
		status
		;;
	stop)
		stop
		;;
	restart)
		stop
        start
		;;
    log)
        log
        ;;
	*)
		echo "Usage: ${0} [start|status|stop|restart|log]"
		;;
esac
```

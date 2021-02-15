# Kafka

## Installation

### Installs Jookeeper
```
# getting zookeeper distribution file
user@host> wget http://apache.tt.co.kr/zookeeper/zookeeper-3.6.1/apache-zookeeper-3.6.1-bin.tar.gz
user@host> tar -zxvf ./apache-zookeeper-3.6.1-bin.tar.gz
user@host> ln -s apache-zookeeper-3.6.1-bin apache-zookeeper

# edits configuration file
user@host> cp zoo_sample.cfg zoo.cfg
user@host> vim zoo.cfg
...
# custer mode(optional)
server.1=0.0.0.0:2888:3888
server.2={Node IP}:2888:3888
server.3={Node IP}:2888:3888
...

# start zookeeper
user@host> ./bin/zkServer.sh start

# stop zookeeper
user@host> ./bin/zkServer.sh stop

# tailing logs
user@host> ./logs/zookeeper*.*

```

### Installs Kafka
```
# getting kafka distribution file
user@host> wget http://apache.tt.co.kr/kafka/2.5.0/kafka_2.12-2.5.0.tgz
user@host> tar -cvf ./kafka_2.12-2.5.0.tgz
user@host> ln -s ./kafka_2.12-2.5.0 ./kafka

# creates data directory
user@host> mkdir data

# setting zookeeper
user@host> telnet 127.0.0.1 2181
user@host> vim ./config/server.properties
...
// TODO
...

# starts kafka
user@host> ./bin/kafka-server-start.sh -daemon ./config/server.properties

# stop kafka
user@host> ./bin/kafka-server-stop.sh

```

## Kafka CLI
```
# list of topic
user@host> ./bin/kafka-topics.sh --bootstrap-server localhost:9092 --list

# list consumer group
user@host> ./bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list

# creates topic
user@host> ./bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test2

# describes topic
user@hst> ./bin/kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic test2

# send message
user@host> ./bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test2

# receives message./bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test2 --from-beginning
user@host> 

```

## Throwable shooting

### window10 subsystem(WSL)에 설치시 연결 않되는 문제
> hosts파일(C:\Windows\System32\drivers\etc\hosts)에 subsystem 내용 추가

## Reference

### Consumer Group
https://www.popit.kr/kafka-consumer-group/

### KAFKA Tool (GUI Client)
https://www.kafkatool.com/

# Docker installation
```bash
# removes older version docker and install new version
user@host> sudo apt-get remove remove docker docker-engine docker.io containerd runc
user@host> sudo apt update
user@host> sudo apt install apt-transport-https ca-certificates curl software-properties-common
user@host> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
user@host> sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
user@host> sudo apt update
user@host> apt-cache policy docker-ce
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

# list docker version and process
user@host> sudo service docker start
user@host> sudo docker version
user@host> sudo docker ps
user@host> sudo docker login
```

# Builds docker image(Dockerfile)
```bash
# base image
FROM ubuntu:18.04

# set envrionment
ENV APP_HOME /my_app

# expose
EXPOSE 8080

# copy resources
RUN mkdir {APP_HOME}
COPY *.jar ${APP_HOME}/
COPY *.sh ${APP_HOME}/
COPY lib ${APP_HOME}/lib

# working directory
WORKDIR ${APP_HOME}/
RUN chmod 755 *.sh

# set additional environment
ENV TZ=Asia/Seoul
ENV SPRING_PROFILES_ACTIVE

```

# docker CLI example
```bash
# defines
STAGE=dev
image=837398788587.dkr.ecr.ap-northeast-2.amazonaws.com/sk/core-boot-batch
image=837398788587.dkr.ecr.ap-northeast-2.amazonaws.com/sk/core-boot-api

# remove container and image
sudo docker rm $(sudo docker ps -a -q)
sudo docker rmi $(sudo docker images -q)

# docker login
aws ecr get-login-password --region ap-northeast-2 | sudo docker login --username AWS --password-stdin 837398788587.dkr.ecr.ap-northeast-2.amazonaws.com

# docker build
sudo docker build -t ${image}:${STAGE} .

# docker push
sudo docker push ${image}:${STAGE}

# docker run (batch)
sudo docker run ${image}:${STAGE} com.kakaoinsure.core.exm.batch.sandbox.job.DbToDbWithTaskletJob 20110101 type=Mybatis limit=123

# docker restart (api)
sudo docker stop $(sudo docker container ps -q)
sudo docker run -d -e SPRING_PROFILES_ACTIVE=${STAGE} -p 8080:8080 -v /home/ec2-user/logs:/home/ec2-user/logs ${image}:${STAGE}

# docker inspect
sudo docker run -it --entrypoint /bin/bash ${image}:${STAGE}
```

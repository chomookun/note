# Docker installation
```bash
# 
sudo apt-get install docker-ce docker-ce-cli containerd.io
```


# Docker Build(Dockerfile)
```
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

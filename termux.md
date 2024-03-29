# Termux in android

## Storage setup
```shell
$ termux-setup-storage
```

## Installs packages
```shell
$ pkg install git vim
```

## Instals openssh
```shell
$ pkg install openssh
$ sshd
```

## Installs termux-style
```shell
$ git clone https://github.com/adi1090x/termux-style.git
$ cd ./termux-style && ./install
// run termux-style
$ termux-style
```

# termux settingse
```bash
vim ~.termux/termux.properties
...bash
# theme
use-black-ui=true

# full screen mode
fullscreen=true

# sets language input
enforce-char-based-input=true
...
```

## Installs JDK
```shell
$ git clone https://github.com/MrAdityaAlok/java-in-termux.git
$ cd ./java-in-termux && ./install.sh

#or
$ wget https://github.com/suhan-paradkar/java-in-termux/releases/download/OpenJDK9/openjdk-9-jre-headless_9.2017.8.20-1_arm.deb


$ wget https://github.com/SHivnaTH13/Termux-OpenJDK/releases/download/v8-232/openjdk-8-jdk_8u232.deb

```
and restart termux.

## Installs gradle
```shell
$ wget https://services.gradle.org/distributions/gradle-6.7.1-bin.zip
$ unzip gradle-*.zip
$ ln -s ./gradle-6.7.1 ./gradle
$ vim .profile
...
# gradle
export GRADLE_HOME=/data/data/com.termux/files/home/gradle
export PATH=${PATH}:${GRADLE_HOME}/bin
..
$ source .profile.
$ gradle --version
```

## Installs maven
```
$ wget https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
$ tar -zxvf ./apache-maven-*.tar.gz
$ ln -s ./apache-maven-3.6.3 ./apache-maven
$ vim .profile
...
export MAVEN_HOME=/data/data/com.termux/files/home/apache-maven
export PATH=${PATH}:{MAVEN_HOME}/bin
...
$ source .profile
$ mvn --version
```

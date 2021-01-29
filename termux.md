# Termux in android


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

## Installs JDK
```shell
$ git clone https://github.com/MrAdityaAlok/java-in-termux.git
$ cd ./java-in-termux && ./install.sh
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

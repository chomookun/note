# Installs gradle

```bash
# gets gralde binary 
wget https://services.gradle.org/distributions/gradle-6.8.3-bin.zip

# extract
unzip ./gradle*.zip

# creates link
ln -s ./gradle-6.8.3 ./gradle
```


# Configure profile
```bash
vim ~/.profile
...
# gradle
export GRADLE_HOME=/home/chomookun/gradle
export GRADLE_BIN=${GRADLE_HOME}/bin
export PATH=${PATH}:${GRADLE_BIN}
...
source ~/.profile

# checks version
gradle --version

```
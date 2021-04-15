# Git installation

```bash
# Ubuntu
$ sudo apt-get install git

# CentOS
$ sudo yum install git

# Windows
https://git-scm.com/download/win

```

# Initial configuration
```bash
# sets user info
git config --global user.name "chomookun"
git config --global user.email "chomookun@gmail.com"

# sets default editor
git config --global core.editor vi

# list config
git config --list

```


# Store credential
```shell
$ git config --global credential.helper store
```

# Reset file
```shell
$ git checkout [Source]
```




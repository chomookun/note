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

# sets credentails option
git config --global credential.helper store

# list config
git config --global --list

# unset config
git config --global --unset [item]

```

# Creates new repositoryy
```bash
git init
git remote add [URL]
git branch -M main
git push -u origin main
```

# Branch
```bash
# list branches
git branch

# creates branch
git branch ${branchName}

# switch branch
git checkoutn ${branchName}

# merge branch
git merge ${branchName}

# remote branch
git branch -d ${branchName}
```

# Merge branch
```bash
# checkout branch
git checkout main

# merge branch
git merge ${branchName}
```

# Merge stratege
```bash
# sets config
git config --global merge.ours.driver true

# creates .gitattributes
vim .gitattributes
...
conf/* merge=ours
...
```


# View history
```bash
# prints log with graph
git log --graph
```

# Rebase
```bash
# rebase last 5
git rebase -i @~5
```

# Reset file
```bash
git checkout [Source]
```




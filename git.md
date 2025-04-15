# GitHub workflow

## Main commands
| Working Directory | Staging Area | Local repo | Remote Repo | 
| -------- | ------- | ------- | ------- |
| 1 | 2 | 3 | 4 |

## Install on local machine
Download git client from  [git-scm.com](https://git-scm.com/downloads).

List of configuratiom commands:

```bash
# Configuration of local git
git --version
git config --global user.name "user_name"
git config --global user.email "user_email"
git config --global init.defaultBranch main
```

## Push local files
```bash
# cd to root dir of project

# Init .git dir
git init

# Informational commands
ls -a # list files, including hidden
Get-ChildItem -Force # for power-shell
git status # status of project files
git log # history of commits to local repo ('q' for quit)
git diff . # show difference between working dir anf staging area

# Add files to staging area
git add . # all modifed files

# Add files to local repo
git commit -m 'Some info about init commit or changes'

# Add and commit in single command
git commit -am 'Some info about init commit or changes'
# or 
git add . && git commit -m 'Some info about init commit or changes'

# Add address to remote repo
git remote add origin git@github.com:username/projectname.git
git branch -M main

# Push to remote repo
git push -u origin main

```

## Pull remote files
```bash
# Get files from remote repo to work dir
git pull

# Copy files from remore repo to local repo
git clone "link to remote repo" .

# Copy files from remore repo to local repo without merging
git fetch "link to remote repo" .

# Fork own repo from other repo through github
```

## Git ignore file
.gitignore file specifies list of files, that wil be excluded from version control.

## Generate SSH keys with github
Generate SSH public and private keys:
```bash
# Generate key
ssh-keygen -t rsa -b 4096

# View public key
cat ~/.ssh/id_github | clip
```
Put generated public key to github [profile](https://github.com/settings/keys). 

Check SSH connection to github:
```bash
# Check if ssh agent is running
# Change startup type of OpenSSH auth service to manual
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
ssh-add C:\Users\User/.ssh/id_github

# Check connection
ssh -T git@gitub.com
```
To assign private key to git add lines to `/.ssh/config>`:
```bash
Host github.com
 HostName github.com
 IdentityFile ~/.ssh/id_github
```

## Branching
```bash
# Add new branch
git checkout -b feature/feature_name

# View branches
git branch

# Push to branch
git push -u origin feature/feature_name

# Switch to main
git checkout main

# Merge branch with curent
git merge feature/feature_name

# Delete branch
git branch -d feature/feature_name

```
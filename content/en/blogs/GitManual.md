---
title: "Git commands Manual (Continually updated)"
date: 2024-02-26T00:00:00+08:00
draft: false
author: "GeekerHWH"
category:
    - Technology
en_tags:
    - Git
image: /images/blogs/git.png
description: ""
toc: true
---

## user configuration
```bash
git config --global user.name "GeekerHWH"
git config --global user.email "xxxxxxxxxx@gmail.com"
```
you can check the settings with:
```bash
git config --list
```
to set users for each repository seperately
```bash
git config --local user.name "GeekerHWH"
git config --local user.email "xxxxxxxxxx@gmail.com"
```
### set alias
for example:
```bash
git config --global alias.co checkout
git config --global alias.st status
git config --global alias.l "log --oneline --graph"
```

## .gitignore
```bash
# .gitignore

# ignore secret.yml
secret.yml

#ignore db.yml in config
config/db.yml

# ignore the suffix
/db/*.sqlite3
```

## repository operations
### initialize repository
```bash
git init -b main
```

### add all and commit
```bash
git commit -a -m "xxx"
```

### remove tracking without deletion
```bash
git rm file_name --cached
```

### amend the message of last commit
```bash
git commit --amend -m "new meassage"
```

### amend the file of last commit
```bash
git add file
git commit --amend --no-edit # no edit means no change to the message
```

### Rescue accidentally deleted files
```bash
git checkout file
```

## check commit history
### user-friendly output
```bash
git log --graph
```
### to find someone's commit
```bash
git log --author="A"
git log --author="A\|B" # A or B
```
### to find key word
```bash
git log --grep="LOL"
```
### to find commits during a period of time
```bash
git log --since="9am" --until="12am"
```
### check who wrote this line of code
```bash
git blame file
```

## branchs operations
### Display the branch in the terminal
append the following content to the .bashrc file in your home directory
```bash
function git_branch {
   branch="`git branch 2>/dev/null | grep "^\*" | sed -e "s/^\*\ //"`"
   if [ "${branch}" != "" ];then
       if [ "${branch}" = "(no branch)" ];then
           branch="(`git rev-parse --short HEAD`...)"
       fi
       echo " ($branch)"
   fi
}

export PS1='\u@\h \[\033[01;36m\]\W\[\033[01;32m\]$(git_branch)\[\033[00m\] \$ ' 
```
then every time you enter a directory with a git repo initialized the terminal will be look like this fancy stuff: 
![branchsInTerminal.png](/imagesInBlogs/GitManual/branchsInTerminal.png)

### create new branch
```bash
git branch new_branch
```
### create new branch and point it to any commit
```bash
git branch new_branch [SHA-1]
```
### rename branch
```bash
git branch -m branch_name new_name
```
### force delete branch
```bash
git branch -D branch_name
```
### switch to another branch
```bash
git checkout another_branch
```
### merge branchs
for example, you want to merge test into main, make sure you are at main then:
```bash
git merge test
```
### rebase branch
```bash
git rebase test
```


## Git & Github
### add remote repository endpoint
```bash
git add remote origin xxx@github.com:xxx/xxxxxxxxxx.git # "origin" is the name of this endpoint
```
### push
```bash
git push -u origin main # -u set the origin as the main's upstream, you can use git push to do the same thing next time
```
### change remote endpoint
```bash
git remote set-url <name> <newurl> # git remote set-url origin xxxxx.git
```
### Get latest released version from Github
https://api.github.com/repos/${AuthorName}/${ProjectName}/releases/latest


## Theoriodical questions in Interview(Continually updated)

## Uncommon Usage
### To solve long time clone
Given that **we are in the main branch without any other branch**, we've commited and pushed literally
hundreds and thousands times. In this situation, we may take pretty much long time to
clone the repo from remote(Github). Here are some solutions:

#### clean commit history
1. create a brand new branch with `--orphan` parameter, which only keeps the final commit node
    ```bash
    git checkout --orphan new_branch
    ```
2. add and commit
    ```bash
    git add .
    git commit -m "very new version"
    ```
3. delete the original main branch and rename the present branch
    ```bash
    git branch -D main
    git branch -m main # change new_branch's name to "main"
    ```
    now your local repo has delete all commit history
4. force push to clean remote commit history
    ```bash
    git push -f origin main
    ```
    > P.S. some repositories have branch protection, you need to turn it off first.
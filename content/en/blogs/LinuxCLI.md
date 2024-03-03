---
title: "Linux CLI Tools(continually update)"
date: 2024-02-28T00:00:00+08:00
draft: false
author: "GeekerHWH"
category:
    - Technology
en_tags:
    - Linux
image: /images/projects/bash.jpg
description: "from manual to tutorial"
toc: true
---

> This page only lists some commonly used commands and their options, for more details, please check `man` command or use the official docs!
> `apropos` is another command you should master to search in `man`
> I highly recommend you to read [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line/blob/master/README.md)

## Special Notes
- the `=` in shell scripts is to set value of variable, ` = ` is conditional character

## restart
```bash
sudo shutdown now -r
```

## find
```bash
find [path...] [expression]
```
Expression may consist of: operators, options, tests, and actions.

## stat
```bash
stat filename
```

## free
<details>
  <summary>Click to see options</summary>

  - -h --human-readable sizes in powers of 1024
  - -s N, --seconds N   repeat printing every N seconds
  - -c N, --count N     repeat printing N times, then exit
</details>

## df
<details>
  <summary>Click to see options</summary>
  
  - -h --human-readable sizes in powers of 1024 (e.g., 1023M)
</details>

## lsblk
list blocks information in tree format

## fdisk
It is an interactive tool, here are some commonly used commands:
- n - new a partition
- d - delete a partition
- l - list all available kinds of partition

## mkfs
click tab to see what file systems are supported

## mount
```bash
mount <device> <directory>
```

> to enable mount automatically, you need to edit `/etc/fstab` (file systems table)

## umount
```bash
umount <device>
```

## ln
[Chinese reference](https://runoob.com/note/29134)
```bash
ln f1 f2
ln -s f1 f2
ln -s /path/to/the/sourcefile /path/to/the/link
```

## readlink

## alias
```bash
alias alias_name='command [options]'
# example
alias ll='ls -alh' # pay attention to the charactors`'`
```

## unalias
```bash
unalias <alias_name>
```

## ftp
```bash
ftp -P <port> <ip_addr>
ftp> cd Downloads
ftp> put source_path_of_your_file
```

## wget
<details>
  <summary>Click to see options</summary>

  - -P specify the path of output
  - -c continue interrupted download
  - -b download in backgroud
  - -i download multiple files, you need to specify the config after it
  - -r specify recursive download
</details>

## curl
<details>
  <summary>Click to see options</summary>

  - -f respond status code to help scrips detect error
  - -s silent
  - -S show error, combined with `-s`, it only show messages when error happens
  - -L redirect if recieve redirection code from server
  - -O set the name of output file the same as the file in remote server
</details>

## netstat
```bash
netstat -tulpn
```

## ss
```bash
ss -tulpn
```

## ps
```bash
ps -aux
```

## top

## htop

## tar
<details>
  <summary>Click to see options</summary>

  - -x Extracts
  - -c Create an Archive
  - -r Updates or adds files or directories to an already existing archive without recreating the entire archive.
  - -v Display output
  - -f Specifies the name of archive
  - -C **Specifies the path of outputs**
</details>

```bash
tar xvf file.tar # Extracting files from Archive 
tar xvzf file.tar.gz # Extracting a gzip tar Archive
tar xvjf file.tar.bz2 # Extracting a bzip2 tar Archive

tar cvf file.tar *.cpp # Creating an Archive of all .cpp files
tar cvzf file.tar.gz *.cpp # Extracting a gzip tar Archive of all .cpp files
tar cvjf file.tar.bz2 *.cpp # Extracting a bzip2 tar Archive of all .cpp files
```

## Packages Manager
### apt
#### PPA(Personal Package Archive)
PPA (Personal Package Archive) is a term used in the context of Ubuntu and other Debian-based Linux distributions. It refers to a Personal Software Repository or Personal Package Archive created by individuals or groups to host and distribute software packages. PPAs allow developers and users to share, publish, and update software packages that may not be available in the official repositories of the distribution.

In the context of APT (Advanced Package Tool), you can use PPAs to add additional software sources for obtaining updated packages. Here are the general steps:

Add a PPA: Use the add-apt-repository command to add a PPA. You need to provide the address of the PPA after the command. For example:
```bash
sudo add-apt-repository ppa:example/ppa
```
Here, ppa:example/ppa is the actual address of the PPA. After running this command, the system will ask for confirmation to add the PPA, and you need to confirm.

#### GPG
```bash
apt-key list
```

### dnf

### pipx
[Reference](https://pipx.pypa.io/stable/#overview-what-is-pipx)

## nmtui
My personal understanding for the name is network manager terminal-ui
![nmtui](/imagesInBlogs/LinuxCLI/nmtui.png)

## crontab
usage:
crontab [-u user] [-n] file
crontab [ -u user ] [ -i ] { -e | -l | -r }

- -h	(displays this help message)
- -u user	(choose the user whose crontab is touched)
- -e	(edit user's crontab) will let you specify a text-editor(Vim)
- -l	(list user's crontab)
- -r	(delete user's crontab)
- file	(default operation is replace, per 1003.2)
- -n	(dry run: checks the syntax, then bails out)


## iptables
Firewall system will check the rules from top to bottom, and will end as soon as it match the one.
If there isn't matched rule, the firewall will execute following the default policy.

There are 5 kinds of data packages:
- PREROUTING
- INPUT
- OUTPUT
- FORWARD
- POSTROUTING

and there are 4 policies that can be implemented:
- ACCEPT
- REJECT (drop and respond)
- LOG
- DROP (drop and won't respond)

<details>
  <summary>Commonly used Options</summary>

  - -L list existing rules
  - -P set default policy
  - -F clear all existing rules
  - -A append a rule
  - -I &lt;package_kind&gt; &lt;num&gt; insert a rule to the rules chain
  - -D &lt;package_kind&gt; &lt;num&gt; delete a rule from the chain
  - -s source ip(s), IP/MASK
  - -sport src_port
  - -d destinetion
  - -dport dest_port
  - -p protocol (tcp/udp/icmp/all)
  - -j policy that will be adopted
  - -i specify the name of internet card, match the data that input to this card
  - -o specify the name of internet card, match the data that output to this card
</details>

examples:
```bash
iptables -I INPUT -s 192.168.10.0/24 -p tcp --dport 22 -j ACCEPT # accept all ssh request from 192.168.10.0/24
```
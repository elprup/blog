+++
title = "Bootstrap ubuntu cloud instance guide"
date= "2015-10-22T15:48:00+08:00"
#Modified: 2015-10-22 15:48
categories = ["Cloud Service"]
tags = ["cloud service", "backend"]
#Slug: bootstrap-instance
#Author: elprup
#Summary: bootstrap for linux server on google compute engine
+++

## ssh

### gen ssh key pair on server

```
awayyao@instance-1:~$ ssh-keygen -t rsa
awayyao@instance-1:~$ cd .ssh/
awayyao@instance-1:~/.ssh$ cat id_rsa.pub >> authorized_keys 
awayyao@instance-1:~/.ssh$ cat id_rsa
(content...)
```
now you can use that private key to login remote instance

### client

ssh -i [file name] awayyao@[ip address]

## apt
### update apt repo
```
sudo apt-get update
```

## nginx
### install
```
awayyao@instance-1:~$ sudo apt-get install nginx
```
Now you can visit http://[ip] to view a nginx welcome page.

## Next to go
Now if you want to build a blog site, please click here.

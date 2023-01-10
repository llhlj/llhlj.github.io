---
layout: post
title: linux privilege, sudo, chmod
date: 2023-01-10 16:37
category: 
author: 
tags: []
summary: 
---

#### sudo
ref: https://segmentfault.com/a/1190000007394449  

##### sudoers files:
```
/etc/sudoers/etc/sudoers.d/
```
note:  
	1. do not edit sudoers file directly, use `visudo`  
	2. do not edit `/etc/sudoers`, add file to `/etc/sudoers.d/`, and edit with `visudo`  

##### edit /etc/sudoers.d/any_file_name_you_like
visudo /etc/sudoers.d/any_file_name_you_like
```
vavava ALL=(ALL)ALL                                       
acme ALL=(nobody)NOPASSWD:/usr/bin/cp,NOPASSWD:/usr/bin/ls,/usr/bin/df
```
note:  
	1. vavava as root  
	2. acme can sudo to `nobody` and use cp and ls with no password, use sudo df with password  
	3. this change enabled immediately  

eg: ref https://xyz.cinc.biz/2019/11/linux-sudoers-example.html
```
Cmnd_Alias CMD_COMM = /bin/cd, /bin/ls, /bin/cat, /bin/less, /bin/tail, /bin/head, /bin/last, /bin/lastb
Cmnd_Alias CMD_SERVICE = /bin/systemctl
Cmnd_Alias CMD_NGINX = /bin/vi /etc/nginx/*, /bin/cp -i /etc/nginx/*, /bin/rm -i /etc/nginx/*, /bin/mv -i /etc/nginx/*, /bin/scp /etc/nginx/*
Cmnd_Alias CMD_PHP = /bin/vi /etc/php*, /bin/cp -i /etc/php*, /bin/rm -i /etc/php*, /bin/mv -i /etc/php*, /bin/scp /etc/php*
Cmnd_Alias CMD_HOME = /bin/vi /home/*, /bin/cp -i /home/*, /bin/rm -i /home/*, /bin/mv -i /home/*, /bin/scp /home/*, /bin/chown nginx\:nginx -R /home/*, /bin/chmod [0-9][0-9][0-9] /home/*
 
xyz ALL=(root) CMD_COMM, CMD_SERVICE, CMD_NGINX, CMD_PHP, CMD_HOME
```
note:  
```
如果指令包含「,」、「:」、「=」、「\」，這些特殊字元，須用「\」脫逸。
例如「/bin/chown nginx\:nginx -R /home/*」
除了Cmnd_Alias，其他可用的別名還有User_Alias、Runas_Alias、Host_Alias
```

eg:
```
sudo -u nobody cp /path/to/somewhere   /another/path/to/somewhere
```
note that, nobody must have permission with both path.


#### user, gourp
##### check cmd
```
id
groups
```
##### change cmd
```
# add user to group
chmod -a -G <group_name> <user_name>
# chg ownership
chown -R <user_name>:<user_group>  /path/to/anywhere
```

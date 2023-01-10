---
layout: post
title: [note] linux privilege, sudo, chmod
date: 2023-01-10 16:37
category: 
author: 
tags: []
summary: 
---

#### sudo
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
	2. acme can sudo to `nobody` and use cp and ls with no password, and sudo df with password
	3. this change enabled immediately


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

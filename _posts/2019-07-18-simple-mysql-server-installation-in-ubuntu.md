---
layout: blog
title: Simple Mysql Server Installation in Ubuntu
published: true
date: 2019-07-18T10:18:46.718Z
thumbnail: /images/uploads/install-mysql-ubuntu.png
rating: 1
---
In this post look how to install mysql server install 

Step 1 : Update the repository

```bash
$ sudo apt update
```

Step 2: Install MySql Server and Client package on system

```bash
$ sudo apt install mysql-server mysql-client
```

Step 3: Configuration Mysql Server
```bash
$ sudo mysql_secure_installation
```
Configurator you mysql root password

Step 4: Login using Mysql Client cli
```bash
$ mysql -uroot -p
```
> Enjoy

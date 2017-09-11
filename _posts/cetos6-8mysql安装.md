---
title: cetos6.8mysql安装
date: 2017-09-11 10:20:47
tags: [mysql]
categories: mysql
description: mysql安装 
---
<!--more-->

一，wget http://dev.mysql.com/get/mysql57-community-release-el6-8.noarch.rpm

二，yum localinstall mysql57-community-release-el6-8.noarch.rpm

三，yum install mysql-server

四，mysqld --initialize --user=mysql

五，找到密码 vi /var/log/mysqld.log 末尾localhost：后面就是临时密码

六，启动mysql服务 service mysqld start

七，修改密码 mysqladmin -uroot -p password 输入临时密码或复制也可以，设置新密码

八，chkconfig mysqld on 此步骤可做可不做

九，mysql -u root -p 输入密码启动

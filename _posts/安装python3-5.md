---
title: 安装python3.5
date: 2017-09-11 09:49:47
tags: [python,python3]
categories: python
description: python3安装
---
<!--more-->

## 安装python3.5可能使用的依赖
[root@heweiwei heweiwei]# yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel

## 到python官网找到下载路径, 用wget下载
[root@heweiwei heweiwei]# wget https://www.python.org/ftp/python/3.5.1/Python-3.5.1.tgz

## 解压tgz包
[root@heweiwei heweiwei]# tar -zxvf Python-3.5.1.tgz

## 把python移到/usr/local下面
[root@heweiwei heweiwei]# mv Python-3.5.1 /usr/local

## 删除旧版本的python依赖
[root@heweiwei heweiwei]# ll /usr/bin | grep python
-rwxr-xr-x.   1 root root      20152 May 12  2016 abrt-action-analyze-python
-rwxr-xr-x.   2 root root       9032 Aug 18  2016 python
lrwxrwxrwx.   1 root root          6 Feb 27 16:05 python2 -> python
-rwxr-xr-x.   2 root root       9032 Aug 18  2016 python2.6
-rwxr-xr-x.   1 root root       1418 Aug 18  2016 python2.6-config
lrwxrwxrwx.   1 root root         16 Feb 28 17:21 python-config -> python2.6-config

[root@heweiwei heweiwei]# rm -rf /usr/bin/python

进入python目录
[root@heweiwei heweiwei]# cd /usr/local/Python-3.5.1/
[root@heweiwei Python-3.5.1]# 

## 配置
[root@heweiwei Python-3.5.1]# ./configure

## 编译 make
[root@heweiwei Python-3.5.1]# make

## 编译，安装
[root@heweiwei Python-3.5.1]# make install

## 删除旧的软链接，创建新的软链接到最新的python
[root@heweiwei Python-3.5.1]# rm -rf /usr/bin/python
[root@heweiwei Python-3.5.1]# ln -s /usr/local/bin/python3.5 /usr/bin/python

## 查看版本
[heweiwei@heweiwei ~]$ python -V
Python 3.5.1


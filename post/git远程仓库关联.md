---
title: git远程仓库关联
date: 2017-09-01 12:51:18
tags: [git]
categories: 开发工具
---
## 引言
我的环境：ubantu17.04 其他linux发行版本安装使用大同小异 
安装git：sudo apt-get install git
首先必须有一个githup账号 去官网注册即可 同时绑定邮箱 
有了账号后创建远程仓库 比如我创建的仓库名字为project

## 密钥
本地创建密钥命令如下
ssh-keygen -t rsa -C "邮箱地址"
公钥存放在用户主目录 ~/.ssh/id_rsa.pub 
复制到官网在账户设置一栏添加即可

## 验证连接
验证是否能连接上
ssh -T git@github.com
输入yes后出现类似以下内容表示已经可以连接
Hi he475943497! You've successfully authenticated, but GitHub does not provide shell access.

## 创建本地仓库
执行下列命令
创建仓库并初始化
mkdir project
cd project
git init

## 指明用户
若之前没有使用git执行以下操作
git config --global user.email "邮箱地址"
git config --global user.name "用户名"
 
## 本地与远程关联并推送
touch test.txt 
git add test.txt
git commit -m "test"
git remote add origin git@github.com:用户名/仓库名.git
推送到远程
git push -u origin master 第一次提交加-u参数
之后提交
git add .  git commit -m "test"   git push 即可

## git教程
git作为目前最流行的版本管理工具，我觉得很有学习的必要
有兴趣的同学可以参考：[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
作者写的很幽默通俗易懂
















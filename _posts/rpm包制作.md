---
title: rpm包制作
date: 2017-09-11 09:42:59
tags: [rpm,打包]
categories: linux基础
description: rpm包相当于windows下的软件安装包
---
<!--more-->

## rpm 即软件包管理
关键点：SPEC文档	

## 生成工作目录：
[root@localhost heweiwei]# yum install rpmdevtools
[heweiwei@localhost ~]$ rpmdev-setuptree		此命令用来生成 rpmbuild 目录
[heweiwei@localhost ~]$ ls rpmbuild/
BUILD  RPMS  SOURCES  SPECS  SRPMS

## 目录分配：
SOURCES放置打包资源，包括源码打包文件和补丁文件等；
SPECS目录放置SPEC文档；
BUILD打包过程中的工作目录；
RPMS目录存放生成的二进制包，RPM包根据硬件平台不同分类，i386表示生成i386结构的包将存放在该目录下;	
SRPMS目录存放生成的源码包。

## 撰写SPEC文档 （最为核心）
SPEC文档包括了 rpm打包过程的操作内容和新生成rpm包的基本信息等 它的作用对象是打包程序 rpmbuild
SPEC文档编写参考模板文件然后一步步扩展

生成rpm包的源代码、shell脚本、配置文件都拷贝到SOURCES目录里，注意通常情况下源码的压缩格式都为*.tar.gz格式
然后，将最最最重要的SPEC文件，命名格式一般是“软件名-版本.spec”的形式，将其拷贝到SPECS目录，切换到该目录下执行
rpmbuild -ba 软件名-版本.spec
-ba表示build all，即生成包括二进制包和源代码包的所有RPM包，
如果正常的话，rpmbuild将正常退出，同时在RPMS目录和SRPMS目录中将生成对应的RPM包。

关键就是SPEC文件的写法，我们可以用rpmdev-newspec -o Name-version.spec命令来生成SPEC文件的模板，
然后在上面修改即可

## 创建 安装 卸载
rpmbuild -bb edns_dial.spec	创建rpm包
rpm -qlp /root/rpmbuild/RPMS/x86_64/edns_dial-1.1.1.5-1.el6.x86_64.rpm	查看安装情况
rpm -ivh edns_dial-1.1.1.5-1.el6.x86_64.rpm  	安装
rpm -ivh edns_dial-1.1.1.5-1.el6.x86_64.rpm  --force（发生冲突强制执行）
rpm -e ends_dial    	卸载

## 特殊情况
出现此情况
libcrypto.so.10()(64bit) is needed by edns_dial-1.1.1.5-1.el6.x86_64
file /usr/bin/edns_dial from install of edns_dial-1.1.1.5-1.el6.x86_64 conflicts 
with file from package dnsys-3.1.1.368-ydns_743863.x86_64
用此命令
rpm -ivh edns_dial-1.1.1.5-1.el6.x86_64.rpm --nodeps --force		

rpm包安装后的可执行程序一般为release版本若想要保留调试信息则应该在spec文件加入以下信息
调试信息的作用不大，一般自动删除调试信息

%define debug_package %{nil}
%define __strip /bin/true

[root@localhost BUILD]# rpm -ivh /root/rpmbuild/RPMS/x86_64/edns_dial-1.1.1.7-1.el6.x86_64.rpm 
Preparing...                ########################################### [100%]
1:edns_dial              ########################################### [100%]
[root@localhost BUILD]# md5sum edns_dial-1.1.1.7/edns_dial
f569fa864f820a4a0dc3d30f057ca4e5  edns_dial-1.1.1.7/edns_dial
[root@localhost BUILD]# md5sum /usr/bin/edns_dial 
f569fa864f820a4a0dc3d30f057ca4e5  /usr/bin/edns_dial
[root@localhost BUILD]# rpm -e edns_dial
[root@localhost BUILD]# 
对比后md5值一模一样

## 卸载出现此情况解决方案
[root@localhost heweiwei]# rpm -e edns_dial
error: "edns_dial" specifies multiple packages:
edns_dial-1.1.1.5-enterprise.el6.x86_64
edns_dial-1.1.1.5-1.el6.x86_64
[root@localhost heweiwei]# rpm -e edns_dial --allmatches 
edns_dial-1.1.1.5-enterprise.el6.x86_64 edns_dial-1.1.1.5-1.el6.x86_64

rpmbuild --showrc | grep topdir  	查看系统默认的工作车间
自定义目录（车间）
vi ~/.rpmmacros 
%_topdir        /home/heweiwei/rpmbuild    ##目录可以自定义 
mkdir ~/rpmbuild 
cd ~/rpmbuild  
mkdir -pv {BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS} 


rpm -qi edns_dial 查看 rpm 包安装程序的详细信息














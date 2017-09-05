---
title: DNS BIND 压力测试 queryperf 使用
date: 2017-09-05 17:36:38
tags: [DNS,BIND,queryper]
categories: DNS服务
description: 本文讲述了bind自带压力测试软件queryperf如何使用
---
<!--more-->

## 引言
首先queryperf是bind自带的压力测试软件，使用queryperf可以对DNS服务器作请求测试，操作简单明了易上手，可以多次测试取平均值，特别说明queryperf的测试结果不一定准确，但与实际情况接近，具有一定参考价值。

## 安装
源码安装官网下载[bind源码](https://ftp.isc.org/isc/bind9/9.11.2/)
[heweiwei@heweiwei local]$ cd /usr/local/bind-9.11.1/contrib/queryperf
[root@heweiwei queryperf]# ./configure
[root@heweiwei queryperf]# cp queryperf /usr/bin/

## 测试
queryperf使用格式：
queryperf [-d datafile] [-s server_addr] [-p port] [-q num_queries]
-d: 后面接上一个文件，文件的内容是用户对DNS的请求，一行为一条请求，所以为了测试，我们可以在里面写上几千几万条。
-s: DNS服务器地址
-p: DNS服务器端口
-q: 指定查询的输出的最大数量

编辑一个测试文件
[heweiwei@heweiwei ~]$ vim test.txt
比如把下面内容 当然这些内容少的可怜，测试可以加入上万条记录
www.baidu.com   A
www.baidu.cn    cname
qq.com A
sohu.com A
python.org CNAME
python.org A
isc.org A
163.com A
163.com SOA
163.com MX
163.com ANY
163.com CNAME
csdn.net A
www.jd.com A
www.jd.com NS

执行
[heweiwei@heweiwei ~]$ queryperf -d test.txt -s 192.168.6.189

出现下列结果
DNS Query Performance Testing Tool
Version: $Id: queryperf.c,v 1.12 2007/09/05 07:36:04 marka Exp $

[Status] Processing input data
[Status] Sending queries (beginning with 192.168.6.189)
[Timeout] Query timed out: msg id 2
[Timeout] Query timed out: msg id 4
[Timeout] Query timed out: msg id 5
[Timeout] Query timed out: msg id 8
[Timeout] Query timed out: msg id 11
[Timeout] Query timed out: msg id 14
[Status] Testing complete

Statistics:

  Parse input file:     once
  Ended due to:         reaching end of file

  Queries sent:         15 queries
  Queries completed:    15 queries
  Queries lost:         0 queries
  Queries delayed(?):   0 queries

  RTT max:         	1.022373 sec
  RTT min:              0.015800 sec
  RTT average:          0.135892 sec
  RTT std deviation:    0.313435 sec
  RTT out of range:     0 queries

  Percentage completed: 100.00%
  Percentage lost:        0.00%

  Started at:           Tue Sep  5 17:56:57 2017
  Finished at:          Tue Sep  5 17:57:02 2017
  Ran for:              5.000256 seconds

  Queries per second:   2.999846 qps

## 总结 
1、在作服务器的性能测试时，最好不要在服务器平台自身使用测试软件测试，最好换另外一台机器，这样CPU处理的结果会更准确。
2、测试时先预估平台会遇到的最大请求数，用这个请求数作测试，量力而为，因为如果服务器遇到大流量的DDOS，单一机器性能再好，也扛不住。
3、使用queryperf作性能测试时，最好测试多次，取平均值。
4、可以修改配置文件的部分参数测试，如，开启递归，开启查询日志等功能作测试。

## 特别感谢
本文参考[文章](http://blog.csdn.net/zhu_tianwei/article/details/45202899)













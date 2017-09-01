---
title: linux一些命令
date: 2017-09-01 15:50:01
tags: [命令]
categories: linux命令
---
## dig
	1.查看域名的A记录  dig yahoo.com
	2.查看域名的ip  dig yahoo.com +short
	3.查看域名的MX 记录  dig yahoo.com MX  
	4.查看域名的SOA记录  dig yahoo.com SOA
	5.查询域名的TTL记录  dig yahoo.com TTL
	6.查看内容信息   dig yahoo.com +nocomments +noquestion +noauthority +noadditional +nostats 
	7.查询所有的DNS记录类型  dig yahoo.com ANY +noall +answer 
	8.DNS反向查询  dig -x 72.30.38.140 +short 
	9.查询多个DNS记录   dig yahoo.com mx +noall +answer redhat.com ns +noall +answer 
	10.单独查询    dig yahoo.com A/SOA/MX/NS/PTR +noall +answer	
	11.查询所有    dig yahoo.com ANY +noall +answer
	12.快速回答时，+short  dig www.baidu.com AAAA +shor
	13. +multiline选项获得冗长的多行模式人性化注释的DSN的SOA记录，
	    一般来说，用+multiline选项获得的信息可以显示很多，就像BIND配置文件一样
	     dig +nocmd baidu.com any +multiline +noall +answer
	14.跟踪dig的查询路径	dig v.qq.com +trace
 15.dig qq.com @x.x.x.x

## find
	Linux下find命令在目录结构中搜索文件，并执行指定的操作。Linux下find命令提供了相当多的查找条件
	find pathname -options [-print -exec -ok ...]	    用于在文件树种查找文件，并作出相应的处理 
	find -atime -2 	超找48小时内修改过的文件
	find . -name "*.log"	在当前目录查找 以.log结尾的文件。 ". "代表当前目录 	
	find /opt/soft/test/ -perm 777	查找/opt/soft/test/目录下 权限为 777的文件
	find . -type f -name "*.log"	查找当目录，以.log结尾的普通文件 
	find . -type d | sort	查找当前所有目录并排序
	find . -size +1000c -print	查找当前目录大于1K的文件 
	ubantu 用于16.04以上版本

## ubantu 界面设置
	gsettings set com.canonical.Unity.Launcher launcher-position Bottom 	移动到下方
	gsettings set com.canonical.Unity.Launcher launcher-position Left	移动到左方

## vim
	vim打开一个文件 shift + ： 使用 vsplit 命令再打开一个文件	vsplit + 文件 
	q qall（关闭所有） only（关闭当前）
	在一般模式下输入“:new /root/2.txt” 新编辑文件2.txt
	ctrl + ww 切换 

	把正在编辑的文件另存为
	在一般模式下输入“:w /root/1.txt”

	正在编辑文件时，不退出文件仍可以运行linux命令
	eg：在编辑模式下输入“:! cat /root/1.txt”

	.查找替换的功能使用
	例：在10到15行的行首增加“#”
	在一般模式下输入“:10,15s/^/#/”
	例：在10到15行的行首去掉“#”
	在一般模式下输入“:10,15s/^#//”
	例：在10到15行的行首增加“//”
	在一般模式下输入“:10,15s/^/\/\//”或者“:10,15s@^@//@”或者“:10,15s#^#//#”

	把文件恢复到打开时的状态
	在一般模式下输入“:e!

	vim 中 shift + 3 选中单词 可以突出显示这个单词全部
	
	ctrl +n 自动补全 ctrl + p 也一样

	删除空行：:%s/^\n$//g

	自动对齐 v 进入可视模式后选中要对齐的部分 = 号自动对齐
	
	stat 文件   获取文件详细信息
	fdisk -l 查看磁盘信息
	
	vim 用swap 文件恢复未保存的文件
	vim -r filename 后 删除 swap文件
	vimdiff 
	比较两个文件不同
	vimdiff /home/heweiwei/dial_cpp/Makefile Makefile.jc
	或者使用diff命令重定向也可以达到比较效果
	diff /home/heweiwei/dial_cpp/Makefile Makefile.jc > cmp.txt

## grep 命令
	查找指定进程
	ps -ef|grep svn
	ps -aux|grep svn
	ps -ef|grep svn -c	包含个数
	ps -ef|grep -c svn	

	输出test.txt文件中含有从test2.txt文件中读取出的关键词的内容行
	cat test.txt | grep -f test2.txt
	cat test.txt | grep -nf test2.txt

	从文件中查找关键字	
	grep 'linux' test.txt
	grep 'linux' test.txt test2.txt

	grep不显示本身进程
	ps aux|grep \[s]sh
	ps aux | grep ssh | grep -v "grep"
	
	找出以 u 开头的行内容
	cat test.txt |grep ^u
	找出非 u 开头的行内容
	cat test.txt |grep ^[^u]

	输出以hat结尾的行内容
	cat test.txt |grep hat$

	显示包含ed或者at字符的内容行
	cat test.txt |grep -E "ed|at"

	显示当前目录下面以.txt 结尾的文件中的所有包含每个字符串至少有7个连续小写字符的字符串的行
	grep '[a-z]\{7\}' *.txt
	
## scp
	把本地文件或目录拷到目标机指定目录

	对拷文件夹 (包括文件夹本身)
	scp -r   /home/wwwroot/www/charts/util root@192.168.1.65:/home/wwwroot/limesurvey_back/scp
	/home/wwwroot/www/charts/util	本地文件目录
	root@192.168.1.65:/home/wwwroot/limesurvey_back/scp	目标ip及目录

	对拷文件夹下所有文件 (不包括文件夹本身)
	scp   /home/wwwroot/www/charts/util/* root@192.168.1.65:/home/wwwroot/limesurvey_back/scp
 
 	对拷文件并重命名
	scp   /home/wwwroot/www/charts/util/a.txt root@192.168.1.65:/home/wwwroot/limesurvey_back/scp/b.text

	把目标机文件或者目录拷贝到本地指定目录
	scp -r root@192.168.6.240:/home/heweiwei/studytest /home/heweiwei/




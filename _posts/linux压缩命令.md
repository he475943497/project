---
title: linux压缩命令
date: 2017-09-12 09:41:44
tags: [压缩,tar,gzip]
categories: linux命令
description: 压缩是一个非常常用的功能，linux提供了非常丰富的压缩功能，本文主要介绍了最常用的压缩方法
---
<!--more-->

[root@www ~]# tar [-j|-z] [cv] [-f 创建的档名] filename... <==打包与压缩
[root@www ~]# tar [-j|-z] [tv] [-f 创建的档名]             <==察看档名
[root@www ~]# tar [-j|-z] [xv] [-f 创建的档名] [-C 目录]   <==解压缩
选项与参数：
-c  ：创建打包文件，可搭配 -v 来察看过程中被打包的档名(filename)
-t  ：察看打包文件的内容含有哪些档名，重点在察看『档名』就是了；
-x  ：解打包或解压缩的功能，可以搭配 -C (大写) 在特定目录解开
      特别留意的是， -c, -t, -x 不可同时出现在一串命令列中。
-j  ：透过 bzip2 的支持进行压缩/解压缩：此时档名最好为 *.tar.bz2
-z  ：透过 gzip  的支持进行压缩/解压缩：此时档名最好为 *.tar.gz
-v  ：在压缩/解压缩的过程中，将正在处理的档名显示出来！
-f filename：-f 后面要立刻接要被处理的档名！建议 -f 单独写一个选项罗！
-C 目录    ：这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项。

其他后续练习会使用到的选项介绍：
-p  ：保留备份数据的原本权限与属性，常用於备份(-c)重要的配置档
-P  ：保留绝对路径，亦即允许备份数据中含有根目录存在之意；
--exclude=FILE：在压缩的过程中，不要将 FILE 打包！ 

其实最简单的使用 tar 就只要记忆底下的方式即可：
  ● 压　缩：tar -jcv -f filename.tar.bz2 要被压缩的文件或目录名称
  ● 查　询：tar -jtv -f filename.tar.bz2
  ● 解压缩：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录
  ● 压    缩：tar -zcvf *.tar.gz  dir/file
  ● 解压缩 ：tar -zxvf  *.tar.gz

下面介绍压缩中的战斗机xz
xz这个压缩可能很多都很陌生，不过您可知道xz是绝大数linux默认就带的一个压缩工具。
之前xz使用一直很少，所以几乎没有什么提起。
我是在下载phpmyadmin的时候看到这种压缩格式的，phpmyadmin压缩包xz格式的居然比7z还要小，这引起我的兴趣。
最新一段时间会经常听到xz被采用的声音，像是最新的archlinux某些东西就使用xz压缩。不过xz也有一个坏处就是压缩时间比较长，比7z压缩时间还长一些。不过压缩是一次性的，所以可以忽略。
xz压缩文件方法或命令
xz -z 要压缩的文件
如果要保留被压缩的文件加上参数 -k ，如果要设置压缩率加入参数 -0 到 -9调节压缩率。如果不设置，默认压缩等级是6.
xz解压文件方法或命令
xz -d 要解压的文件
同样使用 -k 参数来保留被解压缩的文件。
创建或解压tar.xz文件的方法
习惯了 tar czvf 或 tar xzvf 的人可能碰到 tar.xz也会想用单一命令搞定解压或压缩。其实不行 tar里面没有征对xz格式的参数比如 z是针对 gzip，j是针对 bzip2。
创建tar.xz文件：只要先 tar cvf xxx.tar xxx/ 这样创建xxx.tar文件先，然后使用 xz -z xxx.tar 来将 xxx.tar压缩成为 xxx.tar.xz
解压tar.xz文件：先 xz -d xxx.tar.xz 将 xxx.tar.xz解压成 xxx.tar 然后，再用 tar xvf xxx.tar来解包。

[更多压缩详解参考地址](http://cn.linux.vbird.org/linux_basic/0240tarcompress.php)

01-.tar格式
解包：[＊＊＊＊＊＊＊]$ tar xvf FileName.tar
打包：[＊＊＊＊＊＊＊]$ tar cvf FileName.tar DirName（注：tar是打包，不是压缩！）

02-.gz格式
解压1：[＊＊＊＊＊＊＊]$ gunzip FileName.gz
解压2：[＊＊＊＊＊＊＊]$ gzip -d FileName.gz
压 缩：[＊＊＊＊＊＊＊]$ gzip FileName

03-.tar.gz格式
解压：[＊＊＊＊＊＊＊]$ tar zxvf FileName.tar.gz
压缩：[＊＊＊＊＊＊＊]$ tar zcvf FileName.tar.gz DirName

04-.bz2格式
解压1：[＊＊＊＊＊＊＊]$ bzip2 -d FileName.bz2
解压2：[＊＊＊＊＊＊＊]$ bunzip2 FileName.bz2
压 缩： [＊＊＊＊＊＊＊]$ bzip2 -z FileName

05-.tar.bz2格式
解压：[＊＊＊＊＊＊＊]$ tar jxvf FileName.tar.bz2
压缩：[＊＊＊＊＊＊＊]$ tar jcvf FileName.tar.bz2 DirName

06-.bz格式
解压1：[＊＊＊＊＊＊＊]$ bzip2 -d FileName.bz
解压2：[＊＊＊＊＊＊＊]$ bunzip2 FileName.bz

07-.tar.bz格式
解压：[＊＊＊＊＊＊＊]$ tar jxvf FileName.tar.bz

08-.Z格式
解压：[＊＊＊＊＊＊＊]$ uncompress FileName.Z
压缩：[＊＊＊＊＊＊＊]$ compress FileName

09-.tar.Z格式
解压：[＊＊＊＊＊＊＊]$ tar Zxvf FileName.tar.Z
压缩：[＊＊＊＊＊＊＊]$ tar Zcvf FileName.tar.Z DirName

10-.tgz格式
解压：[＊＊＊＊＊＊＊]$ tar zxvf FileName.tgz

11-.tar.tgz格式
解压：[＊＊＊＊＊＊＊]$ tar zxvf FileName.tar.tgz
压缩：[＊＊＊＊＊＊＊]$ tar zcvf FileName.tar.tgz FileName

12-.zip格式
解压：[＊＊＊＊＊＊＊]$ unzip FileName.zip
压缩：[＊＊＊＊＊＊＊]$ zip FileName.zip DirName

13-.lha格式
解压：[＊＊＊＊＊＊＊]$ lha -e FileName.lha
压缩：[＊＊＊＊＊＊＊]$ lha -a FileName.lha FileName

14-.rar格式
解压：[＊＊＊＊＊＊＊]$ rar a FileName.rar
压缩：[＊＊＊＊＊＊＊]$ rar e FileName.rar

15-.tar.xz格式
打包：[＊＊＊＊＊＊＊]$ tar -cvf FileName.tar DirName（注：tar是打包，不是压缩！）
压缩：[＊＊＊＊＊＊＊]$ xz -z -9 FileName.tar （-0 到 -9 压缩率 默认-6其实变化不是很大）

解压：[＊＊＊＊＊＊＊]$ xz -d FileName.tar.xz
解包：[＊＊＊＊＊＊＊]$ tar -xvf FileName.tar

总结
常用的压缩格式 	tar.gz 	tar.bz2  bz2能比gz强那么一丢丢，但相差不是很大
压缩king   tar.xz   压缩大小几乎是上面两个的一半 很强 但是压缩速度很慢 有利有弊吧

测试对比如下  具体实际情况灵活运用即可
[root@localhost others]# du -sh *
49M	dhcp
15M	dhcp.tar.bz2
16M	dhcp.tar.gz
8.0M	dhcp.tar.xz




---
title: 我的git使用
date: 2017-09-19 10:48:43
tags: [git]
categories: 开发工具
---
<!--more-->

## 引言

## 安装git
yum apt 均可

## Git add 
将当前工作目录中更改或者新增的文件加入到Git的索引中，加入到Git的索引中就表示记入了版本历史中，
这也是提交之前所需要执行的一步。 
可以递归添加，即如果后面跟的是一个目录作为参数，则会递归添加整个目录中的所有子目录和文件。例如： 
       git add dir1 （ 添加dir1这个目录，目录下的所有文件都被加入 ） 
       Git add f1 f2 （ 添加f1，f2文件） 
       git add .      ( 添加当前目录下的所有文件和子目录 ) 


## Git commit 
提交当前工作目录的修改内容。 
直接调用git commit命令，会提示填写注释。通过如下方式在命令行就填写提交注释：
git commit -m "Initial commit of gittutor reposistory"。 注意，和CVS不同，git的提交注释必须不能为空，否则就会提交失败。 
git commit还有一个 -a的参数，可以将那些没有通过git add标识的变化一并强行提交，但是不建议使用这种方式。 
每一次提交，git就会为全局代码建立一个唯一的commit标识代码，用户可以通过git reset命令恢复到任意一次提交时的代码。 
git commit –-amend –m “message” （在一个commit id上不断修改提交的内容） 


## Git status 
查看版本库的状态。可以得知哪些文件发生了变化，哪些文件还没有添加到git库中等等。 
建议每次commit前都要通过该命令确认库状态。 
最常见的误操作是， 修改了一个文件， 没有调用git add通知git库该文件已经发生了变化就直接调用commit操作， 
从而导致该文件并没有真正的提交。这时如果开发者以为已经提交了该文件，就继续修改甚至删除这个文件，
那么修改的内容就没有通过版本管理起来。如果每次在 提交前，使用git status查看一下，就可以发现这种错误。
因此，如果调用了git status命令，一定要格外注意那些提示为 “Changed but not updated:”的文件。 
这些文件都是与上次commit相比发生了变化，但是却没有通过git add标识的文件。


## 初始化一个Git仓库，使用git init命令。
添加文件到Git仓库，分两步：
第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
第二步，使用命令git commit，完成。

要随时掌握工作区的状态，使用git status命令。
如果git status告诉你有文件被修改过，用git diff可以查看修改内容


## Git必须知道当前版本是哪个版本，
在Git中，用HEAD表示当前版本，
也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），
上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
git reset --hard HEAD^

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向append GPL
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。


git add命令实际上就是把要提交的所有修改放到暂存区（Stage），
然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。git commit只负责把暂存区的修改提交了


git checkout -- file可以丢弃工作区的修改：
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。


Git同样告诉我们，用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区：
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，
分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。


一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
$ rm test.txt
这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：
现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit
现在，文件就从版本库中被删除了。
另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。


## 关联
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改

## 克隆
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
git clone git@github.com:michaelliao/gitskills.git
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

## 分支
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>


## 冲突 合并
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
用git log --graph命令可以看到分支合并图。


Git分支十分强大，在团队开发中应该充分应用。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，
而fast forward合并就看不出来曾经做过合并。


## git stash
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。



## 开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。



## 查看远程库信息，使用git remote -v；
本地新建的分支如果不推送到远程，对其他人就是不可见
从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。


# 我的git使用

git client端需要在～/.ssh/config文件中添加如下信息：
host 192.168.8.253
port 61112

自己新建一个目录在该目录下
git clone git@192.168.8.253:yprobe-c.git

切到目标目录下
[heweiwei@heweiwei work]$ ls
yprobe-c
[heweiwei@heweiwei work]$ cd yprobe-c/

## 查看处于那个分支
[heweiwei@heweiwei yprobe-c]$ git branch
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/enterprise
  remotes/origin/master

[heweiwei@heweiwei yprobe-c]$ 

## 切换分支
[heweiwei@heweiwei yprobe-c]$ git checkout enterprise	/
Branch enterprise set up to track remote branch enterprise from origin.


Switched to a new branch 'enterprise'
[heweiwei@heweiwei yprobe-c]$ 
[heweiwei@heweiwei yprobe-c]$ 

## 查看远程所有分支
[heweiwei@heweiwei yprobe-c]$ git branch -a 
* enterprise
  master

## 查看状态
[heweiwei@heweiwei yprobe-c]$ git status
On branch enterprise

##  接收数据
[heweiwei@heweiwei yprobe-c]$ git pull
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 2), reused 0 (delta 0)
Unpacking objects: 100% (4/4), done.
From 192.168.8.253:yprobe-c
   ccf10bf..9b609d8  enterprise -> origin/enterprise
Updating ccf10bf..9b609d8
Fast-forward
 dial_cpp/Dial_server.cpp |   88 ++++++++++++++++++++++++++++++++++++++++++++-
 1 files changed, 86 insertions(+), 2 deletions(-)

## 保存现场
git stash 	修改了东西  暂存起来

## 我的提交：
[heweiwei@heweiwei dial]$ git add Dial_epoll.cpp
[heweiwei@heweiwei yprobe-c]$ git commit -m "repeat assignment"
[heweiwei@heweiwei yprobe-c]$ git push
[heweiwei@heweiwei yprobe-c]$ git log
commit 13fb4c131309358ce0099295f3a34a3e0a873f6e
Author: heweiwei <heweiwei@heweiwei.(none)>
Date:   Wed Mar 29 10:41:12 2017 +0800

repeat assignment

## 创建分支并切换到该分支
[heweiwei@heweiwei yprobe-c]$ git checkout -b heweiwei_dial
此命令相当于 git branch heweiwei_dial 	git checkout heweiwei_dial

## 把本地分支推送到远程分支
[heweiwei@heweiwei yprobe-c]$ git push origin heweiwei_dial

## 删除远程分支
git push origin :heweiwei_dial 注意origin后面的空格

（只有合并之后）才可以用此命令删除本地分支（因为创建本地分支的目的就是为了开发合并）
[heweiwei@heweiwei yprobe-c]$ git branch -d heweiwei_dial
error: The branch 'heweiwei_dial' is not fully merged.
If you are sure you want to delete it, run 'git branch -D heweiwei_dial'.

## 强制删除本地分支（没有合并的分支也删除）
[heweiwei@heweiwei yprobe-c]$ git branch -D heweiwei_dial


## 合并分支  用 git merge 命令来进行合并
[heweiwei@heweiwei yprobe-c]$ git add edns_dial-1.1.1.8/* 
[heweiwei@heweiwei yprobe-c]$ git commit -m "edns_dial-1.1.1.8 have three log"
[heweiwei@heweiwei yprobe-c]$ git checkout enterprise
[heweiwei@heweiwei yprobe-c]$ git merge heweiwei_dial
[heweiwei@heweiwei yprobe-c]$ git branch -d heweiwei_dial
Deleted branch heweiwei_dial (was 39dc1df).
[heweiwei@heweiwei yprobe-c]$ git push 		// 推到远程服务器
[heweiwei@heweiwei yprobe-c]$ git push origin master.dev		// 推到远程服务器具体某个分支

## 回退到上一个版本
[heweiwei@heweiwei yprobe-c]$ git reset --hard HEAD^
HEAD is now at 1475127 打rpm包和集成服务自启动
上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100

git log --pretty=oneline
[heweiwei@heweiwei yprobe-c]$ git log --pretty=oneline
147512720038d1a40776abae7d119565c36c4451 打rpm包和集成服务自启动
9623de89e686da18311aa26312ad43be1eb07f91 反向代理的https方式拨测的指定端口改变
635c41264d788acaae9530306611032fc7b74bfa Fix Makefile: libthrift.a (v-0.9.0)
80126458120dd8c3499af945b45572b1117e7272 拨测频繁切换，thrift通信架构
只要命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个append GPL的commit id是3628164...，

## 回退到你想回的版本
[heweiwei@heweiwei dial_cpp]$ git reset --hard 1475127
HEAD is now at 1475127 打rpm包和集成服务自启动
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

若要查看各个分支最后一个提交对象的信息，运行 git branch -v：

将远程版本库回滚到上一个版本  或者回滚到以前某个版本

git reset --hard HEAD^	或者 git reset --hard “commit ID”
git push origin master -f


## git clone git@192.168.8.253:yrdns.git 递归代码
## git clone ssh://git@192.168.8.253:61112/clib.git  clib库

增加了授权功能
[heweiwei@heweiwei yrdnsl]$ git pull origin master.dev  
From 192.168.8.253:yrdns
 * branch            master.dev -> FETCH_HEAD
Updating 536db80..83eafe8
Fast-forward
 .gitignore             |    3 +
 Makefile.in.temp       |    1 +
 conf/yrdns.conf        |   21 +-
 include/yr_cache.h     |    4 +-
 include/yr_config.h    |   92 ++-
 include/yr_hash.h      |    1 +
 include/yr_ini.h       |    2 +-
 include/yr_job.h       |    1 +
 include/yr_log.h       |    4 +
 include/yr_packet.h    |    2 +
 include/yr_recursing.h |    2 +
 include/yr_stat.h      |    5 +
 include/zone.h         |  528 +++++++
 src/Makefile           |    2 +-
 src/yr_cache.c         |  121 +-
 src/yr_config.c        | 1281 ++++++++++++++++-
 src/yr_forwarder.c     |    2 +-
 src/yr_hash.c          |    9 +-
 src/yr_iphash.c        |    3 +-
 src/yr_iterator.c      |  117 +-
 src/yr_job.c           |  232 ++--
 src/yr_log.c           |  111 ++-
 src/yr_recursing.c     |   54 +-
 src/yr_thrift.c        |   10 +-
 src/yr_view.c          |    4 +-
 src/zone.c             | 3747 ++++++++++++++++++++++++++++++++++++++++++++++++
 version                |    2 +-
 27 files changed, 6049 insertions(+), 312 deletions(-)
 create mode 100644 include/zone.h
 create mode 100644 src/zone.c




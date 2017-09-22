---
title: 我的Makefile总结
date: 2017-09-22 09:40:37
tags: [inux,makefile]
categories: [linux基础,Makefile]
description: 作为一个好的linux下码农makefile还是要懂，并且要很熟悉，你就不能不自己写makefile了，会不会写makefile，从一个側面说明了一个人是否具备完成大型工程的能力。
---
<!--more-->

## Makefile规则
一句话总结就是依赖关系，简单如下所示
target ... : prerequisites ...
command
...
...
target也就是一个目标文件，能够是Object File，也能够是运行文件。还能够是一个标签（Label），对于标签这样的特性，在后续的“伪目标”章节中会有叙述。
prerequisites就是，要生成那个target所须要的文件或是目标。
command也就是make须要运行的命令。（随意的Shell命令）
这是一个文件的依赖关系，也就是说，target这一个或多个的目标文件依赖于prerequisites中的文件，其生成规则定义在command中。说白一点就是说，prerequisites中假设有一个以上的文件比target文件要新的话，command所定义的命令就会被运行。这就是Makefile的规则。也就是Makefile中最核心的内容。

## Makefile 工作
1、make会在当前文件夹下找名字叫“Makefile”或“makefile”的文件。
2、假设找到，它会找文件里的第一个目标文件（target），在上面的样例中，他会找到“edit”这个文件，并把这个文件作为终于的目标文件。
3、假设edit文件不存在，或是edit所依赖的后面的 .o 文件的文件改动时间要比edit这个文件新，那么，他就会运行后面所定义的命令来生成edit这个文件。
4、假设edit所依赖的.o文件也不存在，那么make会在当前文件里找目标为.o文件的依赖性，假设找到则再依据那一个规则生成.o文件。（这有点像一个堆栈的过程）
5、当然，你的C文件和H文件是存在的啦，于是make会生成 .o 文件，然后再用 .o 文件生命make的终极任务，也就是运行文件edit了。

## Makefile 内容
Makefile里主要包括了五个东西：显式规则、隐晦规则、变量定义、文件指示和凝视。
1、显式规则。显式规则说明了，怎样生成一个或多的的目标文件。这是由Makefile的书写者明显指出，要生成的文件，文件的依赖文件，生成的命令。
2、隐晦规则。由于我们的make有自己主动推导的功能，所以隐晦的规则能够让我们比較粗糙地简略地书写Makefile，这是由make所支持的。
3、变量的定义。在Makefile中我们要定义一系列的变量，变量一般都是字符串，这个有点你C语言中的宏，当Makefile被运行时，当中的变量都会被扩展到对应的引用位置上。
4、文件指示。其包括了三个部分，一个是在一个Makefile中引用另一个Makefile，就像C语言中的include一样；另一个是指依据某些情况指定Makefile中的有效部分，就像C语言中的预编译#if一样；还有就是定义一个多行的命令。有关这一部分的内容，我会在后续的部分中讲述。
5、凝视。Makefile中唯独行凝视，和UNIX的Shell脚本一样，其凝视是用“#”字符，这个就像C/C++中的“//”一样。假设你要在你的Makefile中使用“#”字符，能够用反斜框进行转义，如：“/#”。
最后，还值得一提的是，在Makefile中的命令，必须要以[Tab]键开始。

## make的工作方式
GNU的make工作时的运行步骤入下：（想来其他的make也是相似）
1、读入全部的Makefile。
2、读入被include的其他Makefile。
3、初始化文件里的变量。
4、推导隐晦规则，并分析全部规则。
5、为全部的目标文件创建依赖关系链。
6、依据依赖关系，决定哪些目标要又一次生成。
7、运行生成命令。
1-5步为第一个阶段，6-7为第二个阶段。第一个阶段中，假设定义的变量被使用了，那么，make会把其展开在使用的位置。但make并不会全然立即展开，make使用的是迟延战术，假设变量出如今依赖关系的规则中，那么仅当这条依赖被决定要使用了，变量才会在其内部展开

## 通配符
make支持三各通配符：“*”，“?”和“[...]”。这是和Unix的B-Shell是相同的。
波浪号（“~”）字符在文件名称中也有比較特殊的用途。假设是“~/test”，这就表示当前用户的$HOME文件夹下的test文件夹

## 文件搜寻
在一些大的工程中，有大量的源文件，我们通常的做法是把这很多的源文件分类，并存放在不同的文件夹中。所以，当make须要去找寻文件的依赖关系时，你能够在文件前加上路径，但最好的方法是把一个路径告诉make，让make在自己主动去找。
Makefile文件里的特殊变量“VPATH”就是完成这个功能的，假设沒有指明这个变量，make仅仅会在当前的文件夹中去找寻依赖文件和目标文件。假设定义了这个变量，那么，make就会在当当前文件夹找不到的情况下，到所指定的文件夹中去找寻文件了。
VPATH = src:../headers
上面的的定义指定两个文件夹，“src”和“../headers”，make会依照这个顺序进行搜索。文件夹由“冒号”分隔。（当然，当前文件夹永远是最高优先搜索的地方）
另一个设置文件搜索路径的方法是使用make的“vpath”keyword（注意，它是全小写的），这不是变量，这是一个make的keyword，这和上面提到的那个VPATH变量非常相似，可是它更为灵活。它能够指定不同的文件在不同的搜索文件夹中。这是一个非常灵活的功能。它的使用方法有三种：
1、vpath <pattern> <directories>
为符合模式<pattern>的文件指定搜索文件夹<directories>。
2、vpath <pattern>
清除符合模式<pattern>的文件的搜索文件夹。
3、vpath
清除全部已被设置好了的文件搜索文件夹。
vapth使用方法中的<pattern>须要包括“%”字符。“%”的意思是匹配零或若干字符，比如，“%.h”表示全部以“.h”结尾的文件。<pattern>指定了要搜索的文件集，而<directories>则指定了<pattern>的文件集的搜索的文件夹。比如：
vpath %.h ../headers
该语句表示，要求make在“../headers”文件夹下搜索全部以“.h”结尾的文件。（假设某文件在当前文件夹沒有找到的话）
我们能够连续地使用vpath语句，以指定不同搜索策略。假设连续的vpath语句中出现了相同的<pattern>，或是被反复了的<pattern>，那么，make会依照vpath语句的先后顺序来运行搜索。如：
vpath %.c foo
vpath % blish
vpath %.c bar
其表示“.c”结尾的文件，先在“foo”文件夹，然后是“blish”，最后是“bar”文件夹。
vpath %.c foo:bar
vpath % blish
而上面的语句则表示“.c”结尾的文件，先在“foo”文件夹，然后是“bar”文件夹，最后才是“blish”文件夹

## 伪目标
最早先的一个样例中，我们提到过一个“clean”的目标，这是一个“伪目标”，
clean:
rm *.o temp
正像我们前面样例中的“clean”一样，即然我们生成了很多文件编译文件，我们也应该提供一个清除它们的“目标”以备完整地重编译而用。 （以“make clean”来使用该目标）
由于，我们并不生成“clean”这个文件。“伪目标”并非一个文件，仅仅是一个标签，由于“伪目标”不是文件，所以make无法生成它的依赖关系和决定它是否要运行。我们唯独通过显示地指明这个“目标”才干让其生效。当然，“伪目标”的取名不能和文件名称重名，不然其就失去了“伪目标”的意义了。
当然，为了避免和文件重名的这样的情况，我们能够使用一个特殊的标记“.PHONY”来显示地指明一个目标是“伪目标”，向make说明，无论是否有这个文件，这个目标就是“伪目标”。
.PHONY : clean
仅仅要有这个声明，无论是否有“clean”文件，要运行“clean”这个目标，唯独“make clean”这样。于是整个过程能够这样写：
.PHONY: clean
clean:
rm *.o temp
伪目标一般沒有依赖的文件。可是，我们也能够为伪目标指定所依赖的文件。伪目标相同能够作为“默认目标”，仅仅要将其放在第一个。一个演示例子就是，假设你的Makefile须要一口气生成若干个可运行文件，但你仅仅想简单地敲一个make完事，而且，全部的目标文件都写在一个Makefile中，那么你能够使用“伪目标”这个特性：
all : prog1 prog2 prog3
.PHONY : all
prog1 : prog1.o utils.o
cc -o prog1 prog1.o utils.o
prog2 : prog2.o
cc -o prog2 prog2.o
prog3 : prog3.o sort.o utils.o
cc -o prog3 prog3.o sort.o utils.o
我们知道，Makefile中的第一个目标会被作为其默认目标。我们声明了一个“all”的伪目标，其依赖于其他三个目标。由于伪目标的特性是，总是被运行的，所以其依赖的那三个目标就总是不如“all”这个目标新。所以，其他三个目标的规则总是会被决议。也就达到了我们一口气生成多个目标的目的。“.PHONY : all”声明了“all”这个目标为“伪目标”。
随便提一句，从上面的样例我们能够看出，目标也能够成为依赖。所以，伪目标相同也可成为依赖。看以下的样例：
.PHONY: cleanall cleanobj cleandiff
cleanall : cleanobj cleandiff
rm program
cleanobj :
rm *.o
cleandiff :
rm *.diff
“make clean”将清除全部要被清除的文件。“cleanobj”和“cleandiff”这两个伪目标有点像“子程序”的意思。我们能够输入“make cleanall”和“make cleanobj”和“make cleandiff”命令来达到清除不同种类文件的目的。

##
内容很多，在此停顿，详情参考下列文章 
[makefile参考文章](http://www.cnblogs.com/mfryf/p/3305778.html)
下面提供两个模板
很nice 很perfect


## 模板一

//最终可执行程序
TGT=edns_dial   				

//所有源代码的集合
SRCS=$(wildcard ./src/*.cpp)		

// 编译所需库文件 静态库指明路径
LIBRAYS= -levent -lrt -pthread -lssl -lcrypto -ldl -lz ./lib/*.a	

// 编译所需头文件路径
COMPILE_FLAGS= -g -W  -O2 -DHAVE_NETINET_IN_H -I. -I./include -I./clib/include -I/usr/include/glib-2.0 -I/usr/lib64/glib-2.0/include/ -I/usr/include/openssl	

// 编译器
CC=g++

// 最终
all:$(TGT)
        @echo Generation target!        

// 生成最终可执行程序 $@表示目标的集合 $^表示以来的集合
$(TGT):$(SRCS:.cpp=.o)
        $(CC) -o $@ $^ $(LIBRAYS) $(COMPILE_FLAGS) 
        
//以下这个样例表示了,把全部的[.cpp]文件都编译成[.o]文件
%.o : %.cpp
        $(CC) -c $(COMPILE_FLAGS) $< -o $@
  
// 伪目标      
.PHONY: clean install 

// 删除清理
clean:
        rm -rf $(TGT) $(SRCS:.cpp=.o)

// 制作rpm所需的宏
RPM_VERSION = 1.1.1.8
COMMIT = $(shell git rev-list HEAD |head -1|cut -c 1-6)
RPM_RELEASE = $(shell git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/' -e 's/-/_/g')_$(COMMIT)
RPM_RELEASE = enterprise
RPM_TOP_DIR = $(shell rpm -E %{_topdir})
PRJHOME = $(shell pwd)

// rpm包制作
rpm:
        @echo [RPM] ; \
        sed -e "s/@VERSION@/$(RPM_VERSION)/g" -e "s/@RELEASE@/$(RPM_RELEASE)/g" $(TGT).spec.tmp > ${RPM_TOP_DIR}/SPECS/$(TGT).spec ; \
        cp -a -r ${PRJHOME} /tmp/$(TGT)-$(RPM_VERSION) ; \
        cd /tmp ; \
        tar zcvf $(RPM_TOP_DIR)/SOURCES/$(TGT)-$(RPM_VERSION).tar.gz $(TGT)-$(RPM_VERSION) ; \
        rm -rf $(TGT)-$(RPM_VERSION) ; \
        rpmbuild -bb $(RPM_TOP_DIR)/SPECS/$(TGT).spec ; \

// rpm 清理
rpmclean:       
        cp -r ~/rpmbuild/RPMS/x86_64/$(TGT)*$(RPM_VERSION)* ./  
        rm -rf ~/rpmbuild/SOURCES/$(TGT)* \
        ~/rpmbuild/BUILD/$(TGT)* \
        ~/rpmbuild/RPMS/x86_64/$(TGT)* \
        ~/rpmbuild/SPEC/$(TGT)* 


## 模板二
TGT=edns_dial

SRCS=$(wildcard *.cpp)
TOP=$(shell pwd)

LIBRAYS= -levent -lrt -pthread -lssl -lcrypto -ldl -lz ./lib/*.a
COMPILE_FLAGS= -g -W  -O2 -DHAVE_NETINET_IN_H -I. -I./include -I./clib/include -I/usr/include/glib-2.0 -I/usr/lib64/glib-2.0/include/ -I/usr/include/openssl 

TGT_PLACE=$(TOP)/out
DEP=dep

CC=g++
DIAL_PACKET=dial_query.tar.gz
INSTALL_PACKET=dial_core_install.bin.tar

RPM_VERSION = 1.1.1.8
COMMIT = $(shell git rev-list HEAD |head -1|cut -c 1-6)
RPM_RELEASE = $(shell git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/' -e 's/-/_/g')_$(COMMIT)
RPM_RELEASE = enterprise
RPM_TOP_DIR = $(shell rpm -E %{_topdir})
PRJHOME = $(shell pwd)

all:$(TGT)
	cp $(TGT) $(TGT_PLACE)
	@echo Generation target!	

$(TGT):$(SRCS:.cpp=.o)
	$(CC) -o $@ $^ $(LIBRAYS) $(COMPILE_FLAGS) 
	
$(SRCS:.cpp=.o):
	$(CC) -c $< $(COMPILE_FLAGS)

$(DEP): $(SRCS)
	$(CC) -I $(TOP)/include/ -MM  $(SRCS)  > $@
	sed -i "s/: / $@: /g" $@

sinclude $(DEP)
	
.PHONY:	clean install 

clean:
	rm -rf $(TGT) $(DEP) $(SRCS:.cpp=.o) $(TGT_PLACE)

rpm:
	@echo [RPM] ; \
    	sed -e "s/@VERSION@/$(RPM_VERSION)/g" -e "s/@RELEASE@/$(RPM_RELEASE)/g" edns_dial.spec.tmp > ${RPM_TOP_DIR}/SPECS/edns_dial.spec ; \
    	cp -a -r ${PRJHOME} /tmp/edns_dial-$(RPM_VERSION) ; \
    	cd /tmp ; \
    	tar zcvf $(RPM_TOP_DIR)/SOURCES/edns_dial-$(RPM_VERSION).tar.gz edns_dial-$(RPM_VERSION) ; \
    	rm -rf edns_dial-$(RPM_VERSION) ; \
    	rpmbuild -bb $(RPM_TOP_DIR)/SPECS/edns_dial.spec













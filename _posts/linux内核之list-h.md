---
title: linux内核之list.h
date: 2017-09-14 19:28:08
tags: [list,linux基础,链表基础操作]
categories: linux基础
description: linux内核提供了一套完整简单实用的链表操作方法，非常值得值得学习，极具可移植可拓展
---
<!--more-->

[大而全参考地址](http://www.linuxidc.com/Linux/2011-10/44627.htm)
非常详细

##  内核中经常采用链表来管理对象，先看一下内核中对链表的定义
struct list_head {
	struct list_head *next, *prev;
};

一般将该数据结构嵌入到其他的数据结构中，从而使得内核可以通过链表的方式管理新的数据结构，看一个例子:
struct example {
	member a;
	struct list_head list;
	member b;
};

## 1、链表的定义和初始化

您可以通过两种方式来定义和初始化一个链表头结点，例如，您想定义一个链表头结点mylist，那么您可以这么做：
① LIST_HEAD(mylist);  // 使用LIST_HEAD宏定义并初始化一个链表
也可以这么做：
② struct list_head mylist;  // 定义一个链表
INIT_LIST_HEAD(&mylist); // 使用INIT_LIST_HEAD函数初始化链表

可以看出方式①稍微简单一点，我们先来分析一下LIST_HEAD宏：
#define LIST_HEAD_INIT(name) { &(name), &(name) }
#define LIST_HEAD(name) /
struct list_head name = LIST_HEAD_INIT(name)

很容易看出LIST_HEAD(mylist);会被扩展为：
struct list_head mylist = { &(mylist), &(mylist) };

list_head结构只有两个成员：next和prev。从上面的代码可以看出，next和prev都被赋值为链表mylist的地址，也就是说，链表初始
化后next和prev都是指向自己的。

大多数情况下，list_head是被嵌入到其他数据结构中的，比如上面的example结构里的list成员，那么如何对list成员进行初始化？通过调用INIT_LIST_HEAD函数：
struct example test;
INIT_LIST_HEAD(&test.list); 
该函数简单地将list成员的prev和next指针指向自己。

可以看出链表结点在初始化时，都将prev和next指向自己。注意：对链表的初始化非常重要，因为如果使用一个未被初始化的链表结点，很有可能会导致内核异常。例如，在对一个链表结点调用list_del函数后，接着再去对该结点进行一些操作。后面会有分析的：）

## 2、对链表常用的操作

对链表常用的操作无非就是增加、删除、遍历等。当然内核还提供很多其他的操作，如替换某个结点、将某个结点移动到链表尾端等等，这些操作都是通过调用基本的增加、删除等操作完成的。

### ① 增加：list_add和list_add_tail
调用list_add可以将一个新链表结点插入到一个已知结点的后面；
调用list_add_tail可以将一个新链表结点插入到一个已知结点的前面；

下面分析它们的具体实现，它们都以不同的参数调用了相同的函数__list_add：
static inline void __list_add(struct list_head *new,
		struct list_head *prev,
		struct list_head *next)
{
	next->prev = new;
	new->next = next;
	new->prev = prev;
	prev->next = new;
}
该函数将new结点插入到prev结点和next之间；

static inline void list_add(struct list_head *new, struct list_head *head)
{
	__list_add(new, head, head->next);
}
list_add函数中以new、head、head->next为参数调用__list_add，将new结点插入到head和head->next之间，也就是将new结点插入到特定的已知结点head的后面；

static inline void list_add_tail(struct list_head *new, struct list_head *head)
{
	__list_add(new, head->prev, head);
}
而list_add_tail函数则以new、head->prev、head为参数调用__list_add，将new结点插入到head->prev和head之间，也就是将new结点插入到特定的已知结点head的前面。

有了list_add和list_add_tail，我们可以很方便地实现栈（list_add）和队列（list_add_tail），在本文的最后一节，我们再做详细的分析。

### ② 删除：list_del和list_del_init
调用list_del函数删除链表中的一个结点；
调用list_del_init函数删除链表中的一个结点，并初始化被删除的结点（也就是使被删除的结点的prev和next都指向自己）；

下面分析它们的具体实现，它们都调用了相同的函数__list_del：
static inline void __list_del(struct list_head * prev, struct list_head * next)
{
	next->prev = prev;
	prev->next = next;
}
该函数实际的作用是让prev结点和next结点互相指向；

static inline void list_del(struct list_head *entry)
{
	__list_del(entry->prev, entry->next);
	entry->next = LIST_POISON1;
	entry->prev = LIST_POISON2;
}
该函数中以entry->prev和entry->next为参数调用__list_del函数，使得entry结点的前、后结点绕过entry直接互相指向，然后将entry结点的前后指针指向LIST_POISON1和LIST_POISON2，从而完成对entry结点的删除。此函数中的LIST_POISON1和LIST_POISON2有点让人费解，因为一般情况下我们删除entry后，应该让entry的prev和next指向NULL的，可是这里却不是，原因有待调查。

static inline void list_del_init(struct list_head *entry)
{
	__list_del(entry->prev, entry->next);
	INIT_LIST_HEAD(entry);
}
与list_del不同，list_del_init将entry结点删除后，还会对entry结点做初始化，使得entry结点的prev和next都指向自己。

##  3、几个重要的宏

内核提供了一组宏，以方便对链表进行管理，下面我只介绍到目前为止，我遇到过的，可能会很少，因为我接触到的很有限，以后遇到其他的会添加进来的。下面开始我们的分析啦:)

### ① list_entry
前面说过，list_head结构通常被嵌入到其他数据结构中，以便内核可以通过链表的方式管理这些数据结构。假设这样一种场景：我们已知类型为example的对象的list成员的地址ptr（struct list_head *ptr），那么我们如何通过ptr来得到该example对象的地址呢？答案很明显，使用container_of宏。不过，在这样的情况下我们应该通过使用list_entry宏来完成container_of宏的功能，因为这样更容易理解一点。其实list_entry宏很简单：#define list_entry(ptr, type, member)  container_of(ptr, type, member) ......
上述情况，我们可以这样： list_entry(ptr, struct example, list); 来获取example对象的指针。

### ② list_for_each_entry
对链表的一个重要的操作就是对链表进行遍历，以达到某种应用目的，比如统计链表结点的个数等等。先来看看内核中对该宏的定义：

#define list_for_each_entry(pos, head, member)    /
for (pos = list_entry((head)->next, typeof(*pos), member); /
		prefetch(pos->member.next), &pos->member != (head);  /
		pos = list_entry(pos->member.next, typeof(*pos), member))

其中，pos是指向宿主结构的指针，在for循环中是一个迭代变量；head是要进行遍历的链表头指针；member是list_head成员在宿主结构中的名字。



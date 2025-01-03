---
title: Linux文件系统的硬连接和软连接
date: 2018-02-06T22:42:00
tags: ['Linux']
categories: 文件系统
---

为了更好地说明文件链接是什么，先讲一下Linux的磁盘管理方式。

Linux的文件系统格式被称为ExtN(N=2,3,4)，是一种基于inode(索引节点)的文件系统，是所有类Unix系统都有的一种数据结构也是文件系统的核心，每一个新创建的文件都会被分配一个inode，且每个文件都有一个唯一的inode编号。inode可以简单理解成一个指针，指向文件所在的物理位置，同时文件的属性也保存在inode中。


ExtN文件系统读取数据过程示意图


![](http://p1f1jwe7c.bkt.clouddn.com/18-2-6/7849364.jpg)

inode只是记录文件保存的位置，实际的文件保存在data block区域。

而编号为3的inode就是文件的一个链接。

当我们用`ls -l`命令查看文件信息的时候，其中的连接数就是inode的个数，我们可以通过`ln`建立硬链接为同一个文件建立多个连接，它可以使得同一个文件能够拥有不同的路径，还能方式被恶意删除，但有以下几点需要注意：



- 使用`ln`建立硬链接是直接引用目标文件的inode，所有的属性，包括文件的权限信息也会被一同引用进来。

- 只是复制了inode，没有复制data block信息，因此额外的磁盘占用很小，但也使得硬链接只能在同一分区中建立。



建立软连接需要增加`-s`参数，软连接相当于Windows中的快捷方式，可以被建立在任何位置，和目标inode的文件属性不相同，也不会有连接数+1，同时不能起到备份的作用，删除原文件后软连接也会失效。
    
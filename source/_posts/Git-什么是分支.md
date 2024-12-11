---
title: Git-什么是分支
date: 2017-10-01T22:00:00
tags:
categories:
---

为了理解什么是分支，我们先要回顾Git是如何存储数据的。

Git并不会保存文件的差异值或者说变化量，而是直接保存文件的快照。
在Git中提交时，会保存一个commit对象，该对象包含一个指向暂存内容快照的指针、零个或多个指向该提交对象的父对象的指针(首次提交是没有父对象的，普通提交有一个父对象，合并后的提交会有多个父对象)和本次提交的作者等的相关信息。

为直观起见，我们假设在工作目录中有三个文件，准备将它们暂存后提交。暂存操作会对每一个文件计算校验和，然后把当前版本的文件快照保存到 Git 仓库中，并将校验和加入暂存区域：
```
$ git add README test.rb LICENSE
$ git commit -m 'initial commit of my project'
```
当使用 git commit 新建一个提交对象前，Git 会先计算每一个子目录（本例中只有项目根目录）的校验和，然后在 Git 仓库中将这些目录保存为树（tree）对象。之后 Git 创建的提交对象，除了包含相关提交信息以外，还包含着指向这个树对象（项目根目录）的指针，如此它就可以在将来需要的时候，重现此次快照的内容了。

现在，Git 仓库中有五个对象：三个表示文件快照内容的 blob 对象；一个记录着目录树内容及其中各个文件对应 blob 对象索引的 tree 对象；以及一个包含指向 tree 对象（根目录）的索引和其他提交信息元数据的 commit 对象。
![](http://osxdn70ll.bkt.clouddn.com/17-10-1/34394191.jpg)

做些修改再次提交，那么这次的提交对象会包含一个指向上次提交对象的(parent)指针。两次提交后，仓库历史会变成下面的样子：
![](http://osxdn70ll.bkt.clouddn.com/17-10-1/20201845.jpg)
每次提交都会新建一个commit对象，parent指针指向上次提交的结点。

那么分支是什么？Git中的分支其本质仅仅是一个指向commit对象的可变指针，Git会使用master作为分支的默认名字，每次提交master指针都会指向最近一次提交的commit对象。
![](http://osxdn70ll.bkt.clouddn.com/17-10-1/67915593.jpg)

分支就是某个对象的提交历史，当我们创建新分支时实际上是创建了一个新的分支。
比如创建一个testing分支：
```
git branch testing
```
实际上是创建了一个分支指针。
![](http://osxdn70ll.bkt.clouddn.com/17-10-1/49829366.jpg)

因为有多个分支指针，因此Git会保存一个名为HEAD的特殊指针，它始终指向正在工作的本地分支。当我们用`git branch`新建一个分支时，并不会自动切换到这个分支去，要切换当前工作的分支，还需要`git checkout`命令。
```
git checkout testing
```
![](http://osxdn70ll.bkt.clouddn.com/17-10-1/5774344.jpg)

这样当我们再提交一次后，会是下面的结果
![](http://osxdn70ll.bkt.clouddn.com/17-10-1/36775941.jpg)

master指针没有变化，testing分支向后移动了一个结点，而HEAD指针始终指向正在工作的分支。此时，如果我们切换到master分支会是下图的结果
```
git checkout master
```
![](http://osxdn70ll.bkt.clouddn.com/17-10-1/27857677.jpg)

这条命令做了两件事：
1. HEAD指针指向master分支。
2. 用master指针指向的快照内容替换工作目录中的文件。

如果我们此时做些修改再提交：
![](http://osxdn70ll.bkt.clouddn.com/17-10-1/53774059.jpg)
我们的项目会产生分叉，就像一棵横着的树。我们可以在不同分支里反复切换，并在适当的时候合并分支。而所有的这些工作，仅仅需要`branch`和`checkout`两条命令。
    
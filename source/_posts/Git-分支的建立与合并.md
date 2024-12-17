---
title: Git-分支的建立与合并
date: 2017-10-01T22:01:00
tags:
categories: Git
---

举一个实际工作中可能会遇到的分支建立与合并的例子：

1. 开发某个网站。

2. 为实现某个新的需求，创建一个分支。

3. 在这个分支上开展工作。

假设此时，你突然接到一个电话说有个很严重的问题需要紧急修补，那么可以按照下面的方式处理：



1. 返回到原先已经发布到生产服务器上的分支。

2. 为这次紧急修补建立一个新分支，并在其中修复问题。

3. 通过测试后，回到生产服务器所在的分支，将修补分支合并进来，然后再推送到生产服务器上。

4. 切换到之前实现新需求的分支，继续工作。



### 分支的建立与切换

假设当前的工作状态是下面的样子：

![](http://osxdn70ll.bkt.clouddn.com/17-10-1/77464811.jpg)



现在，假设我们要修补#53问题，为此我们新建一个iss53分支并切换到该分支上

```

git checkout -b iss53

```

这相当于执行了下面两条命令

```

$ git branch iss53

$ git checkout iss53

```



经过若干次更新后，状态如下图：

![](http://osxdn70ll.bkt.clouddn.com/17-10-1/98947538.jpg)



此时我们接到一个电话，有紧急漏洞需要修补。此时我们需要切换到`master`分支。

但在切换之前，确保你所做的更改都已经提交，即你的暂存区是干净的。

```

$ git checkout master

Switched to branch 'master'

```

切换到`master`分支后，工作目录中的内容应该是和解决#53问题之前一样。然后，我们新建一个紧急修补分支`hotfix`并在其中修补漏洞。

```

$ git checkout -b hotfix

Switched to a new branch 'hotfix'

$ vim index.html

$ git commit -a -m 'fixed the broken email address'

[hotfix 3a0874c] fixed the broken email address

 1 files changed, 1 deletion(-)

```

![](http://osxdn70ll.bkt.clouddn.com/17-10-1/84814125.jpg)



在我们确定修补成功后，需要回到master分支并把它合并起来然后发布到生产服务器。合并命令是`git merge`

```

$ git checkout master

$ git merge hotfix

Updating f42c576..3a0874c

Fast-forward

 README | 1 -

 1 file changed, 1 deletion(-)

```



合并时出现的`Fast-forward`提示是由于当前的`master`分支是要并入的`hotfix`分支的直接上游，Git只需要移动`master`指针，因为这种单线的历史分支不存在任何需要解决的冲突，所以称这种合并为`Fast-forward`快进。

合并之后master和hotfix指向同一位置

![](http://osxdn70ll.bkt.clouddn.com/17-10-1/94288487.jpg)



由于补丁已经完成，所以hotfix已经完成了历史使命，使用`git branch -d`删掉分支

```

$ git branch -d hotfix

Deleted branch hotfix (was 3a0874c).

```



然后回到iss53继续我们的工作，



```

$ git checkout iss53

Switched to branch 'iss53'

$ vim index.html

$ git commit -a -m 'finished the new footer [issue 53]'

[iss53 ad82d7a] finished the new footer [issue 53]

 1 file changed, 1 insertion(+)

```

![](http://osxdn70ll.bkt.clouddn.com/17-10-1/51034040.jpg)

在修补漏洞期间iss53并未受影响。



### 分支的合并

在问题 #53 相关的工作完成之后，可以合并回 master 分支。实际操作同前面合并 hotfix 分支差不多，只需回到 master 分支，运行 git merge 命令指定要合并进来的分支：

```

$ git checkout master

$ git merge iss53

Auto-merging README

Merge made by the 'recursive' strategy.

 README | 1 +

 1 file changed, 1 insertion(+)

```

请注意，这次合并操作的底层实现，并不同于之前 `hotfix` 的并入方式。因为这次你的开发历史是从更早的地方开始分叉的。由于当前 `master` 分支所指向的提交对象（C4）并不是 iss53 分支的直接祖先，Git 不得不进行一些额外处理。就此例而言，Git 会用两个分支的末端（C4 和 C5）以及它们的共同祖先（C2）进行一次简单的三方合并计算。

![](http://osxdn70ll.bkt.clouddn.com/17-10-1/63057729.jpg)



这次，Git 没有简单地把分支指针右移，而是对三方合并后的结果重新做一个新的快照，并自动创建一个指向它的提交对象（C6）。这个提交对象比较特殊，它有两个祖先（C4 和 C5）。



值得一提的是 Git 可以自己裁决哪个共同祖先才是最佳合并基础；这和 CVS 或 Subversion不同，它们需要开发者手工指定合并基础。所以此特性让 Git 的合并操作比其他系统都要简单不少。

![](http://osxdn70ll.bkt.clouddn.com/17-10-1/84509515.jpg)



合并后我们可以删除iss53分支

```

git branch -d iss53

```



### 遇到冲突时的合并

有时候合并操作并不会如此顺利。如果在不同的分支中都修改了同一个文件的同一部分，Git 就无法干净地把两者合到一起，逻辑上说，这种问题只能由人来裁决。如果你在解决问题 #53 的过程中修改了 hotfix 中修改的部分，将得到类似下面的结果：

```

$ git merge iss53

Auto-merging index.html

CONFLICT (content): Merge conflict in index.html

Automatic merge failed; fix conflicts and then commit the result.

```



上面的输出告诉你在`index.html`这个文件的合并有冲突，自动合并失败，你必须手动修复然后提交结果。



此时，用`git status`查看状态：

```

$ git status

On branch master

You have unmerged paths.

  (fix conflicts and run "git commit")



Unmerged paths:

  (use "git add <file>..." to mark resolution)



        both modified:      index.html



no changes added to commit (use "git add" and/or "git commit -a")

```



任何包含未解决冲突的文件都会以未合并（unmerged）的状态列出。Git 会在有冲突的文件里加入标准的冲突解决标记，可以通过它们来手工定位并解决这些冲突，并用`git add <file>`来标记解决。可以看到此文件包含类似下面这样的部分：



```

<<<<<<< HEAD

<div id="footer">contact : email.support@github.com</div>

=======

<div id="footer">

  please contact us at support@github.com

</div>

>>>>>>> iss53

```



可以看到 ======= 隔开的上半部分，是 HEAD（即 master 分支，在运行 merge 命令时所切换到的分支）中的内容，下半部分是在 iss53 分支中的内容。解决冲突的办法无非是二者选其一或者由你亲自整合到一起。比如你可以通过把这段内容替换为下面这样来解决：

```

<div id="footer">

please contact us at email.support@github.com

</div>

```

这个解决方案各采纳了两个分支中的一部分内容，而且我还删除了 <<<<<<<，======= 和 >>>>>>> 这些行。在解决了所有文件里的所有冲突后，运行 git add 将把它们标记为已解决状态。



再运行`git status`确定所有冲突都解决后就可以用`git commit`来完成合并后的提交。
    
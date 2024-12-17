---
title: git撤销操作
date: 2017-10-01T21:59:00
tags:
categories: Git
---

可以撤销是Git一项非常重要的功能，但需要注意的是，有些撤销操作是不可逆的，因此请务必谨慎小心。



### 修改最后一次提交

有时候我们提交后才发现有文件漏掉没有提交，或是想要提交信息，此时可以用`--amend`重新提交。

```

git commit --amend

```



此选项会覆盖上次提交，如果当前暂存区的快照和上次没有任何改动就相当于重新编辑提交信息。



如果上次提交时忘了暂存修改的文件，也可以先暂存再覆盖上次的提交。

```

git commit -m "initial commit"

git add forgotten_file

git commit --amend

```

上面三条命令只会产生一个提交，第二个提交会有机会重新编辑提交信息。



### 取消已经暂存的文件

有时候我们执行`git add .`命令后突然想把本次的修改分多次提交，可是已经添加到暂存区了，怎么撤销暂存其中的一个文件呢？

其实在我们用`git status`命令查看的时候就已经告诉我们该怎么办了。

```

$ git add .

$ git status

On branch master

Changes to be committed:

  (use "git reset HEAD <file>..." to unstage)



        modified:   README.txt

        modified:   benchmarks.rb

```

很清楚的提示`use git reset HEAD <file> ... to unstage`可以取消文件的暂存。我们可以试试取消`benchmarks.rb`的暂存。

```

$ git reset HEAD benchmarks.rb

Unstaged changes after reset:

M       benchmarks.rb

$ git status

On branch master

Changes to be committed:

  (use "git reset HEAD <file>..." to unstage)



        modified:   README.txt



Changes not staged for commit:

  (use "git add <file>..." to update what will be committed)

  (use "git checkout -- <file>..." to discard changes in working directory)



        modified:   benchmarks.rb

```



### 取消对文件的修改



有时我们改动了一些文件后发现完全没有必要，此时怎么恢复文件呢？`git status`同样提示了

```

Changes not staged for commit:

  (use "git add <file>..." to update what will be committed)

  (use "git checkout -- <file>..." to discard changes in working directory)



        modified:   benchmarks.rb

```

第三行告诉我们可以用`git checkout -- <file>`丢弃工作目录中的修改，回到之前暂存的状态。

```

$ git checkout -- benchmarks.rb

$ git status

On branch master

Changes to be committed:

  (use "git reset HEAD <file>..." to unstage)



        modified:   README.txt

```

可以看到，文件已经回到修改前的版本，但是需要注意的是，因为你没有提交过修改后的文件，对git来说就像它们从未存在过一样，因此这条命令没有办法撤销，你已经完全丢失了你的修改。
    
---
title: Git远程仓库
date: 2017-10-01T22:00:00
tags:
categories: Git
---

>要参与任何一个 Git 项目的协作，必须要了解该如何管理远程仓库。远程仓库是指托管在网络上的项目仓库，可能会有好多个，其中有些你只能读，另外有些可以写。同他人协作开发某个项目时，需要管理这些远程仓库，以便推送或拉取数据，分享各自的工作进展。 管理远程仓库的工作，包括添加远程库，移除废弃的远程库，管理各式远程库分支，定义是否跟踪这些分支，等等。



### 查看当前的远程仓库

可以使用`git remote`命令，它会列出每个仓库的简短的名字。

在clone一个仓库后，你至少可以看到一个名为origin的仓库，这是你所克隆的远程仓库的默认名。

也可以加上`-v`(--verbose),显示对应的url地址。



### 添加远程仓库

要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用，运行 `git remote add [shortname] [url]`



```

$ git remote

origin

$ git remote add pb git://github.com/paulboone/ticgit.git

$ git remote -v

origin    git://github.com/schacon/ticgit.git

pb    git://github.com/paulboone/ticgit.git

```



现在pb就指代远程仓库的地址。



### 从远程仓库抓取更新

可以使用`git fetch [remote-name]`获取远程仓库的内容，它只会获取仓库的最新版本到本地，你需要手动合并。

```

$ git fetch pb

remote: Counting objects: 58, done.

remote: Compressing objects: 100% (41/41), done.

remote: Total 44 (delta 24), reused 1 (delta 0)

Unpacking objects: 100% (44/44), done.

From git://github.com/paulboone/ticgit

 * [new branch]      master     -> pb/master

 * [new branch]      ticgit     -> pb/ticgit

```

现在我们已经得到pb的主干分支，对应的名字是`pb/master`，我们可以将它合并到自己的某个分支上或是切换到这个分支看看有什么新的有趣的更新。







### 推送数据到远程仓库

项目进行到一个阶段，要同别人分享目前的成果，可以将本地仓库中的数据推送到远程仓库。实现这个任务的命令很简单： `git push [remote-name] [branch-name]`。如果要把本地的 master 分支推送到 origin 服务器上（再次说明下，克隆操作会自动使用默认的 master 和 origin 名字），可以运行下面的命令：

```

$ git push origin master

```

只有在所克隆的服务器上有写权限，且同一时刻没有其他人在推数据，这条命令才会如期完成任务。如果在你推数据前，已经有其他人推送了若干更新，那你的推送操作就会被驳回。你必须先把他们的更新抓取到本地，合并到自己的项目中，然后才可以再次推送。



### 查看远程仓库信息

我们可以通过命令 `git remote show [remote-name]`查看某个远程仓库的详细信息，比如要看所克隆的 origin 仓库，可以运行：

```

$ git remote show origin

* remote origin

  URL: git://github.com/schacon/ticgit.git

  Remote branch merged with 'git pull' while on branch master

    master

  Tracked remote branches

    master

    ticgit

```

除了对应的克隆地址外，它还给出了许多额外的信息。它友善地告诉你如果是在 master 分支，就可以用 git pull 命令抓取数据合并到本地。

最后两行还列出了所有处于跟踪状态中的远端分支。



上面的例子非常简单，而随着使用 Git 的深入，`git remote show` 给出的信息可能会像这样：

```

$ git remote show origin

* remote origin

  URL: git@github.com:defunkt/github.git

  Remote branch merged with 'git pull' while on branch issues

    issues

  Remote branch merged with 'git pull' while on branch master

    master

  New remote branches (next fetch will store in remotes/origin)

    caching

  Stale tracking branches (use 'git remote prune')

    libwalker

    walker2

  Tracked remote branches

    acl

    apiv2

    dashboard2

    issues

    master

    postgres

  Local branch pushed with 'git push'

    master:master

```



最后两行告诉我们，运行 `git push` 时缺省推送的分支是什么。

第六行还显示了有哪些远端分支还没有同步到本地且告诉我们在下一次执行`fetch`时会保存到本地的remotes/origin

`Stale tracking branches`下面的两个分支是已同步到本地的远端分支但在远端服务器上已被删除。

以及运行`git pull` 时将自动合并的分支。



### 删除和重命名远程仓库

可以用 `git remote rename` 命令修改某个远程仓库在本地的简称，比如想把 pb 改成 paul，可以这么运行：

```

$ git remote rename pb paul

$ git remote

origin

paul

```

注意，对远程仓库的重命名，也会使对应的分支名称发生变化，原来的 pb/master 分支现在成了 paul/master。



碰到远端仓库服务器迁移，或者原来的克隆镜像不再使用，又或者某个参与者不再贡献代码，那么需要移除对应的远端仓库，可以运行 `git remote rm` 命令：

```

$ git remote rm paul

$ git remote

origin

```
    
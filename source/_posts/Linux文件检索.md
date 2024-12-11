---
title: Linux文件检索
date: 2017-12-11T21:13:00
tags:
categories:
---

---
title: Linux文件检索
date: 2017-12-11 19:03:01
tags: linux
categories: linux 

---


### whereis
只要执行
```
whereis ls
```
就可以搜索到“ls”命令和它的帮助文档的位置。
但它只能搜索man手册、二进制文件和源代码文件。

### which
which是一个比whereis更简单的命令，通常用来判断是否安装某个软件，它只在$PATH环境变量中指定的路径中搜索
```
$ which pacman
/usr/bin/pacman
```

### find

最强大的搜索命令
find []path...] [expression]

-print  输出检索结果
-exec   对匹配的文件执行shell命令

比如要搜索3天前当天发生变化的所有文件
```
$ find / -mtime 3
```

搜索3天内发生变化的所有文件
```
$ find / -mtime -3
```

搜索3天以前发生变化的所有文件
```
$ find / -mtime +3
```

类似的查找条件还有

| 参数                 | 作用                                                       |
| ----------------     | ----------------                                           |
| -user username       | 按所有者名查找                                             |
| -group gname         | 文件属于gname                                              |
| -nouser              | 文件没有有效的所有者，即文件的
| -name filename       | 按文件名检索                                               |
| -atime n             | 最近访问时间（天）                                         |
| -amin n              | 最近访问时间（分钟）                                       |
| -anewer file         | 最近访问时间（比file修改时间要晚）                         |
| -ctime n             | 按文件创建时间查找                                         |
| -mtime n             | 按文件修改时间查找                                         |
| -empty               | 空文件或空目录                                             |
| -size n              | 按照文件大小查找                                           |
| -type b/c/d/p/f/l/s/ | 查找块文件/字符文件/目录/命名管道/普通文件/符号链接/套接字 |
| -maxdepth n          | 最大查找深度                                               |
| -mindepth n          | 最小查找深度                                               |
| -prune pathname      | 忽略一个目录                                               |

例如要在`test`目录下查找不在`test4`目录下的文件

```bash
$ find test -path "test/test4" -prune -o -print
```

-o 可以理解为`or`
上述表达式的伪码表述是

```bash
if -path == "test/test4"
	-prune
else
	-print
```

-exec是要对找到的文件执行的动作
```bash
find . -mtime -1 -exec ls -l {} \;
```
上述命令列出当前目录下一天内变化的文件的详细信息，`ls -l`很容易理解，`{}`是一个占位符，在find执行过程中不断替换为当前找到的文件,后面的`；`是`exec`结束的标记，而`\` 是转义字符。
    
---
title: 把vim插入状态的光标改为竖线
date: 2017-10-06T02:50:00
tags:
categories:
---

和终端有关系，如果是Konsole的终端，把下面两行加到`.vimrc`文件里就可以


```
let &t_SI = "\<Esc>]50;CursorShape=1\x7"
let &t_EI = "\<Esc>]50;CursorShape=0\x7"
```

如果不是Konsole的终端也可以安装Gvim,用Gvim打开默认插入状态光标就是竖线
    
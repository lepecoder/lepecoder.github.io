---
title: 去掉vim的BELL提示音
date: 2017-11-09T15:32:00
tags: vim
categories: 开发工具
---


在vi/vim中使用

```

:set noeb

```

意思是`noerrorbells`,意思是当出现错误时没有提示

```

:set vb

```

意思是`visualbell`可是的响铃，意思是用屏幕的闪烁代替响铃。



但在gvim中，`noeb`是不生效的，可以代替为

```

:set vb t_vb=

```

效果和`set vb`相同，且在vi/vim/gvim中都有效。
    
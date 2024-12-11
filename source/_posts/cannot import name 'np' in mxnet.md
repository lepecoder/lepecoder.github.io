---
title: cannot import name 'np' in mxnet
date: 2020-07-17T20:34:00
tags:
categories:
---

### Mxnet 不能引入np和npx

[ImportError: cannot import name 'np'](https://github.com/apache/incubator-mxnet/issues/15681#)

在github上已有这个问题的issue，是因为numpy分支和master分支还没有合并。不过现在似乎已经修复了这个bug，但是在Windows上你可以通过`pip install mxnet -–pre` to install MXNet with numpy-like API

    
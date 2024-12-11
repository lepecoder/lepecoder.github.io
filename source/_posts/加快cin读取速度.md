---
title: 加快cin读取速度
date: 2017-09-09T21:41:00
tags:
categories:
---

cin在读取大量数据时会比C里的scanf慢很多,但这并不是cin"无能",而是C++为了兼容C,对cin做了scanf的同步,只要关闭这个同步,cin就会有不弱于scanf的速度.

```
std::ios::sync_with_stdio(false);
```
    
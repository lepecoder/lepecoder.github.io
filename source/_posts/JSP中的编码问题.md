---
title: JSP中的编码问题
date: 2017-10-15T14:10:00
tags:
categories:
---

### JSP文件的编码
```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page pageEncoding="UTF-8" %>
```

`contentType`是通知浏览器接收到的是html网页文件，采用字符集`UTF-8`解析。
`pageEncoding="UTF-8`是写的jsp文件本身是`utf-8`编码的。

因为jsp文件要经过两次编码，第一次是jsp编译成.java，他会根据pageEncoding的设定读取jsp文件。

第二阶段是由JAVAC的JAVA源码至java byteCode的编译，不论JSP编写时候用的是什么编码方案，经过这个阶段的结果全部是UTF-8的encoding的java源码。

JAVAC用UTF-8的encoding读取java源码，编译成UTF-8 encoding的二进制码（即.class），这是JVM对常数字串在二进制码（java encoding）内表达的规范。

然后有Tomcat载入和执行阶段二的来的JAVA二进制码，输出的结果，也就是在客户端见到的，这时隐藏在`contentType`参数就发挥了功效。

### 表单传值的编码

#### post提交
在获取参数前，先设置`request`的编码
```jsp
request.setCharacterEncoding("utf-8");
String username = request.getParameter("username");
```
    
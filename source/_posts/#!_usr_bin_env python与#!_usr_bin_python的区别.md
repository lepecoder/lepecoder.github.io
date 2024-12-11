---
title: #!/usr/bin/env python与#!/usr/bin/python的区别
date: 2017-10-23T20:43:00
tags:
categories:
---

我们看Python文件的时候经常看到有
```py
#!/usr/bin/python
```
它只在Linux系统下生效，意思是当作为可执行文件运行时调用的解释器的位置
如果你用`python a.py`来运行就是手动指定了解释器，这一行就不会生效了，但如果你为它添加了可执行权限，就不同了。

```bash
chmod +x a.py
./a.py
```

此时就需要文件指明解释器的位置。

```py
#!/usr/bin/python
```
上面代码的意思是调用`/usr/bin/`下的Python来作为解释程序，同样，你也可以写`#!/usr/bin/python3`或`#!/usr/bin/python2`，但如果不是默认安装位置这个地方可能就找不到，那么文件就是报错，所以就有了另一种写法
```py
#!/usr/bin/env python
```
这表示调用系统环境变量里的`Python`,也就是和你在终端输入`python`调用解释器是一样的，只要你在终端可以运行`python`，上面的命令就可以找到。
    
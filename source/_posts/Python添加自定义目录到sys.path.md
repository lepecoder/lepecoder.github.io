---
title: Python添加自定义目录到sys.path
date: 2020-03-28T21:13:00
tags:
categories:
---

### 

Python导入自定义模块时会从自动从sys.path里查找模块位置，许多默认位置比如`Python37\site-packages`会自动加到`sys.path`，当前脚本所在位置也会自动添加到`sys.path`里，但是如果你需要导入的模块在当前脚本的上一层就需要手动将目录添加到`sys.path`，比如有如下目录结构

```
D:.
├─agents
│  ├─actor_critic_agents
│  ├─DQN_agents
│  ├─hierarchical_agents
│  └─policy_gradient_agents
├─results
   └─data_and_graphs
```



`results`里的脚本需要调用agents里的模块，就需要手动将agents的父目录加到`sys.path`里。

```python
import os
import sys

Updir = "\\".join(__file__.split('/')[0:-2])
sys.path.append(Updir)
print(sys.path)
```

    
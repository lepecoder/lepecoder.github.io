---
title: Python闭包特性
date: 2024-12-21 19:25:42
tags: 闭包
categories: Python
---


## 闭包定义

当在函数内部再定义一个函数时，内部函数会持有外部函数变量的引用，即使外部函数已经执行完毕，这个特性被称为闭包。主要有以下特点：

1. 函数嵌套： 在函数内部定义另一个函数。
2. 变量引用： 内部函数引用外部函数的非局部变量。
3. 持久性： 外部函数返回内部函数，外部函数的变量仍然被保存，而不是被销毁。



```python
def outer_function():
    x = 10
    def inner_function(y):
        return x + y  # 内部函数引用了外部函数的变量 x
    return inner_function

# 使用闭包
closure = outer_function()
print(closure(5))   # 输出 15

# 也可以返回 lambda 表达式
def outer_function():
    x = 10
    return lambda y: x + y

closure = outer_function()
print(closure(8))   # 输出 18

```


## 闭包的使用

#### 封装带有状态的回调函数
上面的例子中，outer_function 是外部函数，inner_function 和 lambda 是内部函数，当外部函数其变量 x 仍被返回的内部函数引用。

利用闭包特性我们可以使得函数记住某些结果，用于延迟执行。比如在有一个列表，点击不同item弹出不同的消息，通常我们会绑定同一个点击事件，然后在点击事件里获取item.id，触发不同的操作。但根据闭包特性，我们可以直接绑定一个lambda函数，并获取外部变量作为参数。

查看如下例子，click_func类似于点击事件，根据不同的外部item传入不同的参数


```python
def outer_function():
    items = [1,2,3,4,5]
    click_func = []
    for item in items:
        click_func.append(lambda :print(item, end=' '))
    return click_func

click_func = outer_function()
for func in click_func:
    func()  # 输出 5 5 5 5 5

```

你会发现上面的例子打印结果并不是 1 2 3 4 5 而是 5 5 5 5 5，这是因为闭包获取的是变量的引用而不是拷贝，当内部函数执行时才获取引用的值，而 item 变量在循环的最后被赋值为5。因此如果你想要保存状态，使用“=”拷贝外部变量

```python
click_func.append(lambda x=item:print(x, end=' '))

# 如果是普通 Python 函数
def inner_func(x=item):
    print(x, end=' ')
```

#### 闭包内修改外部变量的值

闭包只持有外部变量的引用，而且是只读的，因此他是个纯右值，不能出现在等号左边。
使用 nonlocal 关键字标记变量在函数外部，可以在闭包内修改外部变量。


```python
def outer():
    x = 0
    def inner():
        nonlocal x
        x += 1
        return x
    return inner
inner = outer()
print(inner()) # 1
print(inner()) # 2
```


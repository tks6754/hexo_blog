---
title: python命名空间是个啥？
date: 2016-12-22 21:46:10
category:
    - python
tags:
    - python
    - 命名空间
---

刚学py，本来很奇怪，凭什么py的类啊、函数啊什么的，可以有同名的变量名。最让我奇怪的是：它们竟然还是不同的变量！！这让写Java的我觉得有点不可思议，要知道，在Java中同名变量是会冲突的。那啥？这就涉及到py的命名空间了。Python的命名空间是个啥东西？

## 命名空间(Namespace)的概念

> 命名空间(Namespace)是一个字典，key是对象名，value是对象。就是一个“名字”到“对象”的映射。

在py中，一切都是对象，这里的一切包括基本数据类型、函数、类。确定一个对象需要id（可以理解为类似内存地址）、类型、值，但是这不方便使用，为此我们另外一般会给对象“标识符”。
> 标识符(identifer)是对象的名字，可以是变量名、常量明、函数名、类名

```python
>>> a = 9
>>> id(9)
36970648
>>> b = a
>>> id(b)
36970648
>>> b = 10
>>> b
10
>>> a
9
```
py中的赋值都是把对象绑定到名字，对象本身并没有变化。例子后面把b绑定到“10”，a还是原来的值（这里需要注意，基本数据类型和其他“引用”类型的内存模型有不同，这里不讨论）。

## 作用域(scope)与LEGB法则
py中有很多命名空间，不同“地方”有不同的不同的命名空间，这些命名空间事通过作用域来区分的，不同作用域内的命名空间相互独立存在。
> 作用域(scope)是一个文本域的概念。说白了，就是在py代码中位置区域

py中作用域有四种，就是常说的LEGB啦。
- 局部命名空间（locals）：函数内部的名字空间，一般包括函数的局部变量以及形式参数
- 闭包命名空间（enclosing function）：在嵌套函数中外部函数的名字空间。常见于闭包
- 全局命名空间（globals）：读入一个模块（也即一个.py文档）后产生的名称空间。
- 内建命名空间（\_\_builtins\_\_）：内置模块空间，Python 解释器启动时自动载入__built__模块后所形成的名称空间

当程序引用某个变量的名字时，就会从当前名字空间开始搜索。搜索顺序规则便是: LEGB.
> locals（内层locals->外层locals） -> enclosing function -> globals -> \_\_builtins\_\_

```python
#!/usr/bin/env python
# encoding: utf-8


def func1():
    x = 1
    print 'func1_head globals:', globals()
    print 'func1_head locals:', locals()

    def func2():
        a = 1
        print 'fun2_head lacals:', locals()
        print 'fun2_head globals:', globals()
        a += x
        print 'fun2_end locals:', locals()
        print 'fun2_head globals:', globals()

    func2()
    print 'after_end func1:', locals()
    print 'func1_end globals:', globals()

if __name__ == '__main__':
    func1()
```

```
# 结果
➜  my-python python legb.py
func1_head globals: {'func1': <function func1 at 0x7f7721b295f0>, '__builtins__': <module '__builtin__' (built-in)>, '__file__': 'legb.py', '__package__': None, '__name__': '__main__', '__doc__': None}
func1_head locals: {'x': 1}
fun2_head lacals: {'a': 1, 'x': 1}
fun2_head globals: {'func1': <function func1 at 0x7f7721b295f0>, '__builtins__': <module '__builtin__' (built-in)>, '__file__': 'legb.py', '__package__': None, '__name__': '__main__', '__doc__': None}
fun2_end locals: {'a': 2, 'x': 1}
fun2_head globals: {'func1': <function func1 at 0x7f7721b295f0>, '__builtins__': <module '__builtin__' (built-in)>, '__file__': 'legb.py', '__package__': None, '__name__': '__main__', '__doc__': None}
after_end func1: {'x': 1, 'func2': <function func2 at 0x7f7721b296e0>}
func1_end globals: {'func1': <function func1 at 0x7f7721b295f0>, '__builtins__': <module '__builtin__' (built-in)>, '__file__': 'legb.py', '__package__': None, '__name__': '__main__', '__doc__': None}
```
主要关注局部命名空间，每个函数都有自己的局部命名空间，当内层函数需要某个引用时，就会一层一层的向外层查找，这个向外层查找的顺序就是LEGB法则。例子中，func2中需要用到x，命名空间会自动向外找到x并加入到命名空间。如果没找到就会报“NameError”的错误。

## 生命周期
每个命名空间都有自己的生命周期，它们并不是同时生成的。

- \_\_builtins\_\_: 在python解释器启动的时候，便已经创建,直到退出
- globals: 在模块定义被读入时创建，通常也一直保存到解释器退出。
- locals : 在函数调用时创建, 直到函数返回，或者抛出异常之后，销毁。比较特殊的，递归函数每一次均有自己的名字空间。

一个改造的例子
```python
#!/usr/bin/env python
# encoding: utf-8


def func():
    if False:
        x = 10 #该语句永远不执行
    print 'func locals', locals()
    #print x

if __name__ == '__main__':
    func()
```

结果
```
➜  my-python python legb_life.py
func locals {}

# 取消print打印
➜  my-python python legb_life.py
func locals {}
Traceback (most recent call last):
  File "legb_life.py", line 12, in <module>
    func()
  File "legb_life.py", line 9, in func
    print x
UnboundLocalError: local variable 'x' referenced before assignment
```
可以看到，函数func的局部命名空间中并没有x。在打印x时会报错“UnboundLocalError”，这是由于在编译期间，解释器就将“名字”加入到局部命名空间，但是到了执行时，发现x没有值，就会报错。

## 赋值操作的根本
> 赋值操作的本质是修改命名空间，而不是修改对象

```python
a = 1
b = []
b.append(1)
```
把a加入到命名空间，并将它指向一个值为1的整数对象；把b加入命名空间，并将它指向一个值为空的list对象，而append操作只是改变list对象的值，并没有改变它的内存地址，没有涉及到命名空间变化。要区分这样对象的区别。

## 小结一下
这一番下来，我就可以理解为什么在一个函数或类中可以出现多个同名的变量，并且，还了解了对象是怎么被找到的。

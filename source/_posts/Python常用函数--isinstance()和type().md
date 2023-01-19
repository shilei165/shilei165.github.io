---
title: Python常用函数--isinstance()和type()
tags: 
  - Python
  - 常用函数
  - 编程
categories:
  - 技术杂谈
  - Python
date: 2022-07-20 12:06:54
---
在这篇文章中，我将给大家分享如何使用isinstance()和type()两个Python常用函数，并且分析它们之间的区别。
<!--more-->
***
# **isinstance()函数**
isinstance()函数用来判断一个对象是否是一个已知类型。其语法如下：
```
isinstance(object, classinfo)
```
其中object为实例对象，也就是我们需要判断的对象。classinfo为类的名称，可以是直接类或间接类、基本类型或者由他们组成的元组。
如果对象的类型与classinfo的类型相同，则返回True，否则返回False。
具体举例如下：
```
>>> a = 'good'
>>> isinstance (a, int)
False
>>> isinstance (a, str)
True
>>> isinstance (a,(str, int, list))
True
```
# **type()函数**
type()函数可以返回对象的类型，具体语法如下：
```
type(object)
type(name, bases, dict)
```
一个参数返回对象类型, 三个参数则返回新的类型对象。我们这里重点讨论一个参数的情况。举例如下：
```
>>> type(123)
<type 'int'>

>>> type('good')
<type 'str'>

>>> type([2])
<type 'list'>

>>> type({name:'Leo'})
<type 'dict'>
```
我们会发现，type()函数与isinstance()函数类似，都可以返回对象的类型。但他们在是否考虑类的继承关系有很大区别，具体区别如下：
+ type()不考虑继承关系，也就是说不会认为子类是一种父类类型。
+ isinstance()考虑继承关系，也就是说会认为子类是一种父类类型。

比如：
```
class A:
    pass
 
class B(A):
    pass
 
isinstance(A(), A)    # returns True
type(A()) == A        # returns True
isinstance(B(), A)    # returns True
type(B()) == A        # returns False
```

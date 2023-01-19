---
title: Python教程--条件判断(if, elif, else)
tags:
  - python
  - 教程
  - 编程
  - 计算机
id: '871'
categories:
  - 技术杂谈
  - Python
date: 2021-01-12 20:37:00
---

**这篇文章将会分享关于python中关于条件判断 (if, elif, else)** **的一些相关知识。**
<!-- more -->
条件判断是通过一条或多条判断语句的执行结果（True或者False）来决定执行的代码块。

# **一、if, elif, else 条件判断**

在Python语法中，使用if、elif和else三个关键字来进行条件判断。我们先来看一个例子，来说明条件判断是怎么一回事：

```
language = 'Python'
if language == 'Python':
print('Language is Python')
else:
print('No match')
```

程序运行结果如下：

```
Language is Python
```

这就是一条最为简单的条件判断语句。首先，if语句会对language和Python进行比较。判断他们相同，就会输出'Language is Python'。如果不相同，则会输出'No match'。

如果有两条以上的条件需要进行判读，我们就需要用到elif语句。我们再写一个例子，来看elif是如何使用的：

```
language = 'Java'
if language == 'Python':
print('Language is Python')
elif language =='Java': 
print('Language is Java')
else:
print('No match')
```

运行程序，结果如下：

```
Language is Java
```

请注意，因为if, elif, 和else语句在python中已经足够使用，所以**在Python中没有switch – case语句**。

# **二、Python中的逻辑判断**

逻辑判断对于条件判断语句来说是非常重要的。在Python教程--数字(number)那一篇我们已经提到了四种逻辑判断：

相等：==；不等：!=；大于：>；小于： <；大于等于： >=；小于等于 <=；

还有一种逻辑判断我们之前没有提到过，那就是is判断。is判断可能跟==有点类似，所以我们举个例子来简单说明一下他们之间的区别：

```
a = [1,2,3]
b = [1,2,3]

print(a==b)
print(a is b)
```

如果我们运行这个程序会得到如下结果：

```
True
False
```

我们会发现，列表a等于列表b，但a并不是b。这是因为a和b虽然内容一样，但他们却是不同的列表，占用了不同的内存空间。就像两个双胞胎，虽然他们长得一样，但本质上确实两个不同的人。如果我们运行以下代码，输出他们的id就会发现他们之间的不同：

```
a = [1,2,3]
b = [1,2,3]
print(id(a))
print(id(b))
```

结果如下：

```
2379654490944
2379654511872
```

我们会发现两者含有不同的id，所以is逻辑判断本质上是来比较他们id是否相同。我们再来举一个例子，来看看他们在什么情况下是相同的：

```
a = [1,2,3]
b = a
print(a is b)
print(id(a))
print(id(b))
```

运行结果如下：

```
True
2379654813376
2379654813376
```

我们这次就会发现a和b是相同的，因为他们的id相同，也就是说他们占用了相同的内存空间。

# **三、True和False的定义**

Python中的True和False的定义，在不同版本的Python中是这样定义的：

*   Python 2：None, 0, 和空字符串都被算作 False，其他的均为 True
*   Python 3：None，0，空字符串，空列表，空字典都算是False，所有其他值都是True

因为我所用的是python 3的版本，所以就以我用的python 3为例，来举例说明一下：

```
a = 0
if a:
print(11)
else:
print(22)
```

运行结果如下：

```
22
```

如果把0换成None, ( ), \[ \], { }也会得到相同的结果。我们再来举一个条件判断为True的例子：

```
a = 2
if a:
print(11)
else:
print(22)
```

运行结果为：

```
11
```

这里可以把2可以换为任意不为0的数字，或者是非空的列表、字典、字符串（None除外）。

* * *


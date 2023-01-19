---
title: Python教程--元组(Tuple)和集合(set)
tags:
  - python
  - 元组和集合
  - 编程
  - 计算机
id: '834'
categories:
  - 技术杂谈
  - Python
date: 2021-01-10 17:01:33
---

**这篇文章将会分享关于python中关于元组(Tuple)和集合(set)的一些相关知识**。
<!-- more -->
在上一篇文章中我们分享了关于列表的一些相关知识。如果掌握了上一篇讲的关于列表的知识，那么这一章节的内容就会非常容易理解。

# **一、元组（Tuple）**

先举一个例子，来说明什么是元组，它和列表有什么联系和区别？先看一个列表的例子

```
list_1 = ['History', 'Math', 'Physics', 'CompSci']
list_2 = list_1

print(list_1)
print(list_2)

list_1[0] = 'Art'

print(list_1)
print(list_2)
```

运行结果如下：

```
['History', 'Math', 'Physics', 'CompSci']
['History', 'Math', 'Physics', 'CompSci']
['Art', 'Math', 'Physics', 'CompSci']
['Art', 'Math', 'Physics', 'CompSci']
```

我们发现，列表中的元素是可以进行变更的。元组跟列表看起啦非常相似，但是是用圆括号来括起来的。除了这点不同，还有别的区别吗？我们还同样的例子，再来用元组运行一下：

```
tuple_1 = ('History', 'Math', 'Physics', 'CompSci')
tuple_2 = tuple_1

print(tuple_1)
print(tuple_2)

tuple_1[0] = 'Art'
print(tuple_1)
print(tuple_2)
```

运行结果如下：

```
('History', 'Math', 'Physics', 'CompSci')Traceback (most recent call last):
  File "\\DESKTOP-RGP481O\Users\Leilei\Python\Lists_Tuples_Sets_T4-2.py", line 20, in <module>

('History', 'Math', 'Physics', 'CompSci')
    tuple_1[0] = 'Art'
TypeError: 'tuple' object does not support item assignment
[Finished in 0.3s]
```

我们发现运行结果出现了错误提示。提示“TypeError: 'tuple' object does not support item assignment”。这就揭示了**元组与列表最大的一个不同点：元组不可变更，而列表可以**。

# **二、集合(Set)**

除了列表和元组，python中还有另一个非常相似的概念-集合(set)。从外观来看，集合是用大括号来将里面的不同元素括起来。除了外观的不同之外，集合跟但列表和元组还有一个很大不同：**集合(set)中的元素没有顺序，而列表和元组中的元素有顺序**。我们来看下面这个例子：

```
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}
print(cs_courses)
print('Math' in cs_courses)
```

运行结果如下：

```
{'Physics', 'History', 'CompSci', 'Math'}
True
```

我们会发现，输出结果中的元素顺序跟我们输入的并不相同。如果多次运行，我们都会得到不一样的结果。所以集合更在意里面有什么元素，而不在乎他们的顺序是什么。**对于验证某个某个元素是否在某个集合中，我们采用集合也会更为高效**（虽然列表和元组也可以同样做到这一点）。

另外，要说的一点是，**集合中的元素不可以重复。如果有重复元素，集合将会将多余重复元素删除**。比如我们下面这个例子：

```
cs_courses = {'History', 'Math', 'Physics', 'CompSci'}
art_courses = {'History', 'Math', 'Art', 'Design'}
print(cs_courses.intersection(art_courses)) # show intersection between two sets
print(cs_courses.difference(art_courses)) # show difference between two sets
print(cs_courses.union(art_courses)) # combine two sets
```

输出结果如下：

```
{'History', 'Math'}
{'CompSci', 'Physics'}
{'History', 'Design', 'Math', 'Physics', 'Art', 'CompSci'}
```

我们会发现，虽然两个集合中都含有History和Math元素，但在最后的合并结果中，这些元素都只出现了一次。另外，我们通过这个例子，也学会了怎么使用interesection( )、difference( )和union( )来对不同集合中的元素寻找交集、补集和并集。

# **三、空列表(Empty List)、空元组(Empty Tuple)和空集合(Empty Set)**

对于怎么表示空的列表、元组和集合，这里有一点需要十分注意：空的集合不可用{ }来表示，因为空的大括号表示一个空的字典。字典我们将在下一章中学到，这里先提前提醒一下。

```
# Empty list
empty_list = []
empty_list = list()

# Empty tuples
empty_tuple = ()
empty_tuple = tuple()

# Empty sets
empty_set = {} # 注意：这不是空集合的正确表示方式，这里的大括号代表一个空的字典。
empty_set = set()
```

* * *


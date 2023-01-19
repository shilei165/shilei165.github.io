---
title: Python教程--推导式(Comperhension)
tags:
  - comperhension
  - python
  - 推导式
  - 教程
  - 编程
id: '884'
categories:
  - 技术杂谈
  - Python
date: 2021-01-14 13:29:30
---

**这篇文章中将会分享python中关于推导式(comperhension)相关的一些知识。**
<!-- more -->
Comprehension是Python语言的一个特性，有些略微不太好翻译。Comprehension字面翻译过来，是“理解，包容”的含义，有人翻译为生成器，解析器，或者推导式。我们可以理解为 Comprehension是用来生产列表、元组、集合以及字典的工具。

# **一、列表推导式 (List Comperhension)**

我们先看一个例子，如果我们想要输出列表中的每一个元素，我们首先想到的可能是写出一个如下的for循环代码：

```
nums = [1,2,3,4,5,6,7,8,9,10]
my_list = []
for n in nums:
my_list.append(n)
print (my_list)
```

这么做没有问题，但可能有一点太复杂。我们再来看看List Comperhension是怎么做的：

```
nums = [1,2,3,4,5,6,7,8,9,10]
my_list = [n for n in nums]
print(my_list)
```

我们会发现，它们运行之后会得到同样的结果。但是，list comperhension就要明显简洁的多。再来举一个稍微复杂一点的例子，如果我们想要输出列表中的每一个元素的平方，我们如果用for循环会写出如下代码：

```
nums = [1,2,3,4,5,6,7,8,9,10]
my_list = []
for n in nums:
my_list.append(n*n)
print (my_list)
```

如果用List Comperhension的方法呢？代码如下：

```
nums = [1,2,3,4,5,6,7,8,9,10]
my_list = [n*n for n in nums]
print(my_list)
```

有没有很简洁？接着，我们想要只输出偶数，如果用for循环，将会是如下代码：

```
nums = [1,2,3,4,5,6,7,8,9,10]
my_list = []
for n in nums:
if n%2 == 0:
my_list.append(n)
print(my_list)
```

如果用List Comperhension的方法，则会非常简洁：

```
nums = [1,2,3,4,5,6,7,8,9,10]
my_list = [n for n in nums if n%2 == 0]
print(my_list)
```

接下来，我们想要输出一个嵌套的循环。输出字母'abcd'和数字'0123'的所有组合，用for循环，将会是如下代码：

```
my_list = []
for letter in 'abcd':
for num in '0123':
my_list.append((letter,num))
print(my_list)
```

用List Comperhension的方法也可以同样完成这个任务：

```
my_list = [(letter,num) for letter in 'abcd' for num in range(4)]
print(my_list)
```

# **二、字典推导式 (Dictionary Comperhension)**

字典也同样可以有推导式。举一个例子，我们有如下两个列表，一个是名字(names)，一个是英雄名字(heros)，我们希望得到一个{'name':'hero'}样式的字典。用for 循环需要如下代码：

```
names = ['Bruce','Clark','Peter','Logan','Wade']
heros = ['Batman','Superman','Spiderman','Wolverine','Deadpool']
my_dict = {}
for name, hero in zip(names, heros):
my_dict[name] = hero
print(my_dict)
```

这里的zip是用于将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表。运行以上程序结果如下：

```
{'Bruce': 'Batman', 'Clark': 'Superman', 'Peter': 'Spiderman', 'Logan': 'Wolverine', 'Wade': 'Deadpool'}
```

如果用Dictionary Comperhension的方法，我们可以用如下代码得到同样的结果：

```
my_dict = {name: hero for name, hero in zip(names, heros)}
print(my_dict)
```

如果我们想要添加某个筛选条件，也可以同样的在后面加上if判断语句。比如运行如下代码，将会把 'Peter': 'Spiderman' 从结果中去除出去。

```
my_dict = {name: hero for name, hero in zip(names, heros) if name != 'Peter'}
print(my_dict)
```

# **三、集合推导式 (Set Comperhension)**

同样的，集合也可以用同样的推导式来使得代码更加简洁。因为集合中的元素不可以重复，我们可以利用这个特性来去除列表中的重复元素。用我们前面学过的for循环将会是如下代码：

```
nums = [1,1,2,1,3,4,3,4,5,5,6,7,8,7,9,9]
my_set = set()
for n in nums:
my_set.add(n)
print(my_set)
```

运行结果如下：

```
{1, 2, 3, 4, 5, 6, 7, 8, 9}
```

如果用Set Comperhension的方法，我们可以用如下代码得到同样的结果：

```
nums = [1,1,2,1,3,4,3,4,5,5,6,7,8,7,9,9]
my_set = set(n for n in nums)
print(my_set)
```

* * *


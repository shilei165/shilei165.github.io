---
title: Python教程--切割列表和字符串
tags:
  - python
  - 切割列表和字符串
  - 编程
  - 计算机
id: '882'
categories:
  - 技术杂谈
  - Python
date: 2021-01-14 12:36:23
---

**这篇文章中将会分享python中关于切割列表和字符串(slicing lists and string) 相关的一些知识。**
<!-- more -->
我们有时候不希望输出整个列表和字符串而只是希望输出其中某一部分，这时候应该怎么办呢？今天这篇文章，我们就来学习一下在python中应当怎样分割列表和字符串。

# **一、切割列表**

切割列表的格式为：list\[start:end:step\]。start是输出开始的指针位置，end是输出结束的指针位置，step是输出的间隔数。如果只需要输出某个元素，则只需要在列表后面加上相应的指针位置。先看下面这个例子：

```
my_list = [0,1,2,3,4,5,6,7,8,9]
print (my_list[3])
print (my_list[-1])
print (my_list[0:4])
print (my_list[0:5:2])
print (my_list[2:-1:2])
print (my_list[::-1])
```

输出结果分别为：

```
3
9
[0, 1, 2, 3]
[0, 2, 4]
[2, 4, 6, 8]
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

如果我们在start, end, step的位置不添加任何数值，则表示默认。他们的默认值分别为第一个指针位置(0)，最后一个指针位置(-1)，还有步数1。

# **二、切割字符串**

用同样的方法我们也可以来切割字符串，比如下面的字符串：

```
sample_url = 'http://shileilei.com'
print (sample_url)
```

我们如果直接运行，就会把我们的[http://shileilei.com](http://shileilei.com)的网站给输出出来。但我们怎么才能将它们倒着输出呢？我们可以用前面学到的知识，来这样做：

```
sample_url = 'http://shileilei.com'
print(sample_url[::-1])
```

输出结果为：

```
moc.ielielihs//:ptth
```

同样的，如果我们想知道一级域名是什么，应该这样做：

```
sample_url = 'http://shileilei.com'
print(sample_url[-4::])
```

如果只想输出不带http://的网址，应当这样做：

```
sample_url = 'http://shileilei.com'
print(sample_url[7:])
```

* * *


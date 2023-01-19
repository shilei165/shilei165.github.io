---
title: Python教程--字符串(String)
tags:
  - python
  - string
  - 字符串
  - 编程
id: '812'
categories:
  - 技术杂谈
  - Python
date: 2021-01-09 17:46:44
---

**这篇文章将会分享关于python中关于string的一些相关知识**。
<!-- more -->
在上一篇文章中我们写出了我们的第一个python程序。接下来我们更加深入的学习一些关于字符串的命令和操作。

# **一、结果中输出单引号**

要输出一段字符串我们知道可以这样来输出：

```
message = 'Hello World!'
print = (message)
```

那么我们要输出的字符串中本身就有单引号或者双引号应该怎么办呢？比如我们想要输出Bobby's World，如果还像上面一样这样写代码：

```
message = 'Bobby's World'
print = (message)
```

就会出现问题，因为有三个单引号，计算机无法识别到底哪里是需要输出的内容。要解决这个问题，我们有三个方案。

### 方案一：使用双引号

```
message = “Bobby's World”
print = (message)
```

### 方案二：使用\\来标记

```
message = 'Bobby\'s World'
print = (message)
```

### 方案三：使用三个单引号''' '''

```
message = '‘’Bobby's World'‘’
print = (message)
```

当采用方案三的时候，我们还可以进行多行输出，比如：

```
message = '''I like Bobby\' 
world'''
print (message)
```

就会得到这样的输出结果：

```
I like Bobby'
world
```

# **二、选择输出结果**

如果我们不想要把所有的结果都输出出来，我们可以选择部分输出，比如输入这样的代码：

```
message = 'Bobby\' world'
print (message)
print (message[10])
print (message[0:5])
print (message[:5])
```

就会相应得到这样的输出结果：

```
Bobby' world
l
Bobby
Bobby
```

方括号中可以选择字符的输出范围，注意是从0开始数。\[0:5\]代表的是从第1个字符到第5个字符。

# **三、大小写**

如果我们要指定输出字符的大小写，可以用lower 和upper的方法，代码如下（#后面的部分为注释，不算入执行代码中）：

```
message = 'Bobby\' world'
print (message.lower()) # output lower case
print (message.upper()) # output upper case
```

会得到这样的输出结果：

```
bobby' world
BOBBY' WORLD
```

# **四、计数和查找**

如果想要对字符串进行计数和查找，可以使用count和find的方法，举例如下：

```
message = 'Bobby\' world'
print (message.count('b')) # 对字符串中b字母的个数进行计数
print (message.find('world')) # 对world所在的位置进行输出
print (message.find('hello')) # 如果所查找的字符出不存在则会返回-1
```

会得到这样的输出结果：

```
2
7
-1
```

# **五、字符串组合输出**

如果我们要对两段或两段以上不同的字符串进行输出，我们可以采用最简单的方法：

```
greeting = 'Hello'
name = 'Michael'
message = greeting + ', ' + name +'. Welcome!'
print (message)
```

这时候，我们就可以得到这样的输出结果：

```
Hello, Michael. Welcome!
```

数据量小的时候，还算比较方便。但如果数据量比较大的时候，这种方法就比较麻烦了，那么我们可以采用下面这种format的方法，用{ }来留下空位，然后后面用format来告诉程序，留下的空位需要填上什么：

```
new_message = '{}, {}. Welcome!'.format(greeting, name)
print (new_message)
```

还可以采用更为简单的如下方法，在要输出的字符串前面加上 f 然后同样同{ }留空位，并且在{ }中填写这个位置中需要填上什么。代码如下：

```
new_message = f'{greeting}, {name}. Welcome!' 
print (new_message)
```

以上三种方法，都会输出同样的结果。但针对数据量比较大的时候，第二种和第三种方法就会显现出它的优势。

* * *


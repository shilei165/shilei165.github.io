---
title: Python教程--os 模块
tags:
  - os
  - python
  - 教程
  - 编程
id: '886'
categories:
  - 技术杂谈
  - Python
date: 2021-01-15 11:55:41
---

**这篇文章中将会分享python中关于os 模块相关的一些知识。**
<!-- more -->
os就是“operating system”的缩写，顾名思义，os模块提供的就是各种 Python 程序与操作系统进行交互的接口。通过使用os模块，一方面可以方便地与操作系统进行交互，另一方面页可以极大增强代码的可移植性。

# **一、os模块常用方法**

获取当前工作目录：

```
import os
print(os.getcwd())
```

改变当前路径到桌面：

```
import os
os.chdir('/Users/leileishi/Desktop/')
```

列出路径中的文件和文件夹：

```
import os
print(os.listdir())
```

新建文件夹

```
import os
os.mkdir('OS-Demon-2')
# 或者
os.makedirs('OS-Demon-2/Sub-Dir-1')
```

注意：mkdir只能建立一个文件夹，而makedirs可以创建一个不存在的文件夹以及在它下面创建子文件夹。

删除文件夹:

```
import os
os.rmdir('OS-Demon-2')
#或者
os.removedirs('OS-Demon-2/Sub-Dir-1')
```

同样的，rmdir只能移除一个文件夹，而removedirs可以移除文件夹（如果为空的话）以及下面的子文件夹。也就是说：若目录为空，则删除，并递归到上一级目录，如若也为空，则删除，依此类推

重命名

```
import os
os.rename('Arduino-2.jpg', 'Arduino.jpg')
```

获取目录信息：

```
import os
print(os.stat('Arduino.jpg'))
```

# **二、更多os模块方法总结**

```
os.curdir   返回当前目录: (‘.’)
os.pardir   获取当前目录的父目录字符串名：(‘..’)
os.sep  输出操作系统特定的路径分隔符，win下为\\,Linux下为/
os.linesep  输出当前平台使用的行终止符，win下为\t\n,Linux下为\n
os.pathsep  输出用于分割文件路径的字符串
os.name 输出字符串指示当前使用平台。win->nt; Linux->posix
os.system(“bash command”)   运行shell命令，直接显示
os.environ  获取系统环境变量
os.path.abspath(path)   返回path规范化的绝对路径
os.path.split(path) 将path分割成目录和文件名二元组返回
os.path.dirname(path)   返回path的目录。其实就是os.path.split(path)的第一个元素
os.path.basename(path)  返回path最后的文件名。如果path以／或\结尾，那么就会返回空值。
os.path.exists(path)    如果path存在，返回True；如果path不存在，返回False
os.path.isabs(path) 如果path是绝对路径，返回True
os.path.isfile(path)    如果path是一个存在的文件，返回True。否则返回False
os.path.isdir(path) 如果path是一个存在的目录，则返回True。否则返回False
os.path.join(path1[, path2[,…]])将多个路径组合后返回，第一个绝对路径之前的参数将被忽略
os.path.getatime(path)  返回path所指向的文件或者目录的最后存取时间
os.path.getmtime(path)  返回path所指向的文件或者目录的最后修改时间
```

* * *


---
title: Linux学习笔记 - 1
tags:
  - command
  - Linux
  - 学习笔记
id: '742'
categories:
  - 技术杂谈
  - Linux
date: 2021-01-01 21:11:08
---

**这篇文章将对目录操作，文件编辑，通配符(wildcard)等相关知识进行总结。**
<!-- more -->
# **常用Linux指令：**

*   pwd (print working directory) 显示当前工作路径
*   ls (list files) 显示当前文件夹中所有文件
*   cd (change directory) 改变工作路径
*   cd / 回到root根目录
*   cd .. 回到上一层目录
*   mkdir (make directory) 新建文件夹
*   rmdir (remove directory) 删除文件夹
*   nano 打开nano文本编辑器编辑文件 e.g. nano my\_dogs.txt; nano prog.py
*   cat (catalog) 查看文件
*   mv (move) 移动文件 e.g. mv my\_cars/cars.txt my\_stuff/cars.txt
*   cp (copy) 复制文件 e.g. cp my\_cars/cars.txt my\_stuff/cars.txt
*   rm (remove) 删除文件
*   rm-r (remove recursively) 将目录及以下之档案亦逐一删除
*   touch 新建空文件
*   sudo (superusers do) 获得超级管理权限

# **通配符（wildcard）**

利用通配符可以大大提高我们的工作效率。比如我们想要移动五个前缀类似的文件(e.g. dog1.txt, dog2.txt, dog3.txt, dog4.txt, dog5.txt)，如果一个一个移动将会十分烦碎。这样的命令将会是：

```
mv my_dogs/dog1.txt my_stuff
mv my_dogs/dog2.txt my_stuff
mv my_dogs/dog3.txt my_stuff
mv my_dogs/dog4.txt my_stuff
mv my_dogs/dog5.txt my_stuff
```

如果使用通配符就可以简化为：

```
mv my_dogs/dog*.txt my_stuff
```

常见的通配符如下表所示：

![](https://shileilei.com/wp-content/uploads/2021/01/image-5-1024x407.png)

# **其他**

上方向键 (up key) 显示上一条指令。（同样或相似的指令就可以不必重复输入）

大于号：将一条命令执行结果（标准输出，或者错误输出，本来都要打印到屏幕上面的）重定向其它输出设备（文件，打开文件操作符，或打印机等等）。大于号分为两种，分别是：

\> 覆盖原有内容

\>> 在原有文件基础上追加内容

* * *


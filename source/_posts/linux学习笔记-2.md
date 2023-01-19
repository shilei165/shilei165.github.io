---
title: Linux学习笔记 – 2
tags:
  - command
  - Linux
  - 学习笔记
id: '774'
categories:
  - 技术杂谈
  - Linux
date: 2021-01-03 12:30:46
---

**这篇文章将对sort排序命令，关机和重启，一些特殊符号的意义，根目录(root directory)与家目录(home directory)的区别，tee 命令，pipe命令，和find、gerp查找命令进行总结。**
<!-- more -->
# **sort命令对文件进行排序**

*   **sort** 对内容按照字母顺序进行排序 (a --> z)
*   **sort -r** (reverse) 对内容按照字母倒序进行排序(z --> a)
*   **sort -n** (number) 对字母从小到大进行排序(1 --> 10)
*   **sort -n** **\-r** 对字母从大到小进行排序( 10 -->1)
*   **sort -M** 对月份进行排序(Jan. -->Dec.)
*   **sort -M -r** 对月份进行逆排序(Dec.-->Jan.)

# **关机和重启**

有些用户会使用直接断掉电源的方式来关闭linux，这是十分危险的。因为linux与windows不同，其后台运行着许多进程，所以强制关机可能会导致进程的数据丢失﹐使系统处于不稳定的状态﹐甚至在有的系统中会损坏硬件设备。所以每次对机器来安全关机是十分有必要的。下面是一些常用的关机和重启命令。

*   **halt** 最简单的关机命令
*   **shutdown -h** 跟halt命令相同，其实也是在调用shutdown下的halt命令
*   **reboot** 重新启动

注意这些命令可能需要sudo来调用超级管理权限才能调用。

# **一些符号的意义**

**~** home目录，也就是家目录

**/** root目录，也就是根目录

**..** 上一层目录

**.** 当前目录

管线命令(pipe command)

通常情况下，我们在终端只能执行一条命令，然后按下回车执行。pipe command就可以帮助我们实现同时执行多条命令。例如：

`cat my_files/my_dogs.txt sort -r`

就可以同时实现在终端查看my\_dogs.txt并且将这个文件进行倒序排列。

# **根目录(root directory)与家目录(home directory)的区别**

*   用户登录后在 家目录 ，可用pwd命令查看，普通用户为 /home/用户名，root用户为/root
*   根目录是在最顶端的目录（因为已经不能cd ..到上一级目录了 ）
*   根目录是所有用户的都可以操作的，家目录用户才有权限操作（管理员可以分配权限）

# **tee 命令**

Linux tee命令用于读取标准输入的数据，并将其内容输出成文件。

tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件。

例如：

```
ls /tee my_root.txt
```

这条指令将会: (1) 列出root目录下的文件; (2) 将文件名保存在my\_root.txt文件中。

```
ls ~ tee my_files.txt -a
```

这条指令将会: (1) 列出home目录下的文件; (2) 将文件名以后缀的形式(-a) 保存在my\_files.txt的文件中。

# **find 查找命令和 grep 查找命令**

Linux中有两个与查找相关的命令，一个是find，另一个是gerp。他们最大的区别是，find用于查找文件，而gerp用于查找文件中的内容。

实例：

```
find . -name "cats.txt"
```

这条指令将会在当前目录中查找名为cats.txt的文件。

```
grep chev my_files/cars.txt
```

这条指令将会在文件cars.txt中查找chev字符。

```
grep -r -i chevy ~
```

\-r 代表recursively; -i 代表ignore case。所以这条指令将会在home以及所属的文件夹中查找chevy并且不区分大小写。

* * *


---
title: Linux学习笔记-4
tags:
  - command
  - Linux
  - Raspberry Pi
id: '800'
categories:
  - 技术杂谈
  - Linux
date: 2021-01-09 12:33:59
---

**本篇文章将会分享关于Linux的包的管理，systemctl系统服务管理器指令，以及使用scp指令来实现不同电脑间的文件传输等相关的一些知识。**
<!-- more -->
# **包(package)的管理**

大多数现代类 Unix 操作系统都提供了一个集中的软件包管理机制，以帮助用户搜索、安装和管理软件。而软件通常以包的形式存储在仓库（repository）中，对软件包的使用和管理被称为包管理。而 Linux 包的基本组成部分通常有：共享库、应用程序、服务和文档。

包管理器又称软件包管理系统，它是在电脑中自动安装、配制、卸载和升级软件包的工具组合，在各种系统软件和应用软件的安装管理中均有广泛应用。**我们这里只介绍最为常见的apt包管理器。**

APT(全称为Advanced Packaging Tools)，常见的以apt- 开头的包管理命令有：

*   **sudo apt list --installed** 查看已经安装的包
*   **sudo apt install** 对包进行安装
*   **sudo** **apt remove** 对包进行移除
*   **sudo** **apt auto remove** 对已经删除的包相关的配置文件进行自动移除
*   **sudo apt update** 更新已经安装的包的列表，查看是否有更新
*   **sudo apt upgrade** 对已经安装的包进行更新升级

# **systemctl系统服务管理指令**

systemd 是 Linux 系统工具，用来启动守护进程，已成为大多数发行版的标准配置。systemctl是systemd的主要命令，用来管理系统。

*   **sudo systemctl stop** 停止某个进程
*   **sudo systemctl start** 开始某个进程
*   **sudo systemctl restart** 重启某个进程
*   **sudo systemctl status** 查看某个进程的状态
*   **sudo systemctl reload** 重新加载某个进程
*   **sudo systemctl disable** 取消开机自动启动某个进程
*   **sudo systemctl enable** 允许开机自动启动某个进程

# **scp跨机远程文件拷贝**

scp是secure copy的简写，用于在Linux下进行远程拷贝文件的命令，和它类似的命令有cp，不过cp只是在本机进行拷贝不能跨服务器，而且scp传输是加密的。当你服务器硬盘变为只读 read only system时，用scp可以帮你把文件移出来。

命令格式为： scp \[参数\] \[原路径\] \[目标路径\]

比如将本机的local\_file文件夹拷贝到 IP为192.168.4.20，用户名为user的home路径下则为：

```
scp -r local_file user@192.168.4.20:/home/
```

相反，如果想要远程将IP为192.168.4.20，用户名为user的home路径下的local\_file复制到本地home路径下则为

```
scp -r user@192.168.4.20:/home/local_file /home/
```

* * *


---
title: Python教程--包管理器-pip
tags:
  - pip
  - python
  - 教程
  - 编程
id: '877'
categories:
  - 技术杂谈
  - Python
date: 2021-01-13 16:20:33
---

**这篇文章中将会分享python中包管理器-pip相关的一些知识。**
<!-- more -->
pip 是 Python 包管理工具，该工具提供了对Python 包的查找、下载、安装、卸载的功能。如果你是从官方下载的python软件，那么已经自带了pip包管理器。另外，Python 2.7.9 + 或 Python 3.4+ 以上版本也都自带 pip 工具。

你可以通过以下命令来判断是否已安装：

```
pip --version
```

# **一、pip 常用命令**

```
pip install PackageName   #安装包 PackageName代指需要下载的包的名称
pip uninstall PackageName    #卸载包
pip list    #列出所有安装的包
pip install --upgrade PackageName    #升级包
pip show PackageName    #显示包的详细信息
pip list -o 或者 pip list --outdated    #显示可升级的包
pip freeze    #把已安装的包以requirements参数的格式输出
```

# **二、打包发送所依赖的安装包**

如果我们创建了一个python程序，想要在别人的电脑上运行。那么我们就需要把这个程序所依赖的所有包也要一同发送给对方，这样对方才能在同一个环境下运行我们所创建的程序。我们就可以把所需要的包用以下代码输出到一个txt文件中：

```
pip freeze > requirements.txt
```

只要把这个requirements.txt文件发送给对方，然后在对方电脑输入以下指令，就可以安装所有所需的包：

```
pip install -r requirements.txt
```

# **三、自动更新所有可升级的包**

如果我们运行pip list -o 或者 pip list --outdated之后，我们发现需要更新的包太多，不想一个一个手动升级更新，我们可以运行以下代码完成一次性更新所有包：

```

pip list --outdated --format=freeze  grep -v '^\-e'  cut -d = -f 1   xargs -n1 pip install -U
```

* * *


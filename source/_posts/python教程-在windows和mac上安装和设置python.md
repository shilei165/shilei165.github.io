---
title: Python教程--在windows和mac上安装和设置python
tags:
  - python
  - 安装和设置
  - 编程
id: '814'
categories:
  - 技术杂谈
  - Python
date: 2021-01-09 17:11:11
---

**这篇文章将会分享如何在windows和mac上安装、设置和使用python。**
<!-- more -->
# **一、Mac上python3的安装和设置**

在mac上一般都已经预装了python，可以通过在terminal 输入以下指令来查看python是否安装以及版本信息：

```
python --version
```

一般mac预装的python版本为python 2.7，如果你想安装最新的python 3可以跟着下面的教程来进行安装。

进入[www.python.org/downloads/](http://www.python.org/downloads/)然后网站会直接检测到所使用的电脑为mac然后点击下载最新版本的python。

![www.shileilei.com 安装python 3](https://shileilei.com/wp-content/uploads/2021/01/python_install-2-1024x661.png)

下载好之后，点击安装。安装好之后，再次打开terminal检查python 3的版本：

```
python3 --version
```

注意：这里python和python3是不同的。调用python会调出python 2，而使用python3才会调用python3。

# **二、windows上python3的安装和设置**

我们先打开终端(win+R键打开运行，然后输入cmd回车)来检查是否在windows上已经安装了python。输入：

```
python --version
```

如果需要安装python，同样进入[www.python.org/downloads/](http://www.python.org/downloads/)然后网站会直接检测到所使用的电脑系统，然后点击下载最新版本的python。

下载好之后，点击安装。安装好之后，再次打开命令窗口，检查python的版本：

```
python --version
```

如果安装成功，则会出现python的版本信息。

# **三、使用python**

我们可以使用terminal来打开python，在命令行中输入：

```
python3
```

我们会看到“>>>”这样的符号，表示我们进入了python3的运行界面。

虽然这样也是可以运行python，但是我们希望可以有一个可编辑的python文件，并且具有更方便的可操作性。我们就需要使用python文本编辑器，python自带的文本编辑器叫做IDLE。找到IELD的应用程序，然后打开就可以写出我们的第一个python程序了：

```
print("Hello World!")
```

将这个文件保存之后，打开terminal 使用就可以运行我们刚刚写好的第一个python程序了。以这个文件的保存路径为桌面，文件名为intro.py为例，在terminal中输入以下代码，然后回车，我们刚刚写好的python程序就会运行，然后terminal就会给我们返回 Hello World! 这段文字。

```
python3 ~/Desktop/intro.py
```

我们还可以用一些界面更加友好的第三方编辑软件，比如PyCharm，Sublime Text等等。

个人比较推荐Sublime Text，可以在这里下载[https://www.sublimetext.com/3](https://www.sublimetext.com/3)。

* * *


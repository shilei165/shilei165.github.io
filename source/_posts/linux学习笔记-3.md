---
title: Linux学习笔记-3
tags:
  - command
  - Linux
  - 学习笔记
  - 树莓派
id: '782'
categories:
  - 技术杂谈
  - Linux
date: 2021-01-03 19:45:38
---

**这篇文章将会分享关于Linux系统IP，用户管理，以及权限方面的知识。**
<!-- more -->
# **查看IP地址**

```
ifconfig
```

ifconfig命令的英文全称是“network interfaces configuring”，即用于配置和显示Linux内核中网络接口的网络参数。

# **添加用户**

```
sudo useradd 
sudo adduser
```

useradd是一个linux命令，但是它提供了很多参数在用户使用的时候根据自己的需要进行设置；  
adduser是一个perl 脚本，在使用的时候会出现类似人机交互的界面，提供选项让用户填写和选择，这个命令比起useradd来说比较简单，也比较傻瓜。

例如使用useradd添加austin用户应使用如下指令：

```
sudo useradd austin -m -s /bin/bash -g users
```

具体代表的意义如下

**\-m:** make all the directories; **\-s /bin/bash:** shell; **\-g users:** users group

使用adduser则相对简单，直接使用：

```
sudo adduser
```

然后会出现交互界面，填写相关信息。

# **登录和退出用户账号**

```
login
logout
```

# **删除用户**

```
sudo userdel
```

如果要同时删除用户所创建的所有文件则要用到

```
sudo userdel -r
```

\-r 的意思是recursively。

# **创建高级用户**

我用的树莓派，所以系统会自带一个pi的用户，权限非常高。如果想要创建一个同样权限的用户则需要首先创建一个账户，以leilei为例：

```
sudo adduser leilei
```

然后输入：

```
sudo usermod -a -G adm,dialout,cdrom,sudo,audio,video,plugdev,games,users,netdev,input,indiecity leilei
```

如果出现提示：

```
usermod: group 'indiecity' does not exist
```

那么将最后的indiecity以及前面的“，”删除即可。

这时候可以操作其他用户的文件，但每次sudo之后都需要输入密码。如果不希望每次都输入账号密码来通过sudo操作，则可以通过visudo命令修改/etc/sudoers文件，代码如下：

```
sudo visudo
```

在nano编辑框中的最后一行下面加上：

```
leilei ALL=(ALL) NOPASSWD: ALL
```

这时候再次进行sudo操作就不需要再次输入密码了。

# **查看文件权限**

输入如下指令就可以查看完整的文件权限信息

```
ls -l
```

然后就会在terminal中出现如下信息

```
total 12
drwxr-xr-x 2 pi pi 4096 Dec  2 07:48 Bookshelf
-rw-r--r-- 1 pi pi    0 Jan  3 16:47 cats.txt
drwxr-xr-x 2 pi pi 4096 Dec  2 08:21 Desktop
-rw-r--r-- 1 pi pi    0 Jan  3 16:47 dogs.txt
drwx------ 2 pi pi 4096 Jan  3 13:08 my_files
```

以第一个文件夹中文件为例：drwxr-xr-x中一共有十个字符。这十个字符分成四组：第一个字符一组，然后剩下的九个字符每三个为一组。

第一个字符代表是目录还是文件。“d”代表是directory，也就是目录或者是是文件夹。“-”则代表是文件。

后面的三组依次代表所有者权限、所在组的权限、以及其他人的权限。其中每一组有三个字符分别是rwx，分别代表可读(read)、可写(write)、可执行(excute)。如果只读的话就会标记为r--，表示只有read权限。

# **修改文件权限**

输入如下指令即可修改文件权限 (###代表三个数字，file代表要修改的文件)

```
chmod ### file
```

那么怎么确定这三个要输入的数字呢？r, w, x分别对应的数字如下

**r**: 4; **w**: 2; **x**:1

然后，每一组的三个数字相加之和即可得到相应组应填的数字。比如，要改变cat.txt文件为所有者可读可写可执行，所在组只可读，其他人没有任何权限，这三个数字应当确定为：7， 4， 0。然后在终端输入：

```
chmod 740 cat.txt
```

再次输入ls -l查看权限，就会发现权限的变化。

如果希望一次性改变所有所属文件夹以及文件的权限则需加上 -r (recursively)来进行操作，比如：

```
chmod 740 -r my_files
```

则会改变my\_files以及所属的所有文件夹及文件的权限。

* * *


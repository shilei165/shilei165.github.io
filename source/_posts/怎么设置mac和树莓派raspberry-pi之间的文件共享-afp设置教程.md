---
title: 怎么设置Mac和树莓派(Raspberry Pi)之间的文件共享  AFP设置教程
tags:
  - Linux
  - Raspberry Pi
id: '794'
categories:
  - 技术杂谈
  - Raspberry Pi & Arduino
date: 2021-01-06 16:29:34
---

**这篇文章将会分享怎么设置AFP (Apple Filing Protocol) 来实现Raspberry Pi 和苹果系统之间的文件共享。**
<!-- more -->
最近我在树莓派(Raspberry Pi 4B)上设置AFP的时候踩了很多坑，希望分享出来，可以帮助到大家。通过这篇文章，我相信大家都可以非常轻松的设置AFP，从而实现树莓派和Mac之间的文件共享。

# **什么是AFP？**

AFP是苹果专有的网络协议，这个协议是用来在不同电脑之间传输文件，实现文件共享。它是苹果对于SMB ([Server Message Block)](https://pimylifeup.com/raspberry-pi-samba/) 和 NFS ([Network File System)](https://pimylifeup.com/raspberry-pi-nfs/)协议的替代产品。

# **怎么下载所需文件？**

在你的树莓派上设置AFP之前，首先请确保你的系统是最新的。输入以下代码来更新Raspberry Pi 的系统：

```
sudo apt update
sudo apt upgrade
```

在确保系统已经更新的前提下，接下来我们要在Raspberry Pi 上需要下载一个叫做Netatalk的包。使用以下代码来下载和安装：

```
sudo apt install netatalk
```

# **怎么配置Netatalk?**

在下载和安装好Netatalk之后，我们就要对它进行一些配置。所以请在Pi的Terminal输入以下代码对 “**afp.conf**” 文件进行修改：

```
sudo nano /etc/netatalk/afp.conf
```

打开之后，是这个样子的：

```
;
; Netatalk 3.x configuration file
;

[Global]
; Global server settings

; [Homes]
; basedir regex = /xxxx

; [My AFP Volume]
; path = /home/pi

; [My Time Machine Volume]
; path = /path/to/backup
; time machine = yes
```

修改成如下的样子：

```
;
; Netatalk 3.x configuration file
;

[Global]
 Global server settings

[Homes]
 basedir regex = /home

[My AFP Volume]
 path = /home/pi

;[My Time Machine Volume]
; path = /path/to/backup
; time machine = yes

 log file = /home/pi/afp.log
```

这里\[Homes\]是对home文件夹的路径进行设置；\[my AFP Volume\]是对要分享的文件夹进行设置；\[My Time Machine Volume\]是对Time Machine的备份进行设置；然后最后一行是设置log file的文件存放路径。

设置完按Ctr+O 然后按Enter进行保存，然后Ctr+X退出。

接下来使用以下代码重启服务器的Netatalk

```
sudo systemctl restart netatalk
```

接下来的步骤需要用到Raspberry Pi的IP，可以通过如下命令来查看IP:

```
sudo hostname -I
```

# **怎么用Mac进行连接？**

在设置完成之后，我们就可以用mac来连接我们的Raspberry Pi来实现文件共享了。

首先，进入Finder，然后Command+K来连接服务器。在地址栏输入afp://+刚才查找的IP，比如：**afp://192.168.0.159**。

然后输入pi的用户名和密码，就可以实现文件共享了。这里一定要记得改用户名哦！

* * *


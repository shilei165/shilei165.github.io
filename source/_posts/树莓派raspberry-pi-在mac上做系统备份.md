---
title: 树莓派(Raspberry Pi) 在mac上做系统备份
tags:
  - Linux
  - Raspberry Pi
  - 树莓派
id: '730'
categories:
  - 技术杂谈
  - Raspberry Pi & Arduino
date: 2021-01-01 15:07:51
---

树莓派Raspberry Pi 是一款基于 ARM 的单板计算机。默认运行一款称为 Raspbian 的操作系统。RaspbianOS 是一个基于 Debian 的 Linux 发行版，专门用于 Raspberry Pi，它是Raspberry 用户的完美通用操作系统。
<!-- more -->
由于树莓派的系统是基于Linux而搭建的，所以是一款非常适合学习Linux语言的小机器。但由于Linux可操控性非常高，所以一些不当的操作往往会导致系统崩溃。所以按时备份系统将会非常有必要。

### 1 . 首先将装有树莓派系统的SD卡插入mac电脑。

### 2\. 打开mac的终端输入以下代码查询盘符

```
diskutil list 
```

![](https://upload-images.jianshu.io/upload_images/3870627-c1a0838ec76f1e92.png?imageMogr2/auto-orient/stripimageView2/2/w/754)

图片中的dev/disk2既对应的树莓派的TF卡

### 3\. 使用dd命令进行备份

```
sudo dd if=/dev/disk2 of=~/Desktop/PiSDCardBackup.dmg
```

这里注意，如果树莓派的盘符不是disk2，要将disk2改为自己的盘符名称。

回车后，读卡器会显示在读取数据灯在闪烁，备份过程有点长，根据卡的大小，可能会在1小时左右。

### 4\. 在备份完成之后将会在桌面出现一个名为PiSDCARDBackup.dmg的镜像文件。接下来我们移除系统SD卡，插上将要备份的SD卡。

### 5\. 用etcher软件讲镜像文件烧录入新的SD卡。

etcher软件下载地址：[https://www.balena.io/etcher/](https://www.youtube.com/redirect?q=https%3A%2F%2Fwww.balena.io%2Fetcher%2F&v=Nc3YyANoOeQ&event=video_description&redir_token=QUFFLUhqbTN4YWNnWWlrLUY5MTNqRl9MRG5WSXBEX2ktUXxBQ3Jtc0tsekNCNkYwRFhXNWNKNGFYelotcUo3a0h6RzJRVnlrSkFHdnlpQzNfX3o3NUtJT1RrUElQV0ROWG1hbTQxM0dUUnpCVjlPNjZPWDRDSVloM3d0V0J6SVJZbGxpVEVhUzFGalNqd3JJR3RJeUJDSGRJbw%3D%3D)

首先选择刚才的镜像文件

![](https://shileilei.com/wp-content/uploads/2021/01/image-1-1024x614.png)

然后，选择新的SD卡。最后选择Flash进行烧录。

![](https://shileilei.com/wp-content/uploads/2021/01/image-3-1024x614.png)

* * *



参考资料：

\[1\] https://oranwind.org/-raspberry-pi-bei-fen-raspbian-shi-yong-macos/

\[2\] https://www.youtube.com/watch?v=Nc3YyANoOeQ&t=448s

\[3\] https://my.oschina.net/u/186291/blog/4544977
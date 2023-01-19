---
title: 黑群晖NAS保姆级安装教程：旧笔记本改造重生成NAS
tags:
  - DIY
  - NAS
  - 群晖
id: '747'
categories:
  - 技术杂谈
  - Cloud & Web
date: 2021-01-03 11:41:36
---

家里扔着一台旧电脑，闲置了很久卖不出去，扔了又觉得可惜？

与其让它趴在墙角吃灰，不如动手把它改造成一台NAS，网络资源各种下下下，图片随便存。
<!-- more -->
## 不知道NAS是什么？

![preview](https://pic1.zhimg.com/72c1c39bd58d1e013be36436a9cd1e9a_r.jpg?source=1940ef5c)

![preview](https://pic4.zhimg.com/4d12fb6aeae4248cbead1154082f396f_r.jpg?source=1940ef5c)![preview](https://pic4.zhimg.com/8b077b1fb2239f751427fe5e222286ae_r.jpg?source=1940ef5c)

怎么样？心动了没有？如果心动了，那么就跟我一起动手改造吧~

1.  首先，下载所需要的软件和镜像：

**百度云盘**: [https://pan.baidu.com/s/1C1UPeboMiMO4QQulpaRpKA](https://pan.baidu.com/s/1C1UPeboMiMO4QQulpaRpKA) 提取码：gi1p

**Google Drive**: [https://bit.ly/2PgqCNh](https://www.youtube.com/redirect?redir_token=QUFFLUhqbG15cTZCLWhmZzFQb01SbHhnS0xzMVhWZHlPZ3xBQ3Jtc0tsN3FKMzVUSmxuNHhpbzJPdndOWDI4ZUxtVGlEMlJKaV92aXVWb0hRZDdoZmF2YVZBWWVwQjdHZ0hkbTNTZGVTVUhxS0FOdk5vbTJpQUlIeDFJMUZIeUJwNURhLWtCZlg5TGhWRHFBZUF3cUtELTlzYw%3D%3D&v=3XwcqRwNQvw&q=https%3A%2F%2Fbit.ly%2F2PgqCNh&event=video_description)

下载完成后的文件如下图所示：

![](https://shileilei.com/wp-content/uploads/2021/01/image-6.png)

2\. 打开第三个文件，也就是win32diskimager，这个是windows的镜像烧录软件（如果是mac的话推荐使用Etcher）。然后安装win32diskimager。

Etcher下载地址：[https://www.balena.io/etcher/](https://www.balena.io/etcher/)

3\. 接下来我们要对群晖的系统进行烧录。这时候我们要准备一个U盘，然后格式化。然后点开win32diskimager把上图列出的最后一个文件烧录入U盘。如下图所示：

![](https://shileilei.com/wp-content/uploads/2021/01/旧笔记本改造NAS-1.png)

4\. 烧录完成之后将U盘插入旧笔记本电脑，并且用一根网线将电脑和路由器相连接。

5\. 启动旧笔记本电脑，进入bios设置，选择U盘启动。然后我们刚刚烧录的群晖系统就会被写入笔记本系统。**注意：这一步的操作将会把电脑中所有文件删除，如果有重要文件，务必要提前保存。**

6\. 当旧笔记本显示出DiskStation login的时候，确保旧笔记本跟路由器用网线连接在了一起。然后我们打开另一台在同一个wifi下的另一台电脑，进行下一步操作。

![](https://shileilei.com/wp-content/uploads/2021/01/image-7-1024x533.png)

7\. 下载并且安装Advance IP Scanner ( [https://www.advanced-ip-scanner.com/](https://www.youtube.com/redirect?redir_token=QUFFLUhqbVZZbGU3VXZESjgtUzlYMVJfZWNhaGZfbWhIZ3xBQ3Jtc0tuRUdreU40NVF4bGlPUmdaZFZQM2lwRjZnc0pBWm5HZFZsME9QVHhzc2R1dkh0bU9ZamVlbEF6QzI4QTRVNHRjdVo4M08wZldJUUU0WDB5X2s0UlRkdktINFZndHpfV2d0bzY0Q1VhWVV3OEVSNEd4VQ%3D%3D&v=3XwcqRwNQvw&q=https%3A%2F%2Fwww.advanced-ip-scanner.com%2F&event=video_description))。当安装完成之后，打开这个软件，将会看到如下图所示界面。

![](https://shileilei.com/wp-content/uploads/2021/01/image-8.png)

8\. 接下来我们要设置要扫描的IP范围，然后进行IP扫描。windows电脑在命令提示符中输入ipconfig就会显示出当前wifi所处的IP范围。

![](https://shileilei.com/wp-content/uploads/2021/01/image-9.png)

![](https://shileilei.com/wp-content/uploads/2021/01/image-10.png)

9\. 将得到的IP地址复制到IP Scanner的IP地址搜索框，并且将最后一个小数点之后的数字改为1-254，然后点击Scan进行扫描。

![](https://shileilei.com/wp-content/uploads/2021/01/image-12-1024x724.png)

10\. 这时候我们会看到一个显示有Synology Web Assistant的IP。点击下面的网页，我们就会进入账户设置页面。

![](https://shileilei.com/wp-content/uploads/2021/01/image-13-1024x536.png)

11\. 点击设置(Set up)，然后我们会看到

![](https://shileilei.com/wp-content/uploads/2021/01/image-14-1024x496.png)

12\. 载入下载好的第一个文件(DSM开头)。

![](https://shileilei.com/wp-content/uploads/2021/01/image-15-1024x526.png)

13\. 安装DiskStation Manager (DSM)。勾选选框，表示理解数据将会被清除。

![](https://shileilei.com/wp-content/uploads/2021/01/image-16-1024x476.png)

14\. 接下来是大概几分钟的等待安装时间。

![](https://shileilei.com/wp-content/uploads/2021/01/image-17-1024x475.png)

15\. 安装完成会自动重启旧笔记本电脑，然后出现如下界面

![](https://shileilei.com/wp-content/uploads/2021/01/image-18-1024x487.png)

16\. 点Next进行下一步设置。然后创建用户名和密码。注意不要勾选选框。

![](https://shileilei.com/wp-content/uploads/2021/01/image-20-1024x495.png)

17\. DSM更新设置。选择第三个。

![](https://shileilei.com/wp-content/uploads/2021/01/image-21-1024x486.png)

18\. 跳过QuickConnect设置。然后选择Yes。

![](https://shileilei.com/wp-content/uploads/2021/01/image-22-1024x488.png)

19\. 然后进入下一个界面。不要勾选选框。然后点Go，就进入到了群晖页面。

![](https://shileilei.com/wp-content/uploads/2021/01/image-24-1024x526.png)

20\. 界面如图所示：

![](https://shileilei.com/wp-content/uploads/2021/01/image-25-1024x485.png)

21\. 到这里我们的群晖NAS系统就安装好了。进行一些简单的设置就可以愉快的使用了。

* * *


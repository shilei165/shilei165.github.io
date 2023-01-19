---
title: 使用Hexo免费搭建个人博客
tags:
  - Hexo
  - 博客
  - 网页
  - GitHub
  - 建站
categories:
  - 技术杂谈
  - Cloud & Web
date: 2022-07-13 15:22:13
---

最近一直在倒腾我使用wordpress搭建的个人博客，因为各种插件导致的问题搞得很头大。所以就萌生了搭建一个非常简洁的原生态的个人博客的想法。在查阅了大量资料之后，发现GitHub+Hexo是一个非常好的方案，尤其是对于有编程基础并且爱倒腾的人来说，简直就是免费大礼包。下面我就分享一下，我使用Hexo搭建博客的过程吧~
<!-- more -->
***

# **一、什么是Hexo?**

Hexo 是一款基于Node.js运行的快速、简洁并且非常高效的博客框架。我们可以将我们撰写的Markdown文档在几秒内渲染为静态的HTML网站，从而可以是我们不用费尽心力去关注前端页面。
更多关于Hexo的介绍，请移步官方文档：https://hexo.io/zh-cn/docs/
至于GitHub，相信每一个接触过编程的人都应该听说过这个神奇的网站吧，我在这里就不多赘述了。

# **二、搭建和配置环境**

## 1. 环境搭建-安装Git & Node.js

简单来说，Git就是一个版本控制工具，我们后期在搭建好网站之后，可以使用Git非常方便的将网页内容更新同步到GitHub上。
由于Git最开始设计用于Unix风格的命令环境中，而Windows系统使用的是Windows命令提示符，是一个非Unix的终端环境，所以我们需要在Windows环境中安装Git Bash。下面列出了Git的下载地址：
```
https://git-scm.com/downloads
```
在选择相应的版本之后，选择默认安装。安装完成之后，可以在CMD命令行中输入git --version来测试是否成功。
由于Hexo是基于Node.js，所以我们还需要安装Node.js。下面是下载地址：
```
https://nodejs.org/en
```
## 2. 安装Hexo
首先，在我们的电脑上新建一个文件夹，可以命名为MyHexoBlog用于存放Hexo框架以及我们自己的网页。在创建好的文件夹下，鼠标点击右键，选择Git Bash Here，然后输入以下命令来一键安装Hexo：
```
npm install -g hexo-cl
```
安装完成后，我们可以输入以下指令，来初始化我们的个人博客：
```
hexo init
```
然后安装Hexo所依赖的包，在Git Bash中输入：
```
npm install
```
接下来在Git Bash中输入以下代码来运行本地hexo:
```
hexo n Test
hexo g
hexo s
```
其中n、g和s分别是new、generate和server的命令缩写，用于新建文章、生成网页，以及启动服务预览。如果我们这时候在浏览器中打开localhost:4000的网页，就应该可以看到我们刚刚生成的名为Test的文章了。
# **三、配置GitHub**
## 1. 创建GitHub个人仓库
首先，要确保有一个GitHub的账户。如果您还没有账号，赶快使用邮箱申请一个免费账户吧。
在登陆GitHub之后，点击右上角的“+”，然后选择“New reposito”来新建一个仓库，仓库名为*username*.github.io，注意要将这里的username替换为你自己的用户名。比如我的GitHub账户为shilei165，那么我的仓库名应为shilei165.github.io。
## 2. 配置GitHub
接下来我们需要配置GitHub从而我们可以直接使用Git Bash来对我们的网页进行管理。在桌面点击右键，选择Git Bash Here。此时就会出现一个Git Bash的命令窗口，我们就可以在这里配置我们的GitHub信息了，具体配置如下：
```
git config --global user.name "username" 
git config --global user.email "email address" 
```
注意要将上面的username和email address替换为你自己的GitHub用户名和邮箱地址。我在这一步的时候出现了找不到配置文件的error：
```
error: could not lock config file G://.gitconfig: No such file or directory
```
这大概率是因为我们没有正确设置HOME文件夹的位置导致的。要解决这个bug，需要在环境变量中修改或添加一个变量HOME，它的值一般为C:\Users\username。这里需要把username替换为您自己的计算机用户名。当然如果你没有出现上面的错误提示，那么恭喜你，你已经成功在Git上配置好了GitHub。
接下来，我们需要创建SSH密钥。输入以下代码，然后一路回车：
```
ssh-keygen -t rsa -C "email address"
```
同样的，需要把email address替换为你自己的GitHub账户的邮箱地址。然后进入C:\Users\username\.ssh的目录（username替换为你自己的用户名），记得要把隐藏目录显示出来，然后打开id_rsa.pub文件，将内容复制出来。
登录你的GitHub账户，点击右上角头像，选择Settings，然后在左侧一栏选择"SSH and GPG keys"，然后点击"New SSH key"。Title可任意填写，然后将刚才复制的密钥粘贴到key中。点击"Add SSH Key"完成添加。此时，我们就完成了将Git和你的GitHub账户的连接配置。我们可以通过在Git Bash中输入以下代码，来测试是否成功：
```
ssh git@github.com
```
如果出现You've successfully....说明配置已经成功。
# **四、推送网页**
上面的步骤只是在本地计算机生成了网页，我们只能在本地进行预览。接下来我们要做的就是将网页发布到网上，这样就可以让更多的人看到我们发布的东西了。这时候我们需要修改Hexo安装目录下的_config.yml文件。下拉至文件末尾，找到deploy，修改为如下格式：
```
deploy:
  type: git
  repository: https://github.com/shilei165/shilei165.github.io.git
  branch: master
  ```
然后搜索ulr，将url: http://example.com替换为https://shilei165.github.io。
同样的搜索skip_render，将其改为skip_render: README.md。
这一步的目的是我们给hexo d这个命令做配置，这样hexo就能知道将我们的blog部署到哪个位置。
最后，我们安装Git部署插件，输入命令：
```
npm install hexo-deployer-git --save
```
然后分别输入三条命令：
```
hexo clean 
hexo g 
hexo d
```
完成后，稍等几分钟，在浏览器输入你的网址，例如shilei165.github.io。你就会发现你的博客已经上线了。
***
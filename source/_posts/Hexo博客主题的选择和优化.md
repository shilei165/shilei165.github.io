---
title: Hexo博客主题的选择和优化
tags:
  - Hexo
  - 博客
  - 网页
  - Next theme
categories:
  - 技术杂谈
  - Cloud & Web
date: 2022-07-15 15:22:13
---
在搭建完网站的主体框架之后，我们就要对我们的网站进行“内部装修”工作了，也就是主题的选择和优化。这样我们就能拥有我们喜欢的博客样式了。在这篇文章中，我将分享我是怎么安装主题，以及怎么对主题进行优化的。
<!-- more -->
***

# **一、主题的选择**
第一个问题，怎么样选择适合自己的主题呢？Hexo的官方主题页展示了三百多个主题的样式，大家可以逐个浏览，找到自己心仪的主题。官网地址如下：
```
https://hexo.io/themes/
```
在选择好主题之后，就可以进行安装了。这里以我选择的Next主题为例进行讲解。在github中搜索hexo next theme，就可以找到与这个主题相关的项目文件。然后在本地计算机Hexo的文件中使用如下git命令，对主题进行下载：
```
git clone https://github.com/theme-next/hexo-theme-next themes/next
```
其中，https://github.com/theme-next/hexo-theme-next 为这个项目所对应的地址，themes/next为本地计算机的存储目录。

一般来说，都有主题说明文件README.md，注意仔细阅读此文档，并且按照文件配置主题信息即可。

# **二、主题的启用**
在下载好主题之后，我们要对Hexo根目录下的配置文件_config.yml进行修改，从而启用我们的主题。搜索并找到theme，然后在后面填写主题的名称。比如我选的Next主题配置好之后的相关代码如下：
```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```
保存这个文件，然后同样使用下面的三联git命令来对网站进行更新和部署：
```
hexo c
hexo g
hexo d
```
这时候如果查看我们的网站，就会发现主题已经成功启用。
# **三、主题的优化**
虽然主题已经成功启动，但对于追求完美的我们来说，我们还远远不能满足。我们还需要对主体进行进一步的优化，从而满足我们的个性化需求。在这里，我给大家分享一下我都做了哪些修改吧。

Hexo对主题的优化，完全是以代码形式来呈现的。所以，首先我们要找到这个主题的配置文件。这个文件的路径为Hexo/themes/next/_config.yml。一般来说，里面的注释会非常详细的解释每一部分代码的功能，我们只需要根据提示来做出相应的修改就可以了。这里还以我所使用的next主题为例来进行讲解。
## 1. 修改网站的logo
网站logo最好使用Adobe Illustrator来制作矢量图，这样文件会比较小，容易加载，清晰度还比较高。在制作完成之后，输出为合适的分辨率和格式即可。我使用Adobe Illustrator制作了logo，并且输出了分率为16×16, 32×32的不同大小的png格式图片，以及SVG格式的矢量图。
然后将图片储存在Hexo\themes\next\source\images文件夹中，并且将相应的文件名复制到主题配置文件中相应的地方来对这些图片进行链接。我的配置如下：
```
# ---------------------------------------------------------------
# Site Information Settings
# See: https://theme-next.org/docs/getting-started/
# ---------------------------------------------------------------

favicon:
  small: /images/favicon-16x16-Shi.png
  medium: /images/favicon-32x32-Shi.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo-Shi.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```
## 2. 主题风格设定
一般来说，同一个主题会有不同的风格。比如next主题就包含了Muse，Mist，Pisces，和Gemini四种不同的风格。默认风格为Muse，但并不是我最喜欢的样式，所以我在尝试了所有风格之后，选择了Gemini的格式。我们只需要将所选风格前面的#去掉就可以使用这个风格了。下面是我修改之后的相关代码：
```
# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
# scheme: Muse
# scheme: Mist
# scheme: Pisces
scheme: Gemini
```
## 3. 侧边栏设置
我们也可以对网站的侧边栏进行设置，比如调整侧边栏位置，展示我们的头像，显示分类和标签，还有显示其他网页以及社交媒体的链接。
首先，我将侧边栏替换为了自己的专属头像（图片存储区域为themes/next/images/avatar-1.gif)， 修改区域如下：
```
# Sidebar Avatar
avatar:
  # Replace the default image and set the url here.
  url: /images/avatar-1.gif
  # If true, the avatar will be dispalyed in circle.
  rounded: false
  # If true, the avatar will be rotated with the cursor.
  rotated: false
```
其次，我链接上了我一直使用的个人博客网址，修改如下：
```
links_settings:
  icon: fa fa-link
  title: links
  # Available values: block | inline
  layout: block

links:
  石磊磊的个人站点: https://shileilei.com
```
## 4. 博客文章设置

对博客的格式，我也做了一些相应的改动。由于默认的是显示item text，比如会显示“发布日期”，“分类”等等这几个字符。我将item_text设置为了false从而取消了这些字符的显示，具体参数如下：
```
# Post meta display settings
post_meta:
  item_text: false
  created_at: true
  updated_at:
    enable: false
    another_day: true
  categories: true
```
## 5. 打赏设置
我也对文章进行了打赏设置，从而在每篇文章的底部都会显示微信和支付宝的打赏二维码。首先，我们要从微信和支付宝上面获取打赏二维码，然后将图片保持在前面一直反复提到的themes/next/images/文件夹中，并且修改文件名为wechatpay.png和alipay.png。然后相关参数的设置如下：
```
# Reward (Donate)
# Front-matter variable (unsupport animation).
reward_settings:
  # If true, reward will be displayed in every article by default.
  enable: true
  animation: false
  comment: 您的支持将鼓励我继续创作

reward:
  wechatpay: /images/wechatpay.png
  alipay: /images/alipay.png
  #paypal: /images/paypal.png
  #bitcoin: /images/bitcoin.png
```
## 6. SEO设置
为了可以让搜索引擎抓取到我们的页面，让更多的人看到我们的文章，我们就需要对SEO进行一些设置。具体可以参照下面几个网站的说明来进行设置，这里就不再赘述：
```
https://www.google.com/webmasters
https://www.bing.com/webmaster
https://webmaster.yandex.ru
https://ziyuan.baidu.com/site
```
## 7.评论区设置
因为Hexo所生成的是纯静态网页，所以我们需要借助第三方的插件才可以实现评论功能。Next主题提供了changyan，disqus，disqusjs，gitalk，livere和valine多种插件的选择。在做了一些research之后，最终选择了disqus这个第三方插件。

在设置这个插件的时候，我们只需要去Disqus首页注册–>点击”Get Started”–>选择”I want to install Disqus on my site”–>输入WebsitName–>选择网站类别–>Create a new site–>选择“I don’t see my platform listed, install manually with Universal Code”–>点击“Complete Setup”。完成上面这些操作之后，确保找到Disqus分配的shortname。然后我们将这个shortname复制粘贴到next主题的配置文件中，就完成了评论区的配置。具体参数如下：

```
# Multiple Comment System Support
comments:
  # Available values: tabs | buttons
  style: tabs
  # Choose a comment system to be displayed by default.
  # Available values: changyan | disqus | disqusjs | gitalk | livere | valine
  active: disqus
  # Setting `true` means remembering the comment system selected by the visitor.
  storage: true
  # Lazyload all comment systems.
  lazyload: false
  # Modify texts or order for any navs, here are some examples.
  nav:
    #disqus:
    #  text: Load Disqus
    #  order: -1
    #gitalk:
    #  order: -2

# Disqus
disqus:
  enable: true
  shortname: https-shilei165-github-io
  count: true
  #post_meta_order: 0
```
在首次设置的时候，遇到了很多难题。下面这两篇文章给了我很多启发，大家也可以参考一下。
https://theme-next.js.org/docs/third-party-services/comments
https://titangene.github.io/article/hexo-disqus.html

如果重新部署，就会发现所有的博客和页面下面都会出现评论区。如果我们对某个页面不想展示留言，我们还需要做一些调整。比如我们不想在“关于”页面下展示留言板，我们就需要找到这个页面对应的md文件，储存在Hexo\source\文件夹中。打开这个md文件，然后添加一行comments: false，就可以关闭此页面的留言板功能了。具体如下：
```
---
title: 关于博主
date: 2022-07-13 19:57:12
comments: false
---
**<center>石磊磊</center>**
```

## 8. 修改站点名和作者
然后，我们还需要对网站的Title和Author进行修改。这样我们的网站名和作者才能展示到网站上。打开Hexo主目录下的_config.yml文件，然后定位到# Site对上面的参数进行设置。比我，我想把博客站点名设置为“石磊磊的个人站点”，并且把作者设置为“石磊磊”，参数如下：
```
# Site
title: 石磊磊的个人站点
subtitle: ''
description: ''
keywords:
author: 石磊磊
language: zh-CN
timezone: ''
```

## 9. 设置Hexo-next主题分类多层级描述
在转移文章的时候，我发现了一个文章分类的问题。同一篇文章可能分属不同的类别，但系统会默认所属的所有的类别是同级别的文章，从而在分类(categories)一栏显得十分混乱。比如我写的《Linux学习笔记-1》属于Linux分类，但同时还有一个父类技术杂谈。在直接从wordpress把文章转移过来的时候，系统就会同时显示这两个类别，在分类的页面中就会显得十分没有层次感。这时候我们就需要对文章的类别进行分层。
首先，我们需要将文章中的
```
categories:
  - - Linux
  - - 技术杂谈
```
改为
```
categories:
  - 技术杂谈
  - Linux
```
这样Linux就会默认为是技术杂谈的子类，而不是并列分类。如果需要将文章同时添加到多个同级别的类，我们可以参考下面这篇文章：

[Hexo-NexT 分类多层级描述](https://cs-cshi.github.io/hexo-blog/Hexo-NexT%20%E5%88%86%E7%B1%BB%E5%A4%9A%E5%B1%82%E7%BA%A7%E6%8F%8F%E8%BF%B0/)

同时为了增加分类的层次感，我们也可以修改/themes/next/source/css/\_common/components/pages/categories.styl代码.category-list-child 的 padding-left 属性改为 60px。
```
.category-list-child {
  padding-left: 60px;
}
```
另外如果需要对标签和文档进行进一步的美化，我们可以参考下面这篇文章：

[Hexo+NexT博客归档/标签/分类页美化](https://jrbcode.gitee.io/posts/be9758cd.html)

到此，我们的“装修”工作就到一个段落了。当然，你也可以继续对其他任何地方进行个性化的修改。在修改完成之后，记得同样使用下面的三联git命令来对网站进行更新和部署：
```
hexo c
hexo g
hexo d
```
这时候如果查看我们的网站 https://shilei165.github.io/ 就会发现所有修改已经成功启用。
***
---
title: Python网络爬虫--第三方库
tags: 
  - Python
  - 编程
  - 网络
  - 爬虫
categories:
  - 技术杂谈
  - Python
date: 2022-07-20 18:41:35
---
这篇文章将会分享总结Python网络爬虫所用到的一些常见的第三方库。
<!--more-->
***
# **请求库**
请求库顾名思义是用来对网络资源发出请求，从而获得服务器的相应。
+ urllib: urllib是Python自带的原生网络请求库，功能强大，但用起来不如requests方便、友好。
+ requests: requrests是对Python内置的urllib的再次封装的请求库。由于它是阻塞式HTTP请求库，当我们发出请求之后，就会一直等待服务器响应，然后才会进行下一步处理。
+ aiohttp: 异步Web服务的库，可以在服务器响应过程中处理一些其他的事情，从而提高抓取效率。

# **解析库**
解析库顾名思义就是用来解析我们所爬取的网页所用到的工具包。我们常用到的解析库如下：
+ lxml：支持HTML和XML的解析，支持XPath解析方式。
+ Beautiful Soup：支持HTML和XML的解析，拥有强大的API和多样的解析方式。Beautiful Soup的HTML和XML解析器依赖于lxml库。
+ pyquery：同样是一个强大的网页解析工具，提供了jQuery类似的语法来解析HTML，支持CSS选择器。
+ tesserocr：tesserocr是Python的一个OCR识别库，可以处理爬取网页过程中遇到的图形验证码。这个库需要预先安装tesseract。
+ chaojiying：除了tesserocr之外，我们还可以选择识别度更高但需要付费库chaojiying来进行图片和验证码的识别。

# **自动化测试工具**
+ Selenium： Selenium是一个自动化测试工具，它可以模拟执行特定的浏览器动作，比如点击、下拉等操作。需要配合浏览引擎以及相应的驱动来操作。支持一般市面上见到的大部分浏览器，比如：
	* Chrome，Firefox，IE，Opera，和Safari浏览器，需要下载安装浏览器（例如Chrome）以及相应的驱动（例如ChromeDriver）来进行操作。
	* Selenium还支持可脚本编程的PhantomJS浏览引擎来操作。PhantomJS运行效率很高，并且支持配置各种参数。

# **数据库**
+ MySQL：MySQL是一个轻量级的关系型数据库。
+ MongoDB：MongoDB是由C++语言编写的非关系型数据库，基于分布式文件存储的开源数据系统。字段值可以包含文档、数组以及文档数组。
+ Redis：Redis是一个基于内存的搞笑的非关系型数据库。

# **存储库**
数据库仅仅提供存储数据服务，想要和Python交互，还需要安装Python存储库。
+ MySQL需要安装PyMySQL存储库。
+ MongoDB需要安装PyMongo存储库。
+ Redis需要安装redis-py存储库。

# **Web库**
Web服务程序可以用来搭建一些API接口，方便我们爬虫使用。例如，我们可以通过Web服务提供一个API接口来获取代理的IP。
+ Flask: 轻量级的Web服务程序，简单、灵活、易用。
+ Tornado：支持异步的Web框架，效率非常高。

# **APP爬取的相关库**
除了爬取网页，Python爬虫也可以抓取APP的数据。这里需要用到抓包技术来抓取数据。
+ Charles：一个强大的网络抓包工具，跨平台支持的很好。
+ mitmproxy：支持HTTP和HTTPS的抓包程序，跟Charles类似，但是是通过控制台的形式操作。mitmproxy有两个关联组件mitmdump和mitmweb。
	* mitmdump是mitmproxy的命令行接口，利用它可以对接Python脚本，实现监听后的处理。
	* mitmweb是一个Web程序，通过它可以清楚地观察到mitmproxy捕获的请求。
+ Appium：移动端的自动化测试工具，类似于Selenium。利用它可以驱动Android，iOS等设备完成自动化测试，比如点击、滑动、输入等操作。

# **爬虫框架**
如果爬取量不大，速度要求不高，直接用requests和Selenium等库写爬虫就可以完全满足我们的需求。但我们可以把这些组件抽取出来模块化形成爬虫框架，从而大大提高代码效率。这样做不仅可以简化代码，结构也会变得更加清晰。所以对于基础好的同学，使用框架会使一个很好的选择。
+ Scrapy：一个十分强大的爬虫框架，主要依赖Twisted 14.0、lxml 3.4和pyOpenSSL 0.14。在不同平台下，所依赖的库也各部不相同。
	* Scrapy-Splash：Scrapy中支持JavaScript渲染的工具。
	* Scrapy-Redis：Scrapy的分布式扩展模块，可以帮助实现Scrapy分布式爬虫的搭建。
	* Scrapyd: 用于部署和运行Scrapy项目的工具，可以把写好的Scrapy项目上传到云主机并通过API来控制它。
+ pyspider：带有强大的WebUI、脚本编辑器、任务监控器、项目管理器以及结果处理器，同时支持多种数据库后端、多种消息队列，另外还支持JavaScript渲染页面的爬取。	
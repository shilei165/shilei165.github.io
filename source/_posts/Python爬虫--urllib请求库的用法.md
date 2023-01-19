---
title: Python网络爬虫--urllib请求库的用法
tags: 
  - Python
  - 编程
  - 网络
  - 爬虫
categories:
  - 技术杂谈
  - Python
date: 2022-07-22 13:49:43
---
这篇文章将跟大家分享Python网络爬虫中经常会用到的网页请求库--urllib的一些用法。

<!--more-->
***

# **urllib库包含的模块**
+ **request**: HTTP请求模块
+ **error**：异常处理模块
+ **parse**：工具模块，提供拆分、解析、合并URL等操作
+ robotparser：识别网站的robots.txt文件，来判断是否可以爬取（很少用到）。

# **request模块**

**该模块包含如下的主要的functions：**

1. urlopen(*url, data = None, [timeout,]\*,cafile = None, capath = None, cadefault = False, context = None*)
	* 用于构建最基本的网页请求
2. install_opener(*opener*)
	* 加载一个OpenerDirector实例作为默认的全局opener。
3. build_opener(*[handler,...]*) 
	* 构建一个由不同handler连接而成的opener。
4. pathname2url(*path*) 
	* 将pathname转换为URL。
5. url2pathname(*path*)
	* 将URL转换为pathname。

**以及如下的classes:**

1. BaseHandler -- 所有其他Handler的父类，可以用于一些更高级的操作（比如处理Cookies，设置代理等）
	* HTTPDefaultErrorHandler -- 用于处理HTTP相应错误
	* HTTPRedirectHandler -- 用于重定向
	* HTTPCookieProcessor(*cookiejar=None*) -- 用于处理Cookies
	* ProxyHanlder(*proxies=None*) -- 用于设置代理
	* HTTPPasswordMgr -- 用于管理密码
	* HTTPBasicAuthHandler -- 用于管理认证
2. OpenerDirector -- 利用Hanlder来构建Opener。

3. Request(*url, data=None, headers={}, origin_req_host=None, unverifiable=False, method=None*) -- 如果需要在请求中加入Header等信息，可以通过Request类来构建一个请求。

# **error：异常处理模块**
1. URLError
	* 有reason属性，可以返回错误原因
	```
	from urllib import request, error
	try:
		response = request.urlopen('https://shileilei.com')
	except error.URLError as e:
		print(e.reason)
	```
2. HTTPError -- URLError的子类，专门用来处理HTTP请求错误，比如认证失败等。有三个属性：
	* code：返回HTTP状态码
	* reason： 同其父类URLError一样，返回错误原因
	* headers: 返回请求头

# **parse：工具模块，提供拆分、解析、合并URL等操作**
1. urlparse() -- 可以实现URL的识别和分段。其API用法为：urllib.parse.urlparse(urlstring, scheme='',allow_fragments=True)
	* urlstring -- 待解析的URL
	* scheme -- 默认协议，比如http或者https
	* allow_fragments -- 是否忽略fragment
2. urlunparse() -- 将不同参数组合在一起构造URL。
3. urlsplit() -- 跟urlparse()类似，不过不单独解析params这一部分。
4. urlunsplit() -- 跟urlunparse()类似，传入参数可以是列表、元组等可迭代对象。
5. urljoint() -- 可以实现链接的解析、拼合与生成。
6. urlencode() -- 可以将字典类型转化为GET请求的URL参数。
7. parse_qs() -- 与urlencode()相反，可以将GET请求参数转化为字典。
8. parse_qsl() -- 可以将GET请求参数转化为元组组成的列表。
9. quote() -- 可以将内容转化为URL编码的格式。尤其是带有中文参数时，可以避免乱码问题。
10. unquote() -- --与quote()相反，可以对URL进行解码。

# **实战演练--使用urllib请求豆瓣电影排行榜**

说了这么多，我们来请求豆瓣电影排行榜来实战演练一下。

```
import urllib
from urllib import request
from urllib.error import URLError, HTTPError
import ssl  #如果出现了认证错误，需要引入ssl模块

ssl._create_default_https_context = ssl._create_unverified_context

url = 'https://movie.douban.com/chart'
opener = request.build_opener(request.HTTPCookieProcessor())
headers = {
	'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36'
}
req = request.Request(url=url, data=None, headers = headers, unverifiable=True, method = 'GET')
try:
	response = opener.open(req)
	print(response.read().decode('utf-8'))
except URLError as e:
	print(e.reason)
```
运行之后，不出意外的话，输出结果应该如下：
```
<!DOCTYPE html>
<html lang="zh-CN" class="ua-windows ua-webkit">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <meta name="renderer" content="webkit">
    <meta name="referrer" content="always">
    <meta name="google-site-verification" content="ok0wCgT20tBBgo9_zat2iAcimtN4Ftf5ccsh092Xeyw" />
    <title>
豆瓣电影排行榜
</title>
    
    <meta name="baidu-site-verification" content="cZdR4xxR7RxmM4zE" />
    <meta http-equiv="Pragma" content="no-cache">
    <meta http-equiv="Expires" content="Sun, 6 Mar 2005 01:00:00 GMT">
    
    <meta name="keywords" content="电影排行榜、新片排行榜、豆瓣电影250"/>
    <meta name="description" content="豆瓣电影排行榜,提供最新电影排行榜、本周电影口碑榜和豆瓣电影TOP250" />
...
...
...
```

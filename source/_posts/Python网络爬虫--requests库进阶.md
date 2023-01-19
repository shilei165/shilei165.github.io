---
title: Python网络爬虫--requests库进阶
tags: 
  - Python
  - 编程
  - 网络
  - 爬虫
categories:
  - 技术杂谈
  - Python
date: 2022-07-30 20:47:56
---

这篇文章将跟大家分享Python网络爬虫中另一个强大的第三方网页请求库--requests库的一些其他用法。

<!--more-->
***
# **Cookies--识别登录**

Cookie,是网站为了识别用户身份、进行session追踪二存储在用户本地计算机上面的数据。比如，某些网站需要登录才能访问，那么就需要在请求中添加Cookie相关信息才能爬取网站。
前面我们使用urllib这个库处理Cookies的时候，需要先生命CookieJar对象。然后利用HTTPCookieProcessor()来构建Handler，最后再利用build_opener()来构建Opener，最后我们才可以用opener的open()函数来完成请求。这样的写法比较复杂，而有了requests这个请求库，我们就可以非常方便的设置Cookies。接下来，我们来看一个例子来说明如何处理Cookies吧。
```
import requests

r = requests.get("https://www.google.com")
print(r.cookies)

for k,v in r.cookies.items():
    print(k + '=' + v)

```
运行结果如下：
```
<RequestsCookieJar[<Cookie 1P_JAR=2022-07-29-00 for .google.com/>, <Cookie AEC=AakniGOkbXuoHM4--93l8g4QiXNnw2nf7S4VxTMHVZzP2DJQNyiV0T6vp4g for .google.com/>, <Cookie NID=511=g1ObAMG2H7aUlRE2wvPCMVXxH8aCwHqjOMJzy7cXdYIZCyvRk3NdNUhic37IydzmXyv0i9S8A02PoI3SflxpoC6Pq3N4CkscuiWAOXgmyUHKNG6Ip-M9t2i-jC0_awX2nvuJ9vtn5-Z4K_GzGYuBGDtvFdx9wgC_kH6sPzE9suc for .google.com/>]>
1P_JAR=2022-07-29-00
AEC=AakniGOkbXuoHM4--93l8g4QiXNnw2nf7S4VxTMHVZzP2DJQNyiV0T6vp4g
NID=511=g1ObAMG2H7aUlRE2wvPCMVXxH8aCwHqjOMJzy7cXdYIZCyvRk3NdNUhic37IydzmXyv0i9S8A02PoI3SflxpoC6Pq3N4CkscuiWAOXgmyUHKNG6Ip-M9t2i-jC0_awX2nvuJ9vtn5-Z4K_GzGYuBGDtvFdx9wgC_kH6sPzE9suc
```
当然，我们也可以使用cookie来维持登录状态。比较简单的做法是从浏览器登录账户，然后找到并将Cookie内容复制下来。将其设置到请求的headers里面。这样做的唯一问题是会有一些繁琐。如果有机会的话，我们会专门写一篇文章来分享如何使用程序来自动获取并且使用cookie来模拟登录网站。

# **Session--会话维持**
我们一般是通过http协议来访问网页的，而http协议是无法维持会话之间的状态。比如说我们成功登录一个网站之后，去访问这个网站的其他页面的时候，登录状态会消失。所以导致页面刷新后就需要反复重新登录来维持会话，这就会非常繁琐。所以我们需要想办法来维持会话。除了我们上面提到的cookie，我们也可以使用一种更为简洁的方法-- session对象来维持登录状态。当使用它来成功登录某个网站之后，该网站的其它网页都会默认使用该session之前使用的cookie等参数。这样，我们就可以方便的维护一个会话，而不用担心cookies的问题。举例如下：
```
import requests
import re

session = requests.session()
data ={
    'username': 'xxxxxxxx',
    'password': 'xxxxxxxx', 
    'usecookie': '0',
    'action':'login'
}

url ="https://www.ptwxz.com/login.php"
result = session.post(url,data=data)
# print(result.text)
print(result.cookies)
# 再次请求 拿取书架上的数据
url2 = "https://www.ptwxz.com/modules/article/bookcase.php"
result2 = session.get(url2)
result2.encoding = 'gb2312'
# print(result2.text)
book = re.findall('<table.*?"checkbox".*?_blank">(.*?)</a></td>',result2.text,re.S)
print(book)
```
输出结果如下：
```
<RequestsCookieJar[<Cookie PHPSESSID=1a0627fa5d220f94aa39c668d6a7bb55 for www.ptwxz.com/>, <Cookie jieqiUserInfo=jieqiUserId%3D436281%2CjieqiUserName%3Dshilei165%2CjieqiUserGroup%3D3%2CjieqiUserVip%3D0%2CjieqiUserName_un%3Dshilei165%2CjieqiUserHonor_un%3D%26%23x6597%3B%26%23x8005%3B%2CjieqiUserGroupName_un%3D%26%23x666E%3B%26%23x901A%3B%26%23x4F1A%3B%26%23x5458%3B%2CjieqiUserLogin%3D1659144657 for www.ptwxz.com/>, <Cookie jieqiVisitInfo=jieqiUserLogin%3D1659144657%2CjieqiUserId%3D436281 for www.ptwxz.com/>]>
['万相之王']
```
我们发现书架上的书被成功获取。相信可以理解如何使用session以及session的作用了吧。

# **Proxies--代理设置**

对于采取了比较强的反爬措施的网站来说，为了顺利爬取网站数据，我们一般需要使用代理IP来防止我们自己的IP被封禁。这时候就需要使用proxies参数来设置代理。

代理主要分为HTTP和SOCKS代理两种。其中，HTTP代理能够代理HTTP访问，主要是代理浏览器访问网页，他的端口一般为80, 8080, 3128等。SOCKS代理，只是简单的传递数据包，而并不关心是何种协议。速度上来说，一般SOCKS代理要比HTTP代理快很多。

我们先看一下不使用代理，来测试下自己的IP：

```
import requests

url = 'http://icanhazip.com'
try:
    response = requests.get(url)
    print(response.status_code)
    if response.status_code == 200:
        print(response.text)
except requests.ConnectionError as e:
    print(e.args)
```
运行上面的代码，我们就会返回类似如下的结果：
```
200
24.163.xx.xxx

```
然后我们再来测试一下使用代理之后的情况。
```
import requests

proxies = {
  
  "http":"http://103.148.178.xxx",
  "https": "https://122.9.101.6",
}
url = 'http://icanhazip.com'
try:
    response = requests.get(url, proxies = proxies)
    print(response.status_code)
    if response.status_code == 200:
        print(response.text)
except requests.ConnectionError as e:
    print(e.args)
```
结果如下：
```
200
103.148.178.xxx
```
这时我们就可以发现返回的是我们代理所使用的IP，说明代理起作用了。
使用SOCKS代理的方法类似，但我们需要首先安装pysock这个库。安装完成之后我们就可以使用SOCKS代理了，示例如下：
```
import requests

proxies = {
  
  "http":"socks5://103.148.178.xxx",
  "https": "socks5://103.148.178.xxx",
}
url = 'http://icanhazip.com'
try:
    response = requests.get(url, proxies = proxies)
    print(response.status_code)
    if response.status_code == 200:
        print(response.text)
except requests.ConnectionError as e:
    print(e.args)
```

在爬取大量数据的时候，往往需要使用多个代理。那我们应该如何构造呢？主要两种方法，一种是使用免费的多个IP，但往往效果不太好，另一种比较靠谱的方法是使用付费的IP代理服务。付费代理在很多网站上都有售卖，比较稳定，大家可以自行选购。

但是无论是免费的IP还是收费的IP，都不能保证可以稳定使用，因为我们用到的IP很有可能已经被目标站点禁掉，或者代理服务器网络繁忙。所以我们就需要搭建一个代理池来提前将不可用的代理筛选掉。以后如果有机会，我们再给大家分享一下如何搭建代理池。
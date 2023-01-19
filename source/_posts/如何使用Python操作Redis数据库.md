
---
title: 如何使用Python来操作Redis数据库
tags: 
  - 数据库
  - 编程
  - 计算机
  - 数据读写
  - Python
categories:
  - 技术杂谈
  - Python
date: 2022-08-10 18:56:23
---

这篇文章跟大家分享一下如何使用Python来操作Redis数据库。
<!--more-->
***
前面的文章中我们详细介绍了如何安装和使用Redis数据库，但这也仅仅是知道了如何利用数据库来储存数据。接下来，我们就来学习如何让数据库跟Python来进行交互，从而大大提高我们的工作效率。

# **redis-py模块**

## 安装redis模块
使用Python来与redis交互，首先我们需要安装redis-py这个模块。推荐使用pip安装，指令如下：
```
pip install redis
```
安装完成之后可以通过以下代码来测试是否安装成功：
```
>>> import redis
>>> redis.VERSION
(4, 3, 4)
>>>
```
如果成功显示了它的版本，说明已经安装成功了redis-py。

## Redis和StrictRedis
之前的版本中redis-py下面有两个类Redis和StrictRedis。Redis是StrictRedis的子类，用于向后兼容旧版本里的几个方法。但在redis-py 3.0版本以后，开发团队就放弃了原先"Redis"的支持，"StrictRedis"也更名为"Redis"。但是，"StrictRedis"的别名也可以继续使用。所以在现行版本中，使用Redis和StrictRedis是完全一致的。

## 连接和使用Redis
在连接Redis之前，确保按照之前的文章的教程在本机安装了Redis数据库，并且运行在6379端口上。我们就可以仿照下面的示例来连接Redis：
```
import redis

r = redis.Redis(host='localhost', port=6379, db=0, password = None)
r.set('name','Leilei')
print(r.get('name'))
```
我们在上面的例子中传入了Redis的地址、运行端口和使用的数据库的信息。在没有改动数据库的默认设置的时候，这里不传入任何信息，也同样可以运行。因为传入的默认值为localhost、6379、0和None。

运行结果如下:
```
b'Leilei'
```
说明我们已经连接成功，并且执行了set()和get()的操作。如果我们希望输出结果直接将byte转换为string，可以在Redis的参数中加入*decode_responses=True*,具体如下：
```
import redis

r = redis.Redis(host='localhost', port=6379, db=0, password = None, decode_responses=True)
r.set('name','Leilei')
print(r.get('name'))
```
运行结果如下:
```
Leilei
```
## 连接池
redis-py使用connection pool来管理对一个redis server的所有连接，避免每次建立、释放连接的开销。

默认每个Redis实例都会维护一个自己的连接池。我们可以直接建立一个连接池，然后作为参数传入Redis，这样就可以实现多个Redis实例共享一个连接池。具体示例如下：
```
import redis

pool = redis.ConnectionPool(host='localhost', port=6379, db=0, password = None)
r = redis.Redis(connection_pool = pool)
r.set('name','Leilei')
print(r.get('name'))
```

# **Redis数据导入和导出**
如果想要把Redis中的数据导入和导出，我们还需要安装一个名为RedisDump的工具。由于这个工具是基于Ruby实现的，所以我们还需要提前安装Ruby。

## 安装Ruby
Ruby的安装可以参考： https://www.ruby-lang.org/zh_cn/documentation/installation/#rubyinstaller

选择相应的平台，安装即可。注意需要安装带DEVKIT的版本。

## 安装RedisDump工具
安装完Ruby之后，我们就可以使用gem命令（类似于python的pip命令）来安装RedisDump了，具体如下：
```
gem install redis-dump
```
执行完成之后，即可完成RedisDump的安装。
## 验证安装
通过运行一下两个指令，可以测试是否安装成功：
```
redis-dump
redis-load
```
如果出现错误，可以尝试重新换一个Ruby版本重新安装，或者注释掉C:\Ruby30-x64\lib\ruby\gems\3.0.0\gems\redis-dump-0.4.0\lib\redis 中的以下代码：
```
# `ps -o rss= -p #{Process.pid}`.to_i # in kb
```

---
title: 数据库--Redis的安装和使用
tags: 
  - 数据库
  - 编程
  - 计算机
  - 数据读写
  - 教程
  - Redis
categories:
  - 技术杂谈
  - Database
date: 2022-08-02 22:05:44
---

这篇文章跟大家分享一下非关系型数据库Redis的安装和使用。
<!--more-->
***
# **Redis简介**
Redis全称"Remote Dictionary Server"是一个非常高效的开源数据存储系统。Redis支持多种数据结构，包括字符串(string), 哈希(hashes), 列表(lists), 集合(sets), 有序集合(sorted sets)等。

我们可以通过Redis来对数据进行原子性操作，例如追加字符串;将元素嵌入到列表;计算集合的交集、并集和差集;或者给数据进行排序，并获取排名最高的数据。这里的原子性是指就像原子不可分割的属性一样，一个事务不能再被继续分割就是指执行事务的原子性。或者简单的理解成一个操作要么成功执行，要么失败完全不执行。

我们可以使用多种计算机语言来对Redis进行操作，包括C, C++, C#, Python, Java, Node.js, Matlab, Perl, PHP, R等。

如果想要了解更多可以参考Redis的官方网站： https://redis.io/ 

# **Redis的安装**
## Windows下安装
下载地址：https://github.com/tporadowski/redis/releases

选择自己计算机支持的位数（32位或者64位），比如Redis-x64-5.0.14.1.msi，点击下载安装即可。在安装过程中最好选择把路径添加到环境变量以便于调取使用。

安装完成之后，我们把目录切换到redis路径下，然后在终端(CMD)输入以下代码来启动并且测试Redis是否安装成功
```
C:\Program Files\Redis>redis-server.exe redis.windows.conf
```
如果返回值如下所示，说明已经成功安装：
```
C:\Program Files\Redis>redis-server.exe redis.windows.conf
[45748] 01 Aug 22:10:16.074 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
[45748] 01 Aug 22:10:16.074 # Redis version=5.0.14.1, bits=64, commit=ec77f72d, modified=0, pid=45748, just started
[45748] 01 Aug 22:10:16.074 # Configuration loaded
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 5.0.14.1 (ec77f72d/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 45748
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

[45748] 01 Aug 22:10:16.079 # Server initialized
[45748] 01 Aug 22:10:16.080 * DB loaded from disk: 0.001 seconds
[45748] 01 Aug 22:10:16.081 * Ready to accept connections
```
这时我们重新打开一个CMD窗口，输入Redis-cli就可以进入Redis命令行模式：
```
C:\Program Files\Redis>redis-cli
127.0.0.1:6379> set 'name' 'Leilei'
OK
127.0.0.1:6379> get 'name'
"Leilei"
```
我们现在暂时还无法远程连接来使用Redis，如果需要远程连接，我们可以修改Redis安装文件下的redis.windows.conf配置文件。
首先，将下面的IP栏注释掉：
```
bind 127.0.0.1
```
另外我们还可以通过取消注释并且修改下面这一行，来给Redis设置密码：
```
requirepass foobared
```
foodbared即当前的默认密码，我们需要修改为自己的密码。

重启Redis服务之后，我们就可以使用密码远程连接Redis了。停止和重启Redis的服务命令如下：
```
net stop redis
net start redis
```

## Mac下安装
在Mac环境下使用Homebrew安装会比较简单，直接执行如下命令即可：
```
brew install redis
```
可以使用如下命令来启动Redis服务：
```
brew services start redis
redis-server /usr/local/ect/redis.conf
```
同样使用redis-cli进入Redis命令行模式。

停止和重启Redis的服务命令如下：
```
brew services stop redis
brew services restart redis
```
# **全局命令**
### 查询所有的key： 
```
keys *
```
### 查询key的总数：
```
dbsize
``` 
### 检测key是否存在：
```
exists key 
```
### 删除指定key：
```
del key [key...] 
```
### 指定key过期时长：
```
expire key seconds  # 当超过过期时间，会自动删除，key在seconds秒后过期
expireat key timestamp # key在秒级时间戳timestamp后过期
pexpire key milliseconds # 当超过过期时间，会自动删除，key在milliseconds毫秒后过期
pexpireat key milliseconds-timestamp # key在豪秒级时间戳timestamp后过期
ttl # 命令可以查看key的剩余过期时间，单位：秒（>0剩余过期时间；-1没设置过期时间；-2键不存在）
pttl #与ttl类似，但单位是毫秒
```
### 查看key的数据结构类型:
```
type key
```
### key重命名
```
rename key newkey
```
### 随机返回一个key
```
randomkey
```
### 切换数据库
```
select index # 默认16个数据库：0-15
```
### 清除数据库
```
flushdb # 清除当前数据库
flushall # 清除所有数据库
```

# **Redis数据类型**
 Redis支持多种数据类型，包括strings, hashes, lists, sets, sorted sets, bitmaps, hyperloglogs, geospatial indexes, 以及streams。其中我们最常用到的为字符串(string), 哈希(hashes), 列表(lists), 集合(sets), 有序集合(sorted sets)，下面我们就来简单介绍下如何使用这几种数据类型。
 ## String(字符串)
 String是Redis最基本的数据类型，该类型的最大储存空间为512M。

常用的Redis字符串命令如下：
|命令|描述|
|-----|-----|
|set key value|设置key值|
|get key|获取key值|
|mset key value [key value...]|批量设置值|
|mget key [key...]|批量获取值|
|incr key|将key中储存的数字值增一|
|decr key|将key中储存的数字值减一|
|strlen key|字符串长度|
|getset key value|设置并返回原值|
|setrange key offset|设置指定位置的字符|
|getrange key start end|获取部分字符串，start和end分别为开始和结束的偏移量|

最常用的存储和获取string的示例如下：
 ```
127.0.0.1:6379> set name leilei
OK
127.0.0.1:6379> get name
"leilei"
 ```

相对来说稍微复杂的incr, setrange, getrange 举例如下：
```
## incr & decr
127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> incr age
(integer) 21
127.0.0.1:6379> decr age
(integer) 20

## setrange
127.0.0.1:6379> set name Leilei
OK
127.0.0.1:6379> setrange name 0 M
(integer) 6
127.0.0.1:6379> get name
"Meilei"

## getrange
127.0.0.1:6379> set name Leilei
OK
127.0.0.1:6379> getrange name 0 3
"Leil"
```


## Hash(哈希)
Redish Hash是一个键值对(key => value)集合。

常用的Redis哈希命令如下：
|命令|描述|
|-----|-----|
|hset key field value|设置值|
|hget key field|获取值|
|hdel key field [field...]|删除field|
|hlen key|field个数|
|hmset key field value [field value...]|批量设置field-value|
|hmget key field [field...]|批量获取field|
|hexists key field|判断field是否存在|
|hkeys key|获取所有field|
|havls key|获取所有value|
|hincrby key field increment|增加field所对应的value, increment必须为整数|
|hidecrbyfloat key field increment|增加field所对应的value()，increment为浮点型|
|hstrlen key field |计算value字符串长度|

在创建单组数据时，我们即可以使用HMSET，也可使用HSET命令，创建时需要输入key field value示例如下：
```
127.0.0.1:6379> hmset students stu1 "Leilei" stu2 "Jason" stu3 "James"
OK
127.0.0.1:6379> hget students stu1
"Leilei"
127.0.0.1:6379> hget students stu2
"Jason"
```
这里HMSET和HSET唯一的不同在于返回值不同，HMSET返回值为字符串(例如OK)，而HSET返回值为整数值(例如0, 1等)。

## List(列表)
Redis列表是简单的字符串列表，字符串可以多次重复出现，可以按照插入顺序排序排列。

列表有四种操作类型，归类如下：
|操作类别|操作|
|------|------|
|添加|rpush, lpush, linsert|
|查找|lrange, lindex, llen|
|删除|lpop, rpop, lrem, ltrim|
|修改|lset|
|阻塞操作|blpop, brpop|

常用的Redis列表命令如下：

|命令|描述|
|--------|---------|
|lpush key value1 [value2]|将一个或多个值插入到列表头部|
|rpush key value1 [value2]|在列表中添加一个或多个值到列表尾部|
|linsert key before\|after pivot value|在列表的元素前或者后插入元素|
|lrange key start stop|获取列表指定范围内的元素|
|lindex key index|通过索引获取列表中的元素。|
|llen key|获取列表长度|
|lpop key|弹出并获取列表的第一个元素|
|rpop key|弹出并获取列表的最后一个元素|
|lrem key count value|移除列表元素。count > 0 : 从表头开始向表尾搜索，移除与value相等的元素，数量为count。count < 0 : 从表尾开始向表头搜索，移除与value 相等的元素，数量为count的绝对值。count = 0 : 移除表中所有与value相等的值。|
|ltrim key start stop|对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。|
|lset key index value|通过索引设置列表元素的值|
|blpop key1 [key2 ...] timeout|弹出并获取列表的第一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|brpop key1 [key2 ...] timeout|弹出并获取列表的最后一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|brpoplpush source destination timeout|弹出列表的最后一个元素，并将该元素添加到另一个列表并返回；如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。|
|rpoplpush source destination|弹出列表的最后一个元素，并将该元素添加到另一个列表并返回|


linsert key before\|after pivot value举例如下：
```
127.0.0.1:6379> rpush newlist a b c d e f g
(integer) 7
127.0.0.1:6379> linsert newlist after b a
(integer) 8
127.0.0.1:6379> lrange newlist 0 -1
1) "a"
2) "b"
3) "a"
4) "c"
5) "d"
6) "e"
7) "f"
8) "g"
```

## Set(集合)
Redis的Set是string类型的无序集合，元素只能出现一次。

常用的Redis集合命令如下：
|命令|描述|
|--------|---------|
|sadd key element [element...]|添加元素|
|srem key element [element...]|删除元素|
|scard key| 计算元素个数|
|sismember key element|判断元素是否在集合中|
|srandmember key [count]|随机从集合返回指定个数元素|
|smembers key|获取所有元素|
|sinter key [key...]|求多个集合的交集|
|sunion key [key...]|求多个集合的并集|
|sdiff key [key...]|求多个集合的差集|
|sinterstore destination key [key ...]|将多个集合的交集保存|
|sunionstore destination key [key...]|将多个集合的并集保存|
|sdiffstore destination key [key...]|将多个集合的差集保存|

## Zset(有序集合)

Redis zset和set一样也是string类型元素的集合,且不允许成员重复。不同的是每个成员都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

常用的Redis有序集合命令如下：
|命令|描述|
|--------|---------|
|zadd key score member|添加成员|
|zcard key|计算成员个数|
|zscore key member|计算成员分数|
|zrank key member|计算成员排名|
|zrem key member [member...]|删除成员|
|zincrby key increment member|增加成员分数|
|zrange key start end|从低分到高分排列|
|zrevrange key start end|从高分到低分排列|
|zrangebyscore key min max|返回指定分数范围的成员，分数从低到高排列|
|zrevrangebyscore key max min|返回指定分数范围的成员，分数从高到低排列|
|zcount key min max|返回指定分数范围的成员个数|
|zremrangebyrank key start end|删除指定排名内的升序元素|
|zremrangebyscore key min max|删除指定分数范围的成员|
|zinterstore destination key [key ...]|将多个集合的交集保存|
|zunionstore destination key [key...]|将多个集合的并集保存|

zrange和zrangebyscore的区别，示例如下：
```
27.0.0.1:6379> zadd school 100 UC
(integer) 1
127.0.0.1:6379> zadd school 90 UH
(integer) 1
127.0.0.1:6379> zadd school 70 CU
(integer) 1
127.0.0.1:6379> zrange school 0 -1
1) "CU"
2) "UH"
3) "UC"
127.0.0.1:6379> zrangebyscore school 80 100
1) "UH"
2) "UC"
```

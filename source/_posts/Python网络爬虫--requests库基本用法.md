---
title: Python网络爬虫--requests库基本用法
tags: 
  - Python
  - 编程
  - 网络
  - 爬虫
categories:
  - 技术杂谈
  - Python
date: 2022-07-28 20:51:59
---

这篇文章将跟大家分享Python网络爬虫中另一个强大的第三方网页请求库--requests库的一些基本用法。

<!--more-->
***
requests库是对urllib请求库的再次封装，从而使得网络请求变得更加容易。
# **requests库包含的主要方法**
requests包含有get(), post(), delete(), head(), requests()等七个方法。但我们平时一般只会用到get()和post()两个主要方法。
+ **get(*url, params={key: value}, args*) -- 用于发送GET请求到特定的url。**


|参数 |描述|
|-------------|------------|
|url	|	Required. The url of the request.|
|params	| Optional. A dictionary, list of tuples or bytes to send as a query string.<br>Default *None*|
|allow_redirects	|	Optional. A Boolean to enable/disable redirection. <br>Default *True* (allowing redirects)|
|auth	|	Optional. A tuple to enable a certain HTTP authentication. <br>Default *None*|
|cert	|	Optional. A String or Tuple specifying a cert file or key. <br>Default *None*|
|cookies	|	Optional. A dictionary of cookies to send to the specified url. <br>Default *None*|
|headers	|	Optional. A dictionary of HTTP headers to send to the specified url. <br>Default *None*|
|proxies	|	Optional. A dictionary of the protocol to the proxy url. <br>Default *None*|
|stream	|	Optional. A Boolean indication if the response should be immediately downloaded (False) or streamed (True). <br>Default *False*|
|timeout	|	Optional. A number, or a tuple, indicating how many seconds to wait for the client to make a connection and/or send a response. <br>Default *None* which means the request will continue until the connection is closed|
|verify	|Optional. A Boolean or a String indication to verify the servers TLS certificate or not. <br>Default *True*|

+ **post(*url,data={key: value},json={key: value},args*) -- 用于发送POST请求到特定的url。**

|参数 |描述|
|-------------|------------|
|url	|	Required. The url of the request|
|data	|	Optional. A dictionary, list of tuples, bytes or a file object to send to the specified url|
|json	|	Optional. A JSON object to send to the specified url|
|files	|	Optional. A dictionary of files to send to the specified url|
|allow_redirects	|	Optional. A Boolean to enable/disable redirection. <br>Default *True* (allowing redirects)|
|auth	|	Optional. A tuple to enable a certain HTTP authentication. <br>Default *None*|
|cert	|	Optional. A String or Tuple specifying a cert file or key. <br>Default *None*|
|cookies	|	Optional. A dictionary of cookies to send to the specified url. <br>Default *None*|
|headers	|	Optional. A dictionary of HTTP headers to send to the specified url. <br>Default *None*|
|proxies	|	Optional. A dictionary of the protocol to the proxy url. <br>Default *None*|
|stream	|	Optional. A Boolean indication if the response should be immediately downloaded (False) or streamed (True). <br>Default *False*|
|timeout|		Optional. A number, or a tuple, indicating how many seconds to wait for the client to make a connection and/or send a response. <br>Default *None* which means the request will continue until the connection is closed|
|verify	|Optional. A Boolean or a String indication to verify the servers TLS certificate or not. <br>Default *True*|

# **基本GET请求**
网页请求中最常见的就是GET请求，下面我们来了解下怎么使用requests来发起网络请求吧。下面的例子，我们通过requests的get()方法来完成对请求www.google.com 网页的请求。
```
import requests

req = requests.get('https://www.google.com')
print(req.text)
```
如果我们想要传入参数，应该怎么办呢？比如我们现在想要添加参数q它的值为python，应该如下表示：
```
import requests

data = {
	'p':'python'
}
req = requests.get('https://www.google.com/search',params=data)
print(req.text)
```

# **添加headers**
因为很多网站都添加了反爬机制，如果我们传递header来模仿浏览器来登录，请求就会遭到拒绝。比如上面的例子中，如果我们把www.google.com 换为 www.zhihu.com 或者 www.movie.douban.com 都会遭到拒绝。下面我们一起通过来看下面的例子来了解一下如何添加header，从而解决此类问题吧。

```
import requests

headers = {'Accept': 'text/html, application/xhtml+xml, image/jxr, */*',
               'Accept - Encoding':'gzip, deflate',
               'Accept-Language':'zh-Hans-CN, zh-Hans; q=0.5',
               'Connection':'Keep-Alive',
               'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36 Edge/15.15063'}

url = 'https://www.zhihu.com'
req = requests.get(url = url, headers = headers)
print(req.text)
```

# 抓取网页
下面我们来以抓取某瓣的电影排行榜来演示下怎么抓取网页内容。首先,我们将网页的html格式输出出来,然后方便我们进一步来查看我们所需要找的电影名的规律。
```
import requests

headers = {'Accept': 'text/html, application/xhtml+xml, image/jxr, */*',
               'Accept - Encoding':'gzip, deflate',
               'Accept-Language':'zh-Hans-CN, zh-Hans; q=0.5',
               'Connection':'Keep-Alive',
               'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36 Edge/15.15063',
}
url = 'https://movie.douban.com/top250'
req = requests.get(url = url, headers = headers)
print(req.text)
```
html格式输出之后，我们截取一部分内容如下：
```
        <li>
            <div class="item">
                <div class="pic">
                    <em class="">1</em>
                    <a href="https://movie.douban.com/subject/1292052/">
                        <img width="100" alt="肖申克的救赎" src="https://img9.doubanio.com/view/photo/s_ratio_poster/public/p480747492.jpg" class="">
                    </a>
                </div>
                <div class="info">
                    <div class="hd">
                        <a href="https://movie.douban.com/subject/1292052/" class="">
                            <span class="title">肖申克的救赎</span>
                                    <span class="title">&nbsp;/&nbsp;The Shawshank Redemption</span>
                                <span class="other">&nbsp;/&nbsp;月黑高飞(港)  /  刺激1995(台)</span>
                        </a>


                            <span class="playable">[可播放]</span>
                    </div>
                    <div class="bd">
                        <p class="">
                            导演: 弗兰克·德拉邦特 Frank Darabont&nbsp;&nbsp;&nbsp;主演: 蒂姆·罗宾斯 Tim Robbins /...<br>
                            1994&nbsp;/&nbsp;美国&nbsp;/&nbsp;犯罪 剧情
                        </p>

                        
                        <div class="star">
                                <span class="rating5-t"></span>
                                <span class="rating_num" property="v:average">9.7</span>
                                <span property="v:best" content="10.0"></span>
                                <span>2661267人评价</span>
                        </div>

                            <p class="quote">
                                <span class="inq">希望让人自由。</span>
                            </p>
                    </div>
                </div>
            </div>
        </li>
        <li>
```
我们会发现电影名位于\<li\> --> \<div class="info"\> --\> \<span class="title"\>肖申克的救赎\</span\>这里面。然后，我们就可以利用正则表达式来筛选出所有的电影名。正则表达式的具体用法可以参照如下资料：

[Python 爬虫入门七之正则表达式](https://cuiqingcai.com/977.html)

[正则表达式 - 语法](https://www.runoob.com/regexp/regexp-syntax.html)

另外，因为每个网页只能存放10个电影，我们就需要做一个循环来提取所有的相关网页，具体代码如下：
```
import requests
import re

headers = {'Accept': 'text/html, application/xhtml+xml, image/jxr, */*',
               'Accept - Encoding':'gzip, deflate',
               'Accept-Language':'zh-Hans-CN, zh-Hans; q=0.5',
               'Connection':'Keep-Alive',
               'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36 Edge/15.15063',
}
base_url = 'https://movie.douban.com/top250?'

results = []
for i in range(0,9):
    param = {
        'start':str(i*25)
    }
    req = requests.get(url = base_url, params = param, headers = headers)
    result = re.findall('<li>.*?info.*?title">(.*?)</span>',req.text,re.S)
    results = results + result
print(results)
```

运行之后，我们应该就会发现某瓣评分前250的电影名称被成功抓取了。
```
['肖申克的救赎', '霸王别姬', '阿甘正传', '泰坦尼克号', '这个杀手不太冷', '美丽人生', '千与千寻', '辛德勒的名单', '盗梦空间', '星际穿越', '忠犬八公的故事', '楚门的世界', '海上钢琴师', '三傻大闹宝莱坞', '机器人总动员', '放牛班的春天', '无间道', '疯狂动物城', '大话西游之大圣娶亲', '熔炉', '控方证人', '教父', '当幸福来敲门', '触不可及', '怦然心动', '龙猫', '末代皇帝', '寻梦环游记', '蝙蝠侠：黑暗骑士', '活着', '哈利·波特与魔法石', '指环王3：王者无敌', '乱世佳人', '素媛', '飞屋环游记', '我不是药神', '摔跤吧！爸爸', 
'何以为家', '十二怒汉', '哈尔的移动城堡', '少年派的奇幻漂流', '鬼子来了', '猫鼠游戏', '大话西游之月光宝盒', '天空之城', '让子弹飞', '钢琴家', '闻香识女人', '指环王2：双塔奇兵', '天堂电影院', '海蒂和爷爷', '罗马假日', '黑客帝国', '指环王1：护戒使者', '大闹天宫', '死亡诗社', '教父2', '绿皮书', '辩护人', '狮子王', '搏击俱乐部', '饮食男女', '美丽心灵', '本杰明·巴顿奇事', '窃听风暴', '穿条纹睡衣的男孩', '情书', '两杆大烟枪', '西西里的美丽传说', '看不见的客人', '音乐之声', '拯救大兵瑞恩', '飞越疯人院', '小鞋子', 
'阿凡达', '哈利·波特与死亡圣器(下)', '沉默的羔羊', '致命魔术', '海豚湾', '禁闭岛', '布达佩斯大饭店', '蝴蝶效应', '美国往事', '心灵捕手', '低俗小说', '春光乍泄', '摩登时代', '喜剧之王', '七宗罪', '功夫', '致命ID', '超脱', '哈利·波特与阿兹卡班的囚徒', '杀人回忆', '加勒比海盗', '红辣椒', '被嫌弃的松子的一生', '狩猎', '请以你的名字呼唤我', '7号房的礼物', '剪刀手爱德华', '断背山', '勇敢的心', '唐伯虎点秋香', '入殓师', '一一', '哈利·波特与密室', '第六感', '爱在黎明破晓前', '重庆森林', '天使爱美丽', '蝙蝠侠：黑暗骑士崛起', 
'幽灵公主', '小森林 夏秋篇', '菊次郎的夏天', '阳光灿烂的日子', '超能陆战队', '完美的世界', '无人知晓', '爱在日落黄昏时', '消失的爱人', '借东西的小人阿莉埃蒂', '甜蜜蜜', '小森林 冬春篇', '倩女幽魂', '侧耳倾听', '时空恋旅人', '幸福终点站', '驯龙高手', '萤火之森', '怪兽电力公司', '玛丽和马克思', '教父3', '一个叫欧维的男人决定去死', '寄生虫', '玩具总动员3', '大鱼', '傲慢与偏见', '告白', '神偷奶爸', '未麻的部屋', '釜山行', '被解救的姜戈', '阳光姐妹淘', '射雕英雄传之东成西就', '哈利·波特与火焰杯', '哪吒闹海', '恐怖直播', '我是山姆', 
'头号玩家', '血战钢锯岭', '新世界', '模仿游戏', '喜宴', '七武士', '花样年华', '黑客帝国3：矩阵革命', '头脑特工队', '电锯惊魂', '三块广告牌', '达拉斯买家俱乐部', '卢旺达饭店', '你的名字。', '九品芝麻官', '疯狂原始人', '上帝之城', '谍影重重3', '惊魂记', '风之谷', '心迷宫', '英雄本色', '纵横四海', '海街日记', '岁月神偷', '记忆碎片', '忠犬八公物语', '爱在午夜降临前', '荒蛮故事', '绿里奇迹', '色，戒', '爆裂鼓手', '小偷家族', '贫民窟的百万富翁', '无敌破坏王', '东邪西毒', '真爱至上', '疯狂的石头', '茶馆', '冰川时代', '雨中曲', 
'你看起来好像很好吃', '恐怖游轮', '黑天鹅', '2001太空漫游', '魔女宅急便', '雨人', '恋恋笔记本', '遗愿清单', '无间道2', '牯岭街少年杀人事件', '大佛普拉斯', '可可西里', '城市之光', '背靠背，脸对脸', '萤火虫之墓', '小丑', '源代码', '东京教父', '虎口脱险', '初恋这件小事', '人工智能', '海边的曼彻斯特', '罗生门', '终结者2：审判日', '青蛇', '波西米亚狂想曲', '二十二', '奇迹男孩', '疯狂的麦克斯4：狂暴之路', '新龙门客栈', '房间', '心灵奇旅', '无耻混蛋', '血钻', '千钧一发']
```
# **抓取图片**
上面的例子中，我们抓取的是相关文字。当遇到图片、音频、以及影视资料的时候，我们又应该如何抓取呢？还是以某瓣为例，如果我们想要抓取电影的相关图片，首先要提取图片对应的二进制码。我们首先找到其中一个图片对应的url地址，然后利用Response的content属性就可以将图片的二进制码输出出来。

```
import requests

img_url = 'https://img9.doubanio.com/view/photo/s_ratio_poster/public/p480747492.jpg'
res = requests.get(img_url)
print(res.content)
```
接下来我们将图片保存下来：
```
import requests

img_url = 'https://img9.doubanio.com/view/photo/s_ratio_poster/public/p480747492.jpg'
res = requests.get(img_url)
with open('Shawshank_Redemption.jpg', 'wb') as f:
    f.write(res.content)
```

# **POST请求**
requests库最常用两个方法是GET()和POST()，前面我们已经了解了GET()的请求方式，接下来我们再来看一下如何使用POST()。
```
import requests

key_dict = {'query':'hello'}
response = requests.post(url = 'http://httpbin.org/post', data = key_dict)
print(response.status_code)
print(response.text)
```
运行之后，其返回结果如下：
```
200
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "query": "hello"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Content-Length": "11", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.28.1", 
    "X-Amzn-Trace-Id": "Root=1-62e47885-6826fed24cbad6c70ff2fcf7"
  }, 
  "json": null, 
  "origin": "24.163.13.126", 
  "url": "http://httpbin.org/post"
}
```
响应值为200，说明已经成功请求。另外，我们也可以在结果的form一项中也发现已经成功提交{"query": "hello"}的表单数据。
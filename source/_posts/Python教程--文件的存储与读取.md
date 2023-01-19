
---
title: Python教程--文件的存储与读取
tags: 
  - Python
  - 编程
  - 计算机
  - 数据读写
  - 教程
categories:
  - 技术杂谈
  - Python
date: 2022-07-31 21:05:05
---

这篇文章将跟大家分享如何使用python来读取和存储文件和数据。

<!--more-->
***
# **读取和存储TXT文本**
存储数据最简单方便并且常用的格式为TXT文本格式，这种格式几乎兼容所有平台。

## 写入文件

### 基本写入

这里会用到python内置的三个函数，open(), write()和close()。其中，open()函数可以将文件打开，write()函数用于将数据写入文件，close()函数用于关闭并保存文件。示例如下：
```
file = open("example.txt", 'w')
file.write("This is an example.")
file.close()
```
执行上面的代码之后，我们就会发现在当前目录下，一个文件名为example.txt的文档被成功创建了。

### 简化写法
另外，文件写入还有一种简化写法，那就是使用with as语法。with控制块结束时，文件会自动关闭，这样就可以将close()方法省略掉了。示例如下：
```
with open ("example_2.txt", 'w') as file:
	file.write("This is another example.")
```

### 改变存储路径
如果想保存到其他目录下，我们也可以在文件名中加入路径，比如：
```
with open ("C:\\users\\shil\\example_2.txt", "w") as file:
	file.write("This is another example.")
```
路径中要使用双\，其中第一个\用作转义字符，也就是让系统识别\是路径的一部分。我们也可以在前面加上r来表示路径：
```
with open (r"C:\users\shil\example_2.txt", "w") as file:
	file.write("This is another example.")
```
另外，注意在Linux和OS系统中，文件路径要使用斜杠(/)而不是反斜杠(\)。

### 读写状态
python的open和C以及其它很多语言类似，有r,r+, w, w+等各种状态。
|参数|状态|
|------|------|
|r|读取(read)|
|w|写入(write)|
|a|追加(append)|
|r+|可读取和写入(r+w)，就若不存在报错|
|w+|可读取和写入(w+r)，若不存在就创建|
|a+|可追加和写入(a+r)，若不存在就创建|

在使用w和w+时要注意，该读写方式会覆盖原先的文件(如果存在一个同名文件的话)，使得原先的文件内容消失。

对应的，如果是二进制文件，就都加一个b就好了:
'rb','wb', 'ab', 'rb+', 'wb+', 'ab+'

## 读取文件
我们也可以使用python来读取TXT文件。但需要注意，使用with语句时，open()返回的文件对象只能在with代码块内使用。
### with语句内读取
首先，我们先用上面提到的方法来创建一个多行的TXT文件。
```
with open ("example.txt", "w") as file:
	file.write("This is the first line.\n")
	file.write("This is the second line.\n")
	file.write("This is the third line.\n")
```
然后，我们就可以逐行进行读取数据了，具体代码如下：
```
with open ("example.txt", "r") as file:
	for line in file:
		print(line)
```
执行之后，结果如下：
```
This is the first line.

This is the second line.

This is the third line.

```
我们发现结果已经成功输出，但同时发现跟原文件相比多了几行空白行。这是因为每一个print语句会自动加上一个换行符。要消除这些空白行，我们只需要在print语句中使用rstrip()，具体代码如下：
```
with open ("example.txt", "r") as file:
	for line in file:
		print(line.rstrip())
```
这时候再次执行，输出结果就跟原文件一致了：
```
This is the first line.
This is the second line.
This is the third line.
```
### with语句外读取
如果要在代码块外访问文件的内容，可以将文件先储存在一个列表中，然后就可以在with代码块之后访问文件的内容了。示例如下：
```
with open ("example.txt", "r") as file:
	lines = file.readlines()

for line in lines:
	print(line.rstrip())
```

# **读取和存储JSON文件**
虽然TXT纯文本格式使用起来比较简单，但它有个缺点，那就是层次结构不清晰，并且不利于检索。相比较来说，JSON就可以通过对象和数组的组合来表示数据，从而提供结构化程度比较高的数据格式。同时这种格式更加有利于人的阅读和编写，易于机器解析和生成，并且有效地提升网络传输效率。
## JSON基本结构
我们先来看一个JSON数据格式的例子：
```
[{
  "province": "Hebei",
  "cities": {
  "city": ["Shijiazhuang", "Baoding"]
  }
}, {
  "province": "Guangdong",
  "cities": {
  "city": ["Guangzhou", "Shenzhen", "Zhuhai"]
  }
}]

```
其中大括号{}包围的为字典，是由{key:value}的键值对结构来组成。方括号包围的为列表，是由0个或多个以英文逗号“,”分隔的值列表组成。JSON可以由这两种形式自由组合，可以无限次嵌套。
## 存储数据为JSON格式
### 基本数据存储
我们可以使用json.jump()的方法来把数据写入JSON文件，或者使用json.jumps()的方法来将JSON格式的对象转化成字符串，然后再使用上面提到的类似的方法写入文件。示例如下：
```
import json

cities = [{
  "province": "Hebei",
  "cities": {
  "city": ["Shijiazhuang", "Baoding"]
  }
}, {
  "province": "Guangdong",
  "cities": {
  "city": ["Guangzhou", "Shenzhen", "Zhuhai"]
  }
}]


with open('example.json', 'w') as file:

	json.dump(cities, file)
```

类似的，我们也可以使用dumps()来将JSON对象转换为字符串，示例如下：
```
with open('example.json', 'w') as file:

	file.write(json.dumps(cities))
```

两种方式的输出结果相同，其内容如下：
```
[{"province": "Hebei", "cities": {"city": ["Shijiazhuang", "Baoding"]}}, {"province": "Guangdong", "cities": {"city": ["Guangzhou", "Shenzhen", "Zhuhai"]}}]
```
如果想要保存的json文件层次更加清晰，我们可以加一个indent参数，使数据带有缩进。示例如下：
```
with open('example.json', 'w') as file:

	json.dump(cities, file, indent = 2)
```
此时写入结果如下：
```
[
  {
    "province": "Hebei",
    "cities": {
      "city": [
        "Shijiazhuang",
        "Baoding"
      ]
    }
  },
  {
    "province": "Guangdong",
    "cities": {
      "city": [
        "Guangzhou",
        "Shenzhen",
        "Zhuhai"
      ]
    }
  }
]
```
### 保存用户生成的数据为JSON
对于用户生成的数据，我们也可以使用JSON格式保存下来。示例如下：
```
import json

username = input("What is your name?")

with open('username.json','w') as file:
	json.dump(username, file)
	print("Hello, " + username + "!")
```
运行此程序，就会得到如下结果：
```
What is your name?Leo
Hello, Leo!
```
## 读取JSON格式文件
跟存储JSON格式的数据类似，我们可以用json.load()方法来读取JSON，或者使用json.loads()的方法将JSON对象转化为字符串，然后进行读取。示例如下：
```
import json

with open('example.json','r')as file:
	cities = json.load(file)

print(cities)
```
或者使用json.loads()的方法：
```
import json

with open('example.json','r')as file:
	str = file.read()
	cities = json.loads(str)

print(cities)
```

输出结果如下：
```
[{'province': 'Hebei', 'cities': {'city': ['Shijiazhuang', 'Baoding']}}, {'province': 'Guangdong', 'cities': {'city': ['Guangzhou', 'Shenzhen', 'Zhuhai']}}]
```
# **读取和存储CSV格式文件**
CSV，全称为Comma-Separated Values，顾名思义是一系列以逗号或者制表符来分隔的值，是一种非常方便的在文本中储存数据的方式。CSV文件对人来说阅读起来比较麻烦，但程序可轻松地提取并处理其中的数据。
## 写入CSV格式数据
### 直接写入
我们先来看一个最简单的例子：
```
import csv

with open('data.csv', 'w', newline='') as file:
	writer = csv.writer(file)
	writer.writerow(['name','age','gender','hobit'])
	writer.writerow(['James','25','male','basketball'])
	writer.writerow(['Louis','13','male','football'])
	writer.writerow(['Jenny','16','female','badminton'])
	
```

运行上面的程序，会生成一个data.csv文件，里面包含的内容如下：
```
name,age,gender,hobit
James,25,male,basketball
Louis,13,male,football
Jenny,16,female,badminton
```
### 多行写入
上面的例子我们采用的是writerow()来逐行写入。除此之外，我们还可以调用writerows()的方法同时写入多行，示例如下：
```
import csv

with open('data.csv', 'w', newline='') as file:
	writer = csv.writer(file)
	writer.writerow(['name','age','gender','hobit'])
	writer.writerows([['James','25','male','basketball'], ['Louis','13','male','football'], ['Jenny','16','female','badminton']])
	
```
注意这里writerows传入的是一个二维列表，包含了两层方括号。
### DictWriter 以字典形式写入
python的csv库也提供了直接将字典写入CSV的函数：DictWriter()。还是上面的例子：
```
import csv

with open('data.csv', 'w', newline='') as file:
	fileheader = ['name','age','gender','hobit']
	writer = csv.DictWriter(file, fileheader)
	writer.writeheader()
	writer.writerow({'name':'James','age':'25','gender':'male','hobit':'basketball'})
	writer.writerow({'name':'Louis','age':'13','gender':'male','hobit':'football'})
	writer.writerow({'name':'Jenny','age':'16','gender':'female','hobit':'badminton'})
```
最终写入的结果是完全相同的，内容如下：
```
name,age,gender,hobit
James,25,male,basketball
Louis,13,male,football
Jenny,16,female,badminton
```
## 读取CSV格式数据
### 一般读取
我们同样可以使用csv库来读取CSV文件。比如，我们可以使用如下代码把刚才写入的文件内容读出来：
```
import csv

with open('data.csv', 'r') as file:
	reader = csv.reader(file)
	for row in reader:
		print(row)
```
运行结果如下：
```
['name', 'age', 'gender', 'hobit']
['James', '25', 'male', 'basketball']
['Louis', '13', 'male', 'football']
['Jenny', '16', 'female', 'badminton']
```
### DictReader 以字典形式读取
跟上面的DictWriter()函数类似，我们也可以使用DictReader()函数将CSV文件中的数据以字典形式读取出来。具体代码如下：
```
import csv

with open('data.csv','r') as file:
	reader = csv.DictReader(file)
	for row in reader:
		print(row)

```
输出结果如下：
```
{'name': 'James', 'age': '25', 'gender': 'male', 'hobit': 'basketball'}
{'name': 'Louis', 'age': '13', 'gender': 'male', 'hobit': 'football'}
{'name': 'Jenny', 'age': '16', 'gender': 'female', 'hobit': 'badminton'}
```

### 使用pandas库读取
另外，我们还可以使用pandas库中的read_csv()方法来更加简单的读取CSV文件中的内容。还是上面的例子：
```
import pandas as pd

df = pd.read_csv('data.csv')
print(df)
```
运行结果如下：
```
    name  age  gender       hobit
0  James   25    male  basketball
1  Louis   13    male    football
2  Jenny   16  female   badminton
```

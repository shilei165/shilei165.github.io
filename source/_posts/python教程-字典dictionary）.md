---
title: Python教程–字典(Dictionary）
tags:
  - python
  - 字典
  - 教程
  - 编程
id: '837'
categories:
  - 技术杂谈
  - Python
date: 2021-01-10 17:48:05
---

**这篇文章将会分享关于python中关于字典(Dictionary）的一些相关知识**
<!-- more -->
前面我们学习了python中三个非常重要的概念：列表(List)、元组(Tuple)和集合(Set)。接下来我们再学习最后一个非常重要的承载数据的集合：字典(Dictionary）。

# **一、什么是字典(Dictionary)?**

字典定义了键(key)和值(value)之间一对一的关系，并且是以无序的方式储存的。跟集合(Set)非常像的一点是：定义 Dictionary 也同样是使用一对大括号 { }。 先看一个例子，来初步了解一下集合：

```
student = {'name':'John','age':25,'courses':['Math','CompSci']}
print (student)
print (student['name'])
print (student['courses'])
```

运行结果如下：

```
{'name': 'John', 'age': 25, 'courses': ['Math', 'CompSci']}
John
['Math', 'CompSci']
```

我们发现字典中的元素都是成对出现的，一个是代表元素名称的“键(key)”，另一个是代表元素内容的“值(value)”。

# **二、get方法来获取字典中的元素**

上面获取字典元素的方法虽然简单，但却有一个问题：如果我们想要的获取的元素不存在，将会出现错误提示，然后整个程序都会无法运行。这个问题，我们可以用get方法来解决。举例如下：

```
student = {'name':'John','age':25,'courses':['Math','CompSci']}
print (student.get('name'))
print (student.get('phone'))
```

运行结果如下：

```
John
None
```

我们会发现，对于字典中没有phone，程序给我们返回了一个默认的None值，并且没有出现错误提示。那么如果我们想要返回一个自己设定的字符串，应该怎么做呢？请看下面的例子：

```
student = {'name':'John','age':25,'courses':['Math','CompSci']}
print (student.get('phone','Not Found'))
```

我们再次运行会发现，结果变为了：

```
Not Found
```

但如果这个phone在字典中存在的时候呢？这种方法就不再给我们返回Not Found，而是会给我们返回它所对应的值。比如：

```
student['phone'] = '555-5555'
print (student.get('phone','Not Found'))
```

运行结果如下：

```
555-5555
```

# **三、update方法更新字典中的元素**

如果我们需要更新字典中的某对元素，我们可以使用update的方法来进行更新。具体使用方法可以参考下面的例子：

```
student = {'name':'John','age':25,'phone':'555-5555'}
print (student)
student.update({'name':'Ann','age':'26','phone':'222-2222'})
print (student)
```

运行结果如下：

```
{'name': 'John', 'age': 25, 'phone': '555-5555'}
{'name': 'Ann', 'age': '26', 'phone': '222-2222'}
```

我们会发现使用update的方法，字典内容得到了更新。

# **四、del或者pop方法删除字典中的元素**

如果想要删除字典中的某对元素，我们可以使用del或者pop来进行删除。先来讲一下del如何使用。我们可以参考下面的例子：

```
student = {'name':'John','age':25,'phone':'555-5555'}
del student['age']
print (student)
```

运行结果如下：

```
{'name': 'John', 'phone': '555-5555'}
```

我们发现age所对应的key和value都被删除掉了。

我们也可以使用pop( )来对字典中某对元素进行删除，并且我们还可以将所删除的元素进行输出。比如：

```
student = {'name':'John','age':25,'phone':'555-5555'}
age = student.pop('age')
print (student)
print (age)
```

运行结果如下：

```
{'name': 'John', 'phone': '555-5555'}
25
```

我们发现跟del方法一样，age这一对元素在字典中被删除掉了。并且我们还可以得到被删除的age所对应的内容。

# **五、输出字典中的key和value**

如果我们只想输出key值，可以使用keys( )的方法。如果只想输出value值，可以使用values( )的方法。而如果想一起输出，我们可以使用item( )的方法。具体使用方法如下：

```
student = {'name':'John','age':25,'phone':'555-5555'}
print (len(student))
print (student.keys())
print (student.values())
print (student.items())
```

运行结果如下：

```
dict_keys(['name', 'age', 'phone'])
dict_values(['John', 25, '555-5555'])
dict_items([('name', 'John'), ('age', 25), ('phone', '555-5555')])
```
# **六、遍历字典中的key和value**
接下来我们来看一下如何遍历字典中的key和value。
## 1. 遍历key值：
```
student = {'name':'John','age':25,'phone':'555-5555'}
for key in student:
  print (key)
```
运行结果如下：
```
name
age
phone
```
## 2. 遍历value值：
```
student = {'name':'John','age':25,'phone':'555-5555'}
for value in student.values():
  print (value)
```
结果如下：
```
John
25
555-5555
```
## 3. 遍历字典项：
```
student = {'name':'John','age':25,'phone':'555-5555'}

for item in student.items():
  print(item)
```
结果如下：
```
('name', 'John')
('age', 25)
('phone', '555-5555')
```
## 4. 遍历字典的key和value：
```
student = {'name':'John','age':25,'phone':'555-5555'}

for key, value in student.items():
  print(key + ":" + str(value)
```
结果如下：
```
name:John
age:25
phone:555-5555
```

* * *


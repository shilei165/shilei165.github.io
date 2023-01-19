---
title: Python教程--列表(List）
tags:
  - python
  - 列表
  - 编程
id: '829'
categories:
  - 技术杂谈
  - Python
date: 2021-01-09 22:21:27
---

**这篇文章将会分享关于python中关于列表(list)的一些相关知识**。
<!-- more -->
前面我们已经学习了字符串(string)和数字(number)，那么接下来我们将会把这些字符串或者数字组合到一起，从而进行一些运算。这些字符串或者数字的组合有四种，包括列表(list)、元组(Tuple)、集合(set)和字典(dictionary)。首先，我们学习最为重要的列表(list)相关知识。

# **一、列表的特征**

列表是Python中最具灵活性的有序集合对象类型，与字符串不同的是：列表可以包含任何种类的对象，包括数字、字符串、甚至是其他列表。

列表都是可变对象，**它支持在原处修改的操作**。也可以通过指定的索引和分片获取元素，用中括号\[ \]来定义。

比如有一个名为courses的列表，包含了几门课程(History, Math, Physics, CompSci)，则应当写成如下格式：

```
courses = ['History', 'Math', 'Physics','CompSci']  
```

# **二、列表的基本操作**

获取列表长度：len( )。举例如下：

```
courses = ['History', 'Math', 'Physics','CompSci']
print(len(courses))
```

则输出结果为其长度：

```
4
```

返回其中某一个元素。举例如下：

```
courses = ['History', 'Math', 'Physics','CompSci']
print(courses[0]) #输出序列号为0的元素
print(courses[-1]) #输出最后一个元素
print(courses[0:2]) #输出序列号从0到2的元素
```

这里需要提一下，在python中序列号是从0开始算起。\[0:2\]代表列表中第一和第二个元素，\[1:3\]则代表第二和第三个元素。

以上代码的输出结果如下：

```
4
History
CompSci
['History', 'Math']
```

# **三、在列表中添加、插入和删除元素**

如果我们想要在一个列表后面加入一个元素，可以用append。还拿上面的courses列表来举例：

```
courses = ['History', 'Math', 'Physics','CompSci'] 
courses.append('Art')
print(courses)
```

将会返回如下结果：

```
['History', 'Math', 'Physics', 'CompSci', 'Art']
```

如果我们想要把元素插入到特定位置，则可以用insert。例子如下：

```
courses = ['History', 'Math', 'Physics','CompSci'] 
courses.insert(0,'PE')
print(courses)
```

则会在index为0的位置插入PE，结果如下：

```
['PE', 'History', 'Math', 'Physics', 'CompSci']
```

那么，我们如果插入的是一个列表(list)那应该怎么办呢？是不是还可用前面提到的append或者insert?我们试一下看看得到什么结果。

```
courses = ['History', 'Math', 'Physics','CompSci'] 
courses_2 = ['Art','Education']
courses.insert(4,courses_2)
print (courses)
```

其返回结果为：

```
['History', 'Math', 'Physics', 'CompSci', ['Art', 'Education']]
```

我们发现，用insert或者append会把列表本身作为一个元素插入进去。一般来说，这并不是我们想要的。那么我们应该怎么在一个列表中插入另一个列表呢？这就用到了extend。用法如下：

```
courses = ['History', 'Math', 'Physics','CompSci'] 
courses_2 = ['Art','Education']
courses.extend(courses_2)
print (courses)
```

其运行的结果为：

```
['History', 'Math', 'Physics', 'CompSci', 'Art', 'Education']
```

我们发现，这次我们得到了想要的结果：第二个列表中的元素分别合并到了第一个列表中。

接下来我们讲怎么从列表中删除某个元素。常用的删除方法有两个：remove( )和pop( )。先说说remove( )，remove( ) 是删除首个符合条件的元素。用法如下：

```
courses = ['History', 'Math', 'Physics', 'CompSci', 'Art', 'Education']
courses.remove('Math')
print (courses)
```

运行程序，Math元素将会被移除，然后得到如下结果：

```
['History', 'Physics', 'CompSci', 'Art', 'Education']
```

我们再来看看pop( )：

```
courses = ['History', 'Math', 'Physics', 'CompSci', 'Art', 'Education']
popped = courses.pop(-1)
print (popped)
print (courses)
```

将会得到如下运行结果：

```
Education
['History', 'Math', 'Physics', 'CompSci', 'Art']
```

我们会发现，pop是根据索引（元素所在位置）来删除的。并且，pop会返回弹出的那个数值。

# **四、列表中元素的排序和筛选**

如果我们想要把一个列表中的元素顺序倒过来，应该怎么做呢？这时候我们可以用到reverse( )。用法如下：

```
courses = ['History', 'Math', 'Physics','CompSci'] 
courses.reverse()
print (courses)
```

那么列表中元素将会反过来排列，得到如下结果：

```
['CompSci', 'Physics', 'Math', 'History']
```

如果我们想要把列表中元素按照字母表的顺序排列，则会用到sort( )。用法如下：

```
courses = ['History', 'Math', 'Physics','CompSci'] 
courses.sort()
print (courses)
```

那么列表中元素将会以首字母按照字母表顺序排列，得到如下结果：

```
['CompSci', 'History', 'Math', 'Physics']
```

如果列表中元素是数字，则会按照从小到大排列。还可以用max( )、min( )和 sum( )对数字进行最大值、最小值和求和的计算。用法如下：

```
nums = [1, 5, 3, 8]
nums.sort()
print(nums)
print (min(nums)) 
print (max(nums)) 
print (sum(nums)) 
```

运行结果如下：

```
[1, 3, 5, 8]
1
8
17
```

如果我们不想按照正序排列，则可以给sort加一个条件sort(reverse = True)来实现。举例如下：

```
ums = [1, 5, 3, 8]
nums.sort(reverse = True)
print(nums)
```

运行结果如下：

```
[8, 5, 3, 1]
```

# **五、在列表中查找元素**

如果我们想要查找某一个元素是否在列表中，或者想要知道某个特定元素的位置，可以用如下方法来实现：

```
courses = ['History', 'Math', 'Physics','CompSci'] 
print ('Art' in courses)
print(courses.index('Math')) 
```

其运行输出结果为：

```
False
1
```

Flase表示列表中没有art元素，第二个输出的1给出了我们Math在列表中处在第二个元素的位置（第一个元素为0）。

# **六、列表中的元素按照指定格式输出和还原**

还是上面那个courses的列表，如果我们不想要按照原来的列表格式输出，而是想要每个元素之间都隔一个逗号或者短横线输出，那该怎么办呢？这时候我们可以用join( )来把列表中元素按照特定格式来组成一个字符串。举例如下：

```
courses = ['History', 'Math', 'Physics','CompSci'] 
course_str = ', '.join(courses)
print(course_str)
course_str = ' - '.join(courses)
print(course_str)
```

将会得到如下输出结果：

```
History, Math, Physics, CompSci
History - Math - Physics - CompSci
```

我们这时候又想把原先变成字符串的列表还原，又该用什么方法呢？这时候可以用split( )将字符串还原为列表。举例如下：

```
course_str = History - Math - Physics - CompSci
new_list = course_str.split(' - ') 
print (new_list)
```

其运行结果如下：

```
['History', 'Math', 'Physics', 'CompSci']
```

* * *


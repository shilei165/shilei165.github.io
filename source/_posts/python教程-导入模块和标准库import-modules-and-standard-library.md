---
title: Python教程--导入模块和标准库(Import Modules and Standard Library)
tags:
  - python
  - 教程
  - 模块和标准库
  - 编程
id: '875'
categories:
  - 技术杂谈
  - Python
date: 2021-01-13 14:35:45
---

**这篇文章中将会分享python中关于导入模块和标准库(Import Modules and Exploring Standard Library**)**相关的一些知识。**
<!-- more -->
前面介绍了Python中函数的用法，通过使用函数我们可以大大提高我们写代码的效率。今天我们介绍一下怎么来导入模块和使用标准库，从而我们可以使用别人写好并且封装好的一些函数。从而，使我们实现各种各样的功能，而不用自己花费大量的时间来写出别人已经写过的代码。

# **一、导入模块(Import Modules)**

首先，我们在同一个文件夹下面建立两个python文件，一个名为my\_module.py的导入模块文件，另一个为my\_courses.py的python主程序文件。通过这两个文件之间的相互调用，我们将了解怎么样在一个文件中导入其他文件中的函数。在my\_module.py中写入如下代码：

```
print('Imported my_module...')

test = 'Test String'

def find_index(to_search, target):
'''Find the index of a value in a sequence'''
for i, value in enumerate(to_search):
if value == target:
return i
return -1
```

第一行的print代码是为了验证我们是否可以导入这个模块。在另一个my\_courses.py的文件中，输入：

```
import my_module
```

如果结果中返回以下代码，说明模块导入成功。

```
Imported my_module...
```

接下来我们看看我们可以通过导入的模块做些什么？在my\_courses.py中写入以下代码：

```
import my_module
courses = ['History', 'Math', 'Physics','CompSci'] 
index = my_module.find_index(courses, 'Math')
print(index)
```

运行结果为：

```
Imported my_module...
1
```

这说明，我们成功导入了my\_module.py模块，并且成功调用了其中的函数。在调用模块的时候，我们不能直接使用被调用模块中的函数，需要在前面加上模块名称，比如在这里我们想要调用my module.py中的find\_index( )函数，我们是这么使用的：my\_module.find\_index(courses, 'Math')。

我们还可以对调入的模块重新命名，比如：

```
import my_module as mm
courses = ['History', 'Math', 'Physics','CompSci'] 
index = mm.find_index(courses, 'Math')
print(index)
```

在这里我们将my\_module重新命名为mm，从而让我们可以比较简洁快速的调用模块。

另外，我们还可以只调用某个模块中的某个特定函数，比如：

```
from my_module import find_index
courses = ['History', 'Math', 'Physics','CompSci'] 
index = find_index(courses, 'Math')
print(index)
```

这样的话，我们就可以直接使用被调用模块中的函数，而不用在前面加上被调用模块的名称。但要注意使用这种方法的时候，我们是无法调用模块中除了这个函数以外的任何其他函数的。

# **二、添加模块导入路径**

在python中我们可以导入刚才的模块是因为我们现在文件夹的位置在sys.path的文件列表中。如果我们将刚才的my\_module.py文件放到其它位置，则我们将无法导入刚才的模块。这是应为my\_module.py代码因为它所在的目录不在sys.path里。我们可以通过以下方法查看sys.path包含的路径：

```
import sys 
print(sys.path)
```

我们会看到相应的路径。如果我们想要自定义一个常见的路径，可以使用以下方法：

```
import sys
sys.path.append('~/Desktop/my_module') 
```

如果使用的是windows系统，则需要在路径前面加一个r来表示string \\是用于表示路径。例如：

```
import sys
sys.path.append(r"C:\Users\Leilei\OneDrive\桌面")
print(sys.path)
```

我们会发现，刚刚添加的桌面路径已经被添加到了sys.path的列表中。我们就可以直接调用存放在桌面上的python模块了。

# **三、标准库的导入和使用**

Python 标准库非常庞大，所提供的组件涉及范围十分广泛。这个库包含了多个内置模块 (以 C 编写)，Python 程序员必须依靠它们来实现系统级功能，例如文件 I/O，此外还有大量以 Python 编写的模块，提供了日常编程中许多问题的标准解决方案。其中有些模块经过专门设计，通过将特定平台功能抽象化为平台中立的 API 来鼓励和加强 Python 程序的可移植性。

比如我们想要在课程中随机选一门课，这时候我们不需要自己再写一个random程序，而只需要导入标准库然后使用就可以了。举例如下：

```
import random
courses = ['History', 'Math', 'Physics','CompSci'] 
random_course = random.choice(courses)
print(random_course)
```

每次运行程序，我们都会随机得到一门课程的输出。

再比如，我们想要将度数转换为弧度角，并且计算它的sin值。这个也有写好的数学标准库可以直接实现这个功能，举例代码如下：

```
import math
rads = math.radians(90)
print(math.sin(rads))
```

运行程序我们就会输出90度弧度角的sin值：1.0。

另外，还有很多非常有用或者有趣的标准库。比如：os标准库可以对文件和目录访问进行操作；datetime和calendar可以提供日历和日期相关函数等等。

更多标准库的详细信息，可以查看这里：

[https://docs.python.org/zh-cn/3/library/index.html](https://docs.python.org/zh-cn/3/library/index.html)

* * *


---
title: Python教程--使用python发送邮件
tags:
  - email
  - python
  - smtplib
  - 教程
  - 编程
id: '889'
categories:
  - 技术杂谈
  - Python
date: 2021-01-16 12:13:13
---

**这篇文章中将会分享如何使用python来发送邮件的相关知识。**
<!-- more -->
如果你希望在用户创建帐户时向用户发送确认邮件，或向组织成员发送邮件以提醒他们支付会费。 这时候如果手动发送邮件将是一项耗时且容易出错的任务，但是使用Python就可以轻松实现自动化。

在使用oython来发送邮件之前，我们需要对邮箱进行一些设置，来允许使用第三方app来对它进行操作。如果使用的是gmail 的话打开这个网页进行设置： [https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps)

选择将允许不够安全的应用设置为启用（ allow less secure apps: on）。然后就可以使用python对邮箱进行操作了。

# **一、发送纯文本邮件**

Python内置了 [smtplib](https://docs.python.org/3/library/smtplib.html) 模块，用于使用简单邮件传输协议（SMTP）发送电子邮件。

当你通过Python发送电子邮件时，应确保你的SMTP连接已加密，以便其他人无法轻松访问你的邮件和登录凭据。有两种方法可以与你的电子邮件服务器建立安全连接：

*   使用不安全的SMTP连接，然后使用 `.starttls()` 加密
*   使用 `SMTP_SSL()` 建立安全的SMTP连接

首先，我们先来看一下第一种情况的一个例子：

```
import smtplib

with smtplib.SMTP('smtp.gmail.com', 587) as smtp:
smtp.ehlo()  # 身份确认
smtp.starttls() # 使用starttls进行加密
smtp.ehlo() # 再次确认身份
smtp.login('username@gmail.com','password') # 使用账号和密码登陆邮箱
subject = 'Grab dinner this weekend?' # 创建邮件主题
body = 'How about go for a dinner at 6pm this Sunday?' # 创建邮件内容
msg = f'Subject: {subject}\n\n{body}' 
smtp.sendmail('username@gmail.com','test@gmail.com',msg) # 发送邮件到test@gamil.com账户
```

这里使用的端口为587。大多数电子邮件提供商使用与本教程中相同的连接端口，但你可以使用Google搜索来进行快速确认。

再来看看使用SMTP\_SSL( )来进行加密的方法：

```
import smtplib

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
smtp.login('username@gmail.com','password') # 使用账号和密码登陆邮箱
subject = 'Grab dinner this weekend?' # 创建邮件主题
body = 'How about go for a dinner at 6pm this Sunday?' # 创建邮件内容
msg = f'Subject: {subject}\n\n{body}' 
smtp.sendmail('username@gmail.com','test@gmail.com',msg) # 发送邮件到test@gamil.com账户
```

是不是比第一种方法简单了一些。我们还可以使用email.message的库来对代码进行进一步的简化，使得更容易理解：

```
import smtplib
from email.message import EmailMessage

msg = EmailMessage()
msg['Subject'] = 'Grab dinner this weekend?' #  创建邮件主题
msg['From'] = 'username@gmail.com' #  设定发送邮箱
msg['To'] = 'test@gmail.com'  # 设定接收邮箱
msg.set_content('How about go for a dinner at 6pm this Sunday?') # 创建邮件内容

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
smtp.login('username@gmail.com','password')  # 使用账号和密码登陆邮箱
smtp.send_message(msg)  # 发送消息
```

是不是这些书写会变得更加有条理一些。这里注意最后一行代码改为了smtp.send\_message(msg) 这是因为我们在前已经设定好了发送邮箱和接收邮箱，只是发送的message所以用的send\_message的方法。

# **二、添加照片附件**

Python不仅可以发送纯文本邮件，还可以在邮件中添加附件。在这里我们以常见的照片和PDF格式的附件为例，来讲述怎么添加附件。首先我们来看看怎么发送单张图片：

```
import smtplib
from email.message import EmailMessage

msg = EmailMessage()
msg['Subject'] = 'Grab dinner this weekend?'
msg['From'] = 'username@gmail.com'
msg['To'] = 'test@gmail.com'
msg.set_content('Image attached...')

with open('1.jpg','rb') as f: # rb的意思是 read byte
file_data = f.read()
file_name = f.name()

msg.add_attachment(file_data, maintype = 'image', subtype = 'jepg', filename = file_name)

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
smtp.login('username@gmail.com','password')
smtp.send_message(msg)
```

如果是多张图片，并且格式不相同，我们可以将要发送的图片放入一个列表，然后使用imghdr的库来获取图片格式。举例如下：

```
import smtplib
import imghdr
from email.message import EmailMessage

msg = EmailMessage()
msg['Subject'] = 'The picture'
msg['From'] = 'username@gmail.com'
msg['To'] = 'test@gmail.com'
msg.set_content('Image attached...')
files = ['1.jpg','2.png']

for file in files:
with open(file,'rb') as f:
file_data = f.read()
file_type = imghdr.what(f.name)
file_name = f.name

msg.add_attachment(file_data, maintype = 'image', subtype = file_type, filename = file_name)

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:

smtp.login('username@gmail.com','password')

smtp.send_message(msg)
```

# **三、添加PDF附件**

添加PDF附件跟添加照片非常类似，唯一的不同的是文件格式的不同。PDF的maintype是'applicaton'，subtype是'octet-stream'。举例如下：

```
import smtplib
from email.message import EmailMessage

msg = EmailMessage()
msg['Subject'] = 'The PDF'
msg['From'] = 'username@gmail.com'
msg['To'] = 'test@gmail.com'
msg.set_content('Image attached...')

files = ['BIOS.pdf']
for file in files:
with open(file,'rb') as f:
file_data = f.read()
file_name = f.name
msg.add_attachment(file_data, maintype = 'application', subtype = 'octet-stream', filename = file_name)

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
smtp.login('username@gmail.com','password')
smtp.send_message(msg)
```

# **四、群发邮件**

如果需要把同一封邮件发送给多人，我们可以创建一个发送邮件的列表，然后将邮件的收件人改为列表即可。举例如下：

```
import smtplib
from email.message import EmailMessage

contacts = ['test1@gmail.com','test2@gmail.com',test3@gmail.com]
msg = EmailMessage()
msg['Subject'] = 'The PDF'
msg['From'] = 'username@gmail.com'
msg['To'] = ', '.join(contacts)
msg.set_content('Image attached...')

files = ['BIOS.pdf']
for file in files:
with open(file,'rb') as f:
file_data = f.read()
file_name = f.name
msg.add_attachment(file_data, maintype = 'application', subtype = 'octet-stream', filename = file_name)

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
smtp.login('username@gmail.com','password')
smtp.send_message(msg)
```

# **五、发送带格式的html邮件**

如果你要格式化电子邮件中的文本（粗体，斜体等），或者超链接或响应式的内容，则使用HTML非常方便。 由于并非所有电子邮件客户端都默认显示HTML内容，并且出于安全原因，有些人仅选择接收纯文本电子邮件，因此为HTML消息添加纯文本的替代非常重要。 由于电子邮件客户端将首先渲染最后一部分的附件，因此请确保在纯文本版本之后添加HTML消息。例如下面这个例子：

```
import smtplib
from email.message import EmailMessage

contacts = ['test1@gmail.com','test2@gmail.com',test3@gmail.com]
msg = EmailMessage()
msg['Subject'] = 'alternative'
msg['From'] = 'username@gmail.com'
msg['To'] = ', '.join(contacts)
msg.set_content('This is a plain text email')

msg.add_alternative("""\
<!DOCTYPE html>
<html>
<body>
<h1 style = "color:SlateGray;">This is an HTML Email!</h1>
</body>
</html>

""", subtype = 'html')   ## alternative will send first

with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:

smtp.login('username@gmail.com','password')

smtp.send_message(msg)
```

* * *


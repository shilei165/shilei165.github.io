---
title: 如何使用Taskkill命令来定时关闭任务或程序
tags: 
  - Taskkill
  - 定时任务
  - Windows
  - 编程
categories:
  - 技术杂谈
  - Windows
date: 2022-07-19 13:22:12
---

前面我们讲解了如何使用SCHTASK来定时执行Windows的特定程序，但是我们还无法做到定时关闭。比如我们希望每天晚上9点开始定时打开迅雷来下载东西，然后每天上午9点来关闭迅雷，以免在使用网络的时候对网速产生影响。于是，我们就需要用到Taskkill指令，并且结合我们之前学过的Schetask来定时执行程序关闭任务。这篇文章，我们就来一起学习一下如何使用这个指令吧

<!-- more -->
***
# **一、TASKKILL基本参数**
TASKKILL这个命令可以用来结束一个或多个进程。可以通过进程id或者进程图像名来结束进程。其常用参数如下：
```
  /S    system           指定要连接到的远程系统。
  /U    [domain\]user    指定应该在哪个用户上下文执行这个命令。
  /P    [password]       为提供的用户上下文指定密码。如果忽略，提示输入。
  /F                     指定要强行终止进程。
  /FI   filter           指定筛选进或筛选出查询的的任务。
  /PID  process id       指定要终止的进程的PID。
  /IM   image name       指定要终止的进程的影像名。通配符 '*'可用来指定所有图像名。
  /T                     Tree kill: 终止指定的进程和任何由此启动的子进程。
  /?                     显示帮助/用法。
```
# **二、程序image name和PID**
首先我们来了解一下什么是程序的image name和PID。简单来说，他们都是程序或进程的身份代码。image name是这个程序的名称，而PID (process identifier) 是这个程序所运行的进程的特有名称。一个程序可以对应多条进程。
比如迅雷的image name为'thunder.exe'，记事本的image name为'notepad.exe'。我们也可以通过Ctrl + Alt + Delete 键组合或者使用命令taskmgr来打开进程管理器去查看正在运行的进程。我们就可以在Details一栏中找到我们需要关闭的程序以及对应的PID。
![Task Manager](/images/taskmgr.png)
对于我们日常使用来说，使用image name和PID来对特定程序进行关闭已经完全足够。下面我列举出几个例子来说明如何使用image name和PID来关闭程序或进程：
```
taskkill /f /im QQ.exe           # 关闭QQ
taskkill /f /im thunder.exe /T   # 关闭迅雷和其所对应的所有子进程
taskkill /f /pid 4256            # 关闭chromdriver所对应的4256进程
```

# **三、筛选器的使用**
如果我们需要关闭多个进程，可以使用筛选器来提高筛选效率。具体参数如下：
```
筛选器名      有效运算符                  有效值
-----------   ---------------           --------------
STATUS        eq, ne                    运行 | 没有响应
IMAGENAME     eq, ne                    图像名
PID           eq, ne, gt, lt, ge, le    PID 值
SESSION       eq, ne, gt, lt, ge, le    会话编号
CPUTIME       eq, ne, gt, lt, ge, le    CPU 时间，格式为hh:mm:ss。
MEMUSAGE      eq, ne, gt, lt, ge, le    内存使用，单位为 KB
USERNAME      eq, ne                    用户名，格式为[domain\]user
MODULES       eq, ne                    DLL 名
SERVICES      eq, ne                    服务名
WINDOWTITLE   eq, ne                    窗口标题
```
注意: 1. 只有带有筛选器的情况下，才能跟 /IM 切换使用通配符 '\*'。
      2. 其中eq代表“equal", ne代表"not equal",gt代表"greater than",lt代表"less than", ge代表"greater than or equal to",le代表"less than or equal to"。
      3. 注意: 远程进程总是要强行终止，不管是否指定了 /F 选项。
举例如下：
```
TASKKILL /F /FI "PID ge 1000" /FI "WINDOWTITLE ne untitle*"
TASKKILL /F /FI "USERNAME eq NT AUTHORITY\SYSTEM" /IM notepad.exe
TASKKILL /S system /U domain\username /FI "USERNAME ne NT*" /IM *
TASKKILL /S system /U username /P password /FI "IMAGENAME eq note*"
```
# **四、结合SCHTASKS命令来定时关闭进程**
在了解了TASKKILL的基本用法之后，我们就可以结合SCHTASKS命令来定时关闭进程。比如我们就可以使用一下命令来定时在每天的17：30来关闭Python程序。
```
schtasks /create /sc daily /mo 1 /tn "STOP_Python" /tr "TASKKILL /F /IM py.exe /T" /st 17:30 /f
```
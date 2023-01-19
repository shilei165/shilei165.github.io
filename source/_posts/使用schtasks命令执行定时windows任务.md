---
title: 使用SCHTASKS命令执行定时Windows任务
tags:
  - SCHTASKS
  - windows
  - 定时任务
id: '988'
categories:
  - 技术杂谈
  - Windows
date: 2022-07-10 14:36:30
---

**这篇文章将会分享如何在Windows系统下使用SCHTASKS命令来定时执行任务（运行脚本）。**

有时候我们需要定期执行一些常规任务，比如上传或者删除文，运行Python或者其他脚本来实现一些自动功能（自动发送邮件、爬取数据、及时交易等等）。这时候，我们就需要在本地的电脑或者服务器上设置一些定时任务。Windows提供了界面化和Command Line指令两种方式来实现设置定时任务。

界面化的Task Scheduler 程序操作相对比较简单，可以满足大部分人的日常需求。但在某些特定条件下，比如脚本数量较多，需要管理多台电脑，或者没有显示接口的情况下，使用SCHTASKS 命令就会更加有效率。
<!-- more -->
* * *

# **一、SCHTASKS基本参数**

SCHTASKS 命令允许管理员创建、删除、查询、更改、运行和中止本地或远程系统上的计划任务，其常用参数包括：

```
SCHTASKS /Create                创建新的计划任务
SCHTASKS /Delete                删除计划任务
SCHTASKS /Query                 显示所有计划任务
SCHTASKS /Change                更改计划任务的属性
SCHTASKS /Run                   根据需要运行计划任务
SCHTASKS /End                   终止当前正在运行的计划任务
SCHTASKS /ShowSID               显示与计划的任务名称相应的安全标识符
```

接下来我们会逐条解释这些参数的使用方法。

# **二** **、创建新的本地计划任务**

```
SCHTASKS /Create [/S system [/U username [/P [password]]]]   
[/RU username [/RP password]] /SC schedule [/MO modifier] [/D day]   
[/M months] [/I idletime] /TN taskname /TR taskrun [/ST starttime]   
[/RI interval] [ {/ET endtime  /DU duration} [/K] [/XML xmlfile] [/V1]]   
[/SD startdate] [/ED enddate] [/IT  /NP] [/Z] [/F] [/HRESULT] [/?]
```

如果我们只是简单的使用本地计算机来执行定时任务，我们就可以省略掉 \[/S system \[/U username \[/P \[password\]\]\]\] \[/RU username \[/RP password\]\] 以及 \[/IT /NP\] 这一块的代码设置。电脑将会自动默认使用本地电脑和当前用户。下面是其他一些重要参数的解释：

```
/SC  schedule     指定计划频率:  MINUTE、 HOURLY、DAILY、WEEKLY、MONTHLY, ONCE, ONSTART, ONLOGON, ONIDLE, ONEVENT。
/MO  modifier     进一步改进计划类型以允许更好地控制计划重复周期。对于相应的计划频率，可以传递的数值如下
                  MINUTE:  1 - 1439 分钟.
                  HOURLY:  1 - 23 小时.
                  DAILY:   1 - 365 天.
                  WEEKLY:  1 - 52周.
                  MONTHLY: 1 - 12,或者FIRST, SECOND, THIRD, FOURTH, LAST, LASTDAY。
/I    idletime    指定运行一个已计划的 ONIDLE 任务之前要等待的空闲时间。有效值范围: 1 到 999 分钟。
/TN   taskname    指定唯一识别这个计划任务的名称。
/TR   taskrun     指定在这个计划时间运行的程序的路径和文件名。例如: C:\windows\system32\calc.exe    
/ST   starttime   指定运行任务的开始时间。时间格式为 hh:mm (24 小时时间)，例如 14:30 表示2:30 PM。如果未指定 /ST，则默认值为当前时间。/SC ONCE 必需有此选项。
/RI   interval    用分钟指定重复间隔。这不适用于计划类型: MINUTE、HOURLY、ONSTART, ONLOGON, ONIDLE, ONEVENT。有效范围: 1 - 599940 分钟。如果已指定 /ET 或 /DU，则其默认值为10 分钟。      
/ET   endtime     指定运行任务的结束时间。时间格式为 hh:mm (24 小时时间)，例如，14:50 表示 2:50 PM。这不适用于计划类型: ONSTART、ONLOGON, ONIDLE, ONEVENT。
/DU   duration    指定运行任务的持续时间。 时间格式为 hh:mm。这不适用于 /ET 和计划类型: ONSTART, ONLOGON, ONIDLE, ONEVENT。 对于 /V1 任务，如果已指定 /RI，则持续时间默认值为1 小时。
/K                在结束时间或持续时间终止任务。这不适用于计划类型: ONSTART、ONLOGON, ONIDLE, ONEVENT。 必须指定 /ET 或 /DU。
/SD   startdate   指定运行任务的第一个日期。格式为 yyyy/mm/dd。默认值为当前日期。这不适用于计划类型: ONCE、ONSTART, ONLOGON, ONIDLE, ONEVENT。
/ED   enddate     指定此任务运行的最后一天的日期。格式是 yyyy/mm/dd。这不适用于计划类型:ONCE、ONSTART、ONLOGON、ONIDLE。
/Z                标记在最终运行完任务后删除任务。
/F                如果指定的任务已经存在，则强制创建任务并抑制警告。
```

接下来我们举一些实际例子来解释如何使用这些指令。

**实例一：创建一个名字叫NotePad的计划任务，每天9点执行notepad.exe文件**

```
SCHTASKS /Create /SC Daily /ST 09:00 /TN NotePad /TR c:\windows\system32\notepad.exe /F
```

**实例二：创建一个名字叫SayHello的计划任务，首次运行时间为9点，然后每分钟执行一次hello.bat文件**

```
SCHTASKS /Create /SC minute /MO 1 /TN "SayHello" /TR "C:\hello.bat" /ST 09:00 /
```

# **三、查找计划任务**

当我们需要查找已创建计划任务的详细信息的时候，就会用到/Query指令。其常用参数如下：

```
/TN Taskname      指定要检索其信息的任务名称，否则会检索所有的任务名称。
/V                显示详细任务输出。
```

接下来我们举一个例子来熟悉如何使用这些指令。

**实例一：查找名字叫NotePad的计划任务**

```
SCHTASKS /Query /TN NotePad
```

其返回值（输出结果）如下：

```
C:\Users\Leilei>SCHTASKS /Query /TN notepad

Folder: \
TaskName                                 Next Run Time          Status
======================================== ====================== ===============
notepad                                  7/11/2022 9:00:00 AM   Ready
```

# **四、删除计划任务**

当我们需要删除一个已创建的计划任务的时候，就会用到/Delete指令。其常用参数如下：

```
/TN Taskname      指定需要删除任务名称。可以使用通配符“*”来删除所有任务。
/F                强制删除任务，并且抑制警告。
```

下面我们举一个例子来熟悉如何使用这些指令。

**实例一：删除名字叫NotePad的计划任务**

```
SCHTASKS /Delete /TN NotePad
```

其返回值（输出结果）如下：

```
C:\Users\Leilei>SCHTASKS /Delete /TN NotePad
WARNING: Are you sure you want to remove the task "NotePad" (Y/N)? y
SUCCESS: The scheduled task "NotePad" was successfully deleted.
```

# **五、批量安排计划任务(.bat file)**

相信上面所列举的SCHTASK指令可以满足我们绝大部分的日常需求。但对于同时管理多台电脑或多种程序的管理员来说，上面的代码并不具有可重复性。那么，我们就需要写一个bat程序来实现代码的可重复性。

下面就给出了一个例子，可以同时安排多个任务执行，并且将执行结果输出到txt文件中进行记录。

```
@ECHO off

REM ****************************************************************
REM
REM Script Name: Task_Sch.bat
REM Author: Leilei Shi
REM Date: July 9th, 2022
REM
REM Description: This script demonstrate how to schedule scripts using
REM WSindows scheduler service and the Schtasks command.
REM

REM ****************************************************************
REM ******** Script Initialization Section ****
REM Abort execution if OS is not Windows NT
IF NOT "%os%" == "Windows_NT" (
ECHO.
ECHO.
ECHO Unsupported Operating system
ECHO.
ECHO.
GOTO :EOF
)

REM Define a variable that specifies the location of this script log file.
SET ReportFile=C:\SchtasksReport.txt

REM ******** Main Processing Section ****
REM Call a procedure that logs this script's execution.
CALL :SetUpSchedLog

REM Call the procedure that sets up scheduled tasks.
CALL :SetUpSchedule

GOTO :EOF

REM ******** Procedure Section ****
REM This procedure writes a date and time entry to a report file
:SetUpSchedLog

ECHO %date% %time% Task_Sch.bat - Now executing. > %ReportFile%

GOTO :EOF

REM This procedure sets up the scheduled execution of other programs/scripts using the schtasks command.
:SetUpSchedule

SCHTASKS /Create /SC minute /MO 1 /tn "SayHello" /tr "C:\hello.bat" /ST 09:00 /F

        SCHTASKS /Create /SC Daily /ST 09:00 /TN NotePad /TR c:\windows\system32\notepad.exe /F

GOTO :EOF
```

接下来只需要在command line执行这个bat脚本，所有的任务将都会被同时执行。注意Windows Shell Script并不区分大小写，所以不必对字母的大小写太过在意。
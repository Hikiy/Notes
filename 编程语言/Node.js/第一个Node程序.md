# 第一个Node程序
如果你的电脑上已经安装了Sublime Text，或者Notepad++，也可以用来编写JavaScript代码，注意用UTF-8格式保存。

输入以下代码：
```
'use strict';

console.log('Hello, world.');
```

第一行总是写上```'usestrict'```;是因为我们总是以严格模式运行JavaScript代码，避免各种潜在陷阱。

保存为```hello.js```通过命令行启动：
```
C:\Workspace>node hello.js
Hello,world.
```


### 命令行模式和Node交互模式
直接输入node进入交互模式，相当于启动了Node解释器，但是等待你一行一行地输入源代码，每输入一行就执行一行。

直接运行node hello.js文件相当于启动了Node解释器，然后一次性把hello.js文件的源代码给执行了，你是没有机会以交互的方式输入源代码的。

<br /><br /><br /><br />
> github: https://github.com/Hikiy  
> 作者：Hiki  
> 创建日期：2019.04.25  
> 更新日期：2019.04.25

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  
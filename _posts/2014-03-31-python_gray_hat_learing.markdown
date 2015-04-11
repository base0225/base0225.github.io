---
author: wangguohao
comments: true
date: 2014-03-31 03:21:35+00:00
layout: post
title: python灰帽子学习
---

**1：实现一个debugger**

[https://github.com/Sn0rt/debugger](https://github.com/Sn0rt/debugger)
按照书上写的，利用windows内建的api可以实现一个简单的debugger。可以了得到如何从dll文件中导出函数，对我这样第一次接触windows api的人是很有帮助的。

**2:环境搭建**

在[这里](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pydbg)可以下载到pydbg，内建pydasm。

还有一些脚本import utils，第一次出现是在，access_violation_handler.py脚本中，但是导入失败了，其实了这个utils是pydbgi框架里面的一个工具集合，你可以在那里下载到。如果运行失败把C:\Python27\Lib\site-packages 下面的pydbg目录删除了，重新安装pydbg文件，好像是绕过了pydasm的版本问题。

**3：immunity debugger问题**

你拿到脚本发现运行时报错，imm不提供这个方法，其实手册写的就是有问题，去看源码吧，在安装目录下面ref可以看到源码的，其实是大小写错的了（吐槽了一下，中文的书 代码不是等宽毫无美感）。

**4：hooks function**

这是第一次接触hooks概念，但是很形象，有点类似于回调函数，当某个函数被触发启用钩子函数。soft hooks是导出自pydbg。硬钩子是导出自immunity imm的库。
但是我还是有点没有想明白，钩子的处理例程是和目标代码同步执行，还是EIP被转跳到钩子的处理例程，等钩子例程结束返回原来的程序。

**5：DLL and injection**

病毒的反分析技术最最基本可以通过windows 提供的调试判定函数(IsDeuggerPresent())，查看是否是是被调试状态,可以通过immlib.wirteMemory()完成参数的覆盖。
判别是否运行在虚拟机里面可以通过windows的Process32Frist()来举出第一个进程，用Process32Next()举出下一个进程，直到Process32Next()调用失败，找出类似于虚拟机拓展包的进程。
还有一个方法可以判别，好像仅仅是适用于判定virtualbox虚拟机，因为可以通过简单的汇编程序可以查到biso的字符串，完成virtualbox的虚拟机判别。这个还没有实现。


清除DEP（NtSetInformationProcess）的函数不是官方函数，this funcation export from the ntdll.dll,可以利用immunity debugger的python command提供的findantidep.py的脚本可以绕过，不过我还没有测试成功，正在尝试。
shellcode由msf生成很方便，此中乐趣一起挖掘吧，举个栗子，这样就可以


    # msfpayload windows/meterpreter/bind_tcp RHOST=192.168.1.123 LPORT=4444 N


还有meterpreter绝对是个好东西！不用可惜了

6：Fuzzing windows Drivers

本身对驱动开发就不是很了解，但是这个书写的让我对启动的工作有个了解，利用ioctl可以传输控制码进去，可以利用immunity debugger提供的库可以非常简单的获取道进行获取ioctl的操作码。

7：小结

这个书利用debugger为起始,让我对用python在windows平台进行逆向工程有了非常好的开始，不过里面的基本知识保质还可以，但是实验自己做起来就非常麻烦了。某些框架各种奇怪的依赖，而且还要新鲜编译，真是让人醉啊。

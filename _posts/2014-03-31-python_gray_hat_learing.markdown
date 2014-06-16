---
author: wangguohao
comments: true
date: 2014-03-31 03:21:35+00:00
layout: post
title: python灰帽子学习
wordpress_id: 108
categories:
- BINARY
---

**1：实现一个debugger**

[https://github.com/wangguohao/fuzzer](https://github.com/wangguohao/fuzzer)

**2:环境搭建**

在[这里](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pydbg)可以下载到pydbg，内建pydasm。

恩，还有一些脚本import utils，第一次出现是在，access_violation_handler.py脚本中，但是很明显导入失败了，其实了这个utils是paimei框架里面的，你可以在那里下载到。如果运行失败把C:\Python27\Lib\site-packages 下面的pydbg目录删除了，重新安装pydbg文件，好像是绕过了 pydasm 的版本问题。

**3：immunity debugger问题**

你拿到脚本发现运行时报错，imm不提供这个方法，其实手册写的就是有问题，去看源码吧，在安装目录下面ref可以看到源码的，其实是大小写错的了（吐槽了一下，中文的书 代码不是等宽毫无美感）。

**4：hooks function
**

钩子挺形象的，soft hooks， hard hooks，在imm里面有很多方法可以用，发挥想象，有待挖掘。

**5：DLL and injection**

MS无量，带有清除DEP（NtSetInformationProcess）的函数，哈哈 被逆向出来了吧，简单的反调试的调试，

还有msf配合的用就是很方便，此中乐趣一起挖掘吧（猥琐流），举个栗子，这样就可以

    
    # msfpayload windows/meterpreter/bind_tcp RHOST=192.168.1.123 LPORT=4444 N


还有meterpreter绝对是个好东西！不用可惜了

6：Fuzzing windows Drivers

本身对驱动开发就不是很了解，ioctl在linux下面看过报错，原来是用来控制设备的io的，作者好血腥暴力，穷举啊，我喜欢。

7：小结

我好嫩啊，我要回去继续学习，继续学习基础知识。

还有一点，最后一个章节距离好像还是远，而且fuzzer需求还不是很大，好像还有一个pyEum的仿真平台留待日后学习。



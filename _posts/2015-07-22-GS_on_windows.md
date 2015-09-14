---
author: wangguohao
comments: true
date: 2015-07-22
layout: post
tag: windows
title: GS & Bypass GS on windows
categories: security
---

###0x00 背景
计算机是个呆萌的设备分不清数据和执行代码，很多缓冲区溢出攻击就是利用”数据“去覆盖”指令”来控制计算机的执行流程。
栈的溢出比较常见主要原因是相对于堆溢出而言难度系数低，如果函数对输入参数不进行检查进行传参很容易应发栈溢出。

###0x01 GS诞生
为了防止既然程序员水平不容易提升，不如在系统机制上面做文章，GS（stack canary）就是windows在Visual C++ 2003中第一次引入的。

###0x02 

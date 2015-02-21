---
author: wangguohao
comments: true
date: 2015-02-26
layout: post
title: The configtion of bochs on gentoo
=======
title: Configtion of bochs on gentoo
categories: blog
---

在gentoo下面安装bochs适合use设置需要注意，debugger 和 dbg 不能并存， dbg这个use是是远程调试内核的设置代码桩必须的。
编程编译器用的是nasm，其余的没有太多要注意的了。
丢一份在bochs中进行调试的配置文件:
<pre><code>
开始 gdb 联合调试,这很重要
gdbstub: enabled=1,port=1234,text_base=0,data_base=0,bss_base=0

内存
megs: 32

软盘
floppya: 1_44=floppy.img, status=inserted
boot: a

启动设备为软盘
boot: floppy

鼠标不启用
mouse: enabled=0

CPU 配置
clock: sync=realtime
cpu: ips=1000000
</code></pre>

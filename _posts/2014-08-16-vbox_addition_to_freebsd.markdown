---
author: wangguohao
comments: true
date: 2014-08-16 01:41:30+00:00
layout: post
slug: vbox%e4%b8%adfreebsd%e7%9a%84%e5%a2%9e%e5%bc%ba
title: Vbox中FreeBSD的增强
wordpress_id: 461
categories:
- 计算机
---

目标：完成一个Freebsd在vbox的安装，可以使用vbox的additions的功能。


1：在vbox中安装Freebsd。

2：安装，配置vbox的additions包

    
    pkg_add -r virtualbox-ose-additions


3:配置Xorg.conf

    
    X -configure


会生成一个新的配置文件，这个和社区wiki有差别在freebsd9.2中，仅仅修改显卡部分使用vboxvideo就行了，鼠标不用修改一样可以工作。

    
      Section "Device"
          ### Available Driver options are:-
          ### Values: <i>: integer, <f>: float, <bool>: "True"/"False",
          ### <string>: "String", <freq>: "<f> Hz/kHz/MHz"
          ### [arg]: arg optional
          Identifier  "Card0"
          Driver      "vboxvideo"  (修改)
          VendorName  "InnoTek Systemberatung GmbH"
          BoardName   "VirtualBox Graphics Adapter"
          BusID       "PCI:0:2:0"
      EndSection


社区wiki上面说使用拷贝HAL fdi文件：90-vboxguest.fdi ，我并没有这样做一样可以的。

4：安装一个DE

    
    pkg_add -r xorg
    pkg_add -r gnome2


如果有足够的空间不推荐最小化安装，这样可以避免一些不必要的麻烦。而且是先安装X ，后安装G2

6：配置rc.conf

    
    #vbox的客户机服务
    vboxguest_enable="YES"
    vboxservice_enable="YES"
    #为X 提供功能服务
    hald_enable="YES"
    dbus_enable="YES"
    #为G2提供功能的服务
    gdm_enable="YES"
    gnome_enable="YES"


7：开启vbox拓展功能

    
    # VBoxClient –clipboard 共享剪切板
    # VBoxClient –display 自动调整分辨率
    # VBoxClient –seamless 启动seamless窗口模式


8：错误配出

具体情况具体分析，X server可以参考日志，G2目前没有遇到过太多的问题。

    
    less /var/log/Xorg.0.log




**Reference:**

https://wiki.freebsdchina.org/software/v/virtualbox-additions

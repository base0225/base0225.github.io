---
author: wangguohao
comments: true
date: 2015-01-01 11:57:53+00:00
layout: post
title: linux下的sublime3中文问题
categories:
- 计算机
---

sublime3原生好像不支持中文显示，于是网上搜索一下能可行的办法，发现了一个靠谱的方案解决方法记录如下：

运行sublime3，输入快捷建C+S+P,会有一个横的条目显示出来，光标向下可以看见package control，选择“package control：install package” ，可以观察到“原来的横栏变成了可安装的拓展包的列表”，安装Codecs33，然后安装ConvertToUTF8（这两个包有依赖关系的，先后错了不能正常工作），然后在File菜单下面看见，set file encode to，问题到这里差不多就解决了。

sublime 字体好像很奇怪，在“Preferences”下面点击“setting-user”，会弹出一个文本等待编辑，我的修改如下

    
    {
    	"color_scheme": "Packages/Color Scheme - Default/Zenburnesque.tmTheme",
    	"font_face": "consolas",
    	"font_size": 12,
    }
    


修改完成，C+s就生效了，字体是事先从C:\windows\system32\Font下面复制至/usr/share/font/的。

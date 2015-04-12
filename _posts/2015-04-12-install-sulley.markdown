---
layout: post
title: Tnstall sulley
category: blog
description: 技能学习之道
---

sulley是一个过去比较出名的fuzz框架，但是已经停止支持了，我第一接触是在python灰帽子那本书里面，但是安装这个框架过程真的是很痛苦！各种依赖过期，我想找个尽量简单的办法去解决这个问题
sulley 各种依赖可以在[here](https://github.com/reider-roque/pydbg-pydasm-paimei)下载到，按照readme进行操作就可以了。

***安装pcapy

先安装vs2010,然后设置环境变量，通过这条指令 SET VS90COMNTOOLS=%VS100COMNTOOLS%.下载[pcapy](https://pypi.python.org/pypi/pcapy),还要下载winpcap的SDK,并配置进vs2010，才可以进行编译。

***安装impacket

重[这里](https://pypi.python.org/pypi/impacket)下载，然后python setup.py install就可以安装.

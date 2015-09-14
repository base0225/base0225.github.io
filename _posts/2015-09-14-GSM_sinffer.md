---
author: wangguohao
comments: true
date: 2015-09-14
layout: post
tag: wireless
title: GSM嗅探
categories:
---

###0x00 道具

一个Linux系统，摩托罗拉c118，T型minusb数据线，Motorola c118/c123 数据连接线，FT232 USB转串口TTL，Motorola C118/c123 滤波器套件(这个需要焊接技能，换滤波器可以捕获uplink信号)，感谢曾哥赞助这些玩具，可以到淘宝店一次买全。

###0x01 环境准备
```shell
mkdir gprs_sniffer
cd gprs_sniffer
git clone git://git.osmocom.org/osmocom-bb.git
git clone git://git.osmocom.org/libosmocore.git
git clone git://git.srlabs.de/gprsdecode.git
```
下载交叉编译环境[here](http://pan.baidu.com/share/link?shareid=739532605&uk=2955852660)，某些64位的linux需要安装32的glibc的库。
实验环境准备好，目录是这个样子
![dir](/media/pic/GSM/file_ready.png)

###0x02 调试环境
```shell
tar xf bu-2.15_gcc-3.4.3-c-c++-java_nl-1.12.0_gi-6.1.tar.bz2
export PATH=$PATH:/root/gprs_sniffer/gnuarm-3.4.3/bin
```
解压交叉编译环境准备设置变量.


```shell
cd libosmocore
autoreconf -i
./configure
make
sudo make install
```
autoreconf工具由automake的包提供的。

```shell
cd gprsdecode
make
```
这个模块并没有注意到什么地方用，是按照参考文档上的敲的。


```shell
cd osmocom-bb
git checkout --track origin/luca/gsmmap
vim /root/gprs_sniffer/osmocom-bb/src/target/firmware/Makefile 	#把CFLAGS += -DCONFIG_TX_ENABLE前的注释符号去掉
cd src
make -j8

```
处理OsmocomBB分支问题，亲测luca/gsmmap可编译通过，而且需要把mocom-bb/src/target/firmwire/下的Makefile中的 CONFIG_TX_ENABLE宏注释取消掉，不然一直在扫描没有结果。

###0x03 开始测试GSM嗅探

```shell
cd host/osmocon/
sudo ./osmocon -p /dev/ttyUSB0 -m c123xor ../../target/firmware/board/compal_e88/layer1.compalram.bin
```
先把c118关机，然后确认和电脑的连接正常，输入上面的命令，按一下c118的红色电源键，刷入layer1的固件，会看到c118的手机屏幕显示。

```shell
cd osmocom-bb/src/host/layer23/src/misc
sudo ./cell_log -O
```
扫描基站信息，PWR数值越大信号越好，注意是负数。

```shell
sudo ./ccch_scan -i 127.0.0.1 -a 56
```
然后使用ccch_scan进行抓包，-a参数为指定ARFCN号

```shell
sudo wireshark -k -i lo -f 'port 4729'
```
注意wireshark的filiter的写成GSM_SMS.

###0x04 效果图
![sms](/media/pic/GSM/gsm.png)


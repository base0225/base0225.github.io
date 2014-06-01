---
author: wangguohao
comments: true
date: 2014-03-31 11:58:51+00:00
layout: post
title: Assembly Language for Intel-Based Computers 开发环境搭建
categories:
---

几经弯路，终有找到了简单易上手的方法了，利用masm32 和 vs 210 配合的用。

**一：安装**

安装VS 2010,使用下载器，安装很快，选择c++版的，其余的不是必须的而且很占空间，发现使用安装器下载比较方便,要用是旗舰版，不然后面不可以安装语法高亮插件。

安装masm32[这里](http://www.masm32.com/)，下载默认安装就好，然后点击[Examples.tar](http://linux323.tk/wp-content/uploads/2014/03/Examples.tar.gz)，把include里面文件放置到masm32安装目录下面的include里面，lib目录也是如法炮制。



**二：配置开发环境
**

1：新建项目：建立一个空项目。

2：鼠标右击项目名称，看到“生成自定义“选项，选择“masm”

3：鼠标放置在项目名称上面，右击点击属性，可以配置连接器，汇编器。

3：配置链接器：继续鼠标右击项目名称，看到“属性”选项，找到“链接器”，“常规”树状选项，编辑附加库目录，如“C:\masm32\lib”，然后在“输入”的树状选项卡，编辑“附加依赖库”选项，在后面添加“Irvine32.lib；kernel32.lib”；在“系统”选项，“子系统”修改为“控制台”，重点就是连接器配置了。

4：配置汇编器：在“链接器”下面，看到“MicroSoft Macro Assembler ”，点击“General”，编辑"include path",如"C:\masm32\include“.

4:建立文件：然后把鼠标放在"源文件"上面，右键“添加”，新建一个.asm结尾的文档就好

基本配置就是这样了，但是实模式的库是没有包含进来的

注意：要先在源文件目录下面建立.asm文件，然后看项目属性才会看到“MicroSoft Macro Assembler”。

**三：配置后，使用**

1：断点功能，举个栗子

    
    INCLUDE Irvine32.inc
    
    .code
    main PROC
    
    	mov eax,10000h		; EAX = 10000h
    	add eax,40000h		; EAX = 50000h
    	sub eax,20000h		; EAX = 30000h
        call DumpRegs
    
    	exit
    main ENDP
    END main%


断点，在call DumpRegs，然后在“调试”选项卡下面，“窗口选项”点击“寄存器”，快捷键“Alt-5”,你就可以看见寄存器了。

2：键盘映射，我喜欢emacs的快捷键，用的很舒服，在[这里](http://visualstudiogallery.msdn.microsoft.com/09dc58c4-6f47-413a-9176-742be7463f92/)有插件，实现了最基本的功能，目前还在调试中，刚上手有好多问题的，简单的移动光标，就和VS自己带的快捷键有冲突，C+b 插入断点，不能后退

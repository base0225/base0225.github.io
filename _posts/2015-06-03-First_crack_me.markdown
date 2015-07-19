---
author: Sn0rt
date: 2015-06-21 20:59:21
layout: post
title: 我的第一个crackme
tag: binary
category: 安全
---

之前在看0day安全,准备学一点binary安全的知识.

#0x01

需要用到的工具IDA,OD,还有一个算地址偏移的计算器,和需要被crack的对象.

{% highlight c%}

#include <stdio.h>
#define PASSWORD "1234567"
int verify_password (char *password)
{
	int authenticated;
	authenticated=strcmp(password,PASSWORD);
	return authenticated;
}

main()
{
	int valid_flag=0;
	char password[1024];
	while(1)
	{
		printf("please input password:       ");
		scanf("%s",password);
		valid_flag = verify_password(password);
		if(valid_flag)
		{
			printf("incorrect password!\n\n");
		}
		else
		{
			printf("Congratulation! You have passed the verification!\n");
			break;
		}
	}
}
{% endhighlight %}

#0x02

IDA可以帮助我们静态分析代码,OD动态分析,地址偏移计算机可以帮助我们找内存中的指令在文件中的哪个位置.

#0x03

```asm
74 0f   JE SHORT crack_me.offset
```

汇编指令道机器码转译,修改关键转跳部分,控制程序流向.

```asm
jne SHORT crack_me.offset
```

利用地址偏移计算机进行计算,内存VA对应的机器指令对用文件在磁盘上的头部的偏移.
文件偏移地址 = 虚拟内存地址(VA) - 装载基址(Image Base) - 节偏移
其中exe文件的装载基址0x00400000,dll文件的是0x10000000,这些定义在windows系统编译过程中.
节偏移需要pe工具看,还是用工具计算省心.

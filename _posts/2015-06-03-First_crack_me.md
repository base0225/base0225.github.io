---
author: Sn0rt
date: 2015-06-21 20:59:21
title: the first crack me
---

֮ǰ�ڿ�0day��ȫ,׼��ѧһ��binary��ȫ��֪ʶ.

#0x0001

��Ҫ�õ��Ĺ���IDA,OD,����һ�����ַƫ�Ƶļ�����,����Ҫ��crack�Ķ���.

```c
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
````

#0x0002

IDA���԰������Ǿ�̬��������,OD��̬����,��ַƫ�Ƽ�������԰����������ڴ��е�ָ�����ļ��е��ĸ�λ��.

#0x0003

```asm
74 0f   JE SHORT crack_me.offset
```
���ָ���������ת��,�޸Ĺؼ�ת������,���Ƴ�������.

```asm
jne SHORT crack_me.offset
```
���õ�ַƫ�Ƽ�������м���,�ڴ�VA��Ӧ�Ļ���ָ������ļ��ڴ����ϵ�ͷ����ƫ��.
�ļ�ƫ�Ƶ�ַ = �����ڴ��ַ(VA) - װ�ػ�ַ(Image Base) - ��ƫ�� 
����exe�ļ���װ�ػ�ַ0x00400000,dll�ļ�����0x10000000,��Щ������windowsϵͳ���������.
��ƫ����Ҫpe���߿�,�����ù��߼���ʡ��.

---
title: C语言资源使用的代码框架
date: '2023-09-05 22:10:36'
toc: true
indexing: false
abbrlink: 5b67
categories:
  - Reverse
---


C 语言使用资源时建议采用下面的代码框架。

```c
#include <stdio.h>
#include <malloc.h>

#define SAFE_FREE_PTR(p) if(NULL!=(p)){free(p);(p)=NULL;}
int main(int argc, char* argv[], char* env[])
{
	// 1. 引用资源的变量在作用域开始处定义，并初始化为错误值。
	int *p_a = NULL;
	int *p_b1 = NULL;
	int *p_b2 = NULL;
	int *p_c = NULL;

	// 2. 申请资源后， 必须检查申请的资源是否有效，无效则需要处理错误。
	p_a = (int*)malloc(sizeof(int));
	if (NULL == p_a)
	{
		// 处理错误的代码
		// ...
		// 3. 处理完错误后，流程转移到统一的退出代码。
		goto EXIT_POINT;
	}

	// 4. 使用资源代码
	*p_a = 0;
	if (argc > 3)
	{
		p_b1 = (int*)malloc(sizeof(int));
		if (NULL == p_b1)
		{
			goto EXIT_POINT;
		}
	}
	else
	{
		p_b2 = (int*)malloc(sizeof(int));
		if (NULL == p_b2)
		{
			goto EXIT_POINT;
		}
	}

	p_c = (int*)malloc(sizeof(int));
	if (NULL == p_c)
	{
		goto EXIT_POINT;
	}
EXIT_POINT:
	SAFE_FREE_PTR(p_c);
	SAFE_FREE_PTR(p_b2);
	SAFE_FREE_PTR(p_b1);
	// 5. 释放资源。在释放资源前必须检查该资源是否有效，无效则不需要处理。
	if (NULL != p_a)
	{
		free(p_a);
		// 6. 释放资源后将引用资源的变量重置为错误值。
		p_a = NULL;
	}
	

	return 0;
}
```


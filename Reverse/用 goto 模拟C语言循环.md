---
title: 用 goto 模拟C语言循环
date: "2023-06-20 23:40:31"
tags: c
categories: 
  - Reverse
description: "介绍各个平台上常用的编译器。"
toc: true
indexing: false
---

# 用 goto 模拟C语言循环

在C语言中常见的循环有`do-while`、`while` 和 `for` 循环。C语法规定了各个循环的执行顺序，下面研究一下汇编的实现方式。最后我们通过 `if + goto` 的判断结构来模拟实现三种循环。

# do-while

```c
do 
{
    num--;
} while (num > 0);
```

在C的语法中，第一步设置循环变量的值，第二步执行循环体的内容，最后一步为判断是否继续循环。

![image-20230620071444784](https://pics-place.oss-cn-shanghai.aliyuncs.com/picimage-20230620071444784.png)

从汇编的实现中，可以确定第一步为设置循环变量：

```assembly
0040D708   mov         dword ptr [ebp-4],3
```

第二步为执行循环体中的内容：

```assembly
0040D70F   mov         eax,dword ptr [ebp-4]
0040D712   sub         eax,1
0040D715   mov         dword ptr [ebp-4],eax
```

最后一步为判断是否继续循环：

```assembly
0040D718   cmp         dword ptr [ebp-4],0
0040D71C   jg          main+1Fh (0040d70f)
```

**goto  模拟：**

```c
	int num2 = 3;
DO:
	num--;
	if (num2 > 0)
	{
		goto DO;
	}
```



# while

```c
int n = 0;
while(argc < 0) 
{
    n++;
}
```

直接查看汇编代码：

![image-20230620220227832](https://pics-place.oss-cn-shanghai.aliyuncs.com/picimage-20230620220227832.png)

**goto模拟：**

```c
	int n = 0;
WHILE_START:
	if (argc >= 0) // 汇编中jge
	{
		goto WHILE_END;
	}
	n++;
	goto WHILE_START;
WHILE_END:
```

**循环判断时，采用汇编 `jge` 指令，和源码中的判断相反。**

# for

```c
for (int n = 0; n < 10; n++)
{
    argc++;
}
```

在C语法中，`int n = 0`先执行。第二步执行判断，第三步执行循环体，第四步执行 `n++`。

观察汇编代码：

![image-20230620231645319](https://pics-place.oss-cn-shanghai.aliyuncs.com/picimage-20230620231645319.png)

**goto 模拟：**

```assembly
FOR_INIT:
	int n = 0;
	goto FOR_CMP;
FOR_STEP:
	n++;
FOR_CMP:
	if (n >= 10)
	{
		goto FOR_END;
	}
FOR_BODY:
	argc++;
	goto FOR_STEP;
FOR_END:
```



# 效率之说

经过汇编代码的分析和使用`goto`模拟，我们可以发现 `do-while` 在汇编代码量以及需要跳转的次数最少，`for` 无论在汇编代码量上还是在跳转次数上都是最多的，所以我们可以说 `do-while` 的效率最高，`for` 效率最低。

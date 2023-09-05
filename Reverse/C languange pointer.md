---
title: C language pointers
date: '2023-09-02 15:34:30'
toc: true
indexing: false
abbrlink: 7b60
categories:
  - Reverse
---

### `ptr + n`

```
type +ptr = ...;
int n = ...;
ptr + n = (int)ptr + sizeof(typr)*n
        = (const type*)((int)ptr + sizeof(typr)*n)
```

![image-20230828210904043](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308282109059.png)

### `ptr - ptr`

计算两个地址间元素的个数。

```
type *ptrA, *ptrB = NULL;
ptrA - ptrB = ((int)ptrA - (int)ptrB) / sizeof(type)
```

![image-20230828212131877](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308282121917.png)

### 指针和数组

指针和数组都可以使用下标访问，效果是一样的。Debug 下指针的汇编代码比数组多，优化版本下，直接使用数组下标访问代替指针访问。

```c
type ary[N];
ptr = ary; // 数组名是数组第 0 个元素的指针常量。

ptr[n] = *(const type*)((int)ptr + sizeof(typr)*n)
ary[n] = *(const type*)((int)ptr + sizeof(typr)*n)
```

![image-20230828210507094](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308282105116.png)

### 函数指针

函数类型一致需要满足以下条件：

1. 参数序列一致（参数个数、类型、顺序一致）；
2. 调用约定一致；
3. 返回值类型一致。

```c
#include <stdio.h>

void sortA(int buf[], int num)
{
	printf("冒泡法");
}

void sortB(int buf[], int num)
{
	printf("选择法");
}

int main(int argc, char* argv[], char* env[])
{
	int ary[] = {1,2,5,4,3};
	// 定义函数指针
	void (__cdecl*pfn_sort)(int[], int) = NULL;
	pfn_sort = sortA;
	pfn_sort(ary, 5);
	return 0;
}
```

用 `typedef` 来定义一个新的函数指针类型，用这个类型就可以定义变量，从而调用函数指针，上例可以改为：

```c
#include <stdio.h>

typedef void (__cdecl*PFN_SORT)(int[], int);
void sortA(int buf[], int num)
{
	printf("冒泡法");
}

void sortB(int buf[], int num)
{
	printf("选择法");
}

int main(int argc, char* argv[], char* env[])
{
	int ary[] = {1,2,5,4,3};
	// 定义函数指针
	PFN_SORT pfn_sort = NULL;
	pfn_sort = sortA;
	pfn_sort(ary, 5);
	return 0;
}
```

在 VC6 中的 Debug 版本会在 0x1000+ 处开始记录自定义的函数指针表。

![image-20230829215608866](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308292156897.png)



### 二维数组指针

 

```c
#include <stdio.h>

int main(int argc, char* argv[], char* env[])
{
	int ary[3][4] = {
		{1,2,3,4},
		{10,20,30,40},
		{100,200,300,400}
	};
	
	// 数组名是第0个元素的指针常量
	// 二维数组的元素为一维数组
	// int[3][4]的元素为int[4]
	// ary是int[4]类型的指针常量 
	printf("%p\r\n", ary);
	// (int)&ary + sizeof(int[4])*1
	printf("%p\r\n", ary + 1);

	// *ary得到int[4]一维数组
	// *ary是int类型的指针常量 
	printf("%p\r\n", *ary);
	// (int)&ary + sizeof(int)*1
	printf("%p\r\n", *ary + 1);
	// int tmp[4] = {1,2,3,4};
	// printf("%p\r\n", tmp + 1);


	// 任何类型的变量取地址得到该类型的指针
	// &ary取地址得到int[3][4] 类型的指针
	printf("%p\r\n", &ary);
	// (int)&ary + sizeof(int[3][4])*1
	printf("%p\r\n", &ary + 1);

	// 二维数组的元素为一维数组
	// ary[0]是int[4]数组
	printf("%p\r\n", ary[0]);
	// 数组名是第0个元素的指针常量
	// ary[0]是int类型的指针常量
	printf("%p\r\n", ary[0]+1);

	// 数组名是第0个元素的指针常量
	// ary是第int[4]的指针常量
	// ary+1 = (int)&ary + sizeof(int[4])*1
	// (*(ary+1)) 得到int[4]数组
	printf("%p\r\n", (*(ary+1))[1]);

	printf("%p\r\n", ary[0][0]);
	return 0;
}
```

![输出结果](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202309021404225.png)

#### 定义二维数组指针

```c++
#include <stdio.h>

int main(int argc, char* argv[], char* env[])
{
	int ary[3][4] = {
		{1,2,3,4},
		{10,20,30,40},
		{100,200,300,400}
	};
	
	int (*p)[4] = ary;
	int (*p_ary)[3][4] = &ary;

	// p是int[4]类型的指针
	printf("%p\r\n", p);
	// *p是int[4]数组
	printf("%p\r\n", *p);
	// p_ary是int[3][4]类型的指针
	printf("%p\r\n", p_ary);
	// *p_ary是int[3][4]数组
	printf("%p\r\n", *p_ary);
	// *p_ary是int[3][4]数组
	// 数组名是第0个元素的指针常量
	// *p_ary是int[4]类型的指针常量
	// **p_ary是int[4]数组
	// **p_ary是int类型的指针常量
	printf("%p\r\n", **p_ary);

	// p是int[4]类型的指针
	// (int)p + sizeof(int[4])*1
	printf("%p\r\n", p + 1);
	// *p是int[4]数组
	// *p是int类型的指针常量
	// (int)p + sizeof(int)*1
	printf("%p\r\n", *p + 1);
	// p_ary是int[3][4]类型的指针
	// (int)p_ary是int + sizeof(int[3][4])*1
	printf("%p\r\n", p_ary + 1);
	// *p_ary是int[4]类型的指针常量
	// (int)p_ary是int + sizeof(int[4])*1
	printf("%p\r\n", *p_ary + 1);
	// **p_ary是int类型的指针常量
	// (int)p_ary是int + sizeof(int)*1
	printf("%p\r\n", **p_ary + 1);
	return 0;
}
```

![image-20230902143650898](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202309021436933.png)

### 二级指针

二级指针：指向指针的指针。

```c
#include <stdio.h>

int main(int argc, char* argv[], char* env[])
{
	int ary[3][4] = {
		{1,2,3,4},
		{10,20,30,40},
		{100,200,300,400}
	};

	int n = 100;
	int *p = &n;
	int **pp = &p;
	printf("%p\r\n", p);
	printf("%p\r\n", pp);
	printf("%p\r\n", *p);
	printf("%p\r\n", *pp);
	printf("%p\r\n", **pp); 
	// 和*间接访问的效果一样
	printf("%p\r\n", pp[0]);
	printf("%p\r\n", pp[0][0]);

	return 0;
}
```

![image-20230902145616848](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202309021456892.png)

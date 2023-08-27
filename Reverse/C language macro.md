---
title: C language macro
date: '2023-08-27 20:48:13'
toc: true
indexing: false
abbrlink: d0d7
categories:
  - Reverse
---



# 1. 数值宏

用宏来表示一个数值。

一般用于：

1. 定义一个变量值。
2. 用有意义的单词句来代替表示不容易记忆和使用的数值，如错误码。

```c
#include <stdio.h>

#define PI 3.1415916
int main()
{
	double area = PI*3*3;
	printf("area = %f\r\n", area);
	return 0;
}
```

# 2. 表达式宏

用宏来表示一个表达式。**注意!!! 宏参数和宏表达式必须加括号**。

```c
#include <stdio.h>

#define PI 3.1415916
#define GET_AREA(r) (PI*(r)*(r))
int main()
{
	double area = GET_AREA(3);
	printf("area = %f\r\n", area);
	return 0;
}
```

**错误：宏表达式未加括号。**

```c
#include <stdio.h>

#define PI 3.1415916
#define GET(r) PI+(r)+(r)
int main()
{
	double area = GET(3)*99;
	printf("area = %f\r\n", area);
	return 0;
}
```

**错误：宏表达式参数未加括号。**

```c
#include <stdio.h>

#define PI 3.1415916
#define GET(r) (PI*r*r)
int main()
{
	double area = GET(1.2+1.8);
	printf("area = %f\r\n", area);
	return 0;
}
```

# 3. 语句块宏

用宏来表达一个语句块。

```c
#include <stdio.h>

#define PRINT_INT(x) \
printf("*************************\r\n");\
printf("%d\r\n", (x));\
printf("*************************\r\n")
int main()
{
	PRINT_INT(338+2);
	return 0;
}
```

这也是为什么要求 for while 等语句即时循环体中只有一条语句也必须使用大括号，否则遇到语句块宏可能出现问题，如：

```c
#include <stdio.h>

#define PRINT_INT(x) \
printf("*************************\r\n");\
printf("%d\r\n", (x));\
printf("*************************\r\n")
int main()
{
	for (int i = 0; i < 3; i++)
		PRINT_INT(338+2);
	
	return 0;
}
```

 输出结果为：

```c
*************************
*************************
*************************
340
*************************
```



# 4. 说明性(注释)宏

用空的宏来添加注释。

```c
#include <stdio.h>

#define EXPORT
#define IN

EXPORT int func(IN int a, IN int b)
{
	return a + b;
}
int main()
{	
	return func(1,2);
}
```

# 5. 兼容性宏

提高代码的兼容，方便以后修改。只用修改一处，则处处修改。

```c
#include <stdio.h>

#define INT16 short int
#define INT32 int
int main()
{
	INT32 n = 3;
	printf("%d\r\n", INT32);
	
	return 0;
}
```

- `##`字符串连接符。可以用于字符串的连接。

- `#`表示参数强制解释为字符串。

  ```c
  #include <stdio.h>
  
  #define func(type, value) func_##type(#value)
  int main()
  {
  	func(int, 12);
  	
  	return 0;
  }
  ```

  使用`/P`查看宏替换后为：

  ```c
  
  int main()
  {
  	func_int("12");
  	
  	return 0;
  }
  
  ```



## 5.1. C 语言模板原型

```c
#include <stdio.h>

#define def_func(type) \
type func_##type(type param1) \
{ \
	return (type)3.14*param1; \
}

#define call_func(type, value) func_##type(value)

def_func(int)
def_func(double)
int main()
{
	printf("%d\r\n", call_func(int, 12));
	printf("%f\r\n", call_func(double, 12));
	return 0;
}
```



# 6. 预编译宏

条件编译，满足某些定义才会进行编译。

```c
#include <stdio.h>

#define Debug

int main()
{
#ifdef Debug
	printf("debug info: hello world!\r\n");
#endif
	return 0;
}
```



# 7. 编译选项宏

在 `cl.exe` 编译器参数增加编译选项。

命令行一般命令为：`cl.exe /c /D"Debug" main.cpp`。 

![image-20230827145555076](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308271455108.png)

# 8. 编译器内置宏

关键字`undef` 能够取消定义，然后可以重新定义。

## 8.1 内置宏

- `_DEBUG` 和 `_NDEBUG` ：表示 **Debug** 版本和 **Release** 版本。

- `__FILE` 和 `__LINE__`：表示当前文件全路径和当前行数。一般会配合`_DEBUG`宏一同使用。

- `_MSC_VER`：编译器版本号。

  ```c
  #include <stdio.h>
  
  int main()
  {
  #ifdef _DEBUG
  	printf("cl version: %d\r\n", _MSC_VER);
  	printf("%s:%d\r\n", __FILE__, __LINE__);
  #endif
  	printf("Hello World!\r\n");
  	return 0;
  }
  ```

  输出结果为：

  ```c
  cl version: 1200
  C:\Users\lyu\Desktop\TestVC6\main.cpp:7
  Hello World!
  ```

  
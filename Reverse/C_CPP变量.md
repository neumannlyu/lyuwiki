变量一般可以简单分为局部变量和全局变量。在这个基础可以使用 *静态* 来修饰这些变量。

# 局部变量

| 类型         | 作用域                     | 生命周期                                 |
| ------------ | -------------------------- | ---------------------------------------- |
| 函数局部变量 | 局部变量定义开始到函数结束 | 进入函数时分配空间，退出函数时释放空间。 |
| 块局部变量   | 变量定义开始到块结束       | 进入函数时分配空间，退出函数时释放空间。 |



# 全局变量

| 类型         | 作用域         | 生命周期                       |
| ------------ | -------------- | ------------------------------ |
| 普通全局变量 | 工程项目作用域 | 从进程模块开始到进程模块退出。 |
| 静态全局变量 | 本源码文件     | 同全局变量                     |

一般情况下程序可以分为四个区，分别为代码区、数据区、栈区以及堆区。

数据区又分为已初始化（常量区）、未初始化分区。

如果全局变量给了初值则会被分配到已初始化区（0值除外）；如果没有给定初值则会被分配到未初始化区。**如果初始化值为零值，仍会被分配到未初始化区。**

**已初始化的变量会在可执行文件中分配对应的空间。**

## 已初始化全局变量

```c++
#include <stdio.h>

int g_number = 0x12345678;
int main()
{
	printf("hello world\n");
	return 0;
}
```

已初始化的全局变量是在编译时被写入到可执行文件中，在装载可执行文件时直接被装载到内存中。

![文件中的全局变量](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305083.png)

 `g_number ` 在内存中的地址为 `0x00424d8c`，所以其对应的文件偏移量为 **内存地址 - 内存基址** `0x00024d8c`。

## 未初始化全局变量

可支持程序中记录了未初始化的空间，在装载可执行程序时分配空间。



# 静态变量

## 静态全局变量

受编译器约束的全局变量。

## 静态局部变量

| 类型         | 作用域         | 生命周期   |
| ------------ | -------------- | ---------- |
| 静态局部变量 | 同函数局部变量 | 同全局变量 |

静态全局变量的生命周期同全局变量，也就是说在函数内定义的静态局部变量的初值会被写入到可执行程序中。

```c++
#include <stdio.h>

int g_number = 0x12345678;
int main()
{
	static int number = 0x11223344;
	printf("%p\n", &number);
	return 0;
}
```

![image-20230709170829068](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305084.png)



## 名称重组

静态变量的生命周期同全局变量，那么在不同作用域定义的静态局部变量是如何实现区分的呢？

```c++
#include <stdio.h>

int g_number = 0x12345678;

void func()
{
	static int number = 0x55667788;
	{
		static int number = 0x1234;
		printf("%p\n", number);
	}
	printf("%p\n", number);
}

int main()
{
	static int number = 0x11223344;
	printf("%p\n", number);
	func();
	return 0;
}
```

![image-20230709181611893](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305085.png)

可以看出静态局部变量即使命名一样的情况下，编译器仍然可以区分每个静态局部变量。这是由于编译器使用名称重组（又称名称粉碎）机制。

**不同的编译器可能会采用不同的名称重组机制，只要能区分变量即可。**所以VC6.0的C名称重组机制和C++名称重组的机制不一样，导致下面的Bug：

![VC6watch窗口bug](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305086.png)

这是因为代码为C++，采用了C++编译器的名称重组方法，但是VC6的watch窗口采用的是C编译器名称重组机制。将源码的后缀名改成 `.c` 即可。

在编译后的 `obj` 文件中存在名称重组后的命名。

![C++名称重组](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305087.png)

```
_?number@?1??main@@9@4HA
_?number@?2??func@@YAXXZ@4HA
_?number@?1??func@@YAXXZ@4HA
?g_number@@3HA
?func@@YAXXZ
```

经过上面的分析我们可以看出，VC6的C++编译器名称重组的方式为：

```
前缀 + 原始变量名 + 函数层级 + 所在作用域
```

把CPP改成C，重新观察一下名称重组：

![image-20230709185433114](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305088.png)

```
_?number@?1??main@@9@9
_?number@?2??func@@9@9
_?number@?1??func@@9@9
_g_number
```

可以看出：VC6中的C++编译器和C编译器的名称重组机制除了一些细节上不同外基本相同的组成结构。

以C编译器的名称重组的对应关系为：

```c
#include <stdio.h>

// _g_number
int g_number = 0x12345678;

void func()
{
	// _?number@?1??func@@9@9
	static int number = 0x55667788;
	{
		// _?number@?2??func@@9@9
		static int number = 0x1234;
		printf("%p\n", number);
	}
	printf("%p\n", number);
}

int main()
{
	// _?number@?1??main@@9@9
	static int number = 0x11223344;
	printf("%p\n", number);
	func();
	return 0;
}
```



### `extern "C"`

为了在C++中能够兼容C的代码，就需要在函数前使用 `extern “C”` 来让函数使用C的函数重组机制进行编译。



## 静态变量初始化

在语法中 `static` 是只会初始化一次，观察静态变量的赋值的代码：

![image-20230709191857211](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305089.png)

可以看到：**定义静态局部变量的代码没有产生汇编代码，所以这里并没有初始化。**根据前面的分析可以知道已初始化的静态变量的初值是直接写到可执行文件中的，在加载程序时直接就被加载到内存中，不需要再额外赋值。



### 只初始化一次的实现方法

**存在一个标志来记录是否初始化过。**没有初始化的变量会被分配到未初始化的全局变量区。

```c++
#include <stdio.h>

int g_number = 0x12345678;
int g_num;
void func(int n)
{
	static int number = n;
	{
		static int number = n;
		printf("%p\n", &number);
	}
	printf("%p\n", &number);
}

int main(int argc, char* argv[], char* env[])
{
	static int number = argc+0x9998;
	printf("%p\n", &number);
	func(0x88888888);
	func(0x77777777);
	return 0;
}
```

![image-20230709194912082](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305090.png)

静态变量1初始化为：`0x9999`，静态变量的地址为`00427E90`。

同时有块内存（地址`00427E88`）中的值也发生改变。

![image-20230709195146959](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305091.png)

静态变量2初始化为：`0x88888888`，静态变量的地址为`00427E94`。

同时有块内存（地址`00427E88`）中的值也发生改变。猜测`00427E88`保存的就是记录静态变量是否初始化的标志。

![image-20230709195327357](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305092.png)

**在同作用域下，**静态变量3初始化为：`0x88888888`，静态变量的地址为`00427E8C`。

同时有块内存（地址`00427E88`）中的值也发生改变。基本验证了`00427E88`保存的就是记录静态变量是否初始化的标志。

![image-20230709195641365](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305093.png)

**在调用第二次时，静态变量并不会被初始化。**



**标志值不是唯一的，可能会有多个值来对应多个静态变量。**

```c++
#include <stdio.h>

int g_number = 0x12345678;
int g_num;
void func2(int n)
{
	static int number = n;
	printf("%p\n", &number);
}

void func(int n)
{
	static int number = n;
	{
		static int number = n;
		printf("%p\n", &number);
	}
	printf("%p\n", &number);
}

int main(int argc, char* argv[], char* env[])
{
	static int number = argc+0x9998;
	printf("%p\n", &number);
	func(0x88888888);
	func2(0x77777777);
	return 0;
}
```

![image-20230709200033837](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305094.png)

**这里三个作用域，也使用三个标志值。标志位也不是不变的，也不一定就在变量附近，只要能记录是否初始化了即可。**



**手动修改标志位来每次都初始化：**

```c
#include <stdio.h>

int g_num;

void func(int n)
{
	static int number = n;
	printf("%p\n", number);
	(&number)[-1] = 0;
}

int main(int argc, char* argv[], char* env[])
{
	func(0x88888888);
	func(0x77777777);
	return 0;
}
```

![image-20230709205331655](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307101305095.png)

这样就实现了静态变量多次初始化的效果，打破了C++的语法规则。

# 总结

- 静态全局变量和静态局部变量，本质上都属于全局变量。
- 全局、静态全局、静态局部都存储在内存的数据区可读写区（根据是否初始化，分为已初始化的数据区和未 初始化的数据区）。 
- 名称重组（粉碎）实现了静态变量的访问控制。
- 静态变量检查是编译器检查，可通过数组下标法修改其值。

# 练习

```
链接：https://pan.baidu.com/s/1fV7P1DeczSB2gph06tpH5Q?pwd=9fhw
提取码：9fhw
--来自百度网盘超级会员V5的分享
```


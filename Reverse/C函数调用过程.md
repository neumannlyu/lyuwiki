---
title: 函数调用过程的分析
date: '2023-06-28 09:33:32'
toc: true
indexing: false
categories:
  - Reverse
abbrlink: 9d44
---



函数调用的过程就是压栈和出栈的过程。以下面的源码进行函数调用的分析:

```c
#include <stdio.h>

int func(int a, int b, int c ,int d)
{
	return a + 2 + b + c + d;
}

int main(int argc, char* argv[], char* env[])
{
	int a = 1;
	a = func(argc, argc+1, argc + 2, argc + 3);

	return 0;
}
```

在开始函数调用之前, 先记录当前的栈底和栈顶.

![image-20230629215813409](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292158428.png)

# 1. 按照约定传递参数

约定参数传递主要涉及的方面有：

- 参数传递方向。从左往右传递参数或者从右往左传递参数。
- 参数传递方法。常见有栈传参和寄存器传参。

- 释放参数空间。如果是不定参数时，只能由调用方释放参数空间。
- 函数返回值存放位置。

### 常见的调用约定

|       -        | `__cdecl` | `__stdcall` |                   `__fastcall`                   |
| :------------: | :-------: | :---------: | :----------------------------------------------: |
|  参数传递方向  | 从右往左  |  从右往左   |                     从右往左                     |
|  参数传递方法  |  栈空间   |   栈空间    | 做数前两个参数使用寄存器传递，其余参数使用栈传递 |
|  释放参数空间  |  调用方   |   被调方    |                      被调方                      |
| 返回值存放位置 |  寄存器   |   寄存器    |                      寄存器                      |

> - `__cdecl`、  `__stdcall`  支持跨平台的调用约定。
> - `__fastcall`为微软专有。
> - C默认调用约定为`__cdecl` 。

![传参](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292202971.png)

我们使用默认的C 调用约定进行函数调用, 发现从右往左进行压栈.

# 2. 保存返回地址

在栈空间保存下一条指令的地址到栈顶作为返回地址。

![image-20230629220619587](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292206611.png)

保存下条地址的地址为返回地址, `call` 的下条语句地址为: `007A181A`.

# 3. 保存调用方信息

保存调用方的栈信息即保存调用方的栈底。

![image-20230629220850157](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292208199.png)

# 4.更新栈底

将调用方的栈顶作为被调用方的栈底. 例如 funA 调用 funcB, 当前 funcA 的栈顶地址为 0x4000, 那么就会将 0x4000 作为 funcB 的栈底.

![image-20230629221034400](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292210444.png)

将当前的栈顶 `004FF92C` 保存到栈底 EBP 寄存器完成保存调用方栈底.

# 5.申请局部变量空间

调高栈顶,为局部变量申请空间. 根据调试版和发布版不同,申请空间的大小也会不一样. 

**Debug:** 大于等于局部变量空间之和.

**Release:**小于等于局部变量空间之和.

![image-20230629221407483](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292214532.png)

申请局部变量的空间. 因为这里是调试版本, 在不需要局部变量空间情况下也开辟了 0xC0 字节的空间.

# 6.保存寄存器值

把可能要使用的寄存器值保存到栈中, 为后面返回到原函数时恢复寄存器中的值.

![image-20230629221757992](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292217054.png)

通过栈空间分别保存了 ebx esi 和 edi 寄存器的值.

# (可选)7.调试信息

如果编译选项使用了调试信息(/ZI /Zi)将局部变量初始化为 0xCC CC CC CC

![image-20230629222030800](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292220827.png)

用 CC 来初始化局部变量的空间.

# 8.执行函数体

执行函数中的代码。直到函数执行完成开始准备返回到调用方代码。

![image-20230629222206906](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292222966.png)

执行了`a+2+b+c+d`, 结果保存在 eax 寄存器中.

# 9.恢复寄存器环境

将第 6 步保存的寄存器值重新恢复到原本的寄存器中.

![image-20230629222428163](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292224234.png)

按照先进后出的栈原则,分别恢复 edi esi 和 ebx 寄存器原本的值, 同时栈顶也会随之改变.

# 10.释放局部变量空间

释放由第 5 步申请的局部变量中间.

![image-20230629222635011](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292226085.png)

释放之前申请的 0xC0 字节的局部变量空间.

# 11.更新栈底

和第 4 步一样需要更新栈底,将之前保存的栈底信息恢复到栈寄存器中.

![image-20230629223016013](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292230099.png)

将保存的栈底信息进行恢复.

# 12.清理参数空间

**根据调用约定的不同, 使用不同的方法来清理参数空间.**

- `__cdecl`: 取出当前栈顶作为返回的流程地址,返回到调用方后,由调用方清理参数空间.
- 其他约定: 取出当前栈顶作为返回的流程地址,由被调用方清理参数空间后返回.

这里是 `__cdecl` 取出当前的栈顶的值作为返回值, 并且由调用者进行清理参数的空间. 这里有个 ret 指令, 会将函数流程回到调用者. 

![image-20230629223549142](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292235235.png)

![image-20230629223924745](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292239835.png)

到这里函数调用的完整过程已经全部结束了, 可以看到函数在调用结束后的栈顶(esp)栈底(ebp)和函数函数调用之前是一样的.

最后因为代码里有保存返回值, 所以在最后进行了保存返回值(eax 寄存器).

![image-20230629224103674](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306292241709.png)














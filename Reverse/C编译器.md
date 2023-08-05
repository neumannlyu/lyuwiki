---
title: C常用编译器
date: '2023-06-12 10:37:59'
toc: true
indexing: false
abbrlink: edd2
categories:
  - Reverse
---

# cl.exe

cl是微软的Visual Studio自带的编译器，一般都是在Windows平台使用。

![cl编译器](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202306120949919.png)

**编译：**在 Windows 系统下用 `cl.exe` 仅编译出 `obj` 文件。

![image-20230804214317423](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308042143894.png)

**链接：**在 Windows 系统下用 `link.exe` 来进行 obj 链接，生成对应的可执行程序。

![image-20230805100318245](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308051003272.png)

## 编译链接选项

### **优化**

| 选项   | 中文说明                   | 英文说明                        |
| ------ | -------------------------- | ------------------------------- |
| /O1    | 最小化空间                 | minimize space                  |
| /Op[-] | 改善浮点数一致性           | improve floating-pt consistency |
| /O2    | 最大化速度                 | maximize speed                  |
| /Os    | 优选代码空间               | favor code space                |
| /Oa    | 假设没有别名               | assume no aliasing              |
| /Ot    | 优选代码速度               | favor code speed                |
| /Ob    | 内联展开（默认 n=0）       | inline expansion (default n=0)  |
| /Ow    | 假设交叉函数别名           | assume cross-function aliasing  |
| /Od    | 禁用优化（默认值）         | disable optimizations (default) |
| /Ox    | 最大化选项。(/Ogityb2 /Gs) | maximum opts. (/Ogityb1 /Gs)    |
| /Og    | 启用全局优化               | enable global optimization      |
| /Oy[-] | 启用框架指针省略           | enable frame pointer omission   |
| /Oi    | 启用内建函数               | enable intrinsic functions      |

### 代码生成

| 选项       | 中文说明                           | 英文说明                             |
| ---------- | ---------------------------------- | ------------------------------------ |
| /G3        | 为 80386 进行优化                  | optimize for 80386                   |
| /G4        | 为 80486 进行优化                  | optimize for 80486                   |
| /GR[-]     | 启用 C++ RTTI                      | enable C++ RTTI                      |
| /G5        | 为 Pentium 进行优化                | optimize for Pentium                 |
| /G6        | 为 Pentium Pro 进行优化            | optimize for Pentium Pro             |
| /GX[-]     | 启用 C++ 异常处理（与 /EHsc 相同） | enable C++ EH (same as /EHsc)        |
| /EHs       | 启用同步 C++ 异常处理              | enable synchronous C++ EH            |
| /GD        | 为 Windows DLL 进行优化            | optimize for Windows DLL             |
| /GB        | 为混合模型进行优化（默认）         | optimize for blended model (default) |
| /EHa       | 启用异步 C++ 异常处理              | enable asynchronous C++ EH           |
| /Gd        | __cdecl 调用约定                   | __cdecl calling convention           |
| /EHc       | extern“C”默认为 nothrow            | extern "C" defaults to nothrow       |
| /Gr        | __fastcall 调用约定                | __fastcall calling convention        |
| /Gi[-]     | 启用增量编译                       | enable incremental compilation       |
| /Gz        | __stdcall 调用约定                 | __stdcall calling convention         |
| /Gm[-]     | 启用最小重新生成                   | enable minimal rebuild               |
| /GA        | 为 Windows 应用程序进行优化        | optimize for Windows Application     |
| /Gf        | 启用字符串池                       | enable string pooling                |
| /QIfdiv[-] | 启用 Pentium FDIV 修复             | enable Pentium FDIV fix              |
| /GF        | 启用只读字符串池                   | enable read-only string pooling      |
| /QI0f[-]   | 启用 Pentium 0x0f 修复             | enable Pentium 0x0f fix              |
| /Gy        | 分隔链接器函数                     | separate functions for linker        |
| /GZ        | 启用运行时调试检查                 | enable runtime debug checks          |
| /Gh        | 启用钩子函数调用                   | enable hook function call            |
| /Ge        | 对所有函数强制堆栈检查             | force stack checking for all funcs   |
| /Gs[num]   | 禁用堆栈检查调用                   | disable stack checking calls         |

### 输出文件

| 选项      | 中文说明           | 英文说明                     |
| --------- | ------------------ | ---------------------------- |
| /Fa[file] | 命名程序集列表文件 | name assembly listing file   |
| /Fo       | 命名对象文件       | name object file             |
| /FA[sc]   | 配置程序集列表     | configure assembly listing   |
| /Fp       | 命名预编译头文件   | name precompiled header file |
| /Fd[file] | 命名 .PDB 文件     | name .PDB file               |
| /Fr[file] | 命名源浏览器文件   | name source browser file     |
| /Fe       | 命名可执行文件     | name executable file         |
| /FR[file] | 命名扩展 .SBR 文件 | name extended .SBR file      |
| /Fm[file] | 命名映射文件       | name map file                |

### 预处理器

| 选项       | 中文说明                           | 英文说明                       |
| ---------- | ---------------------------------- | ------------------------------ |
| /FI        | 命名强制包含文件                   | name forced include file       |
| /C         | 不吸取注释                         | don't strip comments           |
| /U         | 移除预定义宏                       | remove predefined macro        |
| /D`{=\|#}` | 定义宏                             | define macro                   |
| /u         | 移除所有预定义宏                   | remove all predefined macros   |
| /E         | 将预处理定向到标准输出             | preprocess to stdout           |
| /I         | 添加到包含文件的搜索路径           | add to include search path     |
| /EP        | 将预处理定向到标准输出，不要带行号 | preprocess to stdout, no #line |
| /X         | 忽略“标准位置”                     | ignore "standard places"       |
| /P         | 预处理到文件                       | preprocess to file             |

### 语言

| 选项        | 中文说明                       | 英文说明                            |
| ----------- | ------------------------------ | ----------------------------------- |
| /Zi         | 启用调试信息                   | enable debugging information        |
| /Zl         | 忽略 .OBJ 中的默认库名         | omit default library name in .OBJ   |
| /ZI         | 启用调试信息的“编辑并继续”功能 | enable Edit and Continue debug info |
| /Zg         | 生成函数原型                   | generate function prototypes        |
| /Z7         | 启用旧式调试信息               | enable old-style debug info         |
| /Zs         | 只进行语法检查                 | syntax check only                   |
| /Zd         | 仅要行号调试信息               | line number debugging info only     |
| /vd`{0\|1}` | 禁用/启用 vtordisp             | disable/enable vtordisp             |
| /Zp[n]      | 在 n 字节边界上包装结构        | pack structs on n-byte boundary     |
| /vm         | 指向成员的指针类型             | type of pointers to members         |
| /Za         | 禁用扩展（暗指 /Op）           | disable extensions (implies /Op)    |
| /noBool     | 禁用“bool”关键字               | disable "bool" keyword              |
| /Ze         | 启用扩展（默认）               | enable extensions (default)         |

### 杂项

| 选项      | 中文说明                   | 英文说明                          |
| --------- | -------------------------- | --------------------------------- |
| /?, /help | 打印此帮助消息             | print this help message           |
| /c        | 只编译，不链接             | compile only, no link             |
| /W        | 设置警告等级（默认 n=1）   | set warning level (default n=1)   |
| /H        | 最大化外部名称长度         | max external name length          |
| /J        | 默认 char 类型是 unsigned  | default char type is unsigned     |
| /nologo   | 取消显示版权消息           | suppress copyright message        |
| /WX       | 将警告视为错误             | treat warnings as errors          |
| /Tc       | 将文件编译为 .c            | compile file as .c                |
| /Yc[file] | 创建 .PCH 文件             | create .PCH file                  |
| /Tp       | 将文件编译为 .cpp          | compile file as .cpp              |
| /Yd       | 将调试信息放在每个 .OBJ 中 | put debug info in every .OBJ      |
| /TC       | 将所有文件编译为 .c        | compile all files as .c           |
| /TP       | 将所有文件编译为 .cpp      | compile all files as .cpp         |
| /Yu[file] | 使用 .PCH 文件             | use .PCH file                     |
| /V        | 设置版本字符串             | set version string                |
| /YX[file] | 自动的 .PCH 文件           | automatic .PCH                    |
| /w        | 禁用所有警告               | disable all warnings              |
| /Zm       | 最大内存分配（默认为 `%`） | max memory alloc (`%` of default) |

### 链接

| 选项  | 中文说明                  | 英文说明                        |
| ----- | ------------------------- | ------------------------------- |
| /MD   | 与 MSVCRT.LIB 链接        | link with MSVCRT.LIB            |
| /MDd  | 与 MSVCRTD.LIB 调试库链接 | link with MSVCRTD.LIB debug lib |
| /ML   | 与 LIBC.LIB 链接          | link with LIBC.LIB              |
| /MLd  | 与 LIBCD.LIB 调试库链接   | link with LIBCD.LIB debug lib   |
| /MT   | 与 LIBCMT.LIB 链接        | link with LIBCMT.LIB            |
| /MTd  | 与 LIBCMTD.LIB 调试库链接 | link with LIBCMTD.LIB debug lib |
| /LD   | 创建 .DLL                 | Create .DLL                     |
| /F    | 设置堆栈大小              | set stack size                  |
| /LDd  | 创建 .DLL 调试库          | Create .DLL debug libary        |
| /link | [链接器选项和库]          | [linker options and libraries]  |



# gcc

gcc为GNU操作系统的编译器。

# clang

clang是LLVM的C语言族前端。

ff

# `if-else` 与 `switch-case`

- `if-else` 可以控制分支的概率，如果把概率较高的分支放在前面效率会更高些。
- `if-else` 可以进行区间比较，switch-case为单值比较。
- `if-else`可以进行复杂条件环境的比较。

# `switch-case` 实现

## case 数量较少

```c
switch (argc)
{
case 0x1:
    printf("case 0x1");
    break;
case 0x200:
    printf("case 0x200");
    break;
case 0x30:
    printf("case 0x30");
    break;
}
```

![image-20230621213759544](https://pics-place.oss-cn-shanghai.aliyuncs.com/picimage-20230621213759544.png)

**在case分支小于等于3(具体看编译器和编译环境)个时，会将case的值进行排序，排序后按从小到大的顺序进行比较。**

## case 数量较多且相对连续

```c
switch (argc)
{
case 0x1:
    printf("case 0x1");
    break;
case 0x8:
    printf("case 0x4");
    break;
default:
    printf("case default");
    break;
case 0x5:
    printf("case 0x3");
    break;
case 0x2:
    printf("case 0x2");
    break;
}
```

![image-20230621221025976](https://pics-place.oss-cn-shanghai.aliyuncs.com/picimage-20230621221025976.png)

![image-20230621222423002](https://pics-place.oss-cn-shanghai.aliyuncs.com/picimage-20230621222423002.png)

![image-20230621222554060](https://pics-place.oss-cn-shanghai.aliyuncs.com/picimage-20230621222554060.png)

**经过分析汇编代码可以得到一些规律：**

- 先根据case的值进行排序。
- 当case不是0开始时，会平移到以0开始。
- 如果中间存在缺省值，则用default分支的地址进行填充；如果分支中没有default分支则用下条语句的地址填充。
- 将case值（经过平移后）作为数组的下标进行寻址，直接跳转到对应地址进行执行。
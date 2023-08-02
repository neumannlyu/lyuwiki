在研究一个APK应用的时候发现这个APK不能在Jadx中正常解析，经过初步分析后，发现APK中的Android Manifest.xml文件不能够正常反编译。所以猜测AndroidManifest.xml文件被修改了，导致了不能够正常解析，所以需要针对AndroidManifest.xml编译后的文件进行文件格式的分析，以后CTF出题也可以用相关知识点，丰富公司的题型。

# 原始AndroidManifest.xml文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.TestEmpty"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>

            <meta-data
                android:name="android.app.lib_name"
                android:value="" />
        </activity>
    </application>

</manifest>
```



把编译后的文件放到010中，利用010的模板进行分析。

![image-20230113163535341](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615481.png)



# Header

![image-20230113163647043](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615482.png)

文件头主要就是magic number和file size。分别表示文件类型和文件大小。



# StringChunk

stringchunk主要用来存储字符串内容，文件中的可见字符串一般都存储在这里。

![image-20230113164350899](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615483.png)

这里面的内容非常多，主要可以分为两个部分，前面用来描述StringChunk，后面是每具体的个字符串。在表示字符串时采用了2个字节表示一个字符。

## signature

用来标识StringChunk。

## size

标识StringChunk的大小。

## StringCount

标识字符串的总个数。

## StyleCount

Style 的个数。具体意义不清楚，所有的apk这里的值应该都为0。

## Unkown

占位4字节，具体意义不明确，固定值为0。

## StringPoolOffset

字符串池偏移量，4个字节，偏移量相对StringChunk头部位置。

![image-20230116095808467](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615485.png)

StringCtunk开始的偏移值为8，字符串池的偏移值为0xDC，所以字符串池实际从E4位置开始。

## StylePoolOffset

样式池偏移量，4个字节，偏移量相对于StringChunk头部位置。固定值 0x00000000。

## StringOffsets

每个字符串在字符串池中的相对偏移量，int数组，它的大小是StringCount*4个字节.

![image-20230116101334939](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615486.png)

![image-20230116101456297](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615487.png)

![image-20230116101519075](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615488.png)

以前面2项为例，第1项偏移值为0，第2项偏移值为0xE。

字符串1：字符串池的偏移值（0xE4)+偏移值（0），所以字符串从0xE4开始。

字符串2：字符串池的偏移值（0xE4)+偏移值（E），所以字符串从0xF2开始。



# ResourceIDChunk

ResourceID是用来描述资源的编号，id一般情况下都会保存在`public.xml`文件中。

![image-20230117143026710](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615489.png)

前面两个4字节分别表示Signature和Size。

Signature表示是资源标号。

Size表示ResourceIDChunk总共的大小，即Signature + Size + id数组。

0x58h = 88.

id数量 = (88-4-4)/4 = 20.

![image-20230117143948245](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615490.png)

例如rcItem[0]=1010000 h,这个ID对应着安卓系统源码 http://androidxref.com/9.0.0_r3/xref/frameworks/base/core/res/res/values/public.xml 。

![image-20230117154008397](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615491.png)



# StartNamespaceChunk

这个结构体表示命令空间的开始，在文件的最后有一个EndNamespaceChunk与之对应。

![image-20230117155016099](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615492.png)

![image-20230117155349548](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615493.png)

这里就是表示前缀名称为字符串池下标为0x19，对应的字符串为`android`，然后`uri`对应的字符串为`http://schemas.android.com/apk/res/android`。

![image-20230117155555601](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615494.png)



# TagChunk

TagChunk表示的是一个节点，startTagChunk表示的是一个节点的开始，endTagChunk表示的是一个节点的结束。

startTagChunk和endTagChunk可以根据Signature来判断。

![image-20230117160344366](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615495.png)

## startTagChunk

![image-20230117160955715](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615496.png)

前面几个内容都是比较容易理解的，其中比较重要是NamespaceUri、Name和AttributeCount。

AttributeCount表示属性的个数，比如说这里就有7个属性值。

## attributeChunk

这个结构体表示了元素的名称和值。

![image-20230117161747446](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615497.png)

Name表示属性名，Data表示的属性的值。根据Type可以判断Data的类型。

|   Type值   |     Data Type     |                   备注                   |
| :--------: | :---------------: | :--------------------------------------: |
| 0x10000008 |        Int        |                                          |
| 0x3000008  |      String       |                                          |
| 0x1000008  | R.class中的资源ID |                                          |
| 0x12000008 |       bool        | 值为0xFFFFFFFF表示true；值为0表示false。 |

**字符串0:**

acNamespaceUri 对应的字符串为：`http://schemas.android.com/apk/res/android`

acName 对应的字符串为：versionCode

根据acType知道数据是一个整形值，值为acData中的值。

结果就是：android:version="1"

**字符串1:**

acNamespaceUri 对应的字符串为：`http://schemas.android.com/apk/res/android`

acName 对应的字符串为：versionName

根据acType知道数据是一个字符串，值为acData中的值查找字符串池，字符串为：1.0

结果就是：android:versionName="1.0"



## endTagChunk

![image-20230117165209036](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615498.png)

endTagChunk和startTagChunk是一一对应的，endTagChunk表示上一个startTagChunk结束。



# EndNamespaceChunk

![image-20230117165630364](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308021615499.png)

这里表示命令空间内容结束。





# 总结

通过以上的分析，可以明白一个编译之后的AndroidManifest.xml文件的完整二进制结构。

主要用途有：

- 我们可以通过这样的结构逆向出原始的XML文件内容，类似于Jadx、Jeb逆行分析效果。
- 通过一些固定值校验AndroidManifest.xml是否存在错误的数据。
- 自己写一个编译器/编译工具/手动方式来修改AndroidManifest.xml文件，达到不能够被一般逆向分析工具分析的目的。
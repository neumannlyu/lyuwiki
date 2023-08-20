---
title: customized yourself windows iso
date: '2023-08-21 00:09:40'
toc: true
indexing: false
abbrlink: 247a
categories:
  - Tool
---
# 工具

1. 虚拟机软件（HyperV）
2. WePE
3. NtLite

# 自定义系统镜像

我们可以将现在C盘中的系统制作成镜像，也可以利用虚拟机来制作镜像。更加推荐使用虚拟机来制作的系统镜像，因为这样制作出来的系统镜像更加纯净和通用。

## HyperV

1. 利用HyperV安装好系统，并且把自己常用的软件安装好。

2. 关闭虚拟机。

3. 如果有检查点，需要删除所有的检查点。

![image-20230813220546657](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147083.png)

![开始进行合并检查点](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147085.png)

4. （可选）把虚拟硬盘进行备份。



# 启动 PE

# 运行PE

安装**WePE**时选择**生成启动的ISO**，这样就可以得到一个 PE 镜像。

![image-20230813222005987](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147086.png)

默认选项生成`iso`文件，`iso`文件路径默认为`C:\WePE_64_V2.1.iso`。

HyperV光驱加载刚刚生成的wepe启动镜像，并且设置为默认启动。

![image-20230813222559551](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147087.png)

![image-20230813222702433](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147088.png)

开启虚拟机，进入PE。

利用系统的磁盘工具，将预留的100G磁盘创建出来。或者将剩余空间压缩出来保存系统镜像。

分区的操作最好用系统的工具，资源管理器都能够顺利读取出来。

打开**dism++**，打开C盘。

选择工具箱，选择系统备份。

![image-20230813223055103](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147089.png)

![image-20230813223748797](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147091.png)

![正在保存系统wim](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147092.png)



# 替换 install.wim

解压原始的win11镜像文件。

用刚刚保存的 `install.wim` 来替换 `sources` 文件夹的 `install.wim` 文件。

用**NTLite**打开这个文件夹。

![image-20230815212941542](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147093.png)

![image-20230815213117388](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147094.png)

设置完卷标后，就开始导出了。

ISO卷标就是ISO镜像文件刻录成光盘或U盘后，在资源管理器界面中显示的盘符名称，类似“本地磁盘”这种。其实ISO卷标是可以修改的。

![image-20230815213356186](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147095.png)

这样就能得到一个新的ISO镜像。

![image-20230815213922504](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147096.png)



# 测试

新建一个虚拟机，测试一下，可以直接使用。

![image-20230815215624377](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147097.png)

自带有之前创建的用户信息，可以自己再次创建其他用户。





# Tools

1. Virtual Machine Software(HyperV)
2. WePE
3. NtLite

# Customize the system image

We can make the image of the current system in the C disk, or we can use the virtual machine to make an image. System images made with virtual machine are preferred because they're cleaner and more versatile.



## HyperV

1. Use HyperV to install the system, and install your commonly used software.

2. Shut down the VM.

3. Remove all checkpoints, if any.

   ![image-20230813220546657](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147083.png)

   ![Start to merge checkpoints](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147085.png)

4. (optional)  Backup the virtual hard disk.



# Start PE

Select  the **generate startup ISO** option  when installing  the **WePE** to get a PE image.

![image-20230813222005987](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147086.png)

The default option generated an ISO file. The default path of the iso file is `C:\WePE_64_V2.1.iso`.

The HyperV drive loads the newly generated wepe boot image, and set it to default boot.

![image-20230813222559551](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147087.png)

![image-20230813222702433](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147088.png)

Startup the virtual machine and enter the PE.

Use the system disk tool to create the reserved 100G disk space. Or compress the remaining space to save the system image.

Partition operations are best to use the system's disk tool, explorer can identify the disk partition infomation successfully.

Open the **dism++**, and open the drive  C.

Select the toolbox, and select the system backup. 

![image-20230813223055103](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147089.png)

![image-20230813223748797](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147091.png)

![saving the system wim](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147092.png)

# replace the install.wim

Unzip the original win11 system image file.

Replace the `install.wim`  in the `sources ` folder with the `install.wim` file you just saved.

Open the unzip folder with **NtLite**.

![image-20230815212941542](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147093.png)

![image-20230815213117388](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147094.png)

Once the label is set, the export begins.

The ISO label is the disk character name displayed in the explorer after the ISO image file is burned to the CD or U disk, similar to "local disk". The ISO label can be changed.

Then we get a new system mirror image.

![image-20230815213922504](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147096.png)

# Test

Create a new virtual machine, and use the iso file saved just now as the system image, and confirm that it can be use directly.

![image-20230815215624377](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202308202147097.png)

It comes with the user information you created earlier, and you can create another user yourself.

 
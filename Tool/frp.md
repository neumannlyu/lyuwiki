
We can use frp to achieve intranet penetration, to achieve remote desktop to our LAN Windows computers.

# Preparation

- VPS
- A intranet Windows computer
- frp

## VPS

Remote Desktop has a high instantaneity requirement, i recommend to use a domestic server with a large bandwith. I use Ali Cloud's 80M bandwidth VPS. The VPS charging by flow.

## Windows computer

A  computer on the company's intranet.

## frp

Frp is a open source tool, and the open source address is: https://github.com/fatedier/frp. The official documentation configuration file: https://github.com/fatedier/frp#configuration-files.

# Remote Desktop

## Server Setting

Download different frp file depend on the CPU architecture and system. For example, I am running on an x86_64 Ubuntu VPS, so i  need to download corresponding [frp_0.51.2_linux_amd64.tar.gz](https://github.com/fatedier/frp/releases/download/v0.51.2/frp_0.51.2_linux_amd64.tar.gz) file.

Extract the `frp_0.51.2_linux_amd64.tar.gz` file, and rename it to `frp`.

### Update the configuration file on the server

```ini
[common]
bind_port = 7000
token = 123456
```

- `bind_port` indicates the port enabled by the server. The server will use this port to provide the frp service.
- `token`: The token for the connection between the client and the server. The client needs to fill the corresponding tpken during configuration.

### Run Server Program

Check whether the frp program has executable permission. If it doesn't have executable permission, use the `chmod  a+x` command to give executable permission. Execute the `./frp -c frps.ini` command to runthe frp server program.

Execute the  `./frp  -c frps.ini` command to run the frp server program.

### Running in the background 

使用 `nohup` 命令 frp 需要在后台一直运行。

Use the `nohup` command to keep the frp program running in the background.

```
nohup ./frp -c frps.ini &
```

### Quit

if the task is running in the current shell terminal, you can use the `jobs` command to query relevant infomation and kill the progress.

```sh
# View background task progress infomation in current shell terminal.
$ jobs
[1]+ Running nohup java -jar adapter-minisite.jar /tomcat-1 /tomcat-2 > logs.txt 2>&1 &

# kill the task id
$ kill %1

# query the pid
$ jobs -l
[1]+ 11076 Running nohup java -jar adapter-minisite.jar /tomcat-1 /tomcat-2 > logs.txt 2>&1 &

$ kill 11076

# set running in the foreground
$ fg %n 
```

if current terminal is not shell, you can use the `ps auxf | grep 'adapter-minisite'`  command to get `pid` and `kill pid`.



## Client Setting

Download  the corresponding Windows frp program and decompress it.

Config the corresponding **frpc.ini** file.

```ini
[common]
server_addr = xx.xx.xx.xx
server_port = 7000
token = 123456

[rdp]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 7001
```

Run on start

Boot from the Windows schedule task.

---

---


我们可以使用 frp 来实现内网穿透功能，实现远程桌面到我们局域网的Windows电脑。

# 准备工作

- VPS 服务器
- 内网下的 Windows 电脑
- frp

## VPS 服务器

远程桌面对于即时性要求比较高，建议使用国内的大带宽的服务器。我使用的是阿里云的 80M 带宽按量计费的VPS。

## Windows 电脑

在公司内网下的一台 Windows 电脑。

## frp

frp为 Github 上开源的一个工具，开源地址为：https://github.com/fatedier/frp。官方的配置文件说明文档：https://github.com/fatedier/frp#configuration-files。

# 远程桌面

## 服务端设置

根据 CPU 架构和系统不同，下载不同的文件。比如说，我现在是一台 x86_64 架构的 Ubuntu VPS，就需要下载对应的 [frp_0.51.2_linux_amd64.tar.gz](https://github.com/fatedier/frp/releases/download/v0.51.2/frp_0.51.2_linux_amd64.tar.gz) 文件。

解压 `frp_0.51.2_linux_amd64.tar.gz`，并且重命名为`frp`。

### 更新服务器端配置文件

```ini
[common]
bind_port = 7000
token = 123456
```

- `bind_port` 表示服务端开启的端口号。服务器将使用这个端口来提供 frp 服务。
- `token`： 客户端和服务端进行连接的令牌，客户端在配置时需要填写对应的 token。

### 运行服务端程序

检查 frp 程序是否有执行权限，如果没有执行权限，使用`chmod a+x` 来赋予执行权限。

执行 `./frp -c frps.ini` 命令来运行 frp 服务端程序。

### 服务端后台运行

使用 `nohup` 命令 让 frp 在后台一直运行。

```
nohup ./frp -c frps.ini &
```

### 退出任务

如果运行的任务在当前 `shell` 终端，可以通过 `jobs` 命令查询相关信息，并且杀掉进程。

```sh
# 查看当前 shell 终端的后台运行任务进程信息
$ jobs
[1]+ Running nohup java -jar adapter-minisite.jar /tomcat-1 /tomcat-2 > logs.txt 2>&1 &

# 杀掉任务号
$ kill %1

# 或着找到 pid
$ jobs -l
[1]+ 11076 Running nohup java -jar adapter-minisite.jar /tomcat-1 /tomcat-2 > logs.txt 2>&1 &

$ kill 11076

# 或着
$ fg %n # 置为前端运行

Ctrl + c # 退出
```

如果非当前 `shell` 终端，可以通过 `ps auxf | grep 'adapter-minisite'` 获取 `pid` 然后 `kill pid`。



## 客户端设置

下载对应的 Windows frp 程序并且进行解压。

配置对应的 **frpc.ini**。

```ini
[common]
server_addr = xx.xx.xx.xx
server_port = 7000
token = 123456

[rdp]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 7001
```

### 开机自启

通过 Windows 的计划任务来进行开机自启。



---
title: memuc
date: 2021-07-12 15:35:52
tags:
---

### Command

创建新的模拟器

```tex
44
51
71
76 (71x64)
```

```bash
$ memuc create 71
```

列出所有设备的信息

```tex
[--running] 正在运行的设备
[//] 所有信息
[-s] 磁盘信息
[-r] 窗口句柄
```

```bash
$ memuc listvms //
```

关闭所有设备

```bash
$ memuc stopall
```

其它命令

```tex
reboot         重启
randomize      换机
remove         删除
clone          克隆
start          启动
stop           关闭
isvmrunning    运行状态
listvms        指定设备信息
```

```bash
$ memuc <cmdline> -index <index>
```

### 获取设备的内网 IP 地址

```bash
$ memuc -i 0 execcmd "ifconfig wlan0|grep -o \"10\.0\.0\.[0-9]\{1,3\}\"|grep -v 10.0.0.255"
```


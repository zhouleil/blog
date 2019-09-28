---
title: tcpdump
date: 2019-09-21 12:27:26
tags:
categories:
- 后端
- Linux
---

#### 抓包工具 Tcpdump

Linux 系的系统有一个很好用的抓包工具，叫 tcpdump, 可以用来抓取网络上的 Tcp 包，例如我要抓取 8080 端口的包，可以执行以下命令：

```bash
sudo tcpdump port 8080 -n 
```

-n 的意思是端口号用数字表示，还可以加上 -v 或 -vv 显示更详细的信息：

```bash
sudo tcpdump port 8080 -n -v
```

再如我要抓取来自特定源 IP 和发往特定目的的 IP 的包，可以用以下命令：

```bash
sudo tcpdump src host 10.2.200.11 or dst host 10.2.200.11
```

指定 src host 和 dst host ，并用 or/and 做条件的并集和交集。


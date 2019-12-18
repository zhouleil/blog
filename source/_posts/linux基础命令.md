---
title: linux基础命令
date: 2019-12-18 23:17:46
tags:
categories:
- 后端
- Linux
---

------------------

鸟哥的私房菜

## 基础指令的操作

- 显示日期与时间的指令： `date`
- 显示日历的指令: `cal`
- 简单好用的计算器：`bc`
  
1. 显示日期的指令： `date`

```shell
[root@iZwz9ioqjurm6w6aepf9moZ ~]# date
2019年 12月 15日 星期日 20:40:45 CST

## 格式化
[root@iZwz9ioqjurm6w6aepf9moZ ~]# date +%Y/%m/%d
2019/12/15
```

2. 显示日历的指令： `cal`

```shell
2019/12/15
[root@iZwz9ioqjurm6w6aepf9moZ ~]# cal
     十二月 2019
日 一 二 三 四 五 六
 1  2  3  4  5  6  7
 8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30 31

[root@iZwz9ioqjurm6w6aepf9moZ ~]# cal 12 2019
     十二月 2019
日 一 二 三 四 五 六
 1  2  3  4  5  6  7
 8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30 31

```

3. 简单好用的计算机： `bc`
  
```shell
root@iZwz9ioqjurm6w6aepf9moZ ~]# bc
bc 1.06.95
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'.
1+2+3+4
10
7-8+9+10
18
```

## 查询指令用法的 `man page`

例如查询 date 使用说明： `man date`

```shell
# man date
DATE(1)                          User Commands                         DATE(1)

NAME
       date - print or set the system date and time

SYNOPSIS
       date [OPTION]... [+FORMAT]
       date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]

DESCRIPTION
       Display the current time in the given FORMAT, or set the system date.

       Mandatory  arguments  to  long  options are mandatory for short options
       too.

       -d, --date=STRING
              display time described by STRING, not 'now'

       -f, --file=DATEFILE
              like --date once for each line of DATEFILE
```

### man page 大致分成底下这几个部分

| 代号        | 内容说明                                                     |
| ----------- | ------------------------------------------------------------ |
| NAME        | 简短的指令、数据名称说明                                     |
| SYNOPSIS    | 简短的指令下达语法（syntax）简介                             |
| DESCRIPTION | 较为完整的说明，这部分最好仔细看看                           |
| OPTIONS     | 针对SYNOPSIS部分中，有列举的所有可用的选项说明               |
| COMMANDS    | 当这个程序（软件）在执行的时候，可以在此程序（软件）中下达的命令 |
| FILES       | 这个程序或数据所使用或参考或连结到的某些档案                 |
| SEE ALSO    | 可以参考的，跟这个指令或数据有相关的其他说明！               |
| EXAMPLE     | 一些可以参考的范例                                           |
| BUGS        | 是否有相关的臭虫                                             |

### 与`man page`有关的指令

1.`whatis` 相当于 man -f

```shell
[root@iZwz9ioqjurm6w6aepf9moZ ~]# whatis ls
ls (1)               - list directory contents
[root@iZwz9ioqjurm6w6aepf9moZ ~]# man -f ls
ls (1)               - list directory contents
```

2.`apropos` 相当于 man -k

```shell
# man -k man
# apropos man
[root@iZwz9ioqjurm6w6aepf9moZ ~]# apropos
apropos 什么？
[root@iZwz9ioqjurm6w6aepf9moZ ~]# man -k
apropos 什么？
```

## 常用的数字意义

DATE(1) 括号的数字意义是什么？如下表所示

| 数字  | 数字含义  |
| ---- | ------------------------------------------------------------ |
| 1    | **用户在shell环境中可以操作的指令或可执行文件**              |
| 2    | 系统核心可呼叫的函数与工具等                                 |
| 3    | 一些常用的函数（function）与函式库（library），大部分为C的函式库（library） |
| 4    | 装置档案的说明，通常在 `/dev` 下的档案                       |
| 5    | **配置文件或者是某些档案的格式**                             |
| 6    | 游戏（games）                                                |
| 7    | 惯例与协议等，例如Linux文件系统、网络协议、ASCII code 等等的说明 |
| 8    | **系统管理员可用的管理指令**                                 |
| 9    | 跟 kernel 有关的软件                                         |

***1 、5、8 这三个号码特别重要，要记住***

## 查询指令用法的另外一种方式 `info page`

```shell
# info info
# info ls
```

## 本地说明文档目录 `/usr/share/doc`

例 来说，你想要知道这一版的 CentOS 相关的各项信息，可以直接到底下的目录去瞧瞧:

- `/usr/share/doc/centos-release-notes-5.3/`

## 总结上面三个咚咚（man, info, /usr/share/doc/ ）, 请记住哦：

-  在文字接口下，有任何你不知道咚咚指令或者文件格式这种玩意儿，但是你想要了解他，请赶快使用`man` 或者 `info` 来查询
-  而如果你想要架设一些其他的服务，或想要利用一整组软件来达成某项功能时，请赶快到 `/usr/share/doc/` 底下查一查有没有该服务的说明档喔！
-  另外，再次的强调，因为 Linux 毕竟是外国人发明的，所以中文文件确实比较少的！但是不要害怕，拿本英语字典在身边吧！随时查阅！不要害怕英文喔！
  
  
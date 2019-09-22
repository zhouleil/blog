---
title: nginx命令
date: 2019-09-21 20:57:14
tags:
categories:
- 其他
---

#### nginx 常用命令

```
sudo nginx	#打开 nginx
nginx -s reload|reopen|stop|quit	#重新加载配置|重启|停止|退出 nginx
nginx -t	#测试配置是否有语法错误

nginx [-?hvVtq] [-s signal] [-c filename] [-p prefix] [-g directives]

-?,-h	        : 打开帮助信息
-v		: 显示版本信息并退出
-V		:	显示版本和配置选项信息，然后退出
-t		: 检测配置文件是否有语法错误，然后退出
-q		: 在检测配置文件期间屏蔽非错误信息
-s signal       :给一个 nginx 主进程发生信号： stop (停止)， quit (退出)， reopen (重启) ， reload （重新加载配置文件）
-p prefix	: 设置前缀路径（默认是： /usr/local/nginx/）
-c filename	: 设置配置文件 （默认是： /usr/local/etc/nginx/nginx.conf）
-g directives	: 设置配置文件外的全局命令
```



[原文地址](https://www.cnblogs.com/linux-centos/p/5790506.html)
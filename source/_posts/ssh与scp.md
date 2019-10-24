---

title: ssh与scp
date: 2019-10-19 22:10:22
tags:
categories:
- 后端
- Linux
---

### 1、ssh 命令连接服务器

```shell
ssh 用户名@IP地址 -p 端口号
```

如果连接成功的话会提示你输入远程服务器的密码。全部成功之后SSH就会显示远程服务器的提示符，这时候就说明连接成功了。

### 2、scp 上传与下载文件

- ##### 从服务器上下载文件

  ```shell
  scp 用户名@host:文件的绝对路径 本机绝对路径
  ```

  例子：`scp root@192.168.0.101:/root/test.txt /Users/home/`

  从 ip 为192.168.0.101的服务器路径/root/test.txt 中的test.txt文件下载到本地/Users/home/ 目录中

- ##### 上传文件到服务器

  ```shell
  scp 本机绝对路径 用户@host:绝对路径
  ```

  例子:	`scp /var/www/test.text root@192.168.0.101:/var/www/` 

  把本机/var/www/目录下的 test.text 文件上传到192.168.0.101 这台服务器上的/var/www/目录中

- ##### 从服务器下载目录(在scp后面加-r)

  ```shell
  scp -r  用户@host:保存文件夹的绝对路径 本机文件夹的绝对路径
  ```

  例子： `scp -r root@192.168.0.101:/var/www/test /var/www/`

  把 192.168.0.101 这台服务器的 /var/www/test 目录 下载到本机的 /var/www 目录

- ##### 上传目录到服务器

  ```bash
  scp -r 本机文件夹的绝对路径 用户@host:保存文件夹的绝对路径
  ```

  例如：`scp -r /var/test root@192.168.0.101:/var/www/ `

  把本机 /var/test 目录上传到服务器的 /var/www/ 目录


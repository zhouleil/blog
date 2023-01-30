---
title: zsh的使用
date: 2023-01-30 17:38:14
tags: mac linux
categories:
- 前端
- 其他
---


## 1、安装

https://github.com/ohmyzsh/ohmyzsh/wiki

安装 oh my zsh

sh -c "$(wget -O- https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)"


## 2、插件

以下面插件安装为例

git zsh-autosuggestions autojump zsh-syntax-highlighting

### 1、 git  默认， 不需要安装

### 2、autojump

https://github.com/wting/autojump

1.OS X

使用 brew install autojump 安装

2.手动安装

git clone git://github.com/wting/autojump.git

注：github 克隆不了可以在 gittee 上面搜索 一个复制项目，克隆下来执行 install

如： git clone https://gitee.com/holyvan/autojump.git

cd autojump
./install.py or ./uninstall.py

### 3、zsh-autosuggestions

https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md

### 4、zsh-syntax-highlighting

https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md

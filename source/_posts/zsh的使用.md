---
title: zsh的使用
date: 2023-01-30 17:38:14
tags: mac linux
categories:
- 前端
- 其他
---


## 1、安装 zsh

github: https://github.com/ohmyzsh/ohmyzsh/wiki

官网：https://ohmyz.sh/

安装 oh my zsh

```bash
# curl 方式
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# wget 方式
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

国内源安装

```bash
sh -c "$(wget -O- https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)"

sh -c "$(curl -O- https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)"
```

## 2、安装 zsh 插件

以下面插件安装为例

- git
- zsh-autosuggestions
- autojump
- zsh-syntax-highlighting
- fortune-zh

### 1、 git  默认， 不需要安装

`vim ~/.zshrc`

可以设置 `git` alias , 如

```bash
# git alias start
alias gst="git status"
alias gcm="git commit -m"
alias gme="git merge --no-f"
alias glg="git log"
# git alias end
```

### 2、autojump

https://github.com/wting/autojump

1.OS X 系统

可使用 brew install autojump 安装

2.手动安装

`git clone git://github.com/wting/autojump.git`

注：github 克隆不了可以在 gittee 上面搜索 一个复制项目，克隆下来执行 install

如： `git clone https://gitee.com/holyvan/autojump.git`

```bash
cd autojump
./install.py or ./uninstall.py
```
### 3、zsh-autosuggestions

1、克隆仓库到 zsh 用户自定义插件目录 $ZSH_CUSTOM/plugins (默认： ~/.oh-my-zsh/custom/plugins)

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

2、编辑 `~/.zshrc`, 在插件列表增加 zsh-autosuggestions

```bash
plugins=(
    # other plugins...
    zsh-autosuggestions
)
```
3、执行 `source ~/.zshrc`

插件安装说明：

https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md

### 4、zsh-syntax-highlighting

1、克隆仓库到 zsh 用户自定义插件目录 $ZSH_CUSTOM/plugins (默认： ~/.oh-my-zsh/custom/plugins)

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

2、编辑 `~/.zshrc`, 在插件列表增加 zsh-syntax-highlighting

```bash
plugins=(
    # other plugins...
   zsh-syntax-highlighting
)
```
3、执行 `source ~/.zshrc`

插件安装说明：
https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md


### 5、fortune-zh 中文随机格言

https://github.com/shenyunhang/fortune-zh


使用已经爬取下来的古诗文数据。

1、克隆仓库

`git clone --recursive https://github.com/shenyunhang/fortune-zh-data.git`

2、安装古诗文

cd fortune-zh-data

sudo cp * /usr/share/games/fortunes/

注： `bat`数据文件好像已经损坏, 进入 fortune-zh-data 目录

先秦 两汉 魏晋 南北朝 隋代 唐代 五代 宋代 金朝 元代 明代 清代

分别 使用 `strfile 先秦` 等重新生成 bat 文件

或者在 fortune-zh-data 目录下建立 generateDat.sh 文件

```bash
#!/bin/bash

echo '开始生成 dat===='

filepath='./'

files=$(ls $filepath | grep -e '[^dat LICENSE sh]$')

# echo $files

for filename in $files
do
 echo $filename
 strfile $filename
done

echo '生成 dat 成功===='
```

执行 generateDat.sh， 没有权限可以 chmod 777 generateDat.s

参考：https://www.ruanyifeng.com/blog/2015/04/fortune.html

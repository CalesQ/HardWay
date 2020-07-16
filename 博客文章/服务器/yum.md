## 简介
yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

## 安装yum

## 卸载yum

## 常见命令

yum的命令形式一般是如下：yum [options] [command] [package ...]
其中的[options]是可选的，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。[command]为所要进行的操作，[package ...]是操作的对象

自动搜索最快镜像插件：   yum install yum-fastestmirror
安装yum图形窗口插件：    yum install yumex
查看可能批量安装的列表： yum grouplist

### 1. 安装

yum install 全部安装
yum install package1 安装指定的安装包package1
yum groupinsall group1 安装程序组group1

### 2. 更新和升级

yum update 全部更新
yum update package1 更新指定程序包package1
yum check-update 检查可更新的程序
yum upgrade package1 升级指定程序包package1
yum groupupdate group1 升级程序组group1

### 3. 查找和显示

yum info package1 显示安装包信息package1
yum list 显示所有已经安装和可以安装的程序包
yum list package1 显示指定程序包安装情况package1
yum groupinfo group1 显示程序组group1信息yum search string 根据关键字string查找安装包

### 4. 删除程序

yum remove &#124; erase package1 删除程序包package1
yum groupremove group1 删除程序组group1
yum deplist package1 查看程序package1依赖情况


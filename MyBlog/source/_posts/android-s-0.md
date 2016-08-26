---
title: Android源代码编译
date: 2016-08-24 15:32:24
tags: Android开发
categories:  Android
---


# Android源码下载及编译

使用科大镜像（国内无法直接访问google）

首先下载 repo 工具
```bash
mkdir ~/bin
PATH=~/bin:$PATH
curl https://storage-googleapis.lug.ustc.edu.cn/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```
然后建立一个工作目录（名字任意）
```bash
mkdir AndroidSource
cd AndroidSource
```
初始化仓库：

```bash
repo init -u git://mirrors.ustc.edu.cn/aosp/platform/manifest
## 如果提示无法连接到 gerrit.googlesource.com，可以编辑 ~/bin/repo，把 REPO_URL 一行替换成下面的：
## REPO_URL = 'https://gerrit-googlesource.lug.ustc.edu.cn/git-repo'
```
下载特定的android源码版本
```bash
repo init -u git://mirrors.ustc.edu.cn/aosp/platform/manifest -b android-4.0.1_r1
```
同步源码树
```bash
repo sync
```
可能需要漫长的等待android源码的下载


源码下载完成后，开始编译android源码
```bash
source build/envset.sh
lunch full-eng
make
```
简单的三步即可完成源码的编译

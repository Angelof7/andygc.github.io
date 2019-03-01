---
layout: post
title: git clone后没有代码
categories: [git]
tags: [Git]
---

用git clone 下载项目后，目录中只有.git目录

## 原因
---------------
这个repository中存在多个分支下载的这个分支是个bare repository

## 解决方法
---------------
1. 查看远程分支：
> git branch -r -a  

2. checkout：
> git checkout branch_name

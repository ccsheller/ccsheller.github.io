---
title: git 备忘
date: 2020-10-23 14:35:33
tags:
    - git
categories: 工具
---

git 中有些命令，不常用，常遗忘

<!--more-->

```
$ git daemon --base-path=. --export-all --verbose
$ git status --ignored
$ git pull git://192.168.2.2/.git --rebase --autostash master

```


给项目添加 **.gitattributes** 文件后，源码文件会变为修改状态。试了几个方案没解决，先做个备注，[github这个帮助感觉靠谱](https://docs.github.com/en/free-pro-team@latest/github/using-git/configuring-git-to-handle-line-endings)

```
$ git add --renormalize .
```
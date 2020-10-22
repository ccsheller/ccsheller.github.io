---
title: ubuntu使用遇到的问题
date: 2020-10-22 15:16:51
tags:
    - ubuntu
categories: 工具
---

项目中要用linux，于是在虚拟机中配置了ubuntu 14.04（不想用多台机器）。用了段时间，无法忍受VS Code使用中的反应迟钝，于是准备换实体机。

<!--more-->

弄了台机器，装上ubuntu 14.04，然后配置远程桌面。 xrdp 方式，始终不成功。vnc方式，配置的界面太模糊。

以前linux的远程桌面没这难配的（以前用fedora），用过xrdp（时间久远，好像是这个，试过的软件比较多）,用过nomachine，还用过其他的。

没办法，安装新的ubuntu试试。由于u盘容量不够，只好用ubuntu自升级。结果升级过程中崩溃了，只有重装。

于是又下了ubuntu 16.04，写入u盘，安装后，配置远程桌面。

找了文档，xrdp 加 mate 桌面，终于可用了。看习惯高分屏的界面，mate这种还是有点不习惯，还可接受。

装上VS Code，运行不起来，将其在终端中运行，提示：

```
Xlib:  extension "XInputExtension" missing on display ":1".
```

搜索得知是Electron的问题，如下修改：

```
cd /usr/lib/x86_64-linux-gnu/
cp libxcb.so.1 libxcb.so.1.bak
sudo sed -i 's/BIG-REQUESTS/_IG-REQUESTS/' libxcb.so.1
```

使用时还有个问题，中文输入法始终不起作用，配置远程桌面的文章提到过输入法的问题，不想看了。

多次尝试还是不行，想换用谷歌输入法，好像没有linux版的。那试试搜狗输入法（广告嫌疑）吧，结果很好用，切换成五笔，终于可用了。

搜狗输入法安装：

官网下载deb，然后终端中执行

```
sudo apt remove fcitx* && sudo apt autoremove
sudo dpkg -i ~/Downloads/sogoupinyin*.deb; sudo apt -f install
```

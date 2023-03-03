---
title: xfc4在远程桌面下随机休眠问题
date: 2023-03-03 11:05:12
tags:
    - ubuntu
categories: 工具
---

在ubuntu下配置远程桌面，以前是xrdp加mate desktop。这次升级后远程桌面失效了。

查了下重新配置mate，其太过复杂，各种权限的配置。放弃。

查到也可以结合xfc4 desktop。这个加个.xsession配置就好了。

用了会，发现机器总是一会就休眠了。这没法正常用。关闭所有电源设置，屏幕保护，依然休眠。

最终尝试了如下命令
```
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```
解决问题

看了下输出，其将这些目标都置null


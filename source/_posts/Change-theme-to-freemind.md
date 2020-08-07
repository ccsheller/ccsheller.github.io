---
title: 更改主题为freemind
date: 2020-08-07 11:04:27
tags:
    - Hexo
categories: 杂记
---

NexT主题，虽然被用很广，但我不喜欢页面显示的动画，这样内容显示前有一段等待时间。最主要是没有找到合适的评论功能，最后使用hexo-next-utteranc。utteranc相比其他利用GitHub Issues需要的权限小，其他相似的还要暴露clientSecret（现在还没有相关严重恶意利用爆出）。在安装utteranc时，其依赖的next-util在淘宝镜像中是错的，又是一翻折腾，手工修改了package-lock.json的引用到npm中。终于可用了，换个网络，不是网页连接不了，就是评论显示不了，不得不感慨墙又砌高了。评论不显示主因是utteranc的cdn被墙。

那换个评论功能，找到了Commento，这个也可以自己搭VPS里，但整合进NexT，总是放不到合适的位置（才接触JavaScript）。试了几次放弃，换主题。

看了几个有人气主题，有些风格简单比较适合。当注意到freemind直接支持了评论，它这个评论利用GitHub Issues，只作展示，写评论跳转GitHub页面。这减小了被墙的机率，也不需要授权。

改了下基本配置，换了颜色模式，就切换了。

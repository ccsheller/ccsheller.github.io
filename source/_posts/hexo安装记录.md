---
title: hexo安装记录
date: 2020-08-01 11:43:44
tags:
---

安装还算顺利，照着官网README就可以了。换了主题next。这个next有几个版本。
感觉hexo-theme-next放node_modules目录也可以用。但是.gitignore默认是排除node_modules的，
所以还是将hexo-theme-next拷贝到theme下并重命名为next。

部属到github有点折腾。如果用hexo-deployer-git还是比较简单，但是每次都要手动发布。看好像支持Travis CI ，可以自动发布，这个可以省事。不了解github Token，看hexo和Travis CI应该是生成一个私人Token，但github的Token不分库。

主要问题就是这个部属文件的书写.travis.yml。看doc下面的评论比较靠谱，拷贝了一份和官方的比对。没问题，只是没有缩进。所有提交，Travis CI也自动编译了，感觉没有部署回github。难道是我使用source分支作为主分支的问题？以前github只支持master分支的PAGES(以前可能是以讹传讹)。重新用master分支再来一遍，还是没有部属。难道是缩进的问题，查了YAML语法，其使用空格缩进，用缩进表示层次，用了缩进后提交，还是不行，显示Could not parse。难道是有不可见字符，vs code显示不可见字符，没发现不可见字符。难道是结尾要有空行，还是不行。重新看hexo和Travis CI
的文档，比对，没有发现不同，太折腾。突然看到hexo 中的配置用的-(减号，markdown怎么给-加粗)，Travis CI中用的_下划线，修改后还是Could not parse。我看Travis CI不要缩进也是可以用的，删除所有空格，还是不行。没招了，只有检查不可见字符了，找到[config travis-ci](https://config.travis-ci.com/explore)。从vs code贴过去居然是彩色的格式，用nodepad过渡，显示undefined: undefined。删除缩进，试了几次可以解析了。从nodepad贴回vs code，加上缩进，提交，Travis CI运行正常了。

**小结：**

注意不可见字符
注意-(减号）和_(下划线)
注意缩进

用ccsheller.github.io是可以访问了。但是ccshell.com还是显示以前的内容。本来昨天我把以前Pages库重命名后，以前的是失效了。后来我又无意调整了它的GitHub Pages配置项（我以为调整的是ccsheller.github.io），以前的和新的项目都可用了，说明github可以启用多个Pages项目（我记得每个用户只能启一个）。调整ccsheller.github.io的配置，弄不好。又调整了以前项目的配置项到None分支，也就是禁用Pages。等待会，ccshell.com也失效了，但是还是不
切换到新页面。看了GitHub Pages配置项，说published at ccsheller.github.io ，可能这里不是自动更新的，我就切换了下其分支，等待一会。ccshell.com终于可以正常显示。
---
title: 记查找Visual Studio 编译慢的原因
date: 2021-01-07 09:14:23
tags:
    - 编译
    - Visual Studio
categories: 笔记
---

工作这么多年，时常能遇到意外（惊喜）（惊吓）。

项目需要Visual Studio 2015编译，装好环境，调整编译脚本，debug编译没问题了。有同事要测试下功能，需要release版本。要他自己装环境，编译，还没不如我给他编好。这样更快。

编release版发现，有些文件居然要十多分钟到半小时，CPU满负荷，感觉就是死循环，这就离谱了。整个项目，debug也只要半小时。

<!--more-->

生活又教我做人。

终端显示，在某cpp文件中实例化模板，正常想法是，VS实例化模板慢。其实是很普遍的模板，std::map->insert(std::pair())，难道2015，标准模板库有什么bug。上网查找模板实例化的bug，看了不少问题，和我这现象感觉不搭。装了一个更新，没有效果。且标准模板库有问题，这个影响也太大。不可能还没修正。

难道是脚本中debug和release文件组织方式导致的，模块中release是把所有cpp引入一个cpp后编译的。调整后，没有变化。调整cpp在编译的位置也没本质变化。

折腾几小时没有进展，项目有的模块带了VS工程，打开来，难道和脚本编译有些参数导致的。比对，是有些参数不一样，查各参数的说明，照理不会影响这么大。调整VS工程，在IDE中编译，在某些文件中编译也很慢，那应该不是参数差异。

问题出在哪。有些文章提到可以加选项，让cl打编译log，报告耗时情况。也只有一试了，加上/d2cgsummary，界面中可以选中项目属性，命令行其中填上；命令行也是可行的。

加上后，输出没什么变化。以为这个选项没起作用。该耗时还是耗时，停止输出好一会后，终于有了新的输出，显示了耗时函数。找到耗时函数，函数实现在头文件中，调用了很普通的一些函数，表面也没看调用之前说的模板实例化。内联函数，编译会耗时点，很正常，但现在耗的时间太多，就不正常了。也有文章提到forceinline，编译慢的问题，此处没看到类似的语句。怎么forceinline，以前没听说过。感觉这个/d2cgsummary选项没什么作用。

先让它编着，继续查找。没查到2015有bug，有些说的bug也是2017，2019的问题。反复说的都是类似的问题，没太大价值。

可能三个多小时，一个模块终于编完了。再次审视输出，找到对应函数，检查函数调用的函数，其调用的函数实现也在头文件中，不同的是后者函数声明前有个意思是inline的宏，看了其宏，在是msvc时会定义为__forceinline。原来问题在这。

__forceinline引起问题，能否加选项进行一定的控制。没找到有用信息。还是通过脚本预定义宏为inline，屏蔽__forceinline，这样效果快。

#### 结论

/d2cgsummary 在查找编译慢时很有用

__forceinline 需要谨慎使用
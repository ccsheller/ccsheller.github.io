---
title: Visual Studio Code 的使用
date: 2020-08-07 15:05:24
tags:
    - Visual Studio Code
categories: 工具
---

VS Code都说牛，其给我的感觉就是功能太丰富，抓不住核心。其中智能提示只显示当前文件内容，搜索一翻，有的说用TSD，有的说用Typings。我就用来学JavaScript，书上的例子，感觉不到变化，反正是没搞明白。换用WebStorm，也没明显的区别，还折腾了好久的主题（喜欢Linux terminal emulator中取消系统配色后的淡黄色），还启动慢，太臃肿。换回VS Code，为此需要弄清其使用，怎么配置好intellisense，其WorkSpace有什么功能性的必要，.vscode文件是否在项目中的必要。

工具用的顺手还是要读文档。此文记录读文档时，觉得重要的内容。

#### 改设置

MenuBar -> File -> Preferences

#### 各种选择模式，列编辑

这快捷键有点复杂，直接装Vim扩展（也可以装Spacemacs），hjkl就导航了，v和ctrl + v 就选择了。

= 格式化

za 切换代码折叠

#### Ctrl 快捷键和Vim冲突

在Settings中搜索vim handle keys，点击Edit in settings.json，对于要屏蔽的项添加

```
"vim.handleKeys": {
    "<C-a>": false,
    "<C-f>": false
  }
```

- Ctrl + o windows习惯用Ctrl来打开文件（Linux Vim 列出文件夹文件不好用，Spacemacs相对好不少）。因此屏蔽Vim的Ctrl + o
- Ctrl + b

#### Ctrl Space 提示与系统冲突

找到Preferences下的Keyboard Shortcuts搜Ctrl Space，鼠标移入想修改项后点击前面的笔，在弹出撒个框中按想设置的快捷键，我设定为 Ctrl + Alt + Space。如果提示重复就按回车。

#### 代码片断输入

在输入过程中可以跟据提示选择项目后按tab快速输入一个对应的片断，并用tab在输入片断内切换位置。比如输入try，选择trycatch，按tab。

#### Emmet的支持

#### 输入检查

设定中搜索check js，启用它

#### 原生快捷键

|||
|---|--|
|Alt + ←|前一个编辑位置|
|Alt + →|后一个编辑位置|
||
|Ctrl + b|切换侧边栏显示|
|Ctrl + <num>|切换编辑组|
|Alt + <num>|切换编辑文件|
|Ctrl + Tab|当前编辑组中切换文件|
||
|Ctrl + p|搜索并打开文件|
|Ctrl + Shift + p|命令面板|
|Ctrl + Shift + o|根据Symbol在当前文件搜索，可以快速跳转|
|Alt + F12|Symbol窥视|
||
|F2|重构重命名|
|F8|错误和警告导航|

#### 扩展

Visual Studio IntelliCode 跟据上下文的AI提示，无需配置。其他语言需要配置。

#### 其他功能

整合了终端系统


---
title: Visual Studio Code 的使用
date: 2020-08-07 15:05:24
tags:
    - Visual Studio Code
categories: 工具
---

VS Code都说牛，其给我的感觉就是功能太丰富，抓不住核心。其中智能提示只显示当前文件内容，搜索一翻，有的说用TSD，有的说用Typings。我就用来学JavaScript，书上的例子，感觉不到变化，反正是没搞明白。换用WebStorm，也没明显的区别，还折腾了好久的主题（喜欢Linux terminal emulator中取消系统配色后的淡黄色），还启动慢，太臃肿。换回VS Code，为此需要弄清其使用，怎么配置好intellisense，其WorkSpace有什么功能性的必要，.vscode文件是否在项目中的必要。

工具用的顺手还是要读文档。此文记录读文档时，觉得重要的内容。

### 改设置

MenuBar -> File -> Preferences

### 各种选择模式，列编辑

这快捷键有点复杂，直接装Vim扩展（也可以装Spacemacs），hjkl就导航了，v和ctrl + v 就选择了。

### Ctrl 快捷键和Vim冲突

在Settings中搜索vim handle keys，点击Edit in settings.json，对于要屏蔽的项添加

```
"vim.handleKeys": {
    "<C-a>": false,
    "<C-f>": false
  }
```

- Ctrl + o windows习惯用Ctrl来打开文件（Linux Vim 列出文件夹文件不好用，Spacemacs相对好不少）。因此屏蔽Vim的Ctrl + o

### Ctrl Space 提示与系统冲突

找到Preferences下的Keyboard Shortcuts搜Ctrl Space，鼠标移入想修改项后点击前面的笔，在弹出撒个框中按想设置的快捷键，我设定为 Ctrl + Alt + Space。如果提示重复就按回车。

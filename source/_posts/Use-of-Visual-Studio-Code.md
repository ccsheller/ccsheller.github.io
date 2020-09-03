---
title: Visual Studio Code 的使用
date: 2020-08-07 15:05:24
tags:
    - Visual Studio Code
categories: 工具
---

VS Code都说牛，其给我的感觉就是功能太丰富，抓不住核心。其中智能提示只显示当前文件内容，搜索一翻，有的说用TSD，有的说用Typings。我就用来学JavaScript，书上的例子，感觉不到变化，反正是没搞明白。换用WebStorm，也没明显的区别，还折腾了好久的主题（喜欢Linux terminal emulator中取消系统配色后的淡黄色），还启动慢，太臃肿。换回VS Code，为此需要弄清其使用，怎么配置好intellisense，其WorkSpace有什么功能性的必要，.vscode文件是否在项目中的必要。

<!--more-->

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
|Ctrl + num|切换编辑组|
|Alt + num|切换编辑文件|
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

|||
|---|---|
|Visual Studio IntelliCode| 跟据上下文的AI提示，无需配置。其他语言需要配置。
|Prettier|  代码格式化
|ESLint| 代码检测

#### 替换终端为git bash

在设置文件setting.json（在设置中搜Automation Shell: Windows，点Edit in setting.json），补全条目到bash目录

```
"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
```

#### Debug功能（node.js）

节选doc

```
You will need to create a debugger configuration file launch.json for your Express application. Click on the Run icon in the Activity Bar and then the Configure gear icon at the top of the Run view to create a default launch.json file. Select the Node.js environment by ensuring that the type property in configurations is set to "node". When the file is first created, VS Code will look in package.json for a start script and will use that value as the program (which in this case is "${workspaceFolder}\\bin\\www) for the Launch Program configuration.

Save the new file and make sure Launch Program is selected in the configuration drop-down at the top of the Run view. Open app.js and set a breakpoint near the top of the file where the Express app object is created by clicking in the gutter to the left of the line number. Press F5 to start debugging the application.
```

#### JavaScript 项目

jsconfig.json 是项目配置文件

判断一个js文件是否属于项目，打开此文件执行命令JavaScript: Go to Project Configuration

实现分项目控制，每个项目需要自己的jsconfig.json

#### 智能提示

编写代码时，没有对应智能提示需要在jsconfig.json中添加如下配置（以nodejs为例）:

```
{
    "typeAcquisition": {
        "include": [
            "node"
        ]
    }
}
```

这样vscode将会引入对应库类型申明，立即生效。如果没有效，等待片刻，vscode需要从网上自动同步数据。如果还是没效，可能需要重启下。重启没效果可能是网络问题，或者库还未公开支持（公开支持[查询](https://microsoft.github.io/TypeSearch/)）。

#### 全局变量和类型检查

（引用文档）

Let's say that you are working in legacy JavaScript code that uses global variables or non-standard DOM APIs:
 ```
window.onload = function() {
  if (window.webkitNotifications.requestPermission() === CAN_NOTIFY) {
    window.webkitNotifications.createNotification(null, 'Woof!', '🐶').show();
  } else {
    alert('Could not notify');
  }
};
 ```
If you try to use // @ts-check with the above code, you'll see a number of errors about the use of global variables:

 ```
Line 2 - Property 'webkitNotifications' does not exist on type 'Window'.
Line 2 - Cannot find name 'CAN_NOTIFY'.
Line 3 - Property 'webkitNotifications' does not exist on type 'Window'.
 ```
If you want to continue using // @ts-check but are confident that these are not actual issues with your application, you have to let TypeScript know about these global variables.

To start, create a jsconfig.json at the root of your project:

 ```
{
  "compilerOptions": {},
  "exclude": ["node_modules", "**/node_modules/*"]
}
 ```
Then reload VS Code to make sure the change is applied. The presence of a jsconfig.json lets TypeScript know that your Javascript files are part of a larger project.

Now create a globals.d.ts file somewhere your workspace:

 ```
interface Window {
  webkitNotifications: any;
}

declare var CAN_NOTIFY: number;
 ```
d.ts files are type declarations. In this case, globals.d.ts lets TypeScript know that a global CAN_NOTIFY exists and that a webkitNotifications property exists on window. You can read more about writing d.ts in the TypeScript documentation. d.ts files do not change how JavaScript is evaluated, they are used only for providing better JavaScript language support.



#### 支持的两种设置作用域

用户设置（User Settings） 全局有效

工作空间设置（Workspace Settings） 当前工作工间有效，存于当前项目根目录下的.vscode目录中。

打开设置（File > Preferences > Settings，Ctrl + ,）,可以进行两种切换。

只影响当前项目的设置或者需要项目组中共享的设置可以放到工作空间设置。

安全原因，如下设置在工作空间设置被忽略：

+ git.path
+ terminal.external.windowsExec
+ terminal.external.osxExec
+ terminal.external.linuxExec

#### 多根工作空间（Multi-root Workspaces）

有多个相关项目时，可以同时打开多个项目目录，然后保存工作空间（.code-workspace），保存位置自己定，以后就可以用这个工作空间文件一次打开多个项目。

节选.code-workspace内容：

```
{
  "folders": [
    {
      // Source code
      "name": "Product",
      "path": "vscode"
    },
    {
      // Docs and release notes
      "name": "Documentation",
      "path": "vscode-docs"
    },
    {
      // Yeoman extension generator
      "name": "Extension generator",
      "path": "vscode-generator-code"
    }
  ]
}
```

支持注释，命名（将代替文件夹名显示），路径指到项目根目录（建议相对路径，变于分享），支持设置（可以添加settings节），支持compounds、launch、tasks、extensions

限定搜索的项目，搜索框先输入./,比如只搜索project1中的.js，可以输入./project1/**/*.js

#### 重构

- 提取方法
- 提取变量
- 符号重命名

#### 任务

配置文件tasks.json在工作空间配置文件夹下.vscode

#### TypeSearch

查询Type是否支持智能提示[TypeSearch](https://microsoft.github.io/TypeSearch/)

#### 其他功能

支持 JSDoc

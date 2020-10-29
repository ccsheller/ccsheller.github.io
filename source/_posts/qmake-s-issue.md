---
title: 记qmake使用中遇到的问题
date: 2020-10-29 09:06:46
tags:
    - qmake
categories: 工具
---

项目中的工程文件是用的qmake的pro文件。

在使用中遇到了生成的路径字符串问题。

项目有个主pro文件，TEMPLATE = subdirs，因此子目录可以用.file和.makefile文件来分别设置项目pro文件和生成的makefile位置。为了让任何生成文件都和源码分离（让项目代码整洁），将.makefile路径设置到了统一输出文件夹中。

<!--more-->

qmake正常，但是make时报其pro找不到也没法生成。看了下生成的makefile，其依赖了pro文件，根据显示的目径，是找不到对应的pro的。

- 换用.makefile路径中使用的变量，失败。
- 换用.makefile路径改用相对路径，失败。
- 将路径指到pro文件目录，成功。
- 能否生成文件中不依赖pro文件（项目每次会重新生成makefile文件），没找到控制变量，失败。
- 项目中定义了ordered，此变量是否能控制不依赖pro文件，失败。
- 改变生成路径的嵌套层次，失败。
- 将.file换用.subdir，失败。
- 将路径指到pro文件目录，编译后自动删除makefile文件（脚本见后面），成功。
- 查看qmake源码是否有控制变量（存在未文档化变量），找到一个QMAKE_PROJECT_DEPTH，调整其值。其只能控制嵌套的深度。失败。

源码（qtbase/qmake/generators/makefile.cpp:MakefileGenerator::fileFixify）中了解，此处逻辑只会按照pro所在目录和生成makefile文件目录，向上找到一个共同的父目录，然后加上向上找的嵌套层次。因此生成makefile文件位置，没有调整的余地。

#### 编译后自动删除makefile文件

在想要清除makefile文件的目录的pro中添加（我们有一个统一的引入pri文件，所以放入其中）：
```
# delete Makefile
QMAKE_EXTRA_TARGETS += selfclean
selfclean.commands = -$(DEL_FILE) $(MAKEFILE)
selfclean.CONFIG = phony

POST_TARGETDEPS += selfclean
```

QMAKE_EXTRA_TARGETS 添加了一个额外目标

commands 是此目标的执行命令，此处就是删除makefile本身

phony 指定此目标是一个伪目标，不会生成文件

POST_TARGETDEPS 将目标依附到所有内置依赖的后面

#### 得出结论

子目录生成的makefile只能放在pro文件同目录或pro所在目录子目录。

最终选择：将路径指到pro文件目录，编译后自动删除makefile文件。

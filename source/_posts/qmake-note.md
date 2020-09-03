---
title: qmake 笔记
date: 2020-09-03 11:26:26
tags:
    - qmake
categories: 工具
---

项目中涉及qmake的pro文件，因此读了其文档，作如下记录以备查。

<!--more-->

#### 操作符(Operators)

=   +=   -=   类似c的赋值，自加（拼接），自减。多个值用空格分，多行用\

```
SOURCES = main.cpp mainwindow.cpp \
          paintwidget.cpp
CONFIG += console
```

*= 添加唯一值，值不存在时添加

~= 替代值，用正则表达式

$$ 获取变量内容

$${...} 获取变量内容，其会依附到值后，且不会添加空格

$$(...) 获取环境变量内容

$(...) 用于生成makefile运行时，获取环境变量内容

$$[...] 获取qmake属性

#### 作用域(Scopes)

当条件为真时，执行作用域内的声明

```
<condition> {
    <command or definition>
    ...
}
```

条件支持逻辑反!

作用域能嵌套

:操作，相当于逻辑与。可以拼接条件，赋值

```
macx:CONFIG(debug, debug|release) {
    HEADERS += debugging.h
}

win32:DEFINES += USE_MY_STUFF
```

| 逻辑或

if 调整操作的优先顺序

```
if(win32|macos):CONFIG(debug, debug|release) {
    # Do something on Windows and macOS,
    # but only for the debug configuration.
}
win32|if(macos:CONFIG(debug, debug|release)) {
    # Do something on Windows (regardless of debug or release)
    # and on macOS (only for debug).
}
```

条件支持通配符配置

```
win32-* {
    # Matches every mkspec starting with "win32-"
    SOURCES += win32_specific.cpp
}
Note: Historically, checking the mkspec name with wildcards like above was qmake's way to check for the platform. Nowadays, we recommend to use values that are defined by the mkspec in the QMAKE_PLATFORM variable.
```

else 作用域

```
win32:xml {
    message(Building for Windows)
    SOURCES += xmlhandler_win.cpp
} else:xml {
    SOURCES += xmlhandler.cpp
} else {
    message("Unknown configuration")
}
```

CONFIG变量特殊对待，其列表每一个值可以作为条件。

```
CONFIG += opengl

opengl {
    TARGET = application-gl
}
```

#### 变量(Variables)

除了内置变量，支持自定义变量

#### 替换函数(Replace Functions)

使用$$将函数返回值赋给变量

```
HEADERS = model.h
HEADERS += $$OTHER_HEADERS
HEADERS = $$unique(HEADERS)
```

自定义替换函数

```
defineReplace(functionName){
    #function code
}

defineReplace(headersAndSources) {
    variable = $$1
    names = $$eval($$variable)
    headers =
    sources =

    for(name, names) {
        header = $${name}.h
        exists($$header) {
            headers += $$header
        }
        source = $${name}.cpp
        exists($$source) {
            sources += $$source
        }
    }
    return($$headers $$sources)
}
```

#### 测试函数(Test Functions)

用于作用域的条件

```
count(options, 2) {
    message(Both release and debug specified.)
}

defineTest(allFiles) {
    files = $$ARGS

    for(file, files) {
        !exists($$file) {
            return(false)
        }
    }
    return(true)
}
```

#### 常用内置变量

Variable|Contents
-|-
CONFIG|General project configuration options.
DESTDIR|The directory in which the executable or binary file will be placed.
FORMS|A list of UI files to be processed by the user interface compiler (uic).
HEADERS|A list of filenames of header (.h) files used when building the project.
QT|A list of Qt modules used in the project.
RESOURCES|A list of resource (.qrc) files to be included in the final project. See the The Qt Resource System for more information about these files.
SOURCES|A list of source code files to be used when building the project.
TEMPLATE|The template to use for the project. This determines whether the output of the build process will be an application, a library, or a plugin.

#### 项目模版(Project Templates)

TEMPLATE 变量的作用，定义项目类型。可能取值：

Template|qmake Output
-|-
app (default)|Makefile to build an application.
lib|Makefile to build a library.
aux|Makefile to build nothing. Use this if no compiler needs to be invoked to create the target, for instance because your project is written in an interpreted language. Note: This template type is only available for Makefile-based generators. In particular, it will not work with the vcxproj and Xcode generators.
subdirs|Makefile containing rules for the subdirectories specified using the SUBDIRS variable. Each subdirectory must contain its own project file.
vcapp|Visual Studio Project file to build an application.
vclib|Visual Studio Project file to build a library.
vcsubdirs|Visual Studio Solution file to build projects in sub-directories.

#### 一般配置(General Configuration)

CONFIG 可以指定release或debug，最后一个将起作用，如果要同时编译使用debug_and_release 

#### SUBDIRS

SUBDIRS 每个值可以限定特定的属性

Modifier|Effect
-|-
.subdir|Use the specified subdirectory instead of SUBDIRS value.
.file|Specify the subproject pro file explicitly. Cannot be used in conjunction with .subdir modifier.
.depends|This subproject depends on specified subproject(s).
.makefile|The makefile of subproject. Available only on platforms that use makefiles.
.target|Base string used for makefile targets related to this subproject. Available only on platforms that use makefiles.

```
SUBDIRS += my_executable my_library
my_executable.subdir = app
my_executable.depends = my_library
my_library.subdir = lib
```


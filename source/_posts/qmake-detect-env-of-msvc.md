---
title: qmake 在windows上判断msvc环境有问题
date: 2021-01-08 10:02:54
tags:
    - qmake
categories: 工具
---

测试那边需要32位的版本。脚本是一定程度支持编32位的，脚本调整下后，build，结果qmake生成的makefile名字还是64位的。

看来qmake（qt是64位）不能从vcvarsall.bat中决定要编32位，难道需要加什么参数。

<!--more-->

一通查找，找到一个qmake选项-spec，一个环境变量QMAKESPEC，一个内部变量QMAKE_TARGET.arch，都不好用。qmake -query QMAKE_MKSPECS 在qt5中已经失效了。在其mkspecs目录中，也只有win32-msvc2015，经查qt将32位和64位统一在一份配置中了。

于是运行vcvarsall.bat x86，输出PATH，看了下没有想像中的一些路径信息，比如VC，x86, include

于是分析了下qmake源码

```
void QMakeEvaluator::loadDefaults()
{
    ...
        vars[ProKey("QMAKE_TARGET.arch")] = msvcArchitecture(
                vcInstallDir,
                m_option->getEnv(QLatin1String("PATH")));
    ...
}
static ProString defaultMsvcArchitecture()
{
#if defined(Q_OS_WIN64)
    return ProString("x86_64");
#else
    return ProString("x86");
#endif
}
static ProString msvcArchitecture(const QString &vcInstallDir, const QString &pathVar)
{
    if (vcInstallDir.isEmpty())
        return defaultMsvcArchitecture();
    QString vcBinDir = vcInstallDir;
    if (vcBinDir.endsWith(QLatin1Char('\\')))
        vcBinDir.chop(1);
    const auto dirs = pathVar.split(QLatin1Char(';'));
    for (const QString &dir : dirs) {
        if (!dir.startsWith(vcBinDir, Qt::CaseInsensitive))
            continue;
        const ProString arch = msvcBinDirToQMakeArch(dir.mid(vcBinDir.length() + 1));
        if (!arch.isEmpty())
            return arch;
    }
    return defaultMsvcArchitecture();
}
static ProString msvcBinDirToQMakeArch(QString subdir)
{
    int idx = subdir.indexOf(QLatin1Char('\\'));
    if (idx == -1)
        return ProString("x86");
    subdir.remove(0, idx + 1);
    idx = subdir.indexOf(QLatin1Char('_'));
    if (idx >= 0)
        subdir.remove(0, idx + 1);
    subdir = subdir.toLower();
    if (subdir == QLatin1String("amd64"))
        return ProString("x86_64");
    // Since 2017 the folder structure from here is HostX64|X86/x64|x86
    idx = subdir.indexOf(QLatin1Char('\\'));
    if (idx == -1)
        return ProString("x86");
    subdir.remove(0, idx + 1);
    if (subdir == QLatin1String("x64"))
        return ProString("x86_64");
    return ProString(subdir);
}
```

其取了环境变量PATH的值，它会过滤出含有VC安装目录的路径，然后判断结尾目录名字决定是x86或x86_64。

明显它总是输出x86_64，现在就两个疑问了，vcvarsall.bat x86为什么没有设置合理的路径，难道配置的环境有问题；qmake只有找到amd64目录时才会输出x86_64，那amd64来自哪里。

先解决第一个疑问，分析vcvarsall.bat代码，运行并回显，下面代码解决了第一个疑问

```
@if not "%WindowsSdkDir%" == "" @set PATH=%WindowsSdkDir%bin\x86;%PATH%
```

其中%WindowsSdkDir%对应到

```
C:\Program Files (x86)\Windows Kits\10
```

记得装VS2015时，勾选了SDK 10。这就说通了，VS在有SDK时用了SDK的库。此时qmake这边其实不知道这个情况，它在这里有缺陷。

那qmake怎么得到x86_64的，再次检查qmake源码，明白了，其在不能确认环境时，返回默认值。

```
static ProString defaultMsvcArchitecture()
{
#if defined(Q_OS_WIN64)
    return ProString("x86_64");
#else
    return ProString("x86");
#endif
}
```

此默认值在编译时已经确定了。

那我能在.pro文件中根据情况修改QMAKE_TARGET.arch值到x86，是否可行。编译之初很好，等待一会后就报错了。

注意到终端中输出，有宏定义为WIN64，说明qmake还是以64位环境运行。记得在qmake的配置文件msvc-desktop.conf有这样的语句

```
contains(QMAKE_TARGET.arch, x86_64) {
    DEFINES += WIN64
    QMAKE_COMPILER_DEFINES += _WIN64
}
```

这说明qmake在解析.pro文件之前就读取了conf文件。

看来强制改QMAKE_TARGET.arch行不通。

最后还是再装个qt32位环境，调整脚本顺利编译。
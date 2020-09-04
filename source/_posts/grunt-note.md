---
title: grunt note
date: 2020-09-04 09:44:16
tags:
    - grunt
categories: 工具
---

项目中涉及grunt，因此又读了相关文档。

grunt是一个JaveScript版的任务运行工具。

<!--more-->

#### 安装

```
npm install -g grunt-cli
```
此命令并不会安装grunt，其会从项目目录开始找最接近的grunt执行

最简单执行npm，来安装grunt配置package.json

```
npm install grunt --save-dev
```

根据情况修改package.json依赖的grunt版本

```
{
  "name": "my-project-name",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "~0.4.5",
    "grunt-contrib-jshint": "~0.10.0",
    "grunt-contrib-nodeunit": "~0.4.1",
    "grunt-contrib-uglify": "~0.5.0"
  }
}
```

如果现有项目配置好了grunt，直接执行

```
npm install
```

现在需要配置Gruntfile，对应文件Gruntfile.js 或 Gruntfile.coffee

#### Gruntfile

Gruntfile 由下面几个部分主成：


- 封装器(wrapper)函数
- 项目和任务的配置
- 需要载入的Grunt插件和任务
- 自定义任务

##### 封装器(wrapper)函数

所有代码都要放在里面

```
module.exports = function(grunt) {
  // Do grunt-related things in here
};
```

##### 项目和任务的配置

将配置数据作为对象传给grunt.initConfig。
对象创建可以使用任何JS语法。其中可用<% %>引用任何配置属性

```
grunt.initConfig({
  pkg: grunt.file.readJSON('package.json'),
  uglify: {
    options: {
      banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
    },
    build: {
      src: 'src/<%= pkg.name %>.js',
      dest: 'build/<%= pkg.name %>.min.js'
    }
  
```

##### 需要载入的Grunt插件和任务

```
// Load the plugin that provides the "uglify" task.
grunt.loadNpmTasks('grunt-contrib-uglify');
```

##### 自定义任务

可以定义一到多个任务，默认任务指定default

```
grunt.registerTask('default', ['uglify']);
```
或

```
grunt.registerTask('default', 'Log some stuff.', function() {
    grunt.log.write('Logging some stuff...').ok();
  });
```

还可以载入外部任务

```
grunt.loadTasks
```

完整例子

```
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // Load the plugin that provides the "uglify" task.
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Default task(s).
  grunt.registerTask('default', ['uglify']);

};
```

#### 运行

配置好的项目，可以运行了

```
grunt
```

#### Gruntfile 输写

确定需求，找对应插件或自己写

- 想监视文件变动
    - grunt-contrib-watch
- 想检查文件书写样式，最佳实践
    - grunt-contrib-jshint
- 想单元测试
    - grunt-contrib-qunit
- 想并接多个文件到一个
    - grunt-contrib-concat
- 想压缩文件大小
    - grunt-contrib-uglify

将插件和自定义任务组合（相关插件要在package.json中配置，以便安装），在grunt.initConfig中进行对应配置


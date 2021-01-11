---
title: npm 安装包时，失败问题
date: 2021-01-11 10:06:52
tags:
    - npm
categories: 工具
---

npm安装包时，时常有包装失败的问题出现，一般都是网络的问题。可以上代理解决，但不是人人都有代理。

前几天做了些查找，可以顺利安装了。

<!--more-->

第一步，在项目根目录或package.json所在目录，新建 **.npmrc** 文件，填上内容：
```
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl=https://npm.taobao.org/mirrors/phantomjs/
electron_mirror=https://npm.taobao.org/mirrors/electron/
registry=https://registry.npm.taobao.org
```
让npm安装包时，到指定源去下载。

大部分情况，就可以正常安装了。有时有些包内部用到的文件需要从github或amazon下载，此时也会出现失败。

第二步，修改hosts文件，可以解决上述问题。以windows为例，用管理员打开notepad，用notepad打开C:\Windows\System32\drivers\etc\hosts，复制如下内容：

```
# GitHub Start
13.250.177.223 gist.github.com
185.199.110.153 github.io

151.101.113.194 github.global.ssl.fastly.net
52.216.227.168 github-cloud.s3.amazonaws.com
52.74.223.119 github.com

199.232.28.133 avatars1.githubusercontent.com
199.232.28.133 avatars2.githubusercontent.com
199.232.28.133 avatars0.githubusercontent.com
199.232.28.133 avatars3.githubusercontent.com
199.232.28.133 raw.githubusercontent.com
199.232.28.133 user-images.githubusercontent.com
199.232.28.133 avatars.githubusercontent.com
199.232.28.133 github.map.fastly.net
199.232.28.133 avatars7.githubusercontent.com

# Amazon AWS Start
54.239.31.69	aws.amazon.com
54.239.30.25	console.aws.amazon.com
54.239.96.90	ap-northeast-1.console.aws.amazon.com
54.240.226.81	ap-southeast-1.console.aws.amazon.com
54.240.193.125	ap-southeast-2.console.aws.amazon.com
54.239.54.102	eu-central-1.console.aws.amazon.com
177.72.244.194	sa-east-1.console.aws.amazon.com
176.32.114.59	eu-west-1.console.aws.amazon.com
54.239.31.128	us-west-1.console.aws.amazon.com
54.240.254.230	us-west-2.console.aws.amazon.com
54.239.38.102	s3-console-us-standard.console.aws.amazon.com
54.231.49.3	s3.amazonaws.com
52.219.0.4	s3-ap-northeast-1.amazonaws.com
54.231.242.170	s3-ap-southeast-1.amazonaws.com
54.231.251.21	s3-ap-southeast-2.amazonaws.com
54.231.193.37	s3-eu-central-1.amazonaws.com
52.218.16.140	s3-eu-west-1.amazonaws.com
52.92.72.2	s3-sa-east-1.amazonaws.com
54.231.236.6	s3-us-west-1.amazonaws.com
54.231.168.160	s3-us-west-2.amazonaws.com
52.216.80.48	github-cloud.s3.amazonaws.com
54.231.40.3	github-com.s3.amazonaws.com
52.216.20.171	github-production-release-asset-2e65be.s3.amazonaws.com
52.216.228.168	github-production-user-asset-6210df.s3.amazonaws.com
# GitHub End
```

当然上述ip有可能失效，可以用脚本ping下各个域，找出失效ip，然后查找域名对应ip。有些域可以和相近的域的ip混用。
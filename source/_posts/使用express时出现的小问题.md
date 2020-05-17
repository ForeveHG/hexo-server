---
title: 使用express时出现的小问题
date: 2017-01-20 17:55:09
categories: 技术
tags: [Node,Express]
---
在使用`cnpm install express -g`全局安装express后,我对全局安装这个概念理解偏差了，我以为全局安装express后，在任何项目中都可以直接使用，所以我创建新项目后没有再在项目中初始化express,直接`require('express')`,出现了以下错误：
<!--more-->
    
    basedir=$(dirname "$(echo "$0" | sed -e 's,\\,/,g')")
          ^^^^^^^
    SyntaxError: missing ) after argument list
![](http://oj056g1gy.bkt.clouddn.com/expresserror.png)
事实证明这是我理解上的偏差，全局安装的模块只是方便我在目录下直接使用命令，并不是在任何新建的项目中都能直接找到全局安装的模块，新建的项目还需要在项目目录下安装相应的模块，或者直接将模块复制过来。
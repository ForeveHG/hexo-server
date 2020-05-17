---
title: Sublime Text3 手动配置Nodejs环境
date: 2017-01-20 11:30:15
tags: [Node]
categories: 技术
---
用了几天命令行，感觉挺麻烦的，因为常用sublime3开发，就想能不能在sublime3中配置一个运行nodejs的环境，果然有很多这方面的教程，参考后手动配置了nodejs环境，下面是配置过程：
<!--more-->
1.首先在github上下载该包![https://github.com/tanepiper/SublimeText-Nodejs](https://github.com/tanepiper/SublimeText-Nodejs)解压后放到sublimeText3的Packages目录下

2.接下来修改配置文件，打开刚才复制进去的文件夹中，找到Nodejs.sublime-settings文件，打开后修改两个地方：
`"node_command"`
`"npm_command"`
修改后文件中的代码：
```
{
  // save before running commands
  "save_first": true,
  // if present, use this command instead of plain "node"
  // e.g. "/usr/bin/node" or "C:\bin\node.exe"
  "node_command": "D:\\Program Files\\nodejs\\node.exe" ,
  // Same for NPM command
  "npm_command": "D:\\Program Files\\nodejs\\npm.cmd",
  // as 'NODE_PATH' environment variable for node runtime
  "node_path": false,

  "expert_mode": false,

  "ouput_to_new_tab": false
}
```
同样，在这个文件夹中找到Nodejs.sublime-build文件，打开修改两个地方：
第一：`"encoding": "utf8"`
第二："windows"中的`"cmd": ["taskkill","/F", "/IM", "node.exe","&","node", "$file"]`
```
{
  "cmd": ["node", "$file"],
  "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
  "selector": "source.js",
  "shell":true,
  "encoding": "utf8",
  "windows":
    {
        "cmd": ["taskkill","/F", "/IM", "node.exe","&","node", "$file"]
    },
  "linux":
    {
        "cmd": ["killall node; node $file"]
    },
    "osx":
    {
        "cmd": ["killall node; node $file"]
    }
}
```
3.测试
新建一个test.js文件，输入`console.log("success!")`
`Ctrl+B`编译一下，就能看到输出的内容
![](http://oj056g1gy.bkt.clouddn.com/sublimit.png)

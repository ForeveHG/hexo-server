---
title: NodeJs小问题：express不是内部或外部命令
date: 2017-01-20 10:27:10
categories: 技术
tags: [Node]
---
1.nodejs安装之后就需要安装express,使用熟悉的npm install -g express命令安装,但是,安装成功之后居然提示express不是内部或外部命令.
<!-- more -->

![](http://oj056g1gy.bkt.clouddn.com/024f78f0f736afc3137f86e1b119ebc4b64512c6.jpg)
![](http://oj056g1gy.bkt.clouddn.com/express-v.jpg)

2. 为什么会这样子呢?当我们找到安装后的express目录发现比之前熟悉的express少了很多东西,原来,最新express4.0版本中将命令工具分家出来了[项目地址]("https://github.com/expressjs/generator"),所以我们还需要安装一个命令工具,命令如下:
    
    npm install -g express-generator

![](http://oj056g1gy.bkt.clouddn.com/express-generation.png)

3.既然安装好了我们就要测试一下新安装的express到底可不可以使用
于是我使用express创建一个工程:
   
    express helloworld

新版本中命令发生了一些改变, 创建好project之后还需要用npm进行添加依赖和启动:

    cd helloworld

    npm install

    npm start

然后新创建的helloworld就已经运行在3000端口上.
![](http://oj056g1gy.bkt.clouddn.com/express%E6%96%B0%E5%BB%BA%E9%A1%B9%E7%9B%AE.jpg)

4.访问http://localhost:3000/就看到熟悉的页面了
![](http://oj056g1gy.bkt.clouddn.com/express3000.png)

5.以及创建出来的目录效果
![](http://oj056g1gy.bkt.clouddn.com/express%E9%A1%B9%E7%9B%AE%E7%9B%AE%E5%BD%95%E6%95%88%E6%9E%9C.png)

转载：[http://www.cnblogs.com/wmkf/articles/3913215.html]("http://www.cnblogs.com/wmkf/articles/3913215.html")
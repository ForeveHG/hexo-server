---
title: Node学习笔记
date: 2017-01-20 15:25:49
tags: [Node]
categories: 技术
---
1.exports和module.exports的区别：
  module.exports的初始值是一个空对象{};
  exports指向module.exports
  require()返回的module.exports而不是exports
<!--more-->
2.express中的中间件：
所谓中间件，就是在收到请求后和发送响应之前这个阶段执行的一些函数。
参考这篇博客，写的非常好，通俗易懂[http://blog.csdn.net/foruok/article/details/47354737](http://blog.csdn.net/foruok/article/details/47354737)

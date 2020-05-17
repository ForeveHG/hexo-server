---
title: HTMLCollection和NodeList
date: 2018-05-8 20:21:02
categories: 技术
tags: [Javascript]
---
#### 问题和解决过程
对这两个概念有疑惑的起因是我想在阿里图标库批量下载当前页面上的图标，但在页面上没找到如何批量加入购物车的功能，在查看了dom结构后我在控制台写了几句代码希望能批量选择这一页的图标，代码如下：
<!--more-->
```javascript
var elements = document.getElementsByClassName("icon-cover");
for(var i = 0; i < elements.length; i++){
    elements[i].getElementsByTagName("span")[0].click();
}
```
但结果是选中不了，检查了dom结构感觉自己并没有写错，无奈之下只能试试看能不能选中一个：
```javascript
elements[0].getElementsByTagName("span")[0].click();
```
上面这句代码执行后，选中了页面上的第一个图标（），奇怪的是拿elements[1]再去执行选中的话选中的第一个图标被删除了？？？我打印了elements元素之后，发现它比之前多出了一个元素

之前:
![](http://oj056g1gy.bkt.clouddn.com/%E5%8A%A0%E5%85%A5%E8%B4%AD%E7%89%A9%E8%BD%A6%E4%B9%8B%E5%89%8D.png)
之后
![](http://oj056g1gy.bkt.clouddn.com/%E9%80%89%E4%B8%AD%E4%B9%8B%E5%90%8E.png)


多出来的这个span的className也是"icon-cover"，但它的click事件是"remove(xxxx)"，也就是删除添加在购物车的图标，这个元素右侧滑块中，点击购物车可以看到这个span元素，鼠标移上去是一个删除的图标
![](http://oj056g1gy.bkt.clouddn.com/delete.png)
总结一下发生了什么，也就是在我选择一个图标加入购物车后，页面的dom结构发生了改变，并且这个改变也影响了改变前我们选中的元素集合，elements。
因为getElementsByClassName得到的是一个HTMLCollection集合，我去MDN看了HTMLCollection，找到了这句话
>HTML DOM 中的 HTMLCollection 是即时更新的（live）；当其所包含的文档结构发生改变时，它会自动更新。

这是我以前没有注意过的，赶紧记在我的小本本上= =

#### 解决方法
事情到现在我又回去看了dom结构，发现我去取document.getElementsByClassName("icon-cover")就是多次一举，我完全可以用
document.getElementsByClassName("icon-gouwuche1")直接取到这个添加到购物车的span，

![](http://oj056g1gy.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20180508142320.png)

控制台上代码：
```javascript
var spans = document.getElementsByClassName("icon-gouwuche1");
for(var i = 0; i <  .length; i++){
    spans[i].click();
}
```
这样就解决了，成功选到了这125个图标，最开始的问题解决了我又开始想，可以取到及时更新的dom集合，但如果只想取到不更新的怎么办呢,我印象中记得querySelectorAll取到的好像是静态的集合，所以尝试了querySelectorAll方法，先用querySelectorAll选中页面中的div赋值给divs变量，再把div删除几个输出divs发现divs没有任何变化，咦，querySelectorAll不是及时更新的，那么下面这种实现也可以了：

```javascript
var elements = document.querySelectorAll("icon-cover");
for(var i = 0; i < elements.length; i++){
    elements[i].querySelector("span").click();
}
```

顺便发现了一点不同，querySelectorAll取到的是NodeList集合，去MDN看NodeList的API，有这么一句
>document.querySelectorAll 返回一个静态的 NodeList, 也就意味着随后对文档对象模型的任何改动都不会影响集合的内容。

#### 总结
问题结束了，我不禁产生了疑问，HTMLCollection和NodeList有什么不同，找到一篇不错的[文章](https://segmentfault.com/a/1190000006782004)，我做一下总结，方便日后复习：

HTMLCollection和NodeList相同点：
1. 都有length属性，所以都是类数组对象
2. 都可以直接用[index]取值，或者.item(index)取值

HTMLCollection和NodeList不同点：
1. 来源不同
  HTMLCollection由getElementById,getElementsByClassName等方法返回
  NodeList由childNodes属性，querySelectorAll方法返回
2. 包含节点的类型不同
  HTMLCollection只包含html元素节点
  NodeList包含元素节点和其他节点，如文本节点，注释节点等
3. 实时和有时实时 
  HTMLCollection集合都是动态的，实时更新
  NodeList由childNodes属性返回时是动态的，querySelectorAll方法返回的是静态的html元素集合，即本质上是一个静态的HTMLCollection集合对象
4. HTMLCollection还有一个nameItem()方法，可以返回集合中name属性和id属性值的元素

#### 结语
出发点很简单但扯出了一堆东西耽误了一点时间，自己对于js基础还是掌握的不好，非常惭愧，继续学习吧，生命不息折腾不止。

> 参考：
[MDN-NodeList](https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList)
[MDN-HTMLCollection](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCollection)
[HTMLCollection与NodeList](https://segmentfault.com/a/1190000006782004)



---
title: js实现鼠标向上滑动显示导航栏，向下滑动隐藏导航栏.md
date: 2017-11-07 20:53:23
tags: JavaScript
categories: [技术,demo]
---
#### 1.引言
这个效果是掘金的导航栏效果，在实现之前先来看一下实现后的样子，打开[掘金](https://juejin.im/timeline "掘金")后，上下滚动鼠标滚轮查看导航栏的表现。
<!-- more -->

#### 2.js滚轮事件
用js实现一个导航栏随着鼠标滚轮的滚动来显示和隐藏的效果，向上滚动时显示导航栏，向下滚动时隐藏导航栏。想实现这个效果首先要了解鼠标的滚轮事件，滚轮事件的在浏览器兼容方面分为火狐和其他，火狐支持DomMouseScroll，包括ie6在内的其他浏览器支持onmousewheel:
```javascript
document.body.addEventListener("DOMMouseScroll", function(event) { //火狐
    console.dir(event);	
});

document.body.onmousewheel = function(event) { //其他
    event = event || window.event;
    console.dir(event);	
};
```
#### 3.判断滚动方向
在onmousewheel事件中，每次向上滚动event.wheelDelta的值是-120，向下滚动是120；
在DOMMouseScroll事件中，每次向上滚动event.detail的值是3，向下滚动是-3，这里要注意区分。
用一个例子来测试一下：
```javascript
var scrollfn = function(e){
    e = e || window.event;
    if(e.wheelDelta){
        if(e.wheelDelta>0){
            console.log('向上滚动：',e.wheelDelta)
        }
        if(e.wheelDelta<0){
            console.log('向下滚动：',e.wheelDelta)
        }
    }else if(e.detail){
        if(e.detail > 0){
            console.log('向下滚动',e.detail)
        }
        if(e.detail < 0){
            console.log('向上滚动',e.detail)
        }
    }
}
if(document.addEventListener){
    document.addEventListener('DOMMouseScroll',scrollfn,false);
}
window.onmousewheel = document.onmousewheel = scrollfn;
```

#### 4.实现掘金导航栏的效果
现在实现起来就很简单了，在上文代码的基础上进行修改，检测到滚轮向下滚动时，隐藏导航栏，检测滚轮向上滚动时，就显示导航栏，为了表现出来的效果不那么生硬，再加入动画效果，下面放上部分代码
[完整代码查看](https://github.com/ForeveHG/Js-Demo/blob/master/juejin-nav/index.html)
[具体的效果查看]
(https://forevehg.github.io/Js-Demo/juejin-nav/index.html)
```css
.nav {
    height: 5rem;
    width: 100%;
    background: #fff;
    position: fixed;
    top: 0;
    left: 0;
    border-bottom: 1px solid #eee;
    transition: all .2s;
    transform: translate3d(0, 0, 0);
    z-index: 2
}
.slide_hide {
    transition: all .2s;
    transform: translate3d(0, -100%, 0);
}
.content_nav {
    width: 100%;
    height: 3rem;
    background: #fff;
    border-bottom: 1px solid #eee;
    position: fixed;
    top: 5rem;
    left: 0;
    transition: all .2s;
    transform: translate3d(0, 0, 0);
}
.content_nav.top {
    transition: all .2s;
    transform: translate3d(0, -5rem, 0);
}
```
```javascript
var showNav = function () {
    if (nav) {
        nav.setAttribute('class', 'nav');
        contentNav.setAttribute('class', 'content_nav');
    }
}
var hideNav = function () {
    if (nav) {
        let classVal = navClassName.concat(' slide_hide');
        let classContentNav = contentNavClassName.concat(' top');
        nav.setAttribute('class', classVal);
        contentNav.setAttribute('class', classContentNav);
    }
}
var scrollFunc = function (e) {
    e = e || window.event;
    if (e.wheelDelta) {  //判断浏览器IE，谷歌滑轮事件               
        if (e.wheelDelta > 0) { //当滑轮向上滚动时
            showNav();
            // console.log("滑轮向上滚动",e.wheelDelta);  
        }
        if (e.wheelDelta < 0) { //当滑轮向下滚动时 
            hideNav();
            // console.log("滑轮向下滚动",e.wheelDelta);  
        }
    } else if (e.detail) {  //Firefox滑轮事件 在火狐中e.detail为负值时是向上滚动，正值是向下滚动 
        if (e.detail < 0) { //当滑轮向上滚动时  
            showNav();
            // console.log("滑轮向上滚动",e.detail);
        }
        if (e.detail > 0) { //当滑轮向下滚动时  
            hideNav();
            // console.log("滑轮向下滚动",e.detail);  
        }
    }
}
//给页面绑定滑轮滚动事件  
if (document.addEventListener) {//firefox  
    document.addEventListener('DOMMouseScroll', scrollFunc, false);
}
//滚动滑轮触发scrollFunc方法  //ie 谷歌  
window.onmousewheel = document.onmousewheel = scrollFunc;
```
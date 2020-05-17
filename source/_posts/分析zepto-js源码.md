---
title: 分析zepto.js源码
date: 2017-10-07 16:26:29
tags: [JavaScript,zepto]
categories: 技术
---
学js学的有些迷茫，就想着尝试去读一读源码，但jquery太复杂了，于是选择了比较简单的zepto来读，读的zepto版本是1.1.6，先从zepto的结构开始分析吧。
<!-- more -->
#### 1.zepto整体结构
zepto的整体结构比较简单，就是将一个立即执行函数表达式赋值给Zepto变量，然后在全局暴露这个Zepto变量，如果$符号没有被占用，$符号也被赋值为Zepto变量，代码结构如下：
```javascript
var Zepto = (function(){
    var $
    $ = function(){}
    return $
})()
window.Zepto = Zepto
window.$ === undefined && (window.$ = Zepto)
```
##### 1.1 立即执行函数表达式(IIFE)
使用立即执行函数表达，可以避免全局作用域被污染，立即执行函数表达式相当于一个容器，容器内可以访问容器外的变量，但容器外不能访问容器内的变量，这样内外不会发生冲突，相当于建立了一个私有命名空间。

#### 2 Zepto集合对象
在Zepto文档中，首先看到的就是几个核心方法,可以知道$()根据传入参数的不同分别进行了不同的处理，最后都是返回了一个zepto集合对象：
```javascript
$(selector, [context])   ⇒ collection
$(<Zepto collection>)   ⇒ same collection
$(<DOM nodes>)   ⇒ collection
$(htmlString)   ⇒ collection
$(htmlString, attributes)   ⇒ collection v1.0+
Zepto(function($){ ... })  
```
源码中$符号被赋值了一个函数，执行$()就相当于在执行这个函数，看一下这个函数的源码：
```javascript
$ = function(selector, context){
    return zepto.init(selector, context)
}
```
这个函数接收了两个参数，看意思是选择器符号和上下文，并返回了zepto.init函数的执行结果，在这里又出现了一个小写的zepto，那来找一下zepto是怎么定义的吧
```javascript
zepto = {},
```
在代码大概30多行的地方找到了zepto的定义，它被定义为一个空对象，那么zepto.init是它的一个属性，属性值是一个function函数，再来看一下zepto.init主要干了什么吧
```javascript
zepto.init = function(selector, context) {
    var dom
    //一堆判断
    return zepto.Z(dom, selector)
}
```
这个方法对传进来的选择器字符串参数进行一系列的判断，最后返回的都是zepto.Z(xxx)的结果，看一下zepto.Z和它用到的Z这个构造函数
```javascript
zepto.Z = function(dom, selector) {
    return new Z(dom, selector)
}

function Z(dom, selector) {
    var i, len = dom ? dom.length : 0
    for (i = 0; i < len; i++) this[i] = dom[i]
    this.length = len
    this.selector = selector || ''
}
```
也就是说zepto.init最后返回的都是一个Z对象，代码到目前为止是在根据传入的参数包装dom节点，创建一个zepto集合对象，现在忽略复杂的判断，假设传入的参数都是一个css选择器，把代码简化成下面这样，引入到一个html文件中试一下
```javascript
//简化的js文件
var Zepto = (function(){
    var $,zepto={}
    function Z(dom,selector){
        var i,len = dom ? dom.length : 0;
        for(i = 0; i<len; i++) this[i] = dom[i]
        this.length = len
        this.selector = selector || ''
    }
    zepto.Z = function(dom,selector){
        return new Z(dom,selector)
    }
    zepto.init = function(selector,context){
        var dom = document.querySelectorAll(selector)
        return zepto.Z(dom,selector)
    }
    $ = function(selector,context){
       return zepto.init(selector,context)
    }
    return $;
})();
window.Zepto = Zepto;
window.$ === undefined && (window.$ = Zepto)
```
```html
<div class="div1">
    <div class="div2"></div>
</div>
<script src="zepto.js"></script>
```
现在可以根据传入的css选择器选择页面上的dom元素并包装成Zepto集合对象了，但只这样接受css选择符不满足复杂的需求，所以，要对用户传入的参数进行分析，最后都要封装成Zepto对象

##### 分析selector
用户传入的selector可能有下面几种情况：
1.空的，就是什么也没传进来
2.字符串
3.Dom元素
4.Zepto元素
5.其他


#### 2. zepto内部函数
##### 2.1 compact删除数组中的null和undefined值
```javascript
function compact(array) { return filter.call(array, function (item) { return item != null }) }
```
这里用的是'!=',undefined也会被隐式转换为null,所以可以删除掉null和undefined的值

##### 2.2 flatten数组扁平化
```javascript
function flatten(array) { return array.length > 0 ? $.fn.concat.apply([], array) : array }
```
array的第二个参数是一个数组，它会自动将数组转换为参数列表传递给方法，参考：[http://www.cnblogs.com/KeenLeung/archive/2012/11/19/2778229.html](巧用apply)zepto中的flatten方法利用这一特性，将二维数组转化为一维数组

#### 2.3 uniq数组去重
```javascript
 uniq = function (array) { return filter.call(array, function (item, idx) { return array.indexOf(item) == idx }) }
```
indexOf函数得到的是元素在数组中第一次出现的位置索引值，如果当前的元素与元素第一次出现的位置不同，说明这个元素在数组中出现了多次，就将这个元素从数组中删除

#### 2.4 数据类型判断

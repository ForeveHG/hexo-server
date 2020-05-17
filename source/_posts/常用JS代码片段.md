---
title: 常用JS代码片段
date: 2017-05-17 08:39:45
tags: JavaScript
categories: 技术
---
#### 1.隐藏部分数字，如手机号码，身份证号码

```javascript
function mask(str,start,length,mask_char){
    return str.replace(str.substr(start,length),Array(length+1).join(mask_char||"*"))
}
```
<!-- more -->

#### 2.获取指定范围内的随机数

```javascript
function randNum(minnum,maxnum){
    return Math.floor(minnum+Math.random()*(maxnum-minnum));
}
```
randNum(0,10)得到的是0到9之间的随机数

#### 3.全选，全不选
```html
<div>
    <input type="checkbox" id="checkAll"/>全选
    <input type="checkbox"/>吃饭
    <input type="checkbox"/>睡觉
    <input type="checkbox"/>打豆豆
</div>
```
```javascript
var checkAll = document.getElementById('checkAll');
var checkBoxs = document.getElementsByTagName('input');
var select = false;
checkAll.addEventListener('click',function(){
    select = !select;
    for(let i in checkBoxs){
        checkBoxs[i].checked = select;
    }
},false)
```

#### 4.js原生判断元素是否隐藏
当容器元素的style.display 被设置为 "none"时（IE和Opera除外），offsetParent属性返回 null。
```javascript
var isHidden = function (element) {
    return (element.offsetParent === null);
};
```

#### 5.jquery中判断对象是否为空对象
空对象{}肯定是没有属性的，所以通过遍历他的属性来判断它是否是空对象
```javascript
function isEmptyObject(e) {  
    var t;  
    for (t in e)  
        return !1;  
    return !0  
}  
```

#### 5.数组去重
判断当前元素的位置是否是该元素第一次出现的位置，如果不是说明元素重复。indexOf是查找元素第一次出现位置的索引值
```javascript
function uniq(array){
    return Array.prototype.filter.call(array,function(item,idx){
        return array.indexOf(item) == idx;
    })
}
```
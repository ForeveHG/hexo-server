---
title: 从头学习HTMl5(一)
date: 2017-06-04 17:54:59
categories: 技术
tags: [HTML5]
---
#### 1.注释标签

<!--注释内容,不会显示在浏览器中-->
注释标签除了在代码中添加注释的功能外，还可以在不支持js的浏览器中隐藏js代码：
<!--more-->

```html
<script type="text/javascript">
    <!--
        alert("支持JavaScript")
    //-->
</script>
```

在不支持js的浏览器中，最后的//-->不会被当成注释，注释标签可以成功闭合，js代码就不会执行；在支持js的浏览器中//后面的内容被解析为js注释，<!-- -->没有闭合，中间的js代码可以执行。
#### 2.html5中新增的结构标签
article、aside、
---
title: vue使用时遇到的小问题及解决
date: 2017-07-20 21:30:15
tags: [Vue]
categories: 技术
---
现在已经是2018年的1月20号了，从2017年7月20号左右开始用vue做东西，差不多半年的时间做了两个项目，一个移动端，一个PC端，第一个项目真的是文档大概浏览一下就直接上手干了，刚开始感觉有点儿艰难，不过熟悉之后觉得vue的开发体验简直太棒了，<!--more-->所以第二个项目也果断选择了vue开发，现在项目第一个阶段已经初步完成马上测试了，感觉选择vue真是太对了，虽然也遇到很多问题，但整体开发过程都很顺畅。从这篇文章也可以看出从最开始的小白一点儿点儿学习进步吧，这些问题在发生的时候都是真真实实的给我造成了一些问题，所以解决后选择记录下来，但现在回过头来看发现其实这些都是很小的一些问题，有粗心造成，有因为对vue-cli一知半解闹的笑话，有在提交代码时跟团队另一个前端代码冲突发生的一些小问题，有使用各种组件时遇见的坑，还有在业务场景中遇见的一些小需求的解决过程，半年里所有的这些都是自己去学习尝试去解决，公司里没有前端这方面的人可以带带我，所以也是自己在琢磨，希望新的一年不仅要知道如何使用vue，更要去学习vue的原理是什么，只要还在用vue，这篇文章就会一直更新下去，加油吧我自己。    ---2018.1.20有感

最近的在使用vue做东西，初次使用遇见很多问题，把这些小问题记录下来，希望给遇到同样问题的同学一个参考，也便于自己以后查阅，持续更新中。
<!--more-->

#### 1.vue-cli构建的项目端口冲突

报错：Error: listen EADDRINUSE :::8080 错误原因是端口冲突，修改当前项目的端口即可，在项目下的config/index.js文件中修改

```javascript
  dev: {
    env: require('./dev.env'),
    port: 8081, //修改端口号
    autoOpenBrowser: true,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {},
    cssSourceMap: false
  }
```

#### 2.解决在vue-cli构建的项目中无法使用stylus的问题

报错：Module build failed: Error: Cannot find module 'stylus'
解决：
##### 2.1 首先在package.json的devDependencies中写入依赖

```javascript
"stylus-loader": "^2.1.2",
"stylus": "^0.52.4",
```

##### 2.2 安装这两个插件

    npm i stylus-loader stylus --save

或者直接在项目文件夹下
    npm install

##### 2.3 重新编译运行

    npm run dev

#### 3.运行后一片空白，chrome控制台报错

报错：Uncaught TypeError: __WEBPACK_IMPORTED_MODULE_2__router__.a is not a constructor
描述：在使用vue-router时出现了这个问题,在网上搜索后发现是版本不同导致的问题，在vue2.x中使用了1.0的vue-router的写法，
在vue1.0中:
    <a v-link="{path:'/goods'}">商品</a>
修改为vue2.0中的写法:
    <router-link to="/goods">商品</router-link>

#### 4.配置默认路径

没有进行配置时，引用组件只能通过下面的方式
    import Goods from '../components/goods/goods';
并且'../'写多了也很烦人，如果不加'../'会报一下错误：
报错：Module not found: Error: Can't resolve 'components/goods/goods' in 'F:\vuetest\vue-project\src\router'
为此我们可以配置默认路径来解决这个问题。
1.找到build文件夹下的webpack.base.conf.js文件
2.在文件中找到这一段代码:

```javascript
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src')
    }
}
```

alias是别名的意思，我们可以为常用的路径起一个别名,如下面这样

```javascript
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
      'components': path.resolve(__dirname, '../src/components') 
    }
},
```

在修改完成后要重新运行，才能生效，现在页面中的`import Goods from 'components/goods/goods';`应该就可以了。

补充：这样配置的路径只在script标签中有效，也就是只能在js中用。

#### 5.使用v-for在获取computed中的值时出现的问题

报错：xxx is not defined on the instance but referenced during render.Make sure to declare reactive data properties in the data option.
这个问题犯得很不应该，错把computed属性写在了props属性中，导致vue找不到computed中定义的计算函数，是个低级错误，不多说了。

#### 6.在vue中实现左右滑动切换

watch路由变化，当路由发生改变时，根据地址中'/'的数量来判断当前路由地址的深浅，以此决定要左滑还是右滑

```javascript
export default {
  name: 'app',
  data(){
    return {
        transitionName :'slide-left', //绑定在组件上面的动效class
    }
  },
  watch: {
    '$route' (to, from){
        const toDepth = to.path.split('/').length
        const fromDepth = from.path.split('/').length
        this.transitionName = toDepth < fromDepth ? 'vux-pop-out' : 'vux-pop-in'
    }
  }
}
```
```css
<style lang="stylus" rel="stylesheet/stylus">
  /* 左右滑动 */
.vux-pop-out-enter-active,
.vux-pop-out-leave-active,
.vux-pop-in-enter-active,
.vux-pop-in-leave-active 
 will-change: transform
 transition: all 350ms
 height: 100%
 top: 0
 position: absolute
 backface-visibility: hidden
 perspective: 1000
.vux-pop-out-enter
 opacity: 0
 transform: translate3d(-100%, 0, 0);
.vux-pop-out-leave-active
 opacity: 0
 transform: translate3d(100%, 0, 0);
 transform-style: preserve-3d;
.vux-pop-in-enter
 opacity: 0
 transform: translate3d(100%, 0, 0);
 transform-style: preserve-3d;
.vux-pop-in-leave-active
 opacity: 0
 transform: translate3d(-100%, 0, 0);
 transform-style: preserve-3d;
</style>
```

#### 7.在keep-alive中只更新某个组件的数据

我有一个列表，点击列表的项可以打开详情页，在我没有使用keep-alive时，一切正常，我点击某一项，详情页中显示的是当前项的数据，不过在使用keep-alive进行组件缓存后就出事了，我不管点击哪一项总是会打开第一次缓存的详情组件，数据也不会改变，我是通过vue-router传参到详情页面的:

```javascript
taskDetail: function(item){
  this.$router.push({path:'/task/detail',query:{item:item}});
}
```

查看官方文档，发现下面这段话：
 >在 2.2.0 及其更高版本中，activated 和 deactivated 将会在 <keep-alive> 树内的所有嵌套组件中触发。

那么我只要每次在activated中做数据更新就行了，如下：

```javascript
activated(){
  this.item = this.$route.query.item;
}
```

#### 8.使用better-scroll时列表不滚动的问题

有必要先了解一下better-scroll的原理,下面的链接中讲的非常清楚：
[better-scroll](https://juejin.im/post/59300b2e2f301e006bcdd91c)
也就是说滚动列表的父元素要有一个固定的高度，只有这样滚动列表的高度超过父元素的高度是列表才会滚动，不超过时就不滚动，总之在better-scroll的引入和写法没错的情况下还不能滚动，那大多数原因就是css的样式没写对，导致不符合better-scroll滚动的条件，那来看一个非常简单的例子吧,主要是css部分：

```javascript
<template>
  <div class="parent">
    <div class="child_div" ref="wrapper">
      <ul>
        <li>1</li>
        <li>2</li>
        ...
        <li>299</li>
        <li>300</li>
      </ul>
    </div>
  </div>
</template>
<script type="text/ecmascritp-6">
  import BScroll from 'better-scroll'
  export default {
    created () {
      this.$nextTick(function () {
        this.scroll = new BScroll(this.$refs.wrapper, {})
      })
    }
  }
</script>

<style scoped>
.parent{
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}
.child_div{
  position: relative;
  height: 100%;
  overflow: hidden;
}
.child_div li{
  height:20vw;
}
</style>
```

#### 9.better-scroll中警告[Intervention] Unable to preventDefault inside passive event listener due to target being treated as passive

这个警告信息是因为在新版的chrome中监听touch类事件时，无法被动侦听事件preventDefault造成的，在better-scroll中暂时解决的方法是在初始化better-scroll时设置preventDefault: false

```javascript
this.scroll = new BScroll(this.$refs.wrapper, {
  preventDefault: false,  //暂时解决chrome中无法被动监听preventdefault
});
```

#### 10.只在一级菜单显示底部导航

在网上搜索之后发现两种方法可以解决，先说第一种：
在router文件夹下的index.js中将需要显示底部导航的路由中加入meta属性,在meta属性中添加一个标志属性，用来标志是否显示底部导航，这里我们用navShow来做为标志属性，如下所示：

```javascript
{
  path: '/home',
  component: home, /*主页组件*/
  meta: {
    title: '首页',
    navShow: true,/*是否一级页面*/
  },
}
```

不需要显示底部导航的设置为下面这样：

```javascript
{
  path: '/login',
    meta: {
      title: '登陆',
      navShow: false,/*是否一级页面*/
    },
    component: login /*登录组件*/
}
```

然后，在APP.vue中，底部导航的组件上加上判断,就完成了

```javascript
<vTab v-show="$route.meta.navShow"></vTab>
```

第二种方法比较笨，在你不希望底部导航显示的组件中，直接设置css样式，让它盖在底部导航上，用户就看不到了，比如可以设置z-index属性为一个较大值。

#### 11.本地跨域

由于刚开始工作，没有什么经验，又是刚开始接触vue，虽然网上有很多文章介绍vue本地跨域，但是还是一知半解，设置了也没有成功，在纠结了好大一通后终于成了，发现哦原来这么简单，但是不明白时却觉得很难，

#### 12.去掉路由中的#号

路由中总是会带有一个#号，如http://192.168.0.144:8098/#/index
看起来很别扭， 要去除这个#号，可以使用路由的history模式，在router/index.js中的new VueRouter方法中添加：

```javascript
var Routes = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

这样路由就会变成http://192.168.0.144:8098/index

#### 13.iview中的upload组件上传图片到七牛云
项目中用到iview中的upload组件将图片上传到七牛云，比较简单，主要是uptoken在测试阶段后台没有弄好，一时不知道在哪儿搞，后来发现可以在线生成uptoken，[在线地址](http://pchou.qiniudn.com/qiniutool/uptoken.html)，下面写一下示例，具体的组件
```html
<Upload :action="url">
  <div>
    <Icon type="camera" size="30"></Icon></div>
</Upload>
```
```javascript
data:function(){
  return{
    domain: config.domain, //如下图中被马赛克掉的内容
    url: config.qiniuUrl,  //http://up-z2.qiniu.com/
    uptoken: '',           //生成或者从后台拿到uptoken
    form: {
      token: '',
      key: null
    },
  }
}
```
[domain](http://olmhb4zos.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170912174726.png)


#### 14.vue中图片路径问题
以下两种情况会导致找不到图片路径的问题：
    1. ``` <img :src="./img/a.jpg" />" ```
    2. ``` <div :style="{ backgroundImage: 'url(./img/a.jpg)' }"></div> ```
可以使用下面的两种方法解决：
```javascript
data () {
    return {
        img: require('path/to/your/source')
    }
}
在标签里使用：
<img :src="img" />
````
如果是背景：
```javascript
data () {
    return {
        img: require('path/to/your/source')
    }
}

<div :style="{backgroundImage: 'url(' + img + ')'}"></div>
```
#### 15.图片加载失败时显示默认图片
在使用img标签时，当图片加载失败的时候显示默认图片，这样的用户体验比较友好。
方法是绑定img标签的onerror，在图片加载失败时就会自动加载errorImg01的图片
```javascript
<div class="bg">
  <img :src="goods.phoneFloorAd.resUrl" :onerror="errorImg01">
</div>

<script type="text/ecmascript-6">
  export default {
    data () {
      return {
        errorImg01: ‘this.src="‘ + require(‘assets/images/load_logo01.png‘) + ‘"‘
  　　};
  　}
  }
</script>
```
第二种情况是设置的背景图片加载失败时显示默认图片，方法是可以利用计算属性获取背景图片，图片获取成功时正常显示，获取失败时显示默认图片，代码如下：

#### 16. vue-cli打包时报错
错误代码：vuex requires a Promise polyfill in this browser.
这个错误的原因是使用了es6的promise，但是ie版本(使用ie11)的浏览器不支持，解决的方法是安装 babel-polyfill ， babel-polyfill 可以模拟es6的使用环境
##### 1.安装 babel-polyfill 
```
npm install  babel-polyfill
```
##### 2.修改webpack.base.config.js
```javascript
module.exports = {
    entry: {
        main: ["babel-polyfill", "./src/main"], //修改后
        // main: './src/main',  //原来
        vendors: './src/vendors'
    }
}
```
>参考[https://www.cnblogs.com/weiqinl/p/6794612.html](https://www.cnblogs.com/weiqinl/p/6794612.html)

#### 17. vue-cli打包时报错
运行npm run dev时没有问题，但运行npm run build时报错
```javascript
Error: "extract-text-webpack-plugin" loader is used without the corresponding plugin
```
是在我把vue-cli升级到@2.9.2之后出现的问题，没有找到原因，删除又安装了@2.8.2后可以打包了

#### 18.vscode格式化vue中template代码
###### 1.安装 vetur
###### 2.在User Setting中增加设置:
```"vetur.format.defaultFormatter.html": "js-beautify-html"```
###### 3.格式化快捷键：Alt+Shift+F

#### 19.vue中通过a标签下载文件
想不通过后台直接下载一个excel文件，我用了
```<a href="/file/filename" download="filename">```
但是提示我找不到文件，想到img标签的src属性，是要把文件通过require引入，再通过url-loader解析一下，才能正常显示图片，所以想这个href属性是不是同样的套路，那excel文件用什么解析呢，搜索之后发现有一个file-loader，所以在webpack的配置文件里加上了下面的几行配置
```javascript
{
  test: /\.(xls)$/,
  loader: 'file-loader',
}
```
在代码中只需要把这个文件require进来，在href中写上这个变量就可以了，跟img一样
```html
<a :href="fileHref" download="fileName"></a>
```
```javascript
data:function(){
  return{
    fileHref: require("/file/filename")
  }
}
```

待续吧...
---
title: Markdown语法学习
date: 2016-12-31 15:08:53
categories: 博客搭建
tags: [Markdown,hexo]
---
### 基础语法
#### 1 标题 
##### 示例
```
一级标题
===
二级标题
---

# 一级标题
## 二级标题
### 三级标题
...
```
<!--more-->
#### 2 列表
##### 无序列表示例
```
<!-- 无序列表,这三种写法效果相同 -->
* 红
* 绿
* 蓝

+ 红
+ 绿
+ 蓝

- 红
- 绿
- 蓝
```

##### 效果
* 红
* 绿
* 蓝

+ 红
+ 绿
+ 蓝

- 红
- 绿
- 蓝

##### 有序列表示例

```
<!-- 有序列表 -->
1. 红
2. 绿
3. 蓝

<!-- 列表项目包含段落 -->
1. This is a list item with two paragraphs. Lorem ipsum dolor
   sit amet, consectetuer adipiscing elit. Aliquam hendrerit
   mi posuere lectus.
2. This is a list item with two paragraphs. Lorem ipsum dolor
   sit amet, consectetuer adipiscing elit. Aliquam hendrerit
   mi posuere lectus.
```
##### 效果
1. 红
2. 绿
3. 蓝


1. This is a list item with two paragraphs. Lorem ipsum dolor
   sit amet, consectetuer adipiscing elit. Aliquam hendrerit
   mi posuere lectus.
2. This is a list item with two paragraphs. Lorem ipsum dolor
   sit amet, consectetuer adipiscing elit. Aliquam hendrerit
   mi posuere lectus.

#### 3 代码 
##### 3.1 代码行
在Markdown中建立代码区块非常简单，只需要一个制表符或者四个空格,显示时制表符和空格会自动删除。
###### 示例
    `    使用四个空格生成代码行`
###### 效果
    使用四个空格生成代码行
##### 3.2 代码块
我们可以使用一个\`包裹行内代码块，使用三个\`包裹多行代码块。
###### 行内代码块示例
```
`使用一个反引号生成行内代码块`
```
###### 效果
`使用一个反引号生成行内代码块`

###### 多行代码块示例
\`\`\`
function test(){
    alert("test");
}
\`\`\`

###### 效果

```
function test(){
    alert("test");
}
```
#### 4 引用
如果需要引用别处的句子可以用>符号
##### 示例
```
> 春眠不觉晓
> 处处闻啼鸟
>> 夜来风雨声
>> 花落知多少
```
##### 效果

> 春眠不觉晓
> 处处闻啼鸟
>> 夜来风雨声
>> 花落知多少

#### 5 链接
markdow有两种形式的链接：行内式和参考式
##### 行内式
`[要显示的链接名](连接地址 "鼠标移上去显示的title")`;

###### 示例
```
这是一个[百度](http://www.baidu.com "百度")行内链接
```
###### 效果
这是一个[百度](http://www.baidu.com "百度")行内链
##### 同一主机
如果要链接到同一主机的资源，你可以使用相对路径
###### 示例
```
链接到主机[关于我](/aubout/)使用相对路径
```
###### 效果
链接到主机[关于我](/aubout/)使用相对路径
##### 参考式
使用 Markdown 的参考式链接，可以让文件更像是浏览器最后产生的结果，让你可以把一些标记相关的元数据移到段落文字之外，你就可以增加链接而不让文章的阅读感觉被打断.
###### 示例
```
<!--这是一个[百度][id]参考式链接-->
[id]: http://www.baidu.com
```
###### 效果

这是一个[百度][id]参考式链接
[id]: http://www.baidu.com

##### 自动链接
用<>括起来，就会自动转换成链接。
###### 示例
```
<http://www.baidu.com/>
```
###### 效果
<http://www.baidu.com/>
#### 6 强调
Markdown 使用星号（*）和底线（_）作为标记强调字词的符号,被 * 或 _ 包围的字词会被转成用 <em> 标签包围,用两个 *或_包起来的话，则会被转成 <strong>标签。
##### 示例
```
<!-- 斜体 -->
*我会被转成em标签* 
_me too_

<!-- 粗体 -->
**我会被转成strong标签**
__me to__
```
##### 效果
*我会被转成em标签*
_me too_

**我会被转成strong标签**
__me to__

注：如果要插入普通的星号和下划线可以使用反斜线\* 和反斜线 \_,Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

| 符号 | 代码 | 效果 |
|:---|:---:|---:|
| \\ | `\\` | \\ |
| \` | `\`\` | \` |
| \* | `\*` | \* |
| \_ | `\_` | \_ |
| \{ | `\{` | \{ |
| \[ | `\[` | \[ |
| \( | `\(` | \( |
| \# | `\#` | \# |
| \+ | `\+` | \+ |
| \- | `\-` | \- |
| \. | `\.` | \. |
| \! | `\!` | \! |

##### 特殊字符转换
```
<!-- 普通星号和反斜线 --> 
\*
\_
```
#### 7 分割线 
##### 示例 
```
***
* * * 
- - -
```
##### 效果 
***
* * * 
- - -
#### 8 图片 
Markdown 使用和链接很相似的语法来标记图片，同样也允许两种样式：行内式和参考式。
##### 行内式的图片示例
```
![头像](/images/avatar.gif)
![头像](/images/avatar.gif "Optional title")
```
##### 效果
![头像](/images/avatar.gif)
![头像](/images/avatar.gif "Optional title")
详细叙述如下：
一个叹号 ! 接着一个方括号[]，里面放上图片的替代文字(alt)
接着一个普通括号()，里面放上图片的网址，最后还可以用引号包住并加上选择性的 'title' 文字。

<!--参考式的图片语法-->
```
![name][id]
「id」是图片参考的名称，图片参考的定义方式则和连结参考一样：
```

##### 参考式图片示例
```
![头像][id]
[id]: /images/avatar.gif  "Optional title attribute"
```
##### 效果
![头像][id]
[id]: /images/avatar.gif  "Optional title attribute"
到目前为止， Markdown 还没有办法指定图片的宽高，如果你需要的话，你可以使用普通的 <img> 标签。

#### 踩坑
1.标题后面必须加一个空格。
2.必须按照标题大小的顺序写才能正确的生成目录，如果大标题用`#`,小标题直接用`###`目录会混乱，用`#`后接下来应该用`##`才会准确的生成文章目录。
3.生成表格前要加一个空行



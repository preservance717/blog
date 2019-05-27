---
title: "Sticky Footer的实现"
date: 2019-05-27T22:45:43+08:00
categories:
- category
- subcategory
tags:
- CSS
keywords:
- sticky Footer
comments:       false
showMeta:       false
showActions:    false
---

<!--more-->
### 需求
实现footer永远在浏览器窗口的底部。当页面内容超过一屏时，在内容的最底端；当页面内容少于一屏时，footer在窗口的底端，见下图。
![demand.png](https://upload-images.jianshu.io/upload_images/3126944-905c0572abe72a7b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 实现方式
> 1. header 和 footer高度固定
> 2. flex布局

html代码
```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>sticky footer</title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <div class="header">
      header
    </div>
    <div class="content">
      content
    </div>
    <div class="footer">
      footer
    </div>
  </body>
</html>
```
####1.固定高度
css代码
```python
.header{
  background: #6f0096;
  height: 60px;
}

.content{
  background: #e8e8e8;
  min-height: calc(100vh - 60px - 60px);
}

.footer{
  background: #000000;
  color:#ffffff;
  height: 60px;
}
```
其中，已知`header`和`footer`的高度，则要使footer在窗口的低端，只需`content`的`min-height`值为`100vh - 60px - 60px`.
####2.flex布局
css代码
```css
body{
  display: flex;
  flex-direction: column;
  height: 100vh;
  text-align: center;
  margin: 0px;
}
.header{
  background: #6f0096;
}

.content{
  background: #e8e8e8;
  flex:1;
}

.footer{
  background: #000000;
  color:#ffffff;
}
```
其中，`body`分为三部分：`header`、`content`和`footer`。`body`的`height`值为100vh，`display`值为`flex`，且以列的方式展示。其次是将`content`中的`flex`属性值为1。这样就实现了footer置底。
在这里有两个知识点的理解：
> * `height:100vh`：其中vh是viewport height视窗的高度。1vh是视窗的1%，则100vh就是视窗的100%。（其中视窗是浏览器内部的可见视区，不包括任务栏标题栏以及底部工具栏的浏览器区域大小）。
>* `flex: 1`：`flex`属性是个复合属性，由`flex-basis`、`flex-grow`和`flex-shrink`三个属性确定，其语法：```flex: none | auto | initial | [  <flex-grow>   <flex-shrink> ? ||  <flex-basis> ]```。
详细见下表（暂不讨论兼容性问题）。若 `flex: 1`，则其计算值为[1,1,0%]，即伸缩比例都是1。`flex-grow`值为`1`，存在多余空间时项目放大；`flex-shrink`值为`1`，空间不足时项目缩小。

属性 | 可选值 | 默认值|含义|备注
|:----:|:------:|:----:|:----:|:----:|
flex-basis|length、percentage、auto|auto|弹性条目的初始主轴尺寸|在`flex`属性中该值如果被省略则默认为`0%`；在`flex`属性中该值如果被指定为`auto`，则伸缩基准值的计算值是自身的 `width`设置;如果自身的宽度没有定义，则长度取决于内容。
|flex-grow|integer|0|当容器有多余的空间时，这些空间在不同条目之间的分配比例|在`flex`属性中该值如果被省略则默认为`0`，不能取负值。
|flex-shrink|integer|1|当容器的空间不足时，各个条目尺寸缩小的比例|在`flex`属性中该值如果被省略则默认为`1`，不能取负值。
#### 总结：
相比较于这两种方式，我比较倾向于使用flexbox方式实现。因为第一种方式需要知道头部和footer的固定高度，然而有时项目中头部和footer的高度并非是固定的。
###相关资料
___
* [阮一峰Flex 布局教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

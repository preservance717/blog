---
title: "JavaScript的for循环闭包"
date: 2019-05-27T22:22:11+08:00
categories:
- category
- subcategory
tags:
- JavaScript
keywords:
- tech
comments:       false
showMeta:       false
showActions:    false
---

<!--more-->

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>
    <ul>
        <li>第一个</li>
        <li>第二个</li>
        <li>第三个</li>
        <li>第四个</li>
    </ul>
</body>
<script>
    var nodeList = document.getElementsByTagName('li');

    for (var i = 0; i < nodeList.length; i++) {
        nodeList[i].onclick = function() {
            console.log(i);
        }
    }
</script>

</html>
```
![result.png](http://upload-images.jianshu.io/upload_images/3126944-c085ce5841714423.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
此时当点击任意li时，控制台输出的都是4。`onclick`是个异步事件。而循环本身执行快的多，等到循化结束后i值为4，此时不管点击哪一个输出都是4。

将`for`循环中的`var`改为`let`时，则正常输出。`let`是严格模式定义变量，每次循环的i的引用是不同的，能正确的把每次循环里面正确的i值传递到`onclick`里面，按`for`表达式`let i = 0; i < nodeList.length; i++`的定义，哪个标签被点击，就输出对应的i值。

```js
 for (var i = 0; i < nodeList.length; i++) {
     nodeList[i].num = i;
     nodeList[i].onclick = function() {
         console.log(this.num);
     }
 }
```
li增加一个新的属性是`num`，此时的num并没有在onclick事件函数中，所以num属性直接被赋值为0，1，2，3，当事件函数值执行的时候，输出的是对应的li的num的值，而这个时候的num已经在上一步的时候被执行了。

```js
for (var i = 0; i < nodeList.length; i++) {
    (function(i) {
        nodeList[i].onclick = function() {
            console.log(i);
        }
    })(i)
}
```
使用立即执行函数解决该问题。此时`i`输出的结果为0，1，2，3。
此外也可以使用增加属性的方法使得i的值理想输出。

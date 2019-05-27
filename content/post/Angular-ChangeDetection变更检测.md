---
title: "Angular ChangeDetection变更检测"
date: 2019-05-27T22:48:38+08:00
categories:
- category
- subcategory
tags:
- Angular
- 变更检测
keywords:
- ChangeDetection
comments:       false
showMeta:       false
showActions:    false
---

<!--more-->
最近在做项目的过程中遇到一个问题：在回调函数里，数据发生变化时，视图并没有相应地更新。于是就在网上搜索解决方案，说是将`Component`中的`changeDetection`值修改为`ChangeDetectionStrategy.OnPush`，再在`constructor`里导入`ChangeDetectorRef`，最后在数据模型发生变化的地方添加**变更检测的代码**。

```js
@Component({
    ...
    changeDetection: ChangeDetectionStrategy.OnPush
})

export class DemoComponent {
    construtor(private cdf: ChangeDetectorRef) {}

    changeData() {
        ... // 回调函数
        this.data = {a: 1}; //变化的数据
        this.cdf.markForCheck(); // 进行标注
        this.cdf.detectChanges(); // 要多加一行这个 执行一次变化检测
    }
}
```
这样一来，修改后的数据更新在视图上，问题得到解决。
但是`changeDetection`是什么？ 为什么加上`changeDetection`相关内容后，数据就会更新在视图上？没加之前，为什么有的数据更新了，有的没有更新呢？等等一系列的问题接踵而至。
问题提出来了，现在要做的就是去解决问题。
### What
changeDetection是什么呢？它是Angular的一个特性。当组件中的数据发生变化时，Angular自动检测到数据**变化**并**更新**相应的视图。
一般情况下，将数据的状态以按钮、表单、链接或者图片形式呈现在视图上，也就是DOM。所以，我们将数据结构作为输入并生成DOM显示给用户这一过程称之为**渲染**。
### Why
上文中提到数据变化，那数据为什么会变化呢？
一般情况下，在进行异步的操作之后，数据就会发生变化。一般有三种情况：

> * **Events**: click、submit、onmouseup...
> * **XHR请求**: 从服务器获取数据
> * **Timers**: setTimeout()、setInterval()

这些都是异步的，所以，有异步操作执行，数据就会发生变化，也是在这个时候**通知**Angular更新视图。
### Who
刚才说通知Angular更新视图，那谁来通知Angular呢？**NgZone**来通知Angular更新视图。
### How
以上，我们了解到ChangeDeteciton是如何触发的，但是怎么运行的呢？目前，我们所需要注意到的是：**每个组件都有它自己的变更检测器**。
设想一个场景：
> 在一个组件中，一个按钮被点击，接下来会发生什么呢，当有数据发生变化时，Ngzone会做出处理并且通知Angular，最终Angular会执行变更检测。

正如我们所知的，每个组件有它自己的变更检测器，一个Angular应用包含一个组件树，因此推断出一个angular应用也有一个**变更检测树**。这个树可以视为有向图，其中，数据始终从顶部流向底部。
![cd-tree(图片来源于网络).png](https://upload-images.jianshu.io/upload_images/3126944-4eedd458236b2293.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

数据从顶部流向底部的原因时因为每个单独的组件，从根组件开始，每个组件也始终从上到下执行变更检测。这样的话，相比循环数据流，单向数据流更容易预测。我们总是知道数据来自何处，因为它只能来自其组件。

### 相关文档
---
* [ANGULAR CHANGE DETECTION EXPLAINED](https://blog.thoughtram.io/angular/2016/02/22/angular-2-change-detection-explained.html)

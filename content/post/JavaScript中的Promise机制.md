---
title: "JavaScript中的Promise机制"
date: 2019-05-27T21:57:29+08:00
categories:
- category
- subcategory
tags:
- JavaScript
keywords:
- Promise
comments:       false
showMeta:       false
showActions:    false
---

<!--more-->

## Promise是什么
Promise是异步处理对象以及对其进行各种操作的组件。一个Promise对象代表一个目前不可用，但是在未来的某个时间点可以被解析值。允许以同步的方式编写异步代码。
## Promise出现的原因
在promise之前，在JS中的异步编程都是采用回调函数和事件的方式，但是这种编程方式在处理复杂的业务情况下，很容易出现回调多层嵌套，使得代码很难理解和被维护。
Promise改善了这种情形下的异步编程的解决方案，它是由社区提出和实现的。ES6将其写进了语言标准，统一了用法，并且提供了一个原生的对象Promise
## Promise的API
![promise.png](http://upload-images.jianshu.io/upload_images/3126944-5ff96b9cb528f310.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
从这个简单的例子可以看出我们需要基本掌握的是

* Promise的构造函数
* resolve()、reject()
* then()

### a、promise的构造函数

![demo.png](http://upload-images.jianshu.io/upload_images/3126944-111469c9dab93f30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行结果如下：
![reslult.png](http://upload-images.jianshu.io/upload_images/3126944-fa877f4052e2af88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到先输出了`promise`，再输出了`end`。
当通过Promise构造函数实例化一个对象时，会传递一个函数作为参数，而且这个函数在新建一个Promise后，会立即执行。
### b、resolve/reject
在Promise中，Promise操作有3中状态，但是其只存在于三种状态的一种。其关系如下：

![关系图.png](http://upload-images.jianshu.io/upload_images/3126944-7fba8fa43f5a16e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注意：这种状态的改变只能从未完状态到完成态或失败态转变，不能逆反。完成态和失败态不能相互转化，而且，状态一旦转化则不能修改。
只有异步操作的结果，才能决定当前状态是哪一种状态，任何其他操作无法改变这一状态。
通常，我们在声明一个Promise对象的实例时，在我们传入的匿名参数中：

* resolve代表完成态后的操作
* reject代表失败态后的操作

### c、then
了解到上面那些之后，我们也许会问`then`方法的作用是什么呢，`resolve`和`reject`又是从哪里传递过来的。
其实，我们在实例化Promise对象时，调用该对象的实例方法then，其中then的第一个参数对应着完成状态的操作，也就是resolve，第二个参数代表着失败态的操作，是reject。
总的来说，Promise通过 `then`方法来指定处理异步操作结果的方法。
## 未完待续。。。

---
title: "JavaScript中的this"
date: 2019-05-27T22:27:02+08:00
categories:
- category
- subcategory
tags:
- JavaScript
keywords:
- this
comments:       false
showMeta:       false
showActions:    false
---

JavaScript中的this的指向在函数定义时，是不确定的。只有在函数运行的时候才能确定this指向谁。this可以是全局对象，当前对象或者任意对象。取决于函数的调用方式。JavaScript中函数调用的方式如下：作为对象方法调用，作为函数调用，作为构造函数调用和使用apply，bind调用。
<!--more-->

### 全局中的this
> * 浏览器中的this

```js
console.log(this);//window对象
```
> * node中的this
```js
console.log(this);//{}
```

### 作为对象方法调用
当一个函数作为一个对象的属性时，此时这个函数称为对象的方法，当方法被调用时，this指向该对象。
```js
var person = {
      name: 'susan',
      sex: 'woman',
      say: function () {
               console.log(this.name); //susan
      }
};

person.say();
```
但是当per.say该方法赋值给一个变量时，this发生变化了,this指向window对象
```js
var person = {
    name: 'susan',
    sex: 'woman',
    say: function () {
          console.log(this.name); //undefined
          console.log(this);//global对象
      }
};

var say = person.say;
say();
```
### 作为函数调用
在作为函数调用时，this在node中输出为global对象（非严格模式），在浏览器中输出为window对象。在严格模式中this输出为undefined。
```js
function say() {
    console.log(this);//global对象（node）或者 window对象（浏览器）
}

say();
```
### 作为构造函数调用
作为构造函数调用时，this指向了构造函数调用时实例化出的对象。
```js
function Say(name) {
       this.name = name;
      console.log(this.name);//susan
      console.log(this);//this指向Say{name:'susan'}对象
}

var say = new Say('susan');
```
### call调用和apply调用
当call调用和apply调用时，更改了this的指向。使this指向了person对象。
```js
var person = {
    name:'susan'
};
function say() {
     console.log(this.name);
}

say.apply(person);
say.call(person);
```
### call调用和bind调用
```js
function Say(name) {
    this.name = name;
    console.log(this.name);
}
var person = {
    name:'Tom'
};
var say = new Say.call(person);
say();
```
TypeError: Say.call is not a constructor，因为new了Say.call函数而不是Say。
```js
function Say(name) {
    this.name = name;
    console.log(this.name);
}
var person = {
    name:'Tom'
};
var say1 = Say.bind(person);
var say2 = new say1('susan');
```
当使用了bind时，this.name仍是susan，由此可以看出bind绑定并没有改变this的指向。
### 箭头函数指定 this
```js
var person = (name)=> {
    console.log(name);//susan
    console.log(this); //{}（node）或者 window对象（浏览器）
};
person('susan');

var person2 = {
    name: 'Tom'
};
person.call(person2, 'Tom');
```
上文中的call调用并没有改变this的指向。从此可以得出箭头函数中的this在定义的时候，已经决定了他的指向，与在哪里调用及如何调用它无关。包括（call，apply，bind）等操作都无法改变this的指向。

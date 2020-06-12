---
title: this、call、apply 和 bind
date: 2020-03-20 21:50:59
tags:
categories:
- 前端
- 设计模式
---

# this、call、apply 和 bind

## 1、this

**和别的语言大相径庭的是，Javascript的this总是指向一个对象，而具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，而非函数声明时的环境。**

### 1.1、this的指向

除去不常用的 with 和 eval 的情况，具体到实际应用中，this 的指向大致可以分为以下 4 种：

> - 作为对象的方法调用
> - 作为普通函数调用
> - 构造器调用
> - Function.prototype.call 或 Function.prototype.apply 调用

下面分别进行介绍

#### 1.作为对象的方法调用
当函数作为对象的方法被调用时， this 指向对象：

```javascript
var obj = {
    a: 1,
    getA: function () {
        console.log( this === obj ) // true
        console.log( this.a )  // 1
    }
}
obj.getA()
```

#### 2.作为普通函数调用
当函数不作为对象的属性被调用时，也就是我们常说的普通函数方式，此时的 this 总是指向全局对象。在浏览器的 Javascript 里， 这个全局对象是 `window` 对象。

```javascript
window.name = 'globalName'

var getName = function () {
    return this.name
}

console.log( getName() ) // globalName

// 或者
var myObject = {
    name: 'sven',
    getName: function () {
        return this.name
    }
}

var getName = myObject.getName
console.log( getName() ) // globalName
```

#### 3.构造器调用
Javascript 中没有类，但是可以从构造器中创建对象，同时也提供了 new 运算符，使得构造器看起来更像一个类。
大部分 Javascript 函数都可以当作构造器使用。构造器的外表跟普通函数一模一样，它们的区别在于被调用的方式。当用 new 运算符调用函数时，该函数总会返回一个对象，通常情况下，构造器里的this就指向返回的这个对象，见代码如下：

```javascript
var MyClass = function () {
    this.name = 'sven'
}

var obj = new MyClass()
console.log(obj.name) // sven
// 但用 new 调用构造器时，还要注意一个问题，如果构造器显示地返回了一个 object 类型的对象，那么此次运算结果最终会返回这个对象，而不是我们之前期待的 this：

var MyClass2 = function () {
    this.name = 'sven'
    return {
        name: 'anne'
    }
}

var obj2 = new MyClass2()
console.log( obj2 ) // anne

// 如果构造器不显示地返回任何数据，或者返回一个非对象类型的数据，就不会造成上述问题

var MyClass3 = function () {
    this.name = 'sven'
    return 'anne'
}

var obj3 = new MyClass()
console.log( obj3.name ) // sven
```

#### 4.Function.prototype.call或者Function.prototype.apply调用
跟普通的函数调用相比，用 Function.prototype.call 和 Function.prototype.apply 可以动态地改变传人函数的 this：

```javascript
var obj1 = {
    name: 'sven',
    getName: function () {
        return this.name
    }
}

var obj2 = {
    name: 'anne'
}
console.log( obj1.getName() ) // sven
console.log( obj1.getName.call(obj2) ) // anne
```

### 1.2、丢失的this

```javascript
var obj = {
    myName: 'sven',
    getName: function () {
        return this.myName
    }
}

console.log( obj.getName() ) // sven

var getName2 = obj.getName // 此时是作为普通函数调用，this 指向 window
console.log( getName2() ) // undefined
```

## 2、call、apply和 bind

### 2.1、 call、apply 和 bind的区别

`Function.prototype.bind` 方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

`Function.prototype.call`和`Function.prototype.apply`都是非常常用的方法。它们的作用一模一样，区别仅在于传入参数形式的不同。

apply 接受两个参数，第一个参数指定了函数体内 this 对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，apply 方法把这个集合中的元素作为参数传给被调用的函数：

```javascript
var func = function (a, b, c) {
    return a + b + c // 输出 6
}

func.apply(null, [1, 2, 3])
```

call 传入的参数数量不固定，跟 apply 相同的是，第一个参数也是代表函数体内的 this 指向，从第二个参数开始往后，每个参数被依次传入函数：

```javascript
var func = function (a, b, c) {
    return a + b + c // 输出 6
}
func.call(null, 1, 2, 3)
```

有时候我们使用 call 或者 apply 的目的不在于指定 this 指向，而是另有用途， 比如借用其他对象的方法。那么我们可以传入 null 来代替某个具体的对象：

`Math.max.apply(null, [1, 2, 3, 7, 9])`

### 2.2、 call、apply 和 bind 实现原理

#### 1、Function.prototype.call

模拟`Function.prototype.call`

```javascript
Function.prototype.call = function (content, ...args) {
  if (!content) {
    content = typeof window === 'undefined' ? global : window
  }

  // this 的指向是当前调用call的函数 func (func.call)
  content.func = this;
  // call 参数给调用函数
  var result = content.func(...args);
  // content 上并没有 func 属性，需要移除
  delete content.func;
  return result;
}

var obj = {
    name: 'sven'
}

function getName () {
  return this.name;
}

getName.call(obj);

```

#### 2、Function.prototype.apply

模拟`Function.prototype.apply`

```javascript
Function.prototype.apply = function (content, args) {
  if (!content) {
    content = typeof window === 'undefined' ? global : window
  }

  // this 的指向是当前调用call的函数 func (func.call)
  content.func = this;
  var result;
  if (!args) {
    // 第二个参数为 null, undefined
    result = content.func();
  } else {
    result = content.func(...args);
  }
  // content 上并没有 func 属性，需要移除
  delete content.func;
  return result;
}

var obj = {
    name: 'sven'
}

function getName () {
  return this.name;
}

getName.apply(obj);

```

#### 3、 Function.prototype.bind

`Function.prototype.bind` ，用来指定函数内部的 this 指向

模拟`Function.prototype.bind` 一：

```javascript
Function.prototype.bind = function (content) {
    var self = this
    return function () {
        self.apply(content, arguments)
    }
}

var obj = {
    name: 'sven'
}

var func = function () {
    console.log(this.name)
}.bind(obj)

func()
```

模拟`Function.prototype.bind` 二，可预选传递参数：

```javascript
Function.prototype.bind = function () {
    var self = this
    var content = [].shift.call(arguments)
    var args = [].slice.call(arguments)
    return function () {
        self.apply(content, [].concat.call(args, [].slice.call(arguments)))
    }
}

var obj = {
    name: 'sven'
}

var func = function () {
    console.log(arguments)
    console.log(this.name)
}.bind(obj, 1, 2)

func()
```

### 2.3、 call、apply 和 bind 的用途

#### 1、改变 this 指向

```javascript
var obj1 = {
    name: 'sven'
}

var obj2 = {
    name: 'anne'
}
window.name = 'window'

var getName = function () {
    console.log(this.name)
}

getName() // window
getName().call(obj1) // sven
getName().call(obj2) // anne
```

#### 2、 借用其他对象的方法

一、借用构造函数,通过这种技术，可以实现一些类似继承的效果

```javascript
var A = function ( name ) {
    this.name = name
}

var B = function () {
    A.apply(this, arguments)
}

B.prototype.getName = function () {
    return this.name
}

var b = new B('sven')
console.log(b.getName())
```




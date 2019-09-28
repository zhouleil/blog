---
title: this绑定规则
date: 2019-09-13 00:06:24
tags:
categories:
- 前端
- 其他
---

##### this绑定规则



##### 1、误解

**this 在任何情况下都不指向函数的词法作用域。**在 JavaScript 内部，作用域确实和对象类似，可见的标识符都是它的属性。但是作用域“对象”无法通过 JavaScript 代码访问，它存在于 JavaScript 内部。

思考下面的代码，它试图（但是没有成功）跨越边境，使用 this 来隐式引用函数的词法作用域：

```javascript
function foo() {
  var a = 2;
  this.bar();
}

function bar() {
  console.log(this.a);
}
foo(); 
```

这段代码非常完美地展示了 this 多么容易误导人。

首页，这段代码试图通过 this.bar() 来引用 bar() 函数。调用 bar() 最自然的方法是省略前面的 this，直接使用词法引用标识符。

此外，这段代码还试图使用 this 联通 foo() 和 bar() 的词法作用域，从而让 bar() 可以访问 foo() 作用域里的变量 a。 你不能使用 this 来引用一个词法作用域内部的东西。

**每当你想把 this 和词法作用域的查找混合使用时，一定要提醒自己， 这是无法实现的。**

##### 2、this 到底是什么

排除一些误解之后，来看看 this 到底是一种什么样的机制。

**this 既不指向函数自身也不指向函数的词法作用域，this 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调用时的各种条件。 this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。**

当一个函数被调用时，会创建一个活动记录（有时候也称为执行上下文）。这个记录会包含函数在哪里被调用（调用栈）、函数的调用方式、传人的参数等信息。 this 就是记录等其中一个属性，会在函数执行的过程中调用。

##### 3、调用位置

在理解 this 的绑定过程之前，首先要理解调用位置： 调用位置就是函数在代码中被调用的位置（而不是声明的位置）。只有仔细分析调用位置才能回答这个问题： 这个 this 到底引用的是什么？

最重要的是分析调用栈（就是为了到达当前执行位置所调用的所有函数）。我们关心的调用位置就在当前执行的函数的前一个调用中。

##### 4、绑定规则

​	1、默认绑定

​	直接使用不带任何修饰的函数引用进行调用的，即独立函数调用。可以把这条规则看作是无法应用其他规则时的默认规则。如果使用严格模式（ strict mode），那么全局对象将无法使用默认绑定，因此 this 会绑定到 undefined。

```javascript
// 默认绑定
function foo() {
  console.log(this.a)
}
var a = 2;
foo(); // 2

// 严格模式
function bar() {
  "use strict";
  console.log(this.b);
}
var b = 2;
bar(); // Cannot read property 'b' of undefined
```

​	2、隐式绑定

​	函数调用位置是否有上下文对象，或者说是否被某个对象拥有或者包含。当函数引用有上下文对象时，隐式绑定规则会把函数调用中的 this 绑定到这个上下文对象。

​	对象属性引用链中只有最顶层或者最后一层会影响调用位置。举例来说：

```javascript
function foo() {
  console.log(this.a)
}
var obj2 = {
  a: 42,
  foo: foo
}
var obj1 = {
  a: 2,
  obj2: obj2
}

obj1.obj2.foo(); // 42
```

**隐式丢失**

一个最常见的 this 绑定问题就是被隐式绑定到函数会丢失绑定对象，也就是说它会应用默认绑定，从而把 this 绑定到全局对象或者 undefined 上，取决于是否是严格模式。举例如下：

```javascript
function foo() {
  console.log(this.a)
}
var obj = {
  a: 2,
  foo: foo
}

var bar = obj.foo; // 函数别名
var a = 'oops, global';
bar(); // oops, global
// 虽然 bar 是 obj.foo 的一个引用，但实际上，它引用的是 foo 函数本身，因此此次的 bar() 其实是一个不带任何修饰的函数调用，因此应用了默认绑定。
function foo1() {
  console.log(this.a1)
}
function doFoo(fn) {
  fn();
}
var obj1 = {
  a1: 3,
  foo: foo1
}
var a1 = 'oops1, global';
doFoo(obj1.foo); // 'oops1, global'
// 参数传递其实就是一种隐式赋值，因此我们传入函数时也会被隐式赋值，所以结果和上一 个例子一样。
```

3、显式绑定

在分析隐式绑定时，我们必须在一个对象内部包含一个指向函数的属性，并通过这个属性间接引用函数，从而把 

this 隐式绑定到这个对象上。

如果我们不想在对象内部包含函数引用，而想在某个对象上强制调用函数，要怎么做呢？可以使用函数的 call(...) 和 apply(...) 方法。

这两个方法是如何工作的呢？它们的第一个参数是一个对象，它们会把这个对象绑定到 this , 接着在调用函数时指定这个 this 。因为你可以直接指定 this 到绑定对象，因此我们称之为显式绑定。

```javascript
function foo() {
  console.log(this.a)
}
var obj = {
  a: 2
}
foo.call(obj); // 2
// 通过 foo.call(...) , 在调用 foo 时，强制把它的 this 绑定到 obj 上。
```

如果你传人了一个原始值（字符串类型、布尔类型或者数字类型） 来当作 this 的绑定对象，这个原始值会被转换成它的对象形式（也就是 new String(...), new Boolean(...) , new Number(...) ） 。 这通常被称为 “装箱”。

**1、硬绑定：** 显示绑定的一个变种

```javascript
function foo() {
  console.log(this.a)
}
var obj = {
  a: 2
}
var bar = function() {
  foo.call(obj);
}
bar(); // 2
setTimeout(bar, 1000); // 2
bar.call(window); // 2
// 无论之后如何调用函数 bar，它 总会手动在 obj 上调用 foo。这种绑定是一种显式的强制绑定，因此我们称之为硬绑定。
```

硬绑定的典型应用场景就是创建一个包裹函数，传入所有的参数并返回接收到的所有值:

```javascript
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
var obj = {
  a: 2
}
var bar = function () {
  return foo.apply(obj, arguments);
}
var b = bar(3); // 2 3
console.log( b ) // 5;
```

另一种使用方法是创建一个 i 可以重复使用的辅助函数:

```javascript
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
function bind(fn , obj) {
  return function() {
    return fn.apply(obj, arguments);
  }
}
var obj = {
  a: 2
}
var bar = bind(foo, obj);
var b = bar(3); // 2 3
console.log( b ); // 5
```

由于硬绑定是一种非常常用的模式，所以在 ES5 中提供了内置的方法 Function.prototype.bind, 它的用法如下：

```javascript
function foo(something) {
  console.log(this.a, something);
  return this.a + something;
}
var obj = {
  a: 2
}
var bar = foo.bind(obj);
var b = bar(3); // 2 3
console.log( b ); // 5
```

**2、API 调用的 “上下文” **

​	第三方库的许多函数，以及 JavaScript 语言和宿主环境中许多新的内置函数，都提供了一个可选的参数，通常被称为 “上下文” （context），其作用和 bind(...) 一样，确保你的回调函数使用指定的 this。举例来说：

```javascript
function foo(el) {
  console.log(el, this.id);
}
var obj = {
  id: 'awesome'
}

[1,2,3].forEach(foo, obj);
// 1 "awesome"
// 2 "awesome"
// 3 "awesome"
```

4、new 绑定

这是第四条也是最后一条 this 的绑定规则。

使用 new 来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。

1、创建（或者说构造）一个全新的对象。

2、这个对象会被执行原型连接。

3、这个对象会绑定到函数调用的 this。

4、如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象。

```javascript
function foo(a) {
  this.a = a;
}
var bar = new foo(2);
console.log(a); // 2
```

**5、总结：判断绑定顺序**

判断this，可以根据优先级来判断函数在某个调用位置应用的是哪条规则。可以按照下面的顺序来进行判断：

​	1、函数是否在 new 中调用（new 绑定）？ 如果是的话 this 绑定的是新创建的对象。

```javascript
var bar = new foo();
```

​	2、函数是否通过 call 、apply （显示绑定）或者硬绑定调用？ 如果是的话，this 绑定的是指定的对象。

```javascript
var bar = foo.call(obj1);
```

​	3、函数是否在某个上下文对象中调用（隐式绑定）？ 如果是的话， this 绑定的是那个上下文对象：

```javascript
var bar = obj1.foo();
```

​	4、如果都不是的话，使用默认绑定。如果在严格模式下， 就绑定到 undefined ， 否则绑定到全局对象。

```javascript
var bar = foo();
```

**6、绑定例外**

​	1、被忽略的 this

```javascript
function foo() {
  console.log(this.a);
}
var a = 2;
foo.call(null); // 2


function foo1(a, b) {
   console.log(this.a , this.b);
}
// apply(...) 展开数组
foo1.apply(null, [2,3]); // 2 3
// 使用 bind(...) 进行柯里化
var bar = foo1.bind(null , 2);
bar(3); // 2 ,3
```

​	2、间接引用

​	你可能（有意或者无意）创建一个函数的 “间接引用” ，在这种情况下，调用这个函数会应用默认绑定规则。

间接引进最容易在赋值时发生：

```javascript
function foo() {
  console.log(this.a)
}

var a = 2;
var o = { a: 3, foo: foo};
var p = { a: 4};
o.foo(); // 3
(p.foo = o.foo)(); // 2
// 赋值表达式 p.foo = o.foo 的返回值是目标函数的引用，因此调用位置是 foo() 而不是 p.foo() 或者 o.foo()

p.bar = o.foo;
p.bar(); // 4
// 这种是在 p 的属性上调用
```


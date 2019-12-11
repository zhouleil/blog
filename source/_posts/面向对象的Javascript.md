---
title: 面向对象的Javascript
date: 2019-12-11 23:18:51
tags:
categories:
- 前端
- 设计模式
---

## 一、鸭子模型

**鸭子模型通俗说法：**如果它走起路来像鸭子，叫起来也是鸭子，那么它就是鸭子。

### 代码模拟这个故事如下

```javascript
var duck = {
    duckSinging: function () {
        console.log('嘎嘎嘎')
    }
}

var chicken = {
    duckSinging: function () {
        console.log('嘎嘎嘎')
    }
}

var choir = [] // 合唱团

var joinChoir = function ( animal ) {
    if (animal && typeof animal.duckSinging === 'function') {
        choir.push(animal);
        console.log('恭喜加入合唱团')
        console.log('合唱团已有成员数量:' + choir.length)
    }
}
joinChoir(duck)
joinChoir(chicken)
```

## 二、多态

**多态：**同一操作作用于不同多对象上面，可以产生不同的解释和不同的执行结果。换句话说，给不同的对象发送同一个消息的时候，这些对象会根据这个消息分别给出不同的反馈。

### 实现如下

```javascript
var makeSound = function (animal) {
    if (animal instanceof Duck) {
        console.log('嘎嘎嘎')
    } else if (animal instanceof Chicken) {
        console.log('咯咯咯')
    }
}
var Duck = function () {}
var Chicken = function () {}

makeSound(new Duck()) // 嘎嘎嘎
makeSound(new Chicken()) // 咯咯咯

/*  
* 这段代码体现了多态性，当我们分别向鸭和鸡发出“叫唤”当消息时，它们根据此消息作出了各自不同当反应。但这样的
* “多态性”是无法令人满意的，如果后来又增加了一只动物，比如狗，显然狗的叫声是“汪汪汪”，此时我们必须得改动
* makeSound 函数，才能让狗也发出叫声。修改代码总是危险的，修改的地方越多，程序出错的可能性就越大，而且当
* 动物的种类越来越多时，makeSound 有可能变成一个巨大的函数。
*/
```

**多态背后的思想是将“做什么”和“谁去做以及怎么去做”分离开来，也就是将“不变的事物”与“可能改变的事物”分离开来。**

```javascript
// 修改后的代码
var makeSound = function (animal) {
    animal.sound()
}

var Duck = function () {}

Duck.prototype.sound = function () {
    console.log('嘎嘎嘎')
}

var Chicken = function () {}

Chicken.prototype.sound = function () {
    console.log('咯咯咯')
}

makeSound(new Duck())
makeSound(new Chicken())

```

假设我们要编写一个地图应用，现在有两家可选的地图API提供商供我们接入自己的应用。目前我们选择的是谷歌地图，谷歌地图的API中提供了 **show** 方法，负责在页面上展示整个地图。
示例代码如下：

```javascript
var googleMap = {
    show: function () {
        console.log('开始渲染谷歌地图')
    }
}

var renderMap = function () {
    googleMap.show()
}
renderMap()

/*  
* 后来，要把谷歌地图换成百度地图，为了让 renderMap 函数保持一定的弹性，我们用一些条件分支来让 renderMap 函* 数保持一定的弹性，我们用一些条件分支来让 renderMap 函数同时支持谷歌地图和百度地图
*/

var baiduMap =  {
    show: function () {
        console.log('开始渲染百度地图')
    }
}
// 重写 renderMap
var renderMap = function (type) {
    if (type === 'google') {
        googleMap.show()
    } else if (type === 'baidu') {
        baiduMap.show()
    }
}
renderMap('google')
renderMap('baidu')

```

可以看到，虽然 renderMap 函数目前保持了一定的弹性，但这种弹性是很脆弱的，一旦需要替换成搜搜地图，那无疑必须得改动 renderMap 函数，继续往里面堆砌条件分支语句。

**我们还是先把程序中相同的部分抽象出来，那就是显示某个地图：**

```javascript
var renderMap = function (map) {
    if (map.show instanceof Function) {
        map.show()
    }
}

renderMap(googleMap) // 开始渲染谷歌地图
renderMap(baidu)    // 开始渲染百度地图

// 这样即使以后增加了搜搜地图， renderMap 函数仍染不需要做任何改变。
var sousouMap = {
    show: function () {
        console.log('开始渲染搜搜地图')
    }
}
renderMap(sousouMap)
```

## 三、封装

**封装：** 封装的目的是将信息隐藏。一般而言，我们讨论的封装是封装数据和封装实现。但更广义的封装，不仅包括封装数据和封装实现，还包括封装类型和封装变化。

### 1.封装数据

在许多语言的对象系统中，封装数据是由语法解析来实现的，这些语言也许提供了 private、public、protected 等关键字来提供不同等访问权限。
但Javascript并没有提供对这些关键字对支持，我们只能依赖变量对作用域来实现封装特性，而且只能模拟出 public 和 private 这两种封装性。

```javascript
// 通过函数作用域来封装
var myObject = (function() {
    var _name = 'sven'
    return {
        getName: function () {
            return _name
        }
    }
})()

console.log(myObject.getName()) // sven
console.log(myObject._name) // undefined
```

### 2.封装实现

从封装实现的细节来讲，封装使得对象内部的变化对其他对象而言是透明的，也就是不可见的。对象对它自己对行为负责。其他对象或者用户都不关心它都内部实现。封装使得对象之间的耦合变松散，对象之间只通过暴露的API接口来通信。当我们修改一个对象时，可以随意修改它当内部实现，只要对外当接口没有变化，就不会影响到程序的其他功能。

例如： 数组的 map、 forEach 等方法

### 3.封装类型

封装类型是静态类型语言中一种重要的封装方式。一般而言，封装类型是通过抽象类和接口来进行的。把对象的真正类型隐藏在抽象类或者接口之后，相比对象的类型，客户更关心对象的行为。
在Javascript中，并没有对抽象类和接口的支持。

### 4.封装变化

通过封装变化的方式，把系统中稳定不变的部分和容易变化的部分隔离开来，在系统的演变过程中，我们只需要替换那些容易变化的部分，如果这些变化是已经封装好的，替换起来也相对容易。这可以最大程度地保证程序的稳定性和可拓展性。

## 四、原型模式和基于原型继承的Javascript对象系统

### 使用克隆的原型模式

从设计模式的角度讲，原型模式是用于创建对象的一种模式，如果我们想要创建一个对象，一种方法是先指定它的类型，然后通过类来创建这个对象。原型模式选择了另外一种方式，我们不再关心对象的具体类型，而是找到一个对象，然后通过克隆来创建一个一模一样的对象。
ECMAScript5 提供了 Object.create 方法，可以用来克隆对象。

```javascript
var Plane = function () {
    this.blood = 100
    this.attackLevel = 1
    this.defenseLevel = 1
}
var plane = new Plane()
plane.blood = 300
plane.attackLevel = 10
plane.defenseLevel = 7

var clonePlane = Object.create(plane)
console.log(clonePlane)
```

在不支持Object.crate方法的浏览器中，则可以使用以下代码：

```javascript
Object.create = Object.create || function (obj) {
    var F = function () {}
    F.prototype = obj
    return new F()
}
```

### 原型编程范型至少包括以下基本原则、

- 所有的数据都是对象
- 要得到一个对象，不是通过实例化类，而是找到一个对象作为原型并克隆它
- 对象会记住它的原型
- 如果对象无法响应某个请求，它会把这个请求委托给它自己的原型

#### 1、所有的数据都是对象

**事实上，Javascript中的根对象是 Object.prototype 对象。**

#### 2、要得到一个对象，不是通过实例化类，而是找到一个对象作为原型并克隆它

```javascript
function Person () {
    this.name = name
}
Person.prototype.getName = function () {
    return this.name
}
var a = new Person('sven')
a.name // sven
a.getName() // sven
Object.getPrototypeOf(a) === Person.prototype // true
```

在这里`Person`并不是类，而是函数构造器，Javascript 的函数既可以作为普通函数调用，也可以作为构造器被调用。当使用 `new` 运算符来调用函数时，此时的函数就是一个构造器。用 `new` 运算符来创建对象的过程，实际上也只是先克隆 `Object.prototype` 对象，再进行一些其他的额外操作的过程。
调用构造函数实际上会经历以下四个步骤：
（1）创建一个新对象；
（2）将构造函数的作用域赋给新对象（因此`this`就指向了新对象）；
（3）执行构造函数中的代码（为这个新对象添加属性）；
（4）返回新对象。

```javascript
function Person (name) {
    this.name = name
}

Person.prototype.getName = function () {
    return this.name
}

var objectFactory = function () {
    var obj = new Object(), // 从 Object.prototype 上克隆一个空的对象
    Constructor = [].shift.call(arguments); // 取得外部传人的构造器，此例是 Person

    obj.__proto__ = Constructor.prototype; // 指向正确的原型，而不是 Object.prototype

    var ret = Constructor.apply(obj, arguments); // 借用外部传人的构造器给 obj 设置属性

    return typeof ret === 'object' ? ret : obj; // 确保构造器总是返回一个对象
}

var a = objectFactory(Person, 'sven');

console.log(a.name); // sven
console.log(a.getName()); // sven
console.log(Object.getPrototypeOf(a) === Person.prototype)

```

#### 3、对象会记住它的原型

如果请求可以在一个链条中依次往后传递，那么每个节点都必须知道它的下一个节点。同理，Javascript语言中的原型链查找机制，每个对象至少应该记住它自己的原型。
对于“对象把请求委托给它自己的原型”这句话，更好的说法是对象把请求委托给它的构造器的原型。
Javascript给对象提供一个名为`__proto__`的隐藏属性，某个对象的`__proto__`属性默认会指向它的构造器的原型对象，即`{Constructor}.prototype`。

```javascript
var a = new Object()
console.log(a.__proto__ === Object.prototype) // true
```

**实际上，`__proto__`就是对象跟“对象构造器的原型”联系起来的纽带**

#### 4、如果对象无法响应某个请求，它会把这个请求委托给它的构造器的原型

这条规则即是原型链继承的精髓所在。当一个对象无法响应某个请求的时候，它会顺着原型链把这个请求传递下去，直到遇到一个可以处理该请求的对象为止。

下面的代码是我们最常用的原型链继承方式：

```javascript
var obj = { name: 'sven' }

var A = function () {}
A.prototype = obj
var a = new A()
console.log( a.name ) // sven
```

我们来看看执行这段代码的时候，引擎做了哪些事情。

- 首先，尝试遍历对象`a`中所有属性，但没有找到`name`这个属性。
- 查找`name` 属性但这个请求被委托给对象`a`的构造器的原型，它被`a.__proto__`记录着并且指向`A.prototype`,而`A.prototype`被设置为对象`obj`。
- 在对象`obj`中找到了`name`属性，并返回它的值。
  
当我们期待得到一个“类”继承自另外一个“类”的效果时，往往会用下面的代码来模拟实现：

```javascript
var A = function ()  {}
A.prototype = { name: 'sven' }

var B = function () {}
B.prototype = new A();

var b = new B()
console.log(b.name) // sven
```

再看看这段代码执行的时候，引擎做了什么事情：

- 首先，尝试遍历对象`b`中所有的属性，但没有找到`name`这个属性。
- 查找`name`属性的请求被委托给对象`b`的构造器的原型，它被 `b.__proto__`记录着并且指向`B.prototype`,而`B.prototype`被设置为一个通过`new A()`创建出来的对象。
- 在该对象中依然没有找到`name`属性，于是请求被继续委托给这个对象构造器的原型`A.prototype`。
- 在`A.prototype`中找到了`name`属性，并返回了它的值。

和把`B.prototype`直接指向一个字面量对象相比，同过`B.prototype = new A()`形式的原型链比之前多了一层。但二者之间没有本质的区别，都是将对象构造器的原型指向另外一个对象，继承总是发生在对象和对象之间。

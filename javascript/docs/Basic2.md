# Javascript基本概念（二）

## 对象

> 所谓对象，就是一种无序的数据集合，由若干“键值对”(key-value)构成.

```js
var cat = {
    name: 'Tom',
    age: 8,
    say: function () {
        console.log('我想睡觉。。。‘);
    }
}
```

### 对象的创建方法

#### 工厂模式

``` js
function createCat(name, age, skinColor){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.skinColor = skinColor;
    o.say = function() {
        console.log('我是' + name);
   };
    return o;
}
var tom = createCat('Tom', 8, 'blue');
var garfield = createCat('Garfield', 7, 'golden');
```

#### 构造函数模式

> 构造函数专门用于创建对象，要创建Cat的实例，必须使用new操作符。会经历一下四个步骤：

- 创建一个新对象；
- 将构造函数的作用域赋予给新对象（因此this就指向了这个新对象）；
- 执行构造函数中的代码（为这个新对象添加属性）；
- 返回新对象。


``` js
function Cat(name, age, skinColor) {
    this.name = name;
    this.age = age;
    this.skinColor = skinColor;
    this.say = function() {
        console.log('我是' + name);
     };
}
var tom = new Cat('Tom', 8, 'blue');
var garfield = new Cat('Garfield', 7, 'golden');
```

#### 构造函数的问题

> 使用构造函数主要是方法在每个实例上都重新创建了一遍。在前面的例子里，tom和garfield都有一个名为say的方法，但是这两个方法不是同一个Function实例。记住，ECMAScript中的函数是对象。从逻辑的角度看，如下：

``` js
function Cat(name, age, skinColor) {
       this.name = name;
       this.age = age;
       this.skinColor = skinColor;
       this.say = new Function("console.log("我是" + this.name)");
}
```

#### 原型模式

创建的每个函数都有一个prototype(原型)属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。

``` js
function Cat() {
}
Cat.prototype.name = 'Tom';
Cat.prototype.age = 8;
Cat.prototype.skinColor = 'blue';
Cat.prototype.say = function() {
    console.log("我是" + this.name);
}
var cat1 = new Cat();
cat1.say(); // "Tom"
var cat2 = new Cat();
cat2.say(); // "Tom"
```

> 原型模式，省略了构造函数的初始化参数。从某种程度带来了不便。

#### 构造函数组合原型模式

创建对象的最常用的模式。该模式，使用构造函数定义实例属性，而原型用于定义方法和共享属性。

```js
function Cat(name, age) {
    this.name = name;
    this.age = age;
}
Cat.prototype = {
    constructor: Cat,
    say: function() {
        console.log(this.name);
   }
}
```

#### 动态原型模式

#### 寄生构造函数

#### 稳妥构造函数

## 原型

### prototype属性

每个构造函数都拥有一个prototype属性。这个属性指向函数的原型对象。默认情况下，原型对象会自动获得一个constructor属性，这个属性指向prototype属性的所在的函数。

### constructor属性

每个对象实例都有constructor属性，它指向该实例的构造器函数。

``` js
tom.constructor
garfield.constructor
```

都将返回Cat()构造器，因为该构造器包含这些原始的定义。

### 原型链

js常常被描述为基于原型的语言（prototype-based language）,每个对象拥有一个原型对象，对象以原型为模板，从原型继承方法和属性。原型对象也可以拥有原型，并从中继承方法和属性，以此类推。这种关系被成为”原型链“。

 > js中使用原型链来实现继承，举个例子。

```js
function Animal(name) {
    this.name = name;
    this.say = function (sentence) {
        console.log(sentence);
    }
}

function Cat() {
    this.catch = function() {
        console.log("I'm catching ...");
    }
}

Cat.prototype = new Animal();
var tom = new Cat('tom');

console.log(tom);
tom.catch();
tom.say("I catch a mouse!"); // I catch a mouse!
```

## 闭包

闭包是指那些能够访问独立变量的函数（变量在本地使用，但定义在一个封闭的作用域中）。换句话说，这些函数可以“记忆”它被创建时候的环境。

``` js
function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    console.log(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc(); // “Mozilla"
```

通常来说，局部变量仅在函数的执行期间可用。一旦makeFunc()执行过后，我们合理地认为name变量不再可用。但事实却不是这样的。原因就是myFunc变成了一个闭包。闭包是一种特殊的对象。它由两部分组成：函数，以及构建该函数的环境。环境由闭包创建在作用域中的任何局部变量组成。在上个例子中，myFunc是一个闭包，由displayName函数和闭包创建时存在的”Mozilla"字符串组成。

### 闭包的应用 & 性能

- [用闭包模拟私有方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures#Emulating_private_methods_with_closures)
- [性能考虑](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures#Performance_considerations)

## 线程

### 浏览器的三个常驻线程

- js引擎线程，js引擎线程是基于事件驱动单线程执行的，js引擎一直等待着任务队列中任务的到来然后加以处理，浏览器任何时候只有一个js线程运行。
- GUI渲染线程，负责浏览器界面的渲染，当界面需要重绘时或由于某种原因引起回流时，线程就会执行。另外，GUI渲染线程和js线程是互斥的。
- 浏览器事件触发线程，当一个事件被触发时该线程会把事件添加到处理队列的队尾，等待js线程处理。这些事件可以来自js引擎当前执行的代码块如setTimeOut，也可以来之鼠标点击，AJAX异步请求等。

### js单线程含义 & Web Worker

js单线程指，js只在一个线程上运行。js只能同时执行一个任务，其他任务都必须在后面排队等待。
为了让利用多核CPU的计算能力，HTML5提出了Web Worker标准，允许js脚本创建多个线程，但是子线程完全受到主线程的控制，且不得操作DOM。

> [Web Work参考阅读](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers)

### js异步编程

#### 回调函数

``` js
function func(cb) {
    doSomething()；
    cb();
}
```

#### 事件监听

``` js
btn.on('click', function () {
    doSomething();
})
me.trigger('click'); // 触发事件
```

#### 发布订阅

假定，存在一个信号中心，当某个任务执行完成，就像信号中心发布（publish）消息，其他任务可以向信号中心订阅（subscribe）这个消息，从而知道什么时候自己可以执行。

``` js
// 订阅消息
me.subscribe('done', function () {
    doSomething();
})
// 发布消息
her.publish('done')
```

#### Promises对象

Promises对象是CommonJS工作组提出的一种规范，目的是为异步编程提供统一接口。
它的思想是，每个异步任务返回一个Promise对象，该对象有一个then方法，允许指定回调函数。

``` js
now().then(function () {
    doSomething();
})
```

> 推荐阅读：[Javascript Promise迷你书](http://liubin.org/promises-book/)
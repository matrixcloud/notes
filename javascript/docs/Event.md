# 事件

事件，就是文档或浏览器窗口发生的一些特定的交互瞬间。可以使用监听器来预定事件，以便事件发生时执行相应的代码。这种在传统软件工程中被称为观察者模式，支持页面的**行为**(js代码)与**页面外观**(HTML&CSS)之间的松散耦合。

## 事件流

当浏览器发展到第四代时（IE4 & Netscape Communicator4），浏览器开发团队遇到一个很有意思的问题：页面的哪一部分会拥有某个特定的事件？要明白这个问题问什么，可以想象画在一张纸上的一组同心圆。如果你把手指放在圆心上，那么你的手指指向的不是一个圆，而是纸上的所有圆。两家公司的浏览器开发团队看待浏览器事件方面还是一致的。如果你单击了按钮的容器元素，甚至也单击了整个页面。

> **事件流**描述的是从页面中接收事件的顺序。但是IE的事件流是事件冒泡流，而Netscape是事件捕获流。

### 事件冒泡

IE的事件流叫做事件冒泡（event bubbling），即事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接受，然后逐级向上传播到较为不具体的节点（文档）。以下面的HTML页面为例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Event Bubbling Example</title>
</head>
<body>
    <div> Click me</div>
</body>
</html>
```

当你点击了页面中的```<div>```元素，那么这个click事件会按照如下顺序传播：

```shell
<div> => <body> => <html> => document
```

> 所有现代浏览器都支持事件冒泡，但在具体实现上还是有一些区别的。

### ~~事件捕获（很少使用）~~

Netscape的事件流叫做事件捕获（event capturing）。事件捕获的思想是不太具体的节点应该早些接受到事件，而最具体的节点应该最后接受到事件。事件捕获的用意在于在事件到达预定目标之前捕获它。如果任以前面的HTML页面作为演示事件捕获的例子，那么单击```<div>```元素就会以下列顺序触发click事件。

```shell
document => <html> => <body> => <div>
```

### DOM事件流

"DOM2级事件“规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。首先发生的是事件捕获，为截获事件提供了机会。然后是实际的目标接收到事件。最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。以前面简单的HTML页面为例，单击```<div>```元素后会按下图所示的顺序触发事件。

在DOM事件流中，实际的目标（```<div>```元素）在捕获阶段不会接收到事件。这意味着在捕获阶段，事件从```document```到```<html>```再到```<body>```后就停止了。

下一个阶段是“处于目标”阶段，于是事件在```<div>```上发生，并在事件处理中被看成冒泡阶段的一部分。然后，冒泡阶段发生，事件又传播回文档。

## 事件处理程序

事件是用户或浏览器自身执行的某种动作。例如click, load 和 mouseover, 都是事件的名字。而响应某个事件的函数就叫做**事件处理程序**（或**事件监听器**）

### HTML事件处理程序

直接在dom元素中绑定事件。

```html
 <input type="button" name="click me" onclick="alert('Clicked')"> ```
### DOM0级事件处理程序

取得dom元素的引用并绑定事件。（书上没明确给出事件所处阶段）

```js
var btn = document.getElementById('btn');
btn.onclick = function () {
    alert('Clicked');
}
```

> 注意：在这些代码运行以前不会指定事件处理程序，因此如果这些代码在页面中位于按钮后面，就有可能在一段时间内怎么单击都没有反应。

> 使用DOM0级方法的事件处理程序被认为是元素的方法。因此，这时候的事件处理程序在元素的作用域中运行；换句话讲，程序中的this引用当前元素。

```js
btn.onclick = function () {
    alert(this.id); // "btn"
}
```

> 事件删除： ``` btn.onclick = null; ```

### DOM2级事件处理程序

"DOM2级事件"定义了两个方法，用于处理指定和删除事件处理程序的操作：addEventListener()和removeEventListener()。所有DOM子节点都包含这两个方法，并且它们都接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。最后这个布尔值参数如果是true，表示在捕获阶段调用事件处理程序；如果是false，表示在冒泡阶段调用事件处理程序。

``` js
var btn = document.getElementById('btn');
btn.addEventListener('click', function() {
    alert(this.id);
}, false);
```

以上代码为按钮添加了onclick事件处理程序，而且该事件会在冒泡阶段被触发。与DOM0级方法一样，这里添加的事件处理程序也是在其依附的元素的作用域中运行。

> DOM2级方法添加事件处理程序的主要好处是可以添加多事件处理程序。

``` js
var btn = document.getElementById('btn');
btn.addEventListener('click', function() {
    alert(this.id);
}, false);
btn.addEventListener('click', function() {
    alert('Hello World');
}, false)
// 以上代码的事件处理程序会按照添加它们的顺序触发
// 因此会先显示ID，再显示"Hello World!"
```

通过addEventListener()添加的事件处理程序只能使用removeEventListener()来移除；移除时传入的参数与添加处理程序时使用的参数相同。这也意味着通过addEventListener()添加的匿名函数将无法移除。

举个例子：

``` js
var btn = document.getElementById('btn');
btn.addEventListener('click', function() {
    alert(this.id);
}, false);

btn.removeEventListener('click', function() {
    alert(this.id);
}, false);
```

以上代码并不能移除事件处理程序，因为匿名函数的原因。

> 大多情况下，都是把事件处理程序添加到事件流的冒泡阶段。

### IE事件处理程序

此处省略一万字。。。

## 事件对象

在触发DOM上的某个事件时，会产生一个事件对象event，这个对象中包含着所有与事件相关的信息。包括导致事件元素，事件类型及其他与事件相关的信息。

### DOM事件对象

兼容DOM的浏览器会将一个event对象传到事件处理程序中。无论事件处理程序使用的是什么方法（DOM0级或DOM2级），都会传入event对象。

```js
btn.onclick = function(event) {
    alert(event.type);
}
btn.addEventListener('click', function(event) {
    alert(event.type)
}, false)
```

event的成员列表如下：
| 成员 | 类型 | 读/写 | 说明 |
|--------|-------|---------|-------|
|  bubbles | Boolean | r | 表明事件是否冒泡 |
| cancelable | Boolean | r | 是否可以取消事件的默认行为 |
| currentTarget | Element | r | 事件处理程序正在处理事件的那个元素 |
| defaultPrevented | Boolean | r | ture表示已经调用了preventDefault()（DOM3级事件中新增） |
| detail | Integer | r | 与事件相关的细节信息 |
| eventPhase | Integer | r  | 调用事件处理程序的阶段：1表示捕获阶段，2表示处于目标，3表示冒泡阶段 |
| preventDefault() | Function | r | 取消事件的默认行为。如果cancelable是true，则可以使用这个方法 |
| stopImmediatePropagation() | Function | r | 取消事件的进一步捕获或冒泡，同时阻止任何事件处理程序被调用（DOM3级事件中新增）
| stopPropagation() | Function | r | 取消事件的进一步捕获或冒泡。如果bubbles为true，则可以使用这个方法 |
| target | Element | r | 事件目标 |
| trusted | Boolean | r | true表示事件由浏览器生成, false表示事件是开发人员通过js创建（DOM3级事件中新增）|
| type | String | r | 被触发的事件类型 |
| view | AbstractView | r | 与事件关联的抽象视图。等同于发生事件的window对象 |

在事件处理程序内部，对象this始终等于currentTarget的值，而target则只包含事件的实际目标。如果直接将事件处理程序指定给了目标元素，则this, currentTarget和target包含相同的值。

For example: 

```js
btn.onclick = function(event) {
    console.log(event.currentTarget === this); // true
    console.log(event.target === this); // true
}
```

这个例子，由于click事件的目标是按钮，因此三个值是相等的。如果事件处理程序存在于按钮的父节点中（如document.body），那么这些值是不同的。

``` js
document.body.onclick = function(event) {
    console.log(event.currentTarget === document.body); // true
    console.log(this === document.body); // true
    console.log(event.target === document.getElementById('btn')); // true
}
```
当单击这个例子中的按钮时，this和currentTarget都等于document.body, 因为事件处理程序是注册到这个元素上的。然而，target元素却等于按钮元素，因为它是click事件真正的目标。由于按钮上并没有注册事件处理程序，结果click事件就冒泡到了document.body，在那里事件得到了处理。

有时候需要处理多个事件可以使用type属性：

```js
var handler = function(e) {
    switch(e.type) {
        case 'click':
            // do something
            break;
        case 'mouseover':
            // do something
            break;
        case 'mouseout':
            // do something
            break;
   }
}
btn.onclickk = handler;
btn.onmouseover = handler;
btn.onmouseout = handler;
```
这个例子的handler函数处理了3个事件：click, mouseover, mouseout。如果要阻止特定事件的默认行为，可以使用preventDefault()方法。例如，链接的默认行为就是在被单击时会导航到其href指定的URL。如果你想阻止链接导航这一默认行为，那么通过链接的onclick事件处理程序可以取消它。

```js
var link = document.getElementById('link');
link.onclick = function (e) {
    e.preventDefault();
};
```

> 注意：只有cancelable属性设置为true的事件，才可以使用preventDefault()。

另外，stopPropagation()方法用于立即停止事件在DOM层次中的传播，即取消进一步的事件捕获或冒泡。举个例子：

``` js
btn.onclick = function (e) {
    console.log('button clicked...');
    e.stopPropagation();
}
document.body.onclick = function (e) {
    console.log('body clicked');
}
```

对于这个例子而言, click事件不会传播到document.body，因此无法执行 ``` console.log('body clicked');  ```

### IE事件对象

略

## 事件类型

| 事件类型 | 触发条件 | 例子 |
|--------------|-------------|--------|
| UI事件 | 当用户与页面上的元素发生交互时触发 | load, unload, abort, error, select, resize, scroll |
| 焦点事件 | 当元素获得或失去焦点时触发 |
| 鼠标事件 | 当使用鼠标执行操作时触发 |
| 滚轮事件 | 当使用鼠标滚轮时触发 |
| 文本事件 | 当文档中输入文本时触发 |
| 键盘事件 | 当用户通过键盘在页面上执行操作时触发 |
| 合成事件 | 当IME(Input Method Editor)输入字符时触发 |
| 变动事件 | 当底层DOM结构发生变化时触发 |

### [Html5事件](http://www.w3chtml.com/html5/ref/event.html)

## 内存和性能

在js中，添加到页面上的事件处理程序事件数量直接关系到页面的整体运行性能。

### 事件委托

对”事件处理程序过多“的问题的解决方案就是**事件委托**。事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。例如，click事件会一直冒泡到document层次。也就是说，我们可以为整个页面指定一个onclick事件处理程序，而不必给每个单击的元素分别添加事件处理程序。

``` js
<body>
    <ul id="links">
        <li id="link-1">link 1</li>
        <li id="link-2">link 2</li>
        <li id="link-3">link 3</li>
    </ul>
    <script>
        var links = document.getElementById('links');
        EventUtil.addHandler(links, 'click', function(e){
            switch(target.id) {
                case 'link-1':
                    // do sth
                    break;
                case 'link-2':
                    // do sth
                    break;
                case 'link-3':
                    // do sth
                    break;
            }
        });
    </script>
</body>
</html>
```

### 移除事件处理程序

每当事件处理程序指定给元素时，运行中的浏览器代码与支持页面交互的js代码之间就会建立一个连接。这种连接建立的越多，页面执行起来就越慢。如前所述，可以采用事件委托技术，限制建立的连接数量。另外，在不需要的时候移除事件处理程序，也是解决问题的一种方案。内存中留着一些过时不用的”空事件处理程序“(dangling event handler)，也是造成Web应用程序内存和性能问题的主要原因。
在两种情况下，可能会造成上述问题。

- 第一种情况是从文档中移除带有事件处理程序的元素时。这可能是通过纯粹的DOM操作，例如使用removeChild()和replaceChild()方法，但更多是发生在使用innerHTML替换页面中某一部分的时候。如果带有事件处理程序的元素被innerHTML删除了，那么原来添加到元素中的事件处理程序极有可能无法被当作垃圾回收。

``` js
<body>
    <div id="myDiv">
        <input type="button" value="Click me" id="my-btn">
    </div>
    <script>
        var btn = document.getElementById('my-btn');
        btn.onclick = function () {
            document.getElementById('myDiv').innerHTML = "替换成你想要的";
        }
    </script>
</body>
```

以上代码的问题是，在<div>元素上设置innerHTML可以把按钮移走，但事件处理程序仍然与按钮保持着引用关系。最好的做法是手工移除事件处理程序。``` btn.onclick = null; ```

- 第二种情况是，在页面卸载时，如果页面被卸载前没有清理干净事件处理程序，那么它们就会滞留在内存中。每次加载完页面再卸载页面时（可能是在两个页面间来回切换，也可能是单击了”刷新“按钮），内存中滞留的对象数目就会增加。
一般来说，最好的做法是在页面卸载之前，先通过onunload事件处理程序移除所有事件处理程序。

## 模拟事件

可以用js在任意时刻来触发特定的事件，而此时的事件就如同浏览器创建的事件一样。也就是说，这些事件该冒泡还会冒泡，而且照样能够导致浏览器执行已经指定的处理它们的事件处理程序。在Web测试应用程序中，模拟触发事件是一种极其有用的技术。DOM2级规范为此规定了模拟特定事件的方法。

### DOM中的事件模拟

js中事件的模拟为三个步骤： ``` 创建事件 -> 初始化事件种类 -> 触发事件 ```

```js
// 创建事件
var event = document.createEvent('MouseEvents');
// 初始化事件种类
event.initMouseEvent('click', true, true, document.defaultView, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
// 触发事件
btn.dispatchEvent(event);
```

> 注意： 在DOM2级中，事件类型的字符串都是英文复数，而在DOM3级中都变成了单数。

| DOM2级事件类型 | DOM3级事件类型 | 含义 |
|-------------------------|-------------------------|--------|
| UIEvents | UIEvent | 鼠标和键盘事件都继承自UI事件 |
| MouseEvents | MouseEvent | 一般化的鼠标事件 |
| MutationEvents | MutationEvent | 一般化的DOM变动事件 |
| HTMLEvents | 无对应事件| 一般化HTML事件 |

> 需要注意的是DOM2级事件并没有专门规定键盘事件，后来的DOM3级事件才正式给出规定。
> *在chrome中测试了下键盘事件无效*

### 自定义事件

在DOM3级中还定义了“自定义事件”。自定义事件不是由DOM原生触发的，它的目的是让开发人员创建自己的事件。

``` js
if(document.implementation.hasFeature('CustomEvents', '3.0')) {
    event = document.createEvent('CustomEvent');
    event.initCustomEvvent('myEvent', true, false, 'Hello World');
    btn.dispatchEvent(event);
}
```
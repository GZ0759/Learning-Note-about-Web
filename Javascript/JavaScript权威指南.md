> JavaScript权威指南第6版  
> David Flanagan 著  
> 淘宝前端团队 译  
> 2012 年 4 月第 1 版
> [试读章节](https://book.2cto.com/201210/7006.html)

# 前言
# 第1章 JavaScript概述

JavaScript 是面向 Web 的编程语言。绝大多数网站都适用了 JavaScript ，并且所有的现代 Web 浏览器——基于桌面系统、游戏机、平板电脑和智能手机的浏览器——均包含了 JavaScript 解释器。这使得 JavaScript 能够称得上史上使用最广泛的编程语言。JavaScript 也是前端开发工程师必须掌握的三种技能之一：描述网页内容的 HTML 、描述网页样式的 CSS 以及描述网页行为的 JavaScript 。

## 1.1 JavaScript语言核心

本节是 JavaScript 语言的一个快速概览，也是本书第一部分的快速概览。将关注 JavaScript 的基础知识，第二章讲解 JavaScript 注释、分号和 Unicode 字符集，第三章讲解 JavaScript 变量和赋值。

```js
// 变量是表示值的一个符号名字
// 变量是通过var关键字声明的
var x;    // 声明一个变量x

// 值可以通过等号赋值给变量
x = 0;    // 现在变量x的值为0
x     // => 0:通过变量获取其值

// JavaScript支持多种数据类型
x = 1;    // 数字
x = 0.01;   // 整数和实数共用一种数据类型
x = "hello world";  // 由双引号内的文本构成的字符串
x = 'JavaScript';  // 单引号内的文本同样构成字符串
x = true;   // 布尔值
x = false;   // 另一个布尔值
x = null;   // null是一个特殊的值，意思是"空"
x = undefined;  // undefined和null非常类似
```

JavaScript 中两个非常重要的数据类型是对象和数组。第六章介绍对象，第七章介绍数组，对象和数组在 JavaScript 中是很重要的。

```js
//JavaScript中的最重要的类型就是对象
//对象是名/值对的集合，或字符串到值映射的集合
var book = {    // 对象是由花括号括起来的
    topic: "JavaScript", // 属性"topic"的值是"JavaScript"
    fat: true   // 属性"fat"的值是true
};      // 右花括号标记了对象的结束

// 通过"."或" []"来访问对象属性
book.topic    // => "JavaScript"
book["fat"]    // => true:另外一种获取属性的方式
book.author = "Flanagan";  // 通过赋值创建一个新属性
book.contents = {};   // {} 是一个空对象，它没有属性

// JavaScript同样支持数组（以数字为索引的列表）
var primes = [2, 3, 5, 7];  // 拥有4个值的数组，由"["和"]"划定边界
primes[0]    // => 2: 数组中的第一个元素（索引为0）
primes.length   // => 4: 数组中的元素个数
primes[primes.length - 1]  // => 7:数组的最后一个元素
primes[4] = 9;   // 通过赋值来添加新元素
primes[4] = 11;   // 或通过赋值来改变已有的元素
var empty = [];   // [] 是空数组，它具有0个元素
empty.length    // => 0

// 数组和对象中都可以包含另一个数组或对象:
var points = [   // 具有两个元素的数组
 {x: 0, y: 0},  // 每个元素都是一个对象
 {x: 1, y: 1}
];
var data = {    // 一个包含两个属性的对象
    trial1: [[1, 2],[3, 4]], // 每一个属性都是数组
    trial2: [[2, 3],[4, 5]]  // 数组的元素也是数组
};
```

上段代码中通过方括号定义数组元素和通过花括号定义对象属性名和属性值之间的映射关系的语法称为初始化表达式（initalizer expression），第四章有专门的介绍。表达式是 JavaScript 中的一个短语，这个短语可以通过运算得出一个值。通过 “.” 和 “[]” 来引用对象属性或数组元素的值就构成一个表达式。

JavaScript 中最常见的表达式写法是想下面代码这样使用运算符（operator）。

```js
// 运算符作用于操作数，生成一个新的值
// 最常见的是算术运算符
3 + 2     // => 5: 加法
3 - 2     // => 1: 减法
3 * 2     // => 6: 乘法
3 / 2     // => 1.5: 除法
points[1].x - points[0].x  // => 1: 更复杂的操作数也能照常工作
"3" + "2"    // => "32": + 可以完成加法运算也可以作字符串连接

// JavaScript定义了一些算术运算符的简写形式
var count = 0;   // 定义一个变量
count++;    // 自增1
count--;    // 自减1
count += 2;    // 自增2：和"count = count + 2;"写法一样
count *= 3;    // 自乘3：和"count = count * 3;"写法一样
count     // => 6: 变量名本身也是一个表达式

// 相等关系运算符用来判断两值是否相等
// 不等、大于、小于运算符的运算结果是true或false
var x = 2, y = 3;   // 这里的 = 等号是赋值的意思，不是比较相等
x == y     // => false: 相等
x != y     // => true: 不等
x < y     // => true: 小于
x <= y     // => true: 小于等于
x > y     // => false: 大于等于
x >= y     // => false: 大于等于
"two" == "three"   // => false: 两个字符串不相等
"two" > "three"   // => true: "tw"在字母表中的索引大于"th"
false == (x > y)   // => true: false和false相等

// 逻辑运算符是对布尔值的合并或求反
(x == 2) && (y == 3)   // => true: 两个比较都是true，&&表示"与"
(x > 3) || (y < 3)   // => false: 两个比较不都是true，||表示"或"
!(x == y)    // => true: ! 求反
```

如果 JavaScript 中的“短语”是表达式的话，那么整个句子就称作语句（statement），第五章会详细讲解。在上述代码中，以分号结束的行均是一条语句（下面的代码中，会看到省略分号的多行语句）。实际上，语句和表达式之间有很多共同之处，粗略地将，表达式仅仅计算出一个值但并不作任何操作，它并不改变程序的运行状态。而语句并不包含一个值（或者说它包含的值我们并不关心），但它们改变程序的运算状态。在上文中已经见过变量声明语句和赋值语句。另一类是“控制结构”（control structure），比如条件判断和循环。

函数是带有名称（named）和参数的 JavaScript 代码段，可以一次定义多次调用。第八章会正式详细地讲解函数。

```js
// 函数是一段带有参数的JavaScript代码端，可以多次调用
function plus1(x) {   // 定义了名为plus1的一个函数，带有参数x
    return x+1;   // 返回一个比传入的参数大的值
}      // 函数的代码块是由花括号包裹起来的部分
plus1(y)      // => 4: y为3，调用函数的结果为 3+1

var square = function(x) {    // 函数是一种值，可以赋值给变量
    return x*x;     // 计算函数的值
};        // 分号标识了赋值语句的结束

square(plus1(y))     // => 16: 在一个表达式中调用两个函数
```

当将函数和对象合写在一起时，函数就变成了“方法”（method）：

```js
// 当函数赋值给对象的属性，我们称为
//  "方法"，所有的JavaScript对象都含有方法
var a = [];     // 创建一个空数组
a.push(1, 2, 3);    // push()方法向数组中添加元素
a.reverse();     // 另一个方法：将数组元素的次序反转

// 我们也可以定义自己的方法，"this"关键字是对定义方法
// 的对象的引用：这里的例子是上文中提到的包含两个点位置信息的数组
points.dist = function() {   // 定义一个方法用来计算两点之间的距离
    var p1 = this[0];   // 通过this获得对当前数组的引用
    var p2 = this[1];   // 并取得调用的数组前两个元素
    var a = p2.x - p1.x;   // X坐标轴上的距离
    var b = p2.y - p1.y;   // Y坐标轴上的距离
    return Math.sqrt(a * a +   // 勾股定理
    b * b);    // 用Math.sqrt()来计算平方根
};
points.dist()    // => 1.414: 求得两个点之间的距离
```

现在，给出一些控制语句的例子。

```js
// 这些JavaScript语句使用该语法包含条件判断和循环
// 使用了类似C、C++、Java和其他语言的语法
function abs(x) {    // 求绝对值的函数
    if (x >= 0) {    // if语句...
        return x;    // 如果比较结果为true则执行这里的代码.
    }      // 子句的结束.
    else {     // 当if条件不满足时执行else子句
        return - x;
    }      // 如果分支中只有一条语句，花括号是可以省略的
}       // 注意if/else中嵌套的return语句
function factorial(n) {   // 计算阶乘的函数
    var product = 1;    // 给product赋值为1
    while (n > 1) {    // 当()内的表达式为true时循环执行{}内的代码
        product *= n;   // "product = product * n; "的简写形式
        n--;     //  "n = n - 1; "的简写形式
    }      // 循环结束
    return product;    // 返回product
}
factorial(4)     // => 24: 1*4*3*2
function factorial2(n) {   // 实现循环的另一种写法
    var i, product = 1;   // 给product赋值为1
    for (i = 2; i <= n; i++)   // 将i从2自增至n
      product *= i;   // 循环体，当循环体中只有一句代码，可以省略{}
    return product;    // 返回计算好的阶乘
}
factorial2(5)    // => 120: 1*2*3*4*5
```

JavaScript 是一种面向对象的编程语言，但和传统的面向对象又有很大区别。第九章将详细讲解 JavaScript 中的面向对象编程。下面的代码展示了如何在 JavaScript 中定义一个类来表示 2D 平面几何中的点。这个类实例化的对象拥有一个名为 `r()` 的方法，用来计算点到原点的距离。

```js
// 定义一个构造函数以初始化一个新的Point对象
function Point(x, y) {   // 按照惯例，构造函数均以大写字母开始
    this.x = x;    // 关键字this指代初始化的实例
    this.y = y;    // 将函数参数存储为对象的属性
}       // 不需要return

// 使用new关键字和构造函数来创建一个实例
var p = new Point(1, 1); // 平面几何中的点 (1,1)

// 通过给构造函数的prototye对象赋值
// 来给Point对象定义方法
Point.prototype.r = function() {
    return Math.sqrt(   // 返回 x2 + y2 的平方根
    this.x * this.x +   // this指代调用这个方法的对象
    this.y * this.y);
};

// Point的实例对象p（以及所有的Point实例对象）继承了方法 r()
p.r()      // => 1.414...
```

第九章是第一部分的精华所在，后续的各章做了一些零星的延伸，将我们对 JavaScript 语言核心的探索带向尾声。第十章主要讲解了正则表达式的语法，并演示了如何使用这些“正则表达式”进行文本的模式匹配。第十一章介绍 JavaScript 语言核心的子集和超集。最后，在进入客户端 JavaScript 的内容之前，第十二章介绍两种在 Web 浏览器之外的两种 JavaScript 运行环境。

## 1.1 客户端JavaScript

第十三章是第二部分的第一章，该章介绍如何让 JavaScript 在 Web 浏览器中运行起来。从该章学到的最重要的内容是， JavaScript 代码可以通过`<script>`标签来嵌入到 HTML 文件中。

```html
<html>
<head>
<script src="library.js"></script> <!-- 引入一个JavaScript库-->
</head>
<body>
<p>This is a paragraph of HTML</p>
<script>
// 在这里编写嵌入到HTML文件中的JavaScript代码
</script>
<p>Here is more HTML.</P>
</body>
</html>
```

第十四章讲解 Web 浏览器脚本技术，并涵盖客户端 JavaScript 中的一些重要全局函数。

```html
<script>
function moveon() {
    // 通过弹出一个对话框来询问用户一个问题
    var answer = confirm("准备好了吗?");
    // 单击"确定"按钮，浏览器会加载一个新页面
    if (answer) window.location = "http://taobao.com";
}
// 在1分钟（6万毫秒）后执行定义的这个函数
setTimeout(moveon, 60000);
</script>
```

第十五章的内容更加务实——通过脚本来操纵 HTML 文档内容。它将展示如何选取特定的 HTML 元素、如何给 HTML 元素设置属性、如何修改元素内容，以及如何给文档添加新节点。

```js
// 在document中的一个指定的区域输出调试消息
// 如果document不存在这样一个区域，则创建一个
function debug(msg) {
    // 通过查看HTML元素id属性来查找文档的调试部分
    var log = document.getElementById("debuglog");

    // 如果这个元素不存在，则创建一个
    if (!log) {
        log = document.createElement("div");  // 创建一个新的<div>元素
        log.id = "debuglog";    // 给这个元素的HTML id赋值
        log.innerHTML = "<h1>Debug Log</h1>";  // 定义初始内容
        document.body.appendChild(log);  // 将其添加到文档的末尾
    }

    // 将消息包装在<pre>中，并添加至log中
    var pre = document.createElement("pre");  // 创建<pre>标签
    var text = document.createTextNode(msg);  // 将msg包装在一个文本节点中
    pre.appendChild(text);    // 将文本添加至<pre>
    log.appendChild(pre);    // 将<pre>添加至log
}
```

第十五章讲述 JavaScript 如何操纵 HTML 中定义 Web 内容的元素。第十六章讲诉如何使用 JavaScript 来进行 CSS 样式操作， CSS 样式定义了内容的展示方式。这通常会使用到 HTML 元素的 style 和 class 属性。

```js
function hide(e, reflow) {   // 通过JavaScript操纵样式来隐藏元素e
    if (reflow) {    // 如果第二个参数是true
        e.style.display = "none"  // 隐藏这个元素，其所占的空间也随之消失
    }
    else {     // 否则
        e.style.visibility = "hidden"; // 将e隐藏，但是保留其所占的空间
    }
}
function highlight(e) {   // 通过设置CSS类来高亮显示e
    // 简单地定义或追加HTML类属性
    // 这里假设CSS样式表中已经有"hilite"类的定义
    if (!e.className) e.className = "hilite";
    else e.className += " hilite";
}
```

可以通过 JavaScript 来操控 Web 浏览器中的HTML内容和文档的CSS样式，同样，也可以通过事件处理程序（event handler）来定义文档的行为。事件处理程序是一个在浏览器中注册的 JavaScript 函数，当特定类型的事件发生时浏览器便调用这个函数。通常我们关心的事件类型是鼠标点击事件和键盘按键事件（在智能手机中则是各种触碰事件）。或者说，当浏览器完成了文档的加载，当用户改变窗口大小或当用户向 HTML表单元素中输入数据时便会触发一个事件。第17章详细描述如何定义、注册事件处理程序，以及在事件发生时浏览器是如何调用它们的。

定义事件处理程序最简单的方法是，给 HTML 的以“on”为前缀的属性绑定一个回调。当写一些简单的测试程序时，最实用的方法就是给“onclick”处理程序绑定回调。假定已经将上文中的`debug()`和`hide()`两个函数保存至名为debug.js和hide.js的文件中，那么就可以写一个简单的HTML测试文件，来给`<button>`元素的onclick属性指定一个事件处理程序：

```html
<script src="debug.js"></script>
<script src="hide.js"></script>
<button onclick="hide(this,true); debug('hide button 1');">Hide1</button>
<button onclick="hide(this); debug('hide button 2');">Hide2</button>
```

下面这些客户端 JavaScript 代码用到了事件，它给一个很重要的事件——“load”事件注册了一个事件处理程序。同时，也展示了注册“click”事件处理函数更高级的一种方法。

```js
// "load"事件只有在文档加载完成后才会触发
//通常需要等待load事件发生后才开始执行JavaScript代码
window.onload = function() { // 当文档加载完成时执行这里的代码
    // 找到文档中所有的<img>标签
    var images = document.getElementsByTagName("img");

    // 遍历 images，给每个节点的"click"事件添加事件处理程序
    // 在点击图片的时候将图片隐藏
    for(var i = 0; i < images.length; i++) {
        var image = images[i];
        if (image.addEventListener)  // 注册事件处理程序的另一种方法
            image.addEventListener("click", hide, false);
        else     // 兼容IE8及以前的版本
            image.attachEvent("onclick", hide);
    }

    // 这便是上面注册的事件处理函数
    function hide(event) { event.target.style.visibility = "hidden"; }
};
```

第十五章到十七章讲诉了如何使用 JavaScript 来操控网页的内容（HTML）、样式（CSS）以及行为（事件处理）。这些章所讨论得 API 多少有些复杂，且至今仍具有糟糕的浏览器兼容性。也正是由于这个原因，很多客户端 JavaScript 程序员选择使用“库”或“框架”来简化他们的编码工作。最流行的库非 jQuery 莫属。第19章将会详细介绍 jQuery 库。 jQuery 定义了一套灵巧易用的 API ，用来操控文档内容、样式和行为。 jQuery 经过了完整的测试，在所有现代主流浏览器，甚至在 IE6 这种早期浏览器中都可以照常运行。

jQuery代码非常易于识别，因为它充分利用了一个名为`$()`的函数。这里用 jQuery 重写了上文中提到的`debug()`函数：

```js
function debug(msg) {
    var log = $("#debuglog");   // 找到要显示msg的元素.
    if (log.length == 0) {   // 如果不存在则创建之
        log = $("<div id='debuglog'><h1>Debug Log</h1></div>");
        log.appendTo(document.body);  // 并将其追加到body里
    }
    log.append($("<pre/>").text(msg)); // 将msg包装在<pre>中，再追加到log里
}
```

目前我们所提到的第二部分的四章都是围绕网页展开讨论的。后续的四章将着眼点转向 Web 应用。这几章的内容并不是讨论如何通过编写操控内容、样式和行为的脚本使用 Web 浏览器来渲染文档；而是讲解如何将 Web 浏览器当做应用平台，并描述了用以支持更复杂精细的客户端 Web 应用的现代浏览器 API 。第十八掌章讲解如何使用 JavaScript 来发起 HTTP 请求。第二十章描述数据存储的机制以及客户端应用中的会话状态的保持。第二十一章涵盖基于 HTML 的`<canvas>`标签的客户端 API ，用来进行任意形状图形的绘制。最后，第二十二章讲解 HTML5 所提供的新一代 Web 应用 API 。网络、存储、图形：这些都是 Web 浏览器提供的操作系统级的服务，它们定义了全新的跨平台的应用环境。如果你正在进行基于那些支持这些新 API 的浏览器的开发，这将是你作为客户端 JavaScript 程序员最激动人心的时刻。最后四章并没有太多示例代码，但下面的例子使用了这些新的API。

## 示例：一个 JavaScript 贷款计算器

这里的例子展示了诸多JavaScript语言核心特性，同样展示了重要的客户端JavaScript技术：

- 如何在文档中查找元素
- 如何通过表单 input 元素来获取用户的输入数据
- 如何通过文档元素来设置 HTML 内容
- 如何将数据存储在浏览器中
- 如何使用脚本发起HTTP请求
- 如何利用`<canvas>`元素绘图

# 第一部分 JavaScript语言核心
# 第2章 词法结构
# 第3章 类型、值和变量
# 第4章 表达式和运算符
# 第5章 语句
# 第6章 对象
# 第7章 数组
# 第8章 函数
# 第9章 类和模块
# 第10章 正则表达式的模式匹配
# 第11章 JavaScript的子集和扩展
# 第12章 服务端JavaScript
# 第二部分 客户端JavaScript
# 第13章 Web浏览器中的JavaScript
# 第14章 Window对象
# 第15章 脚本化文档
# 第16章 脚本化CSS
# 第17章 事件处理
# 第18章 脚本化HTTP
# 第19章 jQuery类库
# 第20章 客户端存储
# 第21章 多媒体和图形编程
# 第22章 HTML5 API
# 第三部分 JavaScript核心参考
# JavaScript核心参考
# 第四部分 客户端JavaScript参考
# 客户端JavaScript参考
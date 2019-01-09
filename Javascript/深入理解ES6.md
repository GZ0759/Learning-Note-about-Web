> 深入理解ES6
> Understanding ES6
> 2017年7月第一版

<!-- TOC -->

- [第一章 块级作用域绑定](#第一章-块级作用域绑定)
  - [1.1 var声明及变量提升（Hoisting）机制](#11-var声明及变量提升hoisting机制)
  - [1.2 块级声明](#12-块级声明)
  - [1.3 循环中的块作用域绑定](#13-循环中的块作用域绑定)
  - [1.4 全局块作用域绑定](#14-全局块作用域绑定)
  - [1.5 块级绑定最佳实践的进化](#15-块级绑定最佳实践的进化)
- [第二章 字符串和正则表达式](#第二章-字符串和正则表达式)
- [第三章 函数](#第三章-函数)
  - [3.1 函数形参的默认值](#31-函数形参的默认值)
  - [3.2 处理无命名参数](#32-处理无命名参数)
  - [3.3 增强的Function构造函数](#33-增强的function构造函数)
  - [3.4 展开运算符](#34-展开运算符)
  - [3.5 name属性](#35-name属性)
  - [3.6 明确函数的多重用途](#36-明确函数的多重用途)
  - [3.7块级函数](#37块级函数)
  - [3.8 箭头函数](#38-箭头函数)
  - [3.9 尾调用优化](#39-尾调用优化)
- [第四章 扩展对象的功能性](#第四章-扩展对象的功能性)
  - [4.1 对象类别](#41-对象类别)
  - [4.2 对象字面量语法扩展](#42-对象字面量语法扩展)
  - [4.3 新增方法](#43-新增方法)
  - [4.4 重复的对象字面量属性](#44-重复的对象字面量属性)
  - [4.5 自由属性枚举顺序](#45-自由属性枚举顺序)
- [第五章 解构：使数据访问更便捷](#第五章-解构使数据访问更便捷)
  - [5.1 为何使用解构功能](#51-为何使用解构功能)
  - [5.2 对象解构](#52-对象解构)
  - [5.3 数组解构](#53-数组解构)
  - [5.4 混合解构](#54-混合解构)
  - [5.5 解构参数](#55-解构参数)

<!-- /TOC -->

# 第一章 块级作用域绑定

## 1.1 var声明及变量提升（Hoisting）机制

在函数作用域或全局作用域中通过关键宇 var 声明的变量，无论实际上是在哪里声明的，都会被当成在当前作用域顶部声明的变量，这就是我们常说的提升（Hoisting）机制 。 

```javascript
function getValue(condition) {

    if (condition) {
        var value = "blue";

        // other code

        return value;
    } else {

        // value exists here with a value of undefined

        return null;
    }

    // value exists here with a value of undefined
}
```



## 1.2 块级声明

块级声明用于声明在指定块的作用域之外无法访问的变量。块级作用域（也被称为词法作用域）存在于：函数内部、块中（字符 { 和 } 之间的区域）。很多类C语言都有块级作用域，而ECMAScript 6 引入块级作用域就是为了让 JavaScript 更灵活也更普适。

let 声明。let 声明的用法与 var 相同。使用 let 来代替声明变量，就可以把变量的作用域限制在当前代码块中。由于 let 声明不会被提升，因此开发者通常将 let 声明语句放在封闭代码块的顶部，以便整个代码块都可以访问。

禁止重声明。假设作用域中已经存在某个标识符，此时再使用 let 关键字声明它机会抛出错误。但如果当前作用域内嵌另一个作用域，便可在内嵌的作用域中用 let 声明同名变量。

const 声明。使用 cons 声明的是常量，其值一旦被设定后不可更改。因此，每个通过 const 声明的变量必须进行初始化。

const 和 let 声明的都是块级标识符，所以常量也只在当前代码块内有效，一旦执行到块外立即被销毁。常量同样也不会被提升至作用域顶部。

与 let 相似，在同一作用域用 const 声明已经存在的标识符也会导致语法错误，无论该标识符是使用 var，还是 let 声明的。

const 声明与 let 声明有一处很大的不同，即无论在严格模式还是在非严格模式下，都不可以为 const 定义的常量再赋值，否则会抛出错误。但是，JavaScript 中的常量如果是对象，则对象中的值可以修改。

```javascript
const person = {
    name: "Nicholas"
};

// works
person.name = "Greg";

// throws an error
person = {
    name: "Greg"
};
```



临时死区TDZ。与 var 不同，let 和 const 声明的变量不会被提升到作用域顶部，如果在声明之前访问这些变量，即使是相对安全的 typeof 操作符也会触发引用错误。但在 let 声明的作用域外对该变量使用 typeof 则不会报错。

JavaScript 引擎在扫描代码发现变量声明时，要么将它们提升至作用域顶部（遇到 var 声明），要么将声明放到 TDZ 中（遇到 let 和 const 声明）。访问 TDZ 中的变量会触发运行时错误。只有执行过变量声明语句后，变量才会从 TDZ 中移出，然后方可正常访问。 

## 1.3 循环中的块作用域绑定

开发者可能最希望实现 for 循环的块级作用域了，因为可以把随意声明的计算器变量限制在循环内部。在 JavaScript 中，由于 var 声明得到了提升，变量在循环结束后仍可访问。如果换用 let 声明变量就能得到想要的结果。

```javascript
for (let i = 0; i < 10; i++) {
    process(items[i]);
}

// i is not accessible here - throws an error
console.log(i);
```



循环中的函数。长久以来，var 声明让开发者在循环中创建函数变得异常困难，因为变量到了循环之外仍能访问。

```javascript
var funcs = [];

for (var i = 0; i < 10; i++) {
    funcs.push(function() { console.log(i); });
}

funcs.forEach(function(func) {
    func();     // outputs the number "10" ten times
});
```



为了解决这个问题，开发者们在循环中使用立即调用函数表达式（IIFE），以强制生成计算器变量的副本。在循环内部，IIFE 表达式为接受的每一个变量 i 都创建了一个副本并存储为变量 value。这个变量的值就是相应迭代创建的函数所使用的值。

```javascript
var funcs = [];

for (var i = 0; i < 10; i++) {
    funcs.push((function(value) {
        return function() {
            console.log(value);
        }
    }(i)));
}

funcs.forEach(function(func) {
    func();     // outputs 0, then 1, then 2, up to 9
});
```



循环中的 let 声明。let 声明模仿上述实例中 IIFE 所做的一切来简化循环过程，每次迭代循环都会创建一个新变量，并以之前迭代中的同名变量的值将其初始化。这意味着彻底删除 IIFE 之后仍可得到预期中的结果。每次循环的时候 let 声明都会创建一个新变量 i，并将其初始化为 i 的当前值，每次循环内部创建的每隔函数都能得到属于它们自己的 i 的副本。对于 for-in 循环和 for-of 循环来说也是一样的。

```javascript
var funcs = [];

for (let i = 0; i < 10; i++) {
    funcs.push(function() {
        console.log(i);
    });
}

funcs.forEach(function(func) {
    func();     // outputs 0, then 1, then 2, up to 9
})
```



循环中的 const 声明。对于普通的 for 循环来说，可以在初始化变量时使用 const，但是更改这个变量的值就会抛出错误。在 for-in 或 for-of 循环中使用 const 时的行为与使用 let 一致，唯一的区别是在循环内不能改变变量的值，每次迭代不会修改已有的绑定，而是会创建一个新绑定。

## 1.4 全局块作用域绑定

let 和 const 相比 var，另外一个区别是它们在全局作用域中的行为。当 var 被用于全局作用域时，它会创建一个新的全局变量作为全局对象（浏览器环境中的 window 对象）的属性。这意味着用 var 很可能会无意中覆盖一个已经存在的全局变量。

如果在全局作用域中使用 let 或 const，会在全局作用域下创建一个新的绑定，但该绑定不会添加为全局对象的属性。换句话说，用 let 或 const 不能覆盖全局变量，而只能屏蔽它。

```javascript
// in a browser
let RegExp = "Hello!";
console.log(RegExp);                    // "Hello!"
console.log(window.RegExp === RegExp);  // false

const ncz = "Hi!";
console.log(ncz);                       // "Hi!"
console.log("ncz" in window);           // false
```



## 1.5 块级绑定最佳实践的进化

当更多的开发者迁移到 ECMAScript 6 后，一种做法日益普及：默认使用 const，只有确实需要改变变量的值时使用 let 。因为大部分变量的值在初始化后不应再改变，而预料外的变量值的改变是很多 bug 的源头。

# 第二章 字符串和正则表达式

# 第三章 函数

## 3.1 函数形参的默认值

JavaScript 函数有一个特别的地方，无论在函数定义中声明了多少形参，都可以传入任意数量的参数。这可以在定义函数时添加针对不同参数的处理逻辑，当已定义的形参无对应的传入参数时为其指定一个默认值。

在 ES5 中模拟默认参数。对于函数的命名参数，如果不显式传值，则其默认值为 undefined。因此我们经常使用逻辑或操作符为缺失的参数提供默认值。在 ECMAScript 5 和早期版本中，可以通过以下模式创建函数并未参数赋予默认值。然而这种方法也有缺陷，如果我们想给函数的形参传入值0，即使这个值是合法的，也会被视为一个 false 值。在这种情况下，更安全的选择是通过 typeof 检查参数类型。

```javascript
function makeRequest(url, timeout, callback) {
    timeout = timeout || 2000;
    callback = callback || function() {};
    // the rest of the function
}

function makeRequest(url, timeout, callback) {
    timeout = (typeof timeout !== "undefined") ? timeout : 2000;
    callback = (typeof callback !== "undefined") ? callback : function() {};
    // the rest of the function
}
```



ECMAScript 6 中的默认参数值。ES6 简化了为形参提供默认值的过程，如果没为参数传入值则为其提供一个初始值，而且不需要添加任何校验值是否缺失的代码，所以函数体会更加地小。对于默认参数值，null 是一个合法值，而如果是 undefined 则会使用默认值。

```javascript
function makeRequest(url, timeout = 2000, callback = function() {}) {
    // the rest of the function
}
```



默认参数值对 arguments 对象的影响。在 ECMAScript 5 非严格模式下，函数命名参数的变化会体现在 arguments 对象中，而在严格模式下，无论参数如何变化，arguments 对象不再随之改变。在 ES6 中，如果一个函数使用了默认参数值，则无论是否显式定义了严格模式，arguments 对象的行为都将与 ES5 严格模式下保持一致。默认参数值的存在使得 arguments 对象保持与命名参数分离。

```javascript
// not in strict mode
function mixArgs(first, second = "b") {
    console.log(arguments.length);
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
    first = "c";
    second = "d"
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
}

mixArgs("a");
// 1
// true
// false
// false
// false
```



默认参数表达式。可以通过函数执行来得到默认参数的值，如果不主动传入参数，那么就会调用方法（表达式）来得到正确的默认值。初次解析函数声明时不会调用该方法，只有当调用函数且不传入参数时才会调用。

```javascript
let value = 5;

function getValue() {
    return value++;
}

function add(first, second = getValue()) {
    return first + second;
}

console.log(add(1, 1));     // 2
console.log(add(1));        // 6
console.log(add(1));        // 7
```



正因为默认参数时在函数调用时求值，所以可以使用自定义的参数作为后定义参数的默认值。更进一步，可以将前面的参数传入一个函数来获得后面参数的默认值。但是，在引用参数默认值的时候，只允许引用前面参数的值，即先定义的参数不能访问后定义的参数。

```javascript
function add(first, second = first) {
    return first + second;
}
console.log(add(1, 1));     // 2
console.log(add(1));        // 2


function getValue(value) {
    return value + 5;
}
function add(first, second = getValue(first)) {
    return first + second;
}
console.log(add(1, 1));     // 2
console.log(add(1));        // 7
```



默认参数的临时死区。默认参数也同样有临死死区，在这里的参数不可访问。与 let 声明类似，定义参数时会为每个参数创建一个新的标识符绑定，该绑定在初始化之前不可被引用，如果试图访问会导致程序抛出错误。当调用函数时，会通过传入的值或参数的默认值初始化该参数。

```javascript
function add(first = second, second) {
    return first + second;
}

console.log(add(1, 1));         // 2
console.log(add(undefined, 1)); // throws error

// JavaScript representation of call to add(1, 1)
let first = 1;
let second = 1;

// JavaScript representation of call to add(undefined, 1)
let first = second;
let second = 1;
```



## 3.2 处理无命名参数

JavaScript 的函数语法规定，无论函数已定义的命名参数有多少，都不限制调用时传入的实际参数数量，调用时总是可以传入任意数量的参数。当传入更少的数量的参数时，默认参数值的特性可以有效简化函数声明的代码；当传入更多数量的参数时，ES6 同样也提供了更好的方案。

ES5 中的无命名参数。早先，JavaScript 提供arguments 对象来检查函数的所有参数，从而不必定义每一个要用的参数。但实际使用起来该对象缺有些笨重。下面的函数返回一个给定对象的副本，包含原始对象属性的特定子集。该函数只定义了一个参数，作为被复制属性的源对象，其他参数为被复制属性的名称。出现的情况，不容易发现这个函数可以接受任意数量的参数，需要从索引1而不是索引0开始遍历 arguments 对象。

```javascript
function pick(object) {
    let result = Object.create(null);

    // start at the second parameter
    for (let i = 1, len = arguments.length; i < len; i++) {
        result[arguments[i]] = object[arguments[i]];
    }

    return result;
}

let book = {
    title: "Understanding ECMAScript 6",
    author: "Nicholas C. Zakas",
    year: 2015
};

let bookData = pick(book, "author", "year");

console.log(bookData.author);   // "Nicholas C. Zakas"
console.log(bookData.year);     // 2015
```



不定参数。在 ES6 中，通过引入不定参数的特性可以解决这些问题。在函数的命名参数前添加三个点（...）就表明这是一个不定参数，该参数为一个数组，包含着自它之后传入的所有参数，通过这个数组名即可逐一访问里面的参数。

```javascript
function pick(object, ...keys) {
    let result = Object.create(null);

    for (let i = 0, len = keys.length; i < len; i++) {
        result[keys[i]] = object[keys[i]];
    }

    return result;
}
```



不定参数有两条使用限制。首先，每个函数最多只能声明一个不定参数，而且一定要放在所有参数的末尾。其次，不定参数不能用于对象字面量 setter 之中。之所以存在这条限制，是因为对象字面量 setter 的参数有且只能有一个，所以在当前上下文不允许使用不定参数。

## 3.3 增强的Function构造函数

Function 构造函数时 JavaScript 语法中很少被用到的一部分，通常我们由它来动态创建新的函数。这种构造函数接受字符串形式的参数，分别为函数的参数及函数体。

ES6 增强了 Function 构造函数的功能，支持在创建函数时定义默认参数和不定参数。唯一需要做的是在参数名后添加一个等号及一个默认值。对于 Function 构造函数，新增的默认参数和不定参数这两个特性使其具备了与声明式创建函数相同的能力。

## 3.4 展开运算符

展开运算符可以让你指定一个数组，将它们打算后作为各自独立的参数传入函数。

JavaScript 内建的 Math.max() 方法可以接受任意数量的参数并返回值最大的哪一个，如果希望从数组中挑选出最大的值则不能直接用该方法，需要手动遍历数组或使用 apply() 方法。

```javascript
let values = [25, 50, 75, 100]

console.log(Math.max.apply(Math, values));  // 100
```



使用 ES6 的展开运算符可以简化上述示例，向 Math.max() 方法传入一个数组，再在数组前添加不定参数中使用`...`符号，就无须再调用 apply() 方法了。JavaScript 引擎读取这段程序后悔将参数数组分割为各自独立的参数并依次传入。

```javascript
let values = [25, 50, 75, 100]

// equivalent to
// console.log(Math.max(25, 50, 75, 100));
console.log(Math.max(...values));           // 100
```



可以将展开运算符与其他正常传入的参数混合使用。假设想限定返回的最小值为0，防止负数偷偷溜进数组中，可以单独传入限定值。

```javascript
let values = [-25, -50, -75, -100]

console.log(Math.max(...values, 0));        // 0
```



## 3.5 name属性

ES6 程序中所有的函数的 name 属性都有一个合适的值，一般对应着生命是的函数名称或该匿名函数的变量的名称。

```javascript
function doSomething() {
    // ...
}

var doAnotherThing = function() {
    // ...
};

console.log(doSomething.name);          // "doSomething"
console.log(doAnotherThing.name);       // "doAnotherThing"
```



ES6 做了更多的改进来确保所有函数都有适合的名称。另外还有两个有关函数名称的特例，通过 bind() 函数创建的函数，其名称将带有“bound”前缀，通过 Function 构造函数创建的函数，其名称将带有前缀“anonymous”。

```javascript
var doSomething = function doSomethingElse() {
    // ...
};

var person = {
    get firstName() {
        return "Nicholas"
    },
    sayName: function() {
        console.log(this.name);
    }
}

console.log(doSomething.name);      // "doSomethingElse"
console.log(person.sayName.name);   // "sayName"

var descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor.get.name); // "get firstName"
```



切记，函数 name 属性的值不一定引用同名变量，它只是协助调试用的额外信息，所以不能使用 name 属性的值来获取对于函数的引用。

## 3.6 明确函数的多重用途

ES5 及早期版本中的函数具有多重功能，可以结合 new 使用，函数内的 this 值将指向一个新对象，函数最终会返回这个新对象。

```java
function Person(name) {
    this.name = name;
}

var person = new Person("Nicholas");
var notAPerson = Person("Nicholas");

console.log(person);        // "[Object object]"
console.log(notAPerson);    // "undefined"
```



JavaScript 函数有两个不同的内部方法：`[[Call]]`和`[[Construct]]`。当通过 new 关键字调用函数时，执行的是`[[Construct]]`函数，它负责创建一个通常被称作实例的新对象，然后再执行函数体，将 this 绑定到实例上；如果不通过 new 关键字调用函数，则执行`[[Call]]`函数，从而直接执行代码中的函数体。具有`[[Construct]]`方法的函数被统称为构造函数。

切记，不说所有函数都有`[[Construct]]`方法，因此不是所有函数都可以通过 new 来调用，比如箭头函数就没有这个方法。

在 ES5 中，如果想确定一个函数是否通过 new 关键字被调用，或者是判断该函数是否作为构造函数被调用，最流行的方式时使用 instanceof。

```javascript
function Person(name) {
    if (this instanceof Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person("Nicholas");  // throws error

var notAPerson = Person.call(person, "Michael");    // works!
```



为了解决判断函数是否通过 new 关键字调用的问题，ES6 引入了 new.target 这个元属性。元属性是指非对象的属性，其可以提供非对象目标的补充信息。当调用函数的`[[Construct]]`方法时，new.target 被赋值为 new 操作符的目标，通常是新创建对象实例，也就是函数体内的 this 的构造函数；如果调用`[[Call]]`方法，则 new.target 的值为 undefined。

有了这个元属性，可以通过检查 new.target 是否被定义过来安全第检测一个函数是否通过 new 关键字调用。

```javascript
function Person(name) {
    if (typeof new.target !== "undefined") {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person.call(person, "Michael");    // error!
```



## 3.7块级函数

在 ES3 和早期版本中，在代码块中声明一个块级函数严格来说是一个语法错误，但是所有的浏览器仍然支持这个特性，但兼容程度不一样。为了遏制这种互相不兼容的行为，ES5 的严格模式引入一个错误提示，当在代码块内部声明函数时程序会抛出错误。

```javascript
"use strict";

if (true) {

    // Throws a syntax error in ES5, not so in ES6
    function doSomething() {
        // ...
    }
}

```



在 ES6 中，会将函数视为一个块级声明，从而可以在定义该函数的代码块内访问和调用它。

```javascript
"use strict";

if (true) {

    console.log(typeof doSomething);        // "function"

    function doSomething() {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);            // "undefined"
```



块级函数与 let 函数表达式类似，一旦执行过程流出了代码块，函数定义立即被移除。二者的却别是，在该代码块中，块级函数会被提升至块的顶部，而用 let 定义的函数表达式不会被提升。

```javascript
"use strict";

if (true) {

    console.log(typeof doSomething);        // throws error

    let doSomething = function () {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);        // undefined
```



非严格模式下的块级函数。在 ES6 中，即使处于非严格模式下，也可以声明块级函数，但其行为与严格模式下稍有不同。这些函数不再提升至代码块的顶部，而是提升至外围函数或全局作用域的顶部。

```javascript
// ECMAScript 6 behavior
if (true) {

    console.log(typeof doSomething);        // "function"

    function doSomething() {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);            // "function"
```



## 3.8 箭头函数

箭头函数时一种使用箭头（=>）定义函数的新语法，它与传统的 JavaScript 函数有些许不同，主要集中在以下方面：

+ 没有 this、super、argumetns 和 new.target 绑定
+ 不能通过 new 关键字调用
+ 没有原型，即不存在 prototype 这个属性
+ 不可以改变 this 的绑定
+ 不支持 arguments 对象
+ 不支持重复的命名参数

箭头函数的语法多变，根据实际的使用场景有多重形式。所有变种都由函数参数、箭头、函数体组成，根据使用的需要，参数和函数体可以分别采取多种不同的形式。

当箭头函数只有一个参数时，可以直接写参数名，箭头紧随其后，箭头右侧的表达式被求值后便立即返回。即使没有显式的返回语句，这个箭头函数也可以返回传入的第一个参数，不需要更多的语法铺垫。

```javascript
var reflect = value => value;

// effectively equivalent to:

var reflect = function(value) {
    return value;
};
```



如果要传入两个或两个以上的参数，要在参数的两侧添加一堆小括号。

```javascript
var sum = (num1, num2) => num1 + num2;

// effectively equivalent to:

var sum = function(num1, num2) {
    return num1 + num2;
};
```



如果函数没有参数，也要在声明的时候写一组没有内容的小括号。

```javascript
var getName = () => "Nicholas";

// effectively equivalent to:

var getName = function() {
    return "Nicholas";
};
```



如果希望为函数编写由多个表达式组成的更传统的函数体，那么需要用花括号包括函数体，并显式定义一个返回值。

```javascript
var sum = (num1, num2) => {
    return num1 + num2;
};

// effectively equivalent to:

var sum = function(num1, num2) {
    return num1 + num2;
};
```



如果想要创建一个空函数，需要写一堆没有内容的花括号。或括号代表函数体的部分。

```javascript
var doNothing = () => {};

// effectively equivalent to:

var doNothing = function() {};
```



如果想要在箭头函数外返回一个对象字面量，则需要将该字面量包括在小括号里。

```javascript
var getTempItem = id => ({ id: id, name: "Temp" });

// effectively equivalent to:

var getTempItem = function(id) {

    return {
        id: id,
        name: "Temp"
    };
};
```



创建立即执行函数表达式。JavaScript 函数的一个流行的使用方式时创建立即执行函数表达式（IIFE），可以定义一个匿名函数并立即调用，自始至终不保存对该函数的引用。当希望创建一个与其他程序隔离的作用域时，这种模式非常方便。

```javascript
let person = function(name) {

    return {
        getName: function() {
            return name;
        }
    };

}("Nicholas");

console.log(person.getName());      // "Nicholas"
```



在代码中立即执行函数表达式通过 getName() 方法创建了一个新对象，将参数 name 作为该对象的一个私有成员返回给函数的调用者。只要将箭头函数包裹在小括号里，就可以用它实现相同的功能。注意，小括号只包括箭头函数定义，没有包含（“Nicholas”），这一点与正常函数有所不同，由正产函数定义的立即执行函数表达式既可以用小括号包括函数体，也可以额外包裹函数调用的部分。

```javascript
let person = ((name) => {

    return {
        getName: function() {
            return name;
        }
    };

})("Nicholas");

console.log(person.getName());      // "Nicholas"
```



箭头函数没有 this 绑定。函数内的 this 绑定时 JavaScript 中最常出现错误的因素，函数内的 this 值可以根据函数调用上下文而改变，这有可能错误地影响其他对象。

```javascript
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", function(event) {
            this.doSomething(event.type);     // error
        }, false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```



可以使用 bind() 方法显式地将 this 绑定到 PageHandler 函数上来修正这个问题。

```javascript
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", (function(event) {
            this.doSomething(event.type);     // no error
        }).bind(this), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```



但是调用 bind(this) 后事实上创建了一个额外的函数，可以通过一个更好的方式来修正这段代码，使用箭头函数。箭头函数没有 this 绑定，必须通过查找作用域链来决定其值。如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this；否则，this 的值会被设置为 undefined。

```javascript
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click",
                event => this.doSomething(event.type), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```



箭头函数缺少正常函数所拥有的 prototype 属性，它的设计初衷是“即用即弃”，所以不能用它来定义新的类型。如果尝试通过 new 关键字调用一个箭头函数，会导致程序抛出错误。同时，箭头函数中的 this 值取决于该函数外部非箭头函数的 this 值，且不能通过 call()、apply() 或 bind() 方法来改变 this 的值。

箭头函数和数组。箭头函数的语法简洁，非常适用于数组处理。

```javascript
var result = values.sort(function(a, b) {
    return a - b;
});

var result = values.sort((a, b) => a - b);
```



箭头函数没有 arguments 绑定。箭头函数没有自己的 arguments 对象，且未来无论函数在哪个上下文中执行，箭头函数始终可以访问外围函数的 arguments 对象。

```javascript
function createArrowFunctionReturningFirstArg() {
    return () => arguments[0];
}

var arrowFunction = createArrowFunctionReturningFirstArg(5);

console.log(arrowFunction());       // 5
```



箭头函数的辨识方法。尽管箭头函数与传统函数的语法不同，但它同样可以被识别出来。

```javascript
var comparator = (a, b) => a - b;

console.log(typeof comparator);                 // "function"
console.log(comparator instanceof Function);    // true
```



同样，仍然可以在箭头函数上调用 call()、apply() 及 bind() 方法，但与其他函数不同的是，箭头函数的 this 值不会受这些方法的影响。

## 3.9 尾调用优化

尾调用指的是函数作为另一个函数的最后一条语句被调用。在 ES5 的引擎中，尾调用的实现与其他函数调用的实现类似：创建一个新的栈帧，将其推入栈来表示函数调用。也就是说，在循环调用中，每一个未用完的栈帧都会被保存在内存中，当调用栈变得过大时会造成程序问题。

ES6 缩减了严格模式下尾调用栈的大小，在非严格模式下不受影响，如果满足以下条件，尾调用不再创建新的栈帧，而是清除并重用当前栈帧。

+ 尾调用不访问当前栈帧的变量，也就是说函数不是一个闭包
+ 在函数内部，尾调用是最后一条语句
+ 尾调用的结果作为函数值返回

实际上，尾调用的优化发生在引擎背后，除非尝试优化一个函数，否则无需思考此类问题。递归函数时其最主要的应用场景，此时尾调用优化的结果最显著。

```javascript
function factorial(n) {

    if (n <= 1) {
        return 1;
    } else {

        // not optimized - must multiply after returning
        return n * factorial(n - 1);
    }
}
```



```javascript
function factorial(n, p = 1) {

    if (n <= 1) {
        return 1 * p;
    } else {
        let result = n * p;

        // optimized
        return factorial(n - 1, result);
    }
}
```



# 第四章 扩展对象的功能性

ECMAScript 6 通过多种方式来加强对象的使用，通过简单的语法扩展，提供了更多操作对象及对象交互的方法。

## 4.1 对象类别

ES 6 规范清晰定义了每一个类别的对象。

- 普通对象：具有 JavaScript 对象所有的默认内部行为。
- 特异对象：具有某些与默认行为不符的内部行为。
- 标准对象：ES6 规范中定义的对象，例如 Array、Data等。标准对象既可以是普通对象，也可以是特异对象。
- 内建对象：脚本开始执行时存在于 JavaScript 执行环境中的对象，所有标准对象都是内建对象。

## 4.2 对象字面量语法扩展

对象字面量让我们创建对象不再需要编写冗余的代码，直接通过它简洁的语法就可以实现。而在 ES6 中，通过下面的几种语法，让对象字面量变得更强大、更简洁。

属性初始值的简写。在 ES6 中，通过使用属性初始化的简写语法，可以消除属性名称与局部变量之间的重复书写。当一个对象的属性与本地变量同名时，不必再写冒号和值，简单地只写属性名即可。

```javascript
function createPerson(name, age) {
    return {
        naem,  // name: name,
        age  // age: age
    };
}
```



对象方法的简写语法。在 ES5 及早期版本中，如果为对象添加方法，必须通过指定名称并完整定义函数来实现。而在 ES6 中，语法更简洁，消除了冒号和 function 关键字。在对象中创建一个方法，该属性被赋值为一个匿名函数表达式，它拥有在 ES5 中定义的对象方法所具有的的全部特性。二者唯一的区别是，简写方法可以使用 super 关键字。

```javascript
var person = {
    name: "Nicholas",
    sayName() {  // sayName: function() {
        console.log(this.name);
    }
};
```



可计算属性名。在 ES5 及早期版本的对象实例中，如果想要通过计算得到属性名，就需要用方括号代替点记法。有些包括某些字符的字符串字面量作为标识符会出错，其和变量放在方括号中都是被允许的。

```javascript
var person = {},
    lastName = "last name";

person["first name"] = "Nicholas";
person[lastName] = "Zakas";

console.log(person["first name"]);      // "Nicholas"
console.log(person[lastName]);          // "Zakas"
```



此外，在对象字面量中，可以直接使用字符串字面量作为属性名称。这种模式适用于属性名提前已知或可被字符串字面量表示的情况。

```javascript
var person = {
    "first name": "Nicholas"
};

console.log(person["first name"]);      // "Nicholas"
```



如果属性名称被包含在一个变量中，或者需要通过计算才能得到该变量的值，那么在 ES5 中是无法为一个对象字面量定义该属性的。而在 ES6 中，可在对象字面量中使用可计算属性名称，其语法与引用对象实例的可计算属性名称相同，也是使用方括号。

```javascript
var suffix = " name";

var person = {
    ["first" + suffix]: "Nicholas",
    ["last" + suffix]: "Zakas"
};

console.log(person["first name"]);      // "Nicholas"
console.log(person["last name"]);       // "Zakas"
```



## 4.3 新增方法

从 ES5 开始，避免创建新的全局方法和在 Object.prototype 上创建新的方法。当开发者想向标准添加新方法时，他们会找一个适当的现有对象，让这些方法可用。结果，当没有其他合适的对象时，全局 Object 对象会收到越来越多的对象方法。而在 ES6 中，为了使某些任务更易完成，在全局 Object 对象上引入了一些新方法。

Object.is()方法......

Object.assign()方法......

## 4.4 重复的对象字面量属性

ES5 严格模式中加入了对象字面量重复属性的校验，当同时存在多个同名属性时会抛出错误。但是在 ES6 中重复校验检查被移除了，无论是在严格模式还是非严格模式下，代码不再检查重复属性，对于每一组重复属性，都会选取最后一个取值。

```javascript
"use strict";

var person = {
    name: "Nicholas",
    name: "Greg"        // no error in ES6 strict mode
};

console.log(person.name);       // "Greg"
```



## 4.5 自由属性枚举顺序

ES5 中未定义对象属性的枚举顺序，由 JavaScript 引擎厂商自行决定。然而，ES6 严格规定了对象的自由属性被枚举时的返回顺序，这回影响到 Object.getOwnPropertyNames() 方法及 Reflect.ownKeys 返回属性的方式，Object.assign() 方法处理属性的顺序也将随之改变。

自有属性枚举顺序的基本规则是：

- 所有数字键按升序排序。
- 所有字符串建按照它们被加入对象的顺序排序。
- 所有 symbol 键按照它们被加入对象的顺序排序。

4.6 增强对象原型

4.7 正式的方法定义

# 第五章 解构：使数据访问更便捷

对象和数组字面量是 JavaScript 中两种最常用的数据解构，由于 JSON 数据格式的普及，二者已经成为语言中特别重要的一部分。在编码过程中，我们经常定义许多对象和数组，然后有组织第从中提取相关的信息片段。ES6 添加了可以简化这种任务的新特性：解构。解构是一种打破数据解构，将其拆分为更小部分的过程。

## 5.1 为何使用解构功能

在 ES5 及早期版本中，开发者为了从对象和数组中获取特定数据并赋值给变量，编写了很多看起来同质化的代码。想象一下，如果要提取更多变量，则必须依次编写类似的代码来为变量赋值，如果其中还包含嵌套结构，只靠遍历是找不到真实信息的，必须要深入挖掘整个数据解构才能找到所需数据。

```javascript
let options = {
        repeat: true,
        save: false
    };

// extract data from the object
let repeat = options.repeat,
    save = options.save;
```



所以 ES6 为对象和数组都添加了解构功能，将数据解构打散的过程变得更加简单，可以从打散后更小的部分中获取所需信息。许多语言都通过较少量的语法实现了解构功能，以简化获取信息的过程；而 ES6 中的实现实际上利用早已熟悉的语法：对象和数组字面量语法。

## 5.2 对象解构

对象字面量的语法形式是在一个赋值操作符左边放置一个对象字面量。type、name 都是局部声明的变量，也是用来从 oprions 对象读取相应值的属性名称。

```javascript
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type, name } = node;

console.log(type);      // "Identifier"
console.log(name);      // "foo"
```



可以将对象解构应用到变量的声明中，也同样可以在给变量赋值时使用解构语法。注意的是，一定要用一对小括号包裹解构赋值语句，JavaScript 引擎将一对开放的花括号视为一个代码块，而语法规定，代码块语句不允许出现在赋值语句左侧，添加小括号后可以将块语句转换为一个表达式，从而实现整个解构赋值的过程。

```javascript
let node = {
        type: "Identifier",
        name: "foo"
    },
    type = "Literal",
    name = 5;

// assign different values using destructuring
({ type, name } = node);

console.log(type);      // "Identifier"
console.log(name);      // "foo"
```



解构赋值表达式的值与表达式右侧的值相等，如此一来，在任何可以使用值得地方都可以使用解构赋值表达式。解构赋值表达式如果为 null 或 undefined 会导致程序抛出错误。也就是说，任何尝试读取 null 或 undefined 的属性的行为都会触发运行时错误。

```javascript
let node = {
        type: "Identifier",
        name: "foo"
    },
    type = "Literal",
    name = 5;

function outputInfo(value) {
    console.log(value === node);        // true
}

outputInfo({ type, name } = node);

console.log(type);      // "Identifier"
console.log(name);      // "foo"
```



默认值。使用解构赋值表达式时，如果指定的局部变量名称在对象中不存在，那么这个局部变量会被赋值为 undefined。但是当指定的属性不存在时，可以随意定义一个默认值，在属性名称或添加一个等号（=）和相应的默认值即可。

```javascript
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type, name, value, value1 = true } = node;

console.log(type);      // "Identifier"
console.log(name);      // "foo"
console.log(value);     // undefined
console.log(value1);    // true
```



为非同名局部变量赋值。如果希望使用不同命名的局部变量来存储对象属性的值，ES6 中的一个扩展语法可以满足需求，这个语法与完整的对象字面量属性初始化程序很像。这种语法实际上与传统对象字面量的语法相悖，原来的语法名称在冒号左边，值在右边；现在语法名在冒号右边，而读取的对象值在左边。当使用其他变量名进行赋值时也可以添加默认值，只需在变量名后添加等号和默认值即可。

```javascript
let node = {
        type: "Identifier",
        name: "foo"
    };

let { type: localType, name: localName } = node;

console.log(localType);     // "Identifier"
console.log(localName);     // "foo"
```



嵌套对象解构。解构嵌套对象仍然与对象字面量的语法相似，可以将对象拆解以获取想要的信息。在下面的解构中，所有冒号前的标识符都代表在对象中的检索位置，其右侧为被赋值的变量名；如果冒号后是花括号，则意味着要赋予的最终值嵌套在对象内部更深的层级中。

```javascript
let node = {
        type: "Identifier",
        name: "foo",
        loc: {
            start: {
                line: 1,
                column: 1
            },
            end: {
                line: 1,
                column: 4
            }
        }
    };

let { loc: { start }} = node;

console.log(start.line);        // 1
console.log(start.column);      // 1
```



更进一步，也可以使用一个与对象属性名不同的局部变量名。

```javascript
let node = {
        type: "Identifier",
        name: "foo",
        loc: {
            start: {
                line: 1,
                column: 1
            },
            end: {
                line: 1,
                column: 4
            }
        }
    };

// extract node.loc.start
let { loc: { start: localStart }} = node;

console.log(localStart.line);   // 1
console.log(localStart.column); // 1
```



## 5.3 数组解构

与对象解构的语法相比，数组解构就简单多了，它使用的是数组字面量，且解构操作全部在数组内完成，而不是像对象字面量语法一样使用对象的命名属性。在这个过程中，数组本身不会发生任何变化。

```javascript
let colors = [ "red", "green", "blue" ];

let [ firstColor, secondColor ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```



在解构模式中，也可以直接省略元素，只为感兴趣的元素提供变量名。

```javascript
let colors = [ "red", "green", "blue" ];

let [ , , thirdColor ] = colors;

console.log(thirdColor);        // "blue"
```



解构赋值。数组解构也可用于赋值上下文，但不需要用小括号包裹表达式，这一点与数组解构的约定不同。

```javascript
let colors = [ "red", "green", "blue" ],
    firstColor = "black",
    secondColor = "purple";

[ firstColor, secondColor ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```



数组解构还有一个独特的用例：交换两个变量的值，并且不需要额外的变量。右侧是一个为交换过程创建的临时数组字面量。代码执行过程中，先解构临时数组，将b和a的值赋值到左侧数组的前两个位置，最终结果是变量交换了它们的值。

```javascript
// Swapping variables in ECMAScript 6
let a = 1,
    b = 2;

[ a, b ] = [ b, a ];

console.log(a);     // 2
console.log(b);     // 1
```



默认值。也可以在数组解构赋值表达式中为数组中的任意位置添加默认值，当指定位置的属性不存在或其值为 undefined 时使用默认值。

嵌套数组解构。嵌套数组解构与嵌套对象解构的语法类似，在原有的数组模式中插入另一个数组模式，即可将解构过程深入到下一个层级。

不定元素。在数组中，通过通过“...”语法将数组中的其余元素赋值给一个特定的变量。

```javascript
let colors = [ "red", "green", "blue" ];

let [ firstColor, ...restColors ] = colors;

console.log(firstColor);        // "red"
console.log(restColors.length); // 2
console.log(restColors[0]);     // "green"
console.log(restColors[1]);     // "blue"
```



补丁元素语法有助于从数组中提取特定元素并保证其余元素可用，它还有数组复制的功能。concat() 方法的设计初衷是连接两个数组，如果调用时不传递参数就会返回当前函数的副本。在 ES6 中，可以通过不定元素的语法来实现相同的目标。

```javascript
// cloning an array in ECMAScript 6
let colors = [ "red", "green", "blue" ];
let [ ...clonedColors ] = colors;

console.log(clonedColors);      //"[red,green,blue]"
```



## 5.4 混合解构

可以混合使用对象解构和数组解构来创建更多复杂的表达式，如此一来，可以从任何混杂着对象和数组的数据解构中提取想要的信息。

## 5.5 解构参数

当定义一个接受大量可选参数的 JavaScript 函数时，通常会创建一个可选对象，将额外的参数定义为这个对象的属性。

````javascript
// properties on options represent additional parameters
function setCookie(name, value, options) {

    options = options || {};

    let secure = options.secure,
        path = options.path,
        domain = options.domain,
        expires = options.expires;

    // code to set the cookie
}

// 可以简化为这样
function setCookie(name, value, { secure, path, domain, expires }) {

    // code to set the cookie
}


// third argument maps to options
setCookie("type", "js", {
    secure: true,
    expires: 60000
});
````



必须传值的结构参数。解构参数有一个奇怪的地方，默认情况下，如果调用函数时不提供被解构的参数会导致程序抛出错误。如果解构赋值表达式的右值为 null 或 undefined，则程序也会报错。如果希望将解构参数定义为可选的，那么久必须为其提供默认值来解决这个问题。也可以为解构参数指定默认值，就像在解构赋值语句中做的那样，只需在参数后添加等号并且指定一个默认值即可。

```javascript
function setCookie(name, value, { secure, path, domain, expires } = {}) {

    // ...
}
```


> 深入理解ES6
> Understanding ES6
> 2017年7月第一版

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

## 3.4展开运算符

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










> 自己总结的 JavaScript 面试题  
> [参考-冴羽博客](https://github.com/mqyqingfeng/Blog/tree/master/articles)   
> [参考-后盾人](http://houdunren.gitee.io/note/js/1%20%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.html)
> [参考-木易杨](https://github.com/yygmind/blog)

# 深浅拷贝

如果数组元素是基本类型，就会拷贝一份，互不影响，而如果是对象或者数组，就会只拷贝对象和数组的引用，这样我们无论在新旧数组进行了修改，两者都会发生变化。我们把这种复制引用的拷贝方法称之为浅拷贝，与之对应的就是深拷贝，深拷贝就是指完全的拷贝一个对象，即使嵌套了对象，两者也相互分离，修改一个对象的属性，也不会影响另一个。

浅拷贝

1. slice、concat
2. for/in
3. Object.assign
4. 展开语法 Spread syntax

如果是数组，我们可以利用数组的一些方法比如：slice、concat 返回一个新数组的特性来实现拷贝。

```js
var arr = ["old", 1, true, null, undefined];
var new_arr = arr.concat();
// 或者
// var new_arr = arr.slice();
new_arr[0] = "new";
console.log(arr); // ["old", 1, true, null, undefined]
console.log(new_arr); // ["new", 1, true, null, undefined]
```

使用 for/in 执行对象拷贝。遍历对象，然后把属性和属性值都放在一个新的对象中。

```js
var shallowCopy = function (obj) {
  if (typeof obj !== "object") return;
  var newObj = obj instanceof Array ? [] : {};
  for (var key in obj) {
    if (obj.hasOwnProperty(key)) {
      newObj[key] = obj[key];
    }
  }
  return newObj;
};
```

Object.assign 函数可简单的实现浅拷贝，它是将两个对象的属性叠加后面对象属性会覆盖前面对象同名属性。

```js
let user = { name: "后盾人" };
let hd = Object.assign({}, user);
hd.name = "hdcms";
console.log(user.name); //后盾人
```

使用展示语法也可以实现浅拷贝。

```js
let obj = { name: "后盾人" };
let hd = { ...obj };
hd.name = "hdcms";
console.log(hd);
console.log(obj);
```

深拷贝

1. JSON序列化和解析
2. 递归for/of

那如何深拷贝一个数组呢？这里介绍一个技巧，不仅适用于数组还适用于对象！但是该方法有以下几个问题。
- 会忽略 undefined
- 会忽略 symbol
- 不能序列化函数
- 不能解决循环引用的对象
- 不能正确处理`new Date()`
- 不能处理正则

```js
var arr = ["old", 1, true, ["old1", "old2"], { old: 1 }];
var new_arr = JSON.parse(JSON.stringify(arr));
console.log(new_arr);
```

在拷贝的时候判断一下属性值的类型，如果是对象，我们递归调用深拷贝函数。

```js
var deepCopy = function (obj) {
  if (typeof obj !== "object") return obj;
  var newObj = obj instanceof Array ? [] : {};
  for (const [k, v] of Object.entries(obj)) {
    newObj[k] = deepCopy(v);
  }
  return newObj;
};
```

尽管使用深拷贝会完全的克隆一个新对象，不会产生副作用，但是深拷贝因为使用递归，性能会不如浅拷贝，在开发中，还是要根据实际情况进行选择。

我们知道 JSON 无法深拷贝循环引用，遇到这种情况会抛出异常。解决方案很简单，其实就是循环检测，我们设置一个数组或者哈希表存储已拷贝过的对象，当检测到当前对象已存在于哈希表中时，取出该值并返回即可。也可以使用了 ES6 中的 WeakMap 来处理。

# 函数节流和防抖

节流（`throttle`）:不管事件触发频率多高，只在单位时间内执行一次。有两种方式可以实现节流，使用时间戳和定时器。防抖（`debounce`）：不管事件触发频率多高，一定在事件触发`n`秒后才执行，如果你在一个事件触发的 `n` 秒内又触发了这个事件，就以新的事件的时间为准，`n`秒后才执行，总之，触发完事件 `n` 秒内不再触发事件，`n`秒后再执行。

节流

1. 时间戳实现
2. 定时器实现
3. 时间戳/定时器结合

时间戳实现。第一次事件肯定触发，最后一次不会触发

```js
function throttle(event, time) {
  let pre = 0;
  return function (...args) {
  if (Date.now() - pre > time) {
    pre = Date.now();
    event.apply(this, args);
  }
  }
```

定时器实现。第一次事件不会触发，最后一次一定触发

```js
function throttle(event, time) {
  let timer = null;
  return function (...args) {
    if (!timer) {
      timer = setTimeout(() => {
        timer = null;
        event.apply(this, args);
      }, time);
    }
  };
}
```

定时器和时间戳的结合版，也相当于节流和防抖的结合版，第一次和最后一次都会触发

```js
function throttle(event, time) {
  let pre = 0;
  let timer = null;
  return function (...args) {
    if (Date.now() - pre > time) {
      clearTimeout(timer);
      timer = null;
      pre = Date.now();
      event.apply(this, args);
    } else if (!timer) {
      timer = setTimeout(() => {
        event.apply(this, args);
      }, time);
    }
  };
}
```


防抖应用场景  
- 窗口大小变化，调整样式`window.addEventListener('resize', debounce(handleResize, 200));`
- 搜索框，输入后 300 毫秒搜索`debounce(fetchSelectData, 300);`
- 表单验证，输入 1000 毫秒后验证`debounce(validator, 1000);`

```js
function debounce(event, time) {
  let timer = null;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => {
      event.apply(this, args);
    }, time);
  };
}
```

有时候我们需要让函数立即执行一次，再等后面事件触发后等待`n`秒执行，我们给`debounce`函数一个`flag`用于标示是否立即执行。

```js
function debounce(event, time, flag) {
  let timer = null;
  return function (...args) {
    clearTimeout(timer);
    if (flag && !timer) {
      event.apply(this, args);
    }
    timer = setTimeout(() => {
      event.apply(this, args);
    }, time);
  };
}
```

# 数组去重

- 双层循环
- 开辟存储对象
- ES5方法filter和indexOf
- ES5方法sort排序去重
- ES6数据结构Set 
- ES6数据结构Map 

最原始的方法的双层循环

```js
function unique(array) {
  var res = [];
  for (var i = 0, arrayLen = array.length; i < arrayLen; i++) {
    // 第二层遍历
    for (var j = 0, resLen = res.length; j < resLen; j++ ) {
      if (array[i] === res[j]) {
          break;
      }
    }
    if (j === resLen) {
      res.push(array[i])
    }
    // 用indexOf/includes方法简化内层的循环
    // var current = array[i];
    // if (res.indexOf(current) === -1) {
    //   res.push(current)
    // }
  }
  return res;
}
```

开辟一个外部存储空间用于标示元素是否出现过。

```js
const unique = (array)=> {
  var container = {};
  return array.filter((item, index) => {
    return container.hasOwnProperty(item) ? false : (container[item] = true)
  });
}
```

ES5方法filter和indexOf

```js
const unique = arr => arr.filter((e,i) => arr.indexOf(e) === i);
// 不同于上面的去重，这里是只要数字出现了重复次，就将其移除掉。
// const filterNonUnique = arr => arr.filter(i => 
//   arr.indexOf(i) === arr.lastIndexOf(i)
// )
```

ES5方法sort排序去重

```js
const unique = (array) => {
  array.sort((a, b) => a - b);
  let pre = 0;
  const result = [];
  // 如果是第一个元素或者相邻的元素不相同
  for (let i = 0; i < array.length; i++) {
    if (!i || array[i] != array[pre]) {
      result.push(array[i]);
    }
    pre = i;
  }
  return result;
}
```

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。Set本身是一个构造函数，用来生成 Set 数据结构。Array.from方法可以将 Set 结构转为数组。

```js
const unique = arr => Array.from(new Set(arr));
// 或者
// const unique = arr => [...new Set(arr)];
```

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

```js
function unique (arr) {
  const seen = new Map()
  return arr.filter((a) => !seen.has(a) && seen.set(a, 1))
}
```

# 数组扁平化

- 循环递归的基本实现
- 使用reduce简化循环
- ES6扩展运算符循环
- 字符串toString方法

基本实现。循环数组元素，如果还是一个数组，就递归调用该方法。

```js
const flat = (array) => {
  let result = [];
  for (let i = 0; i < array.length; i++) {
    if (Array.isArray(array[i])) {
      result = result.concat(flat(array[i]));
    } else {
      result.push(array[i]);
    }
  }
  return result;
}
```

使用reduce简化

```js
function flatten(array) {
  return array.reduce(
    (target, current) =>
      Array.isArray(current) ?
        target.concat(flatten(current)) :
        target.concat(current)
    , [])
}
```

ES6 增加了扩展运算符，用于取出参数对象的所有可遍历属性，拷贝到当前对象之中。

```js
function flatten(arr) {
  while (arr.some(item => Array.isArray(item))) {
    arr = [].concat(...arr);
  }
  return arr;
}
```

如果数组的元素都是数字或都是字符串，那么我们可以考虑使用 toString 方法，因为：调用 toString 方法，返回了一个逗号分隔的扁平的字符串。然而这种方法使用的场景却非常有限，如果数组是 [1, '1', 2, '2'] 的话，这种方法就会产生错误的结果。

```js
function flatten(arr) {
  return arr.toString().split(',').map(() => +item)
}
```

# 数组最值

- 直接循环求最值
- 使用reduce简化
- Math.max接收合适参数
- 数组排序方法sort

最最原始的方法，莫过于循环遍历一遍：

```js
var result = arr[0];
for (var i = 1; i < arr.length; i++) {
    result =  Math.max(result, arr[i]);
}
```

既然是通过遍历数组求出一个最终值，那么我们就可以使用 reduce 方法。

```js
array.reduce((c, n) => Math.max(c, n))
```

`Math.max`参数原本是一组数字，只需要让他可以接收数组即可。

```js
const array = [3,2,1,4,5];
Math.max.apply(null, array);
// 或者
// Math.max(...array);
```

如果我们先对数组进行一次排序，那么最大值就是最后一个值：

```js
arr.sort(function (a, b) {
  return a - b;
});
arr[arr.length - 1]
```

# 函数柯里化

在数学和计算机科学中，柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术。通俗易懂的解释：用闭包把参数保存起来，当参数的数量足够执行函数了，就开始执行函数。

判断当前函数传入的参数是否大于或等于`fn`需要参数的数量，如果是，直接执行`fn`。如果传入参数数量不够，返回一个闭包，暂存传入的参数，并重新返回`currying`函数。

实际应用
- 延迟计算：部分求和、bind 函数
- 动态创建函数：添加监听 addEvent、惰性函数
- 参数复用

```js
function currying(fn, length) {
  length = length || fn.length; 	
  return function (...args) {			
    return args.length >= length	
    	? fn.apply(this, args)			
      : currying(fn.bind(this, ...args), length - args.length) 
  }
}
```

ES6 极简写法，更加简洁也更加易懂。

```js
function currying(fn, ...args) {
  if (args.length >= fn.length) {
    return fn(...args);
  } else {
    return (...args2) => currying(fn, ...args, ...args2);
  }
}
```

# 模拟实现call

- 1.判断当前`this`是否为函数，防止` Function.prototype.myCall()` 直接调用
- 2.`context` 为可选参数，如果不传的话默认上下文为 `window`
- 3.为`context` 创建一个 `Symbol`（保证不会重名）属性，将当前函数赋值给这个属性
- 4.处理参数，传入第一个参数后的其余参数
- 4.调用函数后即删除该`Symbol`属性

```js
Function.prototype.myCall = function (context = window, ...args) {
  if (this === Function.prototype) {
    return undefined; // 用于防止 Function.prototype.myCall() 直接调用
  }
  context = context || window;
  const fn = Symbol();
  context[fn] = this;
  const result = context[fn](...args);
  delete context[fn];
  return result;
}
```

# 模拟实现apply

`apply`实现类似`call`，参数为数组

```js
Function.prototype.myApply = function (context = window, args) {
  if (this === Function.prototype) {
    return undefined; // 用于防止 Function.prototype.myCall() 直接调用
  }
  const fn = Symbol();
  context[fn] = this;
  let result;
  if (Array.isArray(args)) {
    result = context[fn](...args);
  } else {
    result = context[fn]();
  }
  delete context[fn];
  return result;
}
```

# 模拟实现bind


- 1.处理参数，返回一个闭包
- 2.判断是否为构造函数调用，如果是则使用`new`调用当前函数
- 3.如果不是，使用`apply`，将`context`和处理好的参数传入

```js
Function.prototype.myBind = function (context,...args1) {
  if (this === Function.prototype) {
    throw new TypeError('Error')
  }
  const _this = this
  return function F(...args2) {
    // 判断是否用于构造函数
    if (this instanceof F) {
      return new _this(...args1, ...args2)
    }
    return _this.apply(context, args1.concat(args2))
  }
}
```

# 模拟实现new
# 模拟实现Promise


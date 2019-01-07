# 第一章 初识Vue.js

## 1.1 Vue.js是什么

Vue.js 的官方文档中是这样介绍它的。 简单小巧的核心，渐进式技术栈，足以应付任何规模的应用。 简单小巧是指 Vue.js 压缩后大小仅有17KB。所谓渐进式（Progressive），就是你可以一步一 步、有阶段性地来使用 Vue.js，不必一开始就使用所有的东西。

使用 Vue.js 可以让 Web 开发变得简单，同时也颠覆了传统前端开发模式。它提供了现代 Web 开发中常见的高级功能，比如 ：

- 解耦视图与数据
- 可复用的组件
- 前端路由
- 状态管理
- 虚拟 DOM ( Virtual DOM)

MVVM（Model-VIew-ViewModel）模式是由经典的软件架构 MVC 衍生来的 。当 View （视图层）变化时，会自动更新到 ViewModel （视图模型），反之亦然。 View 和 ViewModel 之间通过双向绑定（tdata-binding）建立联系。Vue.js 通过 MVVM 的模式拆分为视图与数据两部分，并将其分离。因此，你只需要关心你的数据即可， DOM 的事情 Vue 会帮你自动搞定。

## 1.2 如何使用Vue.js

Vue.js 是一个渐进式的 JavaScript 框架，根据项目需求，可以选择从不同的维度来使用它。如果只是简单开发， 可以直接通过 script 加载 CDN 文件。引入 Vue.js 框架后，在 body 底部使用 new Vue() 的方式创建一个实例，这就是 Vue.js 最基本的开发模式。

```html
`<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>`
```

# 第二章 数据绑定和第一个Vue应用

## 2.1 Vue实例与数据绑定

Vue.js 应用的创建很简单，通过构造函数 Vue 就可以创建一个 Vue 的根实例，并启动 Vue 应用。变量 app 就代表了这个 Vue 实例，基本上所以的代码都是一个对象，写入 Vue 实例的选项内。

```javascript
var app = new Vue({
	// 选项
})
```



Vue 实例对象的一个必不可少的选项就是 el ，用于指定一个页面中已存在的 DOM 元素来挂载 Vue 实例，它可以是 HTMLElement ，也可以是 CSS 选择器。挂载成功后，我们可以通过 app.$el 来访问该元素。Vue 提供了很多常见的实例属性和方法，都以`$`开头。

```javascript
// <div id='app'></div>
var app = new Vue({
		el:"#app" /* 或者是 document.getElementById('app') */
}）
```



通过 Vue 实例的 data 选项，可以声明应用内需要双向绑定的数据。建议所有会用到的数据都预先在 data 内声明，这样不至于将数据散落在业务逻辑中，难以维护。Vue 实例本身也代理了 data 对象里的所有属性。

```javascript
var data = { a: 1 }

// 直接创建一个实例
var vm = new Vue({
	data: data
})
vm.a // => 1
vm.$data === data // => true
```



每个 Vue 实例创建时，都会经历一系列的初始化过程，同时也会调用相应的生命周期钩子， 我们可以利用这些钩子，在合适的时机执行我们的业务逻辑。这些钩子与 el 和 data 类似，也是作为选项写入 Vue 实例内，并且钩子的 this 指向的是调用它的 Vue 实例。

```javascript
new Vue({
	el: '#app',
	data: {
		a: 1
	},
	created: function () {
		// `this` 指向 vm 实例
		console.log('a is: ' + this.a)
	},
		mounted: function () {
		console.log(this.$el); // <div id="app"><div>    
	}
})
```



比较常用的生命周期钩子有，created（实例创建完成后调用）、mounted（el挂载到实例上后调用）、beforeDestroy（实例销毁之前调用）。这些钩子与 el 和 data 类似，也是作为选项写入 Vue 实例内，并且钩子的 this 指向的是调用它的 Vue 实例。

使用双大括号（Mustache语法）`"{{}}"`是最基本的文本插值方法，它会自动将我们双向绑定的数值实时显示出来，可以 v-text 代替。通过任何方法修改数据，大括号的内容都会被实时替换。

```html
<span v-text="msg"></span>
<!-- 和下面的一样 -->
<span>{{msg}}</span>
```



```html
<p>Using mustaches: {{ rawHtml }}</p>   
<!-- 显示 rawHtml 表示的内容，文本形式 -->

<p>Using v-html directive: <span v-html="rawHtml"></span></p>
<!-- 显示rawHtml 表示的内容，如是 html 则解析再显示 -->

<span v-pre>{{ this will not be compiled }}</span>
<!-- 跳过这个元素及子元素的编译过程。可用来显示原始 Mustache 标签 -->
```



在`{{}}`中，除了简单的绑定属性值外，还可以使用 JavaScript 表达式进行简单的运算 、三元运算等。Vue.js只支持单个表达式，不支持语句和流控制。另外在表达式中，不能使用用户自定义的全局变量，只能使用 Vue 白名单内的全局变量，例如 Math 和 Date。

Vue.js 支持在`{{}}`插值的尾部添加一个管道符 “ | ” 对数据进行过滤，经常用于格式化文本。过滤的规则是自定义的，通过给 Vue 实例添加选项 filters 来设置，过滤器可以串联，也可以接受参数。过滤器可以用在两个地方：双花括号插值和 v-bind 表达式。

```html
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```



## 2.2 指令与事件

指令是 Vue.js 模板中最常用的一项功能，它带有前缀 v-，指令的主要职责就是当起表达式的值改变时，相应地将某些行为应用到 DOM 上。

数据驱动 DOM 是Vue.js 的核心理念，所以不到万不得已时不要主动操作 DOM，只需要维护好数据，DOM 的事 Vue 会帮忙优雅处理。

Vue.js 内置了很多指令，帮助快速完成常见的 DOM 操作，比如循环渲染、显示与隐藏等，其中有两个比较重要的指令是 v-bind 和 v-on。

v-bind 的基本用途是动态更新HTML元素上的属性，比如 id、class 等。

v-on 指令用来绑定事件监听器。在普通元素上，v-on 可以监听原生的 DOM 事件，表达式可以是一个方法名，这些方法都写在 Vue 实例的 methods 属性内，并且是函数的形式，函数内的 this 指向的是当前 Vue 实例本身，因此可以直接使用 this.xxx 的形式来访问或修改数据。表达式除了方法名，也可以直接是一个内联语句。

## 2.3 语法糖

语法糖是指在不影响功能的情况下，添加某种方法实现同样的效果，从而方便程序开发。

Vue.js 的 v-bind 和 v-on 指令都提供了语法糖，也可以说是缩写，比如 v-bind 可以省略，直接写一个冒号”:”；v-on 可以直接用"@"来缩写。

# 第三章 计算属性

## 3.1 什么是计算属性

模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。

所有的计算属性都以函数的形式写在 Vue 实例内的 computed 选项内，最终返回计算后的结果。

## 3.2 计算属性用法

在一个计算属性里可以完成各种复杂的逻辑，包括运算、函数调用等，只要最终返回一个结果就可以。除了简单的用法，计算属性还可以依赖多个 Vue 实例的数据，只要其中任一数据变化，计算属性就会重新执行，视图也会更新。  

每一个计算属性都包含一个 getter 和一个 setter，默认只是利用了 getter 来读取。在需要时，也可以提供一个 setter 函数 ， 当手动修改计算属性的值就像修改一个普通数据那样时，就会触发 setter 函数，执行一些自定义的操作。

```javascript
computed: {
	fullName: {
		// getter
		get: function () {
			return this.firstName + ' ' + this.lastName
		},
		// setter
		set: function (newValue) {
			var names = newValue.split(' ')
			this.firstName = names[0]
			this.lastName = names[names.length - 1]
		}
	}
}
vm.fullName = 'John Doe'
```



计算属性除了简单的文本插值外，还经常用于动态地设置元素的样式名称 class 和内联样式 style 。当使用组件时，计算属性也经常用来动态传递 props 。

计算属性还有两个很实用的小技巧，一是计算属性可以依赖其他计算属性，二是计算属性不仅可以依赖当前Vue实例的数据，还可以依赖其它实例的数据。

## 3.3 计算属性缓存

调用 methods 里的方法也可以与计算属性起到同样的作用。

```html
<p>Reversed message: "{{ reversedMessage() }}"</p>
```



```javascript
// 在组件中
methods: {
	reversedMessage: function () {
		return this.message.split('').reverse().join('')
	}
}
```



计算属性是基于它的依赖缓存的。 一个计算属性所依赖的数据发生变化时，它才会重新取值，所以依赖数据只要不改变，计算属性也就不更新。但是 methods 则不同，只要重新渲染，它就会被调用，因此函数也会被执行 。

使用计算属性还是 methods 取决于你是否需要缓存，当遍历大数组和做大量计算时，应当使用计算属性，除非你不希望得到缓存。  

# 第四章 v-bind及class与style绑定

## 4.1 了解b-bind指令

v-bind 的的主要用法是动态更新 HTML 元素上的属性。

在数据绑定中，最常见的两个需求就是元素的样式名称 class 和内联样式 style 的动态绑定，它们也是 HTML 的属性，因此可以使用 v-bind 指令。我们只需要用 v-bind 计算出表达式最终的字符串就可以，不过有时候表达式的逻辑较复杂，使用字符串拼接方法较难阅读和维护，所以 Vue.js 增强了对 class 和 style 的绑定。

## 4.2 绑定class的几种方式

绑定 class 的几种方式，包括对象语法、数组语法和在组件上使用。

给 v-bind:class 设置一个对象，可以动态地切换 class。对象中也可以传入多个属性，来动态切换 class，表达式中项为真时对应的类名就会加载。另外，该设置能够与普通 class 共存。

当 :class 的表达式过长或逻辑复杂时，还可以绑定一个计算属性。

```html
<div class="static"
		 v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```



当需要应用多个 class 时， 可以使用数组语法，给 :class 绑定一个数组，应用一个 class 列表。也可以使用三元表达式来根据条件切换 class。如果 class 有多个条件时，可以在数组语法中使用对象语法。

```html
<div v-bind:class="[activeClass, errorClass]"></div>
<!--  
data: {
	activeClass: 'active',
	errorClass: 'text-danger'
}
-->

<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>

```



如果直接在自定义组件上使用 class 或 :class，样式规则会直接应用到这个组件的根元素上。但只适合于自定义组件的最外层是一个根元素。当不满足这种条件或需要给具体的子元素设置类名时，应当使用组件的 props 来传递。

```html
<my-component class="baz boo"></my-component>
<!--  
渲染后的结果为
<p class="foo bar baz boo">Hi</p>  
-->
<script>
	Vue.component('my-component', {
		template: '<p class="foo bar">Hi</p>'
	})
</script>
```



## 4.3 绑定内联样式

绑定内联样式，使用v-bind:style可以给元素绑定内联样式，方法与:class类似，也有对象语法和数组语法，看起来很像直接在元素上写CSS。

CSS属性名称使用驼峰命名或短横分割命名。大多数情况下直接写一长串的样式不便于阅读和维护，一般以对象写在data或computed，也可以使用数组语法。大多数情况下 ， 直接写一长串的样式不便于阅读和维护，所以一般写在 data 或 computed 里。

```html
<div v-bind:style="styleObject"></div>

<script>
		var app = new Vue({
			el: '#app',
			data: {
				styleObject: {
					color: 'red',
					fontSize: '13px'
				}
			}
		})
</script>
```



另外，Vue.js会自动给特殊的CSS属性名称增加前缀，比如 transform。

# 第五章 内置指令

## 5.1 基本指令

v-cloak 不需要表达式，在 Vue 实例结束编译时从从绑定的HTML元素上移除，经常和 CSS 的 display:none 配合使用。在一般情况下，v-cloak 是一个解决初始化慢导致页面闪动的最佳实践。对于简单的项目很实用，但是在具有工程化的项目里，剩下的内容都是由路由去挂载不同组件完成的，所以不再需要 v-cloak。

```html
<div v-cloak>
	{{ message }}
</div>
```



```css
[v-cloak] {
	display: none;
}
```



v-once 也是一个不需要表达式的指令，作用是定义它的元素或组件只渲染一次，包括元素或组件的所有子节点。首次渲染后，不再随数据的变化重新渲染，将被视为静态内容。v-once 在业务中很少使用，只有需要进一步优化性能时才会用得上。

## 5.2 条件渲染指令

与 JavaScript 的条件语句 if、else、else if 类似，Vue.js 的条件指令可以根据表达式的值在 DOM 中渲染或销毁元素/组件。可以在 Vue.js 内置的`<template>`元素上使用条件指令，最终渲染的结果不会包含该元素。

Vue 在渲染元素时，出于效率考虑，会尽可能地复用已有的元素而非重新渲染。v-if、v-else-if、v-else 是条件渲染指令，利用 key 属性，可以决定是否要复用元素，key 的值必须是唯一的。

v-show 改变元素的 CSS 属性 display，当 v-show 表达式的值为 false 时，元素会隐藏，查看 DOM 结构会看到元素上加载了内联样式`display: none;`。但该指令不能在`<template>`上使用。

v-if 是真正的条件渲染，它会根据表达式适当地销毁或重建元素及绑定的事件或子组件，如果表达式的初始值为 false，则一开始元素/组件并不会渲染，只有当条件第一次变为真时才开始变异。而 v-show 只是简单的 CSS 属性切换，无论条件真与否，都会被编译。相比之下，v-if 更适合条件不经常改变的场景，因为它切换开销相对较大，而 v-show 适用于频繁切换条件。

## 5.3 列表渲染指令v-for

v-for 是列表渲染指令，可将一个数组遍历或枚举一个对象循环显示。也支持用 of 来代替 in 作为分隔符，前者是当前数组元素的别名，后者是数组。遍历数组时，v-for 的表达式支持一个可选参数作为当前项的索引。与 v-if 一样，v-for也可以用在内置标签`<template>`上，将多个元素进行渲染。

除了数组外，对象的属性也是可以遍历的。遍历对象属性时，有两个可选参数，分别是键名和索引。v-for 也可以迭代整数和字符串。

```html
<div v-for="(value, key, index) in object">
	{{ index }}. {{ key }}: {{ value }}
</div>
```



数组更新，Vue 包含了一组观察数组变异的方法，使用它们改变数组也会触发视图更新，例如 push()、pop()、shift()、unshift()、splice()、sort()、reverse()。但非变异方法不会更新视图，例如 filter()、concat()、slice()，它们返回的是一个新数组，在使用这些非变异方法时，可以用新数组来替换原数组。Vue 在检测到数组变化时，并不是直接重新渲染整个列表，而是最大化地复用 DOM 元素，因此可以大胆地用新数组来替换旧数组，不用担心性能问题。

需要注意的是，通过索引直接设置项、修改数组长度，也是不会触发视图更新的。解决第一个问题的方法可以使用 Vue内置的 set 方法，如果是在 webpack 中使用组件化的方式，默认是没有导入 Vue 的，这是可以使用`$set`。另一种方法时直接使用 splice 来解决。

```javascript
var vm = new Vue({
	data: {
		items: ['a', 'b', 'c']
	}
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的

// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```



过滤与排序。当不想改变原数组，想通过一个数组的副本来做过滤或排序的显示时，可以使用计算属性来返回过滤或排序后的数组。

```html
<li v-for="n in evenNumbers">{{ n }}</li>
```



```javascript
data: {
	numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
	evenNumbers: function () {
		return this.numbers.filter(function (number) {
			return number % 2 === 0
		})
	}
}
```



在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法。

```html
<li v-for="n in even(numbers)">{{ n }}</li>
```



```javascript
data: {
	numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
	even: function (numbers) {
		return numbers.filter(function (number) {
			return number % 2 === 0
		})
	}
}
```



## 5.4 方法与事件

在事件绑定上，类似原生 JavaScript 的 onclick 等写法，也是在 HTML 上进行监听的。`@click`的表达式可以直接使用 JavaScript语句，也可以是一个在 Vue 实例中 methods 选项内的函数名。

`@click`调用的方法名后可以不跟括号"()"，如果该方法有参数，默认会将原生事件对象 event 传入。

这种在 HTML 元素上监听事件的设计看似将 DOM 与 JavaScript 紧耦合，违背分离的原理，实则刚好相反。因为通过 HTML 就可以知道调用的是哪个方法，将逻辑与 DOM 解耦，便于维护。最重要的是，当 ViewModel 销毁时，所有的事件处理器都会自动删除，无需自己清理。

Vue 提供了一个特殊变量`$event`，用于访问原生 DOM 事件。

事件可以用修饰符来实现特定功能，例如`event.preventDefault()`，可以用 Vue 事件的修饰符来实现。在绑定的事件后加小圆点“.”，再跟一个后缀来使用修饰符。

- `.stop` - 调用 `event.stopPropagation()`。
- `.prevent` - 调用 `event.preventDefault()`。
- `.capture` - 添加事件侦听器时使用 capture 模式。
- `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
- `.native` - 监听组件根元素的原生事件。
- `.once` - 只触发一次回调。
- `.left` - (2.2.0) 只当点击鼠标左键时触发。
- `.right` - (2.2.0) 只当点击鼠标右键时触发。
- `.middle` - (2.2.0) 只当点击鼠标中键时触发。
- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

在表单元素上监听键盘事件时，还可以使用按键修饰符，比如按下具体某个键时才调用方法。也可以自己配置具体按键，全局定义后即可使用 keycode 和快键名称，甚至组合使用。除了具体的某个 keyCode 外，Vue 还提供了一些快捷名称。这些按键修饰符也可以组合使用，或和鼠标一起配合使用。

## 5.5 实战：利用计算属性、指令等知识开发购物车

购物侧需要展示一个已加入购物车的商品列表，包含商品名称、商品单价、购买数量和操作等信息，还需要实时显示购买的总价。其中购买数量可以增加或减少，每类购买数量还可以从购物车中移除。

注意，将 vue.min.js 和 index.js 文件写在`<body>`的最底部，如果写在`<head>`里，Vue 实例将无法创建，因为此时 DOM 还没有被解析完成，除非通过异步或在事件 DOMContentLoaded（IE是onreadystatechange）触发时再创建 vue 实例，这有点像 jQuery的`$(document).ready()`方法。

练习1：在当前示例基础上扩展商品列表，新增一项是否选中该商品的功能，总价变为只计算选中商品的总价，同时提供一个全选的按钮。

练习2：将商品列表 list 改为一个二维数组来实现商品的分类，比如可分为“电子产品”、“生活用品”和“果蔬”，同类商品聚合在一起。提示，你可能会用到两次 v-for。



# 第六章 表单与v-model

## 6.1 基本用法

Vue.js 提供了 v-model 指令，用于在表单类元素上双向绑定数据。例如在输入框上使用时，输入的内容会实时映射到绑定的数据上。使用 v-model 后，表单控件显示的值只依赖所绑定的数据，不再关心初始化时的 value 属性。

使用 v-model 时，如果是用中文输入法输入中文，一般在没有选定词语前，也就是在拼音阶段，Vue 是不会更新数据的。如果希望实时更新中文数据，可以用`@input`来替代 v-model。事实上，v-model 也是一个特殊的语法糖，只不过它会在不同的表单上智能处理。

```html
<body>
	<div id="app">
		<input type="text" @input="handleInput" placeholder="输入">
		<p>输入的内容是：{{ message }}</p>
	</div>
	<script>
		var app = new Vue({
			el:'#app',
			data:{
				message:''
			},
			methods:{
				handleInput:function(e){
					this.message = e.target.value
				}
			}
		})
	</script>
</body>
```



单选按钮在单独使用时，不需要 v-model，直接使用 v-bind 绑定一个布尔类型的值，为真时选中，为否时不选。如果是组合使用来实现互斥选择的效果，就需要 v-model 配合 value 来使用。

```html
<body>
	<div id="app">
		<input type="radio" name="a" id="html" v-model="picked" value="html">
		<label for="html">html</label>

		<input type="radio" name="a" id="css" v-model="picked" value="css">
		<label for="css">css</label>

		<input type="radio" name="a" id="js" v-model="picked" value="js">
		<label for="js">js</label>
	</div>
	<script>
		var app = new Vue({
			el:'#app',
			data:{
				picked:'js',
			}
		})
	</script>
</body>
```



复选框也分单独使用和组合使用，不过用法稍与单选不同。复选框单独使用时，也是用 v-model 来绑定一个布尔值。组合使用时，也是 v-model 与 value 一起，多个勾选框都绑定到同一个数组类型的数据，value 的值在数组当中，就会选中这一项。这一过程也是双向的，在勾选时，value 的值也会自动 push 到这个数字中。

```html
<body>
	<div id="app">
		<input type="checkbox" name="" id="football" value="football" v-model="checked">
		<label for="football">football</label>

		<input type="checkbox" name="" id="basketball" value="basketball" v-model="checked">
		<label for="basketball">basketball</label>

		<input type="checkbox" name="" id="volleyball" value="volleyball" v-model="checked">
		<label for="volleyball">volleyball</label>
	</div>
	<script>
		var app = new Vue({
			el:'#app',
			data:{
				checked:['football','volleyball']
			}
		})
	</script>
</body>
```



选择列表就是下拉选择器，也是常见的表单控件，同样也分为单选和多选两种方式。`<option>`是备选项，如果含有 value 属性，v-model 就会优先匹配 value 的值；如果没有，就会直接匹配`<option>`的 text。给`<selected>`添加属性 multiple 就可以多选，此时 v-model 绑定的是一个数组，与复选框用法类似。在业务中，`<option>`经常用 v-for 动态输出，value 和 text 也是用 v-bind 来动态输出的。

6.2



## 6.2 绑定值

单选框按钮、复选框和选择列表在单独使用或组合的模式下，v-model 绑定的值是一个动态字符串或布尔值，但在业务中，有时需要绑定一个动态的数据，这时可以用 v-bind 来实现。

单选按钮，通过 data 里面的 a 的值的改变，来动态绑定按钮。

```html
<input type="radio" v-model="pick" v-bind:value="a">
```



复选框，选中时和未勾选时有两种类型，如果再使用 v-bind 则可以在实现动态绑定。

```html
<input
	type="checkbox"
	v-model="toggle"
	true-value="yes"
	false-value="no"
>
```



选择列表，当选中时，app.selected 是一个 Object，所以 app.selected.number === 123。

```html
<select v-model="selected">
		<!-- 内联对象字面量 -->
	<option v-bind:value="{ number: 123 }">123</option>
</select>
```



## 6.3 修饰符

与事件的修饰符类似，v-model 也有修饰符，用于控制数据同步的时机。

lazy 修饰符。在输入框中，v-model 默认是在 input 时间中同步输入框的数据（除了提示中介绍的中文输入法情况外），使用修饰符 .lazy 会转变为在 change 事件中同步。

number 修饰符。使用修饰符 .number 可以将输入转换为 Number 类型，否则虽然输入的是数字，但类型还是 String，在数字输入框中比较有用。

trim 修饰符。使用修饰符 .trim 可以自动过滤输入的首尾空格。

# 第七章 组件详解

## 7.1 组件与复用

Vue.js 的组件就是提高复用性，让代码可复用。那些没见过的自定义标签就是组件，每个标签代表一个组件，在任何使用 Vue 的地方都可以直接使用。

定义组件名的方式有两种：

当使用 kebab-case (短横线分隔命名) 定义一个组件时，你也必须在引用这个自定义元素时使用 kebab-case，例如 `<my-component-name>`。

```
Vue.component('my-component-name', { /* ... */ })
```



当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 `<my-component-name>` 和 `<MyComponentName>` 都是可接受的。注意，尽管如此，直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的。

```
Vue.component('MyComponentName', { /* ... */ })
```



组件与创建 Vue 实例类似，需要注册后才可以使用。注册有全局注册和局部注册两种方式。全局注册后，任何 Vue 实例都可以使用。template 的 DOM 结构必须被一个元素包含。

```javascript
Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
```



```html
<div id="app">
	<component-a></component-a>
	<component-b></component-b>
	<component-c></component-c>
</div>
```



在 Vue 实例中，使用 components 选项可以局部注册组件，注册后的组件只有在该实例作用域下有效，组件中也可以使用 components 选项来注册组件，使组件可以嵌套。

```javascript
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }

new Vue({
	el: '#app',
	components: {
		'component-a': ComponentA,
		'component-b': ComponentB
	}
})
```



Vue 组件的模板在某些情况下会收到 HTML 的限制，比如`<table>`内规定只允许时`<tr>`、`<td>`、`<th>`等这些表格元素，所以在`<table>`内直接使用组件时无效的。这种情况下，可以使用特殊的 is 属性来挂载组件。

除了 template 选项外，组件中还可以像 Vue 实例那样使用其他的选项，比如 data、computed、methods 等。但是在使用 data 时，和实例稍有区别，data 必须是函数，然后将数据 return 出去。一个组件的 data 选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝。如果 return 出的对象引用了外部的一个对象，那这么这个对象就是共享的，任何一方修改都会同步。

```javascript
Vue.component('my-component',{
	template:'<button @click="counter++">{{ counter }}</button>',
	data:function(){
		return {
			counter: 0
		}
	}
})
```



## 7.2 使用props传递数据

组件不仅仅是要把模板的内容进行复用，更重要的是组件间要进行通信。通常父组件的模板中包含子组件，父组件要正向地向子组件传递数据或参数，子组件接收到后根据参数的不同来渲染不同的内容或执行操作。这个正向传递数据的过程就是通过 props 来实现的。

在组件中，使用选项 props 来声明需要从父级接受的数据，props 的值有字符串数组类型和对象类型。由于HTML特性不区分大小写，当使用DOM模板时，驼峰命名的 prop 名称要转为短横分割命名。

```javascript
Vue.component('blog-post', {
	// 在 JavaScript 中是 camelCase 的
	props: ['postTitle'],
	template: '<h3>{{ postTitle }}</h3>'
})
```



```html
<!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```



props 中声明的数据与组件 data 函数 return 的数据主要区别是 props 的来自父级，而 data 中的是组件自己的数据，作用域是组件本身，这两种数据都可以在模板 template 及计算属性 computed 和方法 methods 中使用。

有时候，传递的数据并不是直接写死的，而是来自父级的动态数据，这时可以使用指令 v-bind 来动态绑定 props 的值，党父组件的数据变化时，也会传递给子组件。

```html
<body>
	<div id="app">
		<input type="text" v-model="parentMessage">
		<my-component :message="parentMessage"></my-component>
	</div>
	<script>
		Vue.component('my-component',{
			props:['message'],
			template:'<div>{{ message }}</div>'
		});
		var app = new Vue({
			el:'#app',
			data:{
				parentMessage:''
			}
		})
	</script>
</body>
```



注意，如果只是传递数字、布尔值、数组、对象，而没有使用 v-bind，传递的仅仅是字符串。

单向数据流。Vue 2.x 与  Vue 1.x 比较大的一个改变就是，前者通过 props 传递数据是单向的了，也就是父组件数据变化时会传递给子组件，但是反过来不行。而在 Vue 1.x 里提供了 .sync 修饰符来支持双向绑定。之所以这样设计，是尽可能将父子组件解耦，避免子组件无意中修改了父组件的状态。

业务中会经常遇到两种需要改变 prop 的情况，一种是父组件传递初始值进来，子组件将它作为初始值保存起来，在自己的作用域下可以随意使用和修改。这种情况可以在组件 data 内再声明一个数据，引用父组件的 prop。通过这样，就可以避免直接操作 prop 数据。

```javascript
props: ['initialCounter'],
data: function () {
	return {
		counter: this.initialCounter
	}
}
```



另一种情况就是 prop 作为需要被转变的原始值传入。这种情况用计算属性就可以了。

```javascript
props: ['size'],
computed: {
	normalizedSize: function () {
		return this.size.trim().toLowerCase()
	}
}
```



注意，在 JavaScript 中对象和数组时引用类型，指向同一个内存空间，所以 props 是对象和数组时，在子组件内改变是会影响父组件的。

数据验证。props 选项的值除了一个数组，也可以是对象，当 prop 需要验证时，就需要对象写法。一般组件需要提供给别人使用时，推荐都进行数据验证，比如某个数据必须是数字类型，如果传入字符串，就会在控制台弹出警告。

```javascript
Vue.component('my-component', {
	props: {
		// 基础的类型检查 (`null` 匹配任何类型)
		propA: Number,
		// 多个可能的类型
		propB: [String, Number],
		// 必填的字符串
		propC: {
			type: String,
			required: true
		},
		// 带有默认值的数字
		propD: {
			type: Number,
			default: 100
		},
		// 带有默认值的对象
		propE: {
			type: Object,
			// 对象或数组默认值必须从一个工厂函数获取
			default: function () {
				return { message: 'hello' }
			}
		},
		// 自定义验证函数
		propF: {
			validator: function (value) {
				// 这个值必须匹配下列字符串中的一个
				return ['success', 'warning', 'danger'].indexOf(value) !== -1
			}
		}
	}
})
```



验证的 type 类型可以是 String、Number、Boolean、Object、Array、Function，type也可以是一个自定义构造器，使用 instanceof 检测。当 prop 验证失败时，在开发版本下会在控制台抛出警告。

## 7.3 组件通信

从父组件向子组件通信，通过 props 传递数据就可以了，但 Vue 组件通信的场景不止这一种。

自定义事件。子组件需要向父组件传递数据，则用到自定义事件。v-on 除了监听 DOM 事件外，还可以用于组件之间的自定义事件。

子组件用`$emit()`来触发事件，父组件用`$on()`来监听子组件的事件，父组件也可以直接在子组件的自定义标签上使用 v-on 来监听子组件触发的自定义事件。`$emit()`方法的第一个参数是自定义事件的名称，后面的参数是要传递的数据，可以不填或填写多个。除了用 v-on 在组件上监听自定义事件外，也可以监听 DOM 事件，这时可以用 .native 修饰符表示监听的是一个原生事件，监听的是该组件的根元素。

```javascript
Vue.component('welcome-button', {
	template: `
		<button v-on:click="$emit('welcome')">
			Click me to be welcomed
		</button>
	`
})
```



```html
<div id="emit-example-simple">
	<welcome-button v-on:welcome="sayHi"></welcome-button>
</div>
```



```javascript
new Vue({
	el: '#emit-example-simple',
	methods: {
		sayHi: function () {
			alert('Hi!')
		}
	}
})
```



使用 v-model。Vue 2.x 可以在自定义组件上使用 v-model 指令。

如下面的例子，并没有在`<my-component>`上使用`@input=handler`，而是直接用了 v-model 绑定的一个数据 total，着也可以称为语法糖。也可以间接地用自定义事件来实现。

```javascript
Vue.component('my-component',{
	template:'<button @click="handleClick">1</button>',
	data: function () {
		return { counter: 1}
	},
	methods:{
		handleClick: function(){
			this.$emit('input',this.counter)
		}
	}
});
```



```html
<div id="app">
	<p>总数：{{ total }}</p>
	<my-component v-model="total"></my-component>
	<!--<my-component @input="handleGetCounter"></my-component> -->
</div>
```



```javascript
var app = new Vue({
	el:'#app',
	data:{
		total:0
	},
		/* methods:{
			handleGetCounter(counter) {
			this.total = counter
			}
		} */
})
```



一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件，但是像单选框、复选框等类型的输入控件可能会将 value 特性用于不同的目的。实现一个具有双向绑定的 v-model 组件要满足两个条件：接收一个 value 属性和在有新的 value 时触发 input 事件。因此，v-model 可以用来创建自定义的表单输入组件，进行数据双向绑定。

非父子组件通信。在实际业务中，除了父子组件通信外，还有很多非父子组件通信的场景，非父子组件一般有两种，兄弟组件和跨多级组件。

在Vue.js 1.x中，除了`$emit()`方法外，还提供`$dispatch()`和`$broadcast()`这两个方法。前者用于向上级派发时间，后者是由上级向下级广播时间。这两种方法一旦发出事件后，任何组件都是可以接收到的，就近原则，而且会在第一次接收到后停止冒泡，除非返回 true。

在Vue.js 2.x中，推荐使用中央事件总线 (bus)，也就是一个中介。首先创建名为 bus 的空 Vue 实例，在生命周期 mounted 钩子函数里监控来自 bus 事件的方法，在组件中通过 bus 把事件发送出去。这种方法巧妙而轻量地实现了任何组件间的通信。

除了中央事件总线 bus 外，还有两种方法可以实现组件间的通信：父链和子组件索引。

父链。在子组件中，使用`this.$parent`可以直接访问该组件的父实例或组件，父组件也可以通过`this.$children`访问它所有的子组件，而且可以递归向上或向下无限访问，直到根实例或最内层的组件。在业务中，子组件应该尽可能地避免依赖父组件的数据，更不应该去主动修改它的数据。

子组件索引。当子组件较多时，通过`this.$children`来一一遍历出我们需要的一个组件实例是比较困难的，尤其是组件动态渲染时，它们的序列是不固定的。Vue提供了子组件索引的方法，用特殊的属性 ref 来为子组件指定一个索引名称。在父组件模板中，子组件标签上使用 ref 指定一个名称，并在父组件内通过`this.$refs`来访问指定名称的子组件。

```html
<!-- `vm.$refs.p` will be the DOM node -->
<p ref="p">hello</p>

<!-- `vm.$refs.child` will be the child component instance -->
<child-component ref="child"></child-component>
```



注意，`$refs`只在组件渲染完成后才填充，并且它是非响应式的，它仅仅作为一个直接访问于组件的应急方案，应当避免在模板或计算属性中使用`$refs`。

## 7.4 使用slot分发内容

当需要让组件组合使用，混合父组件的内容与子组件的模板时，就会用到 slot，这个过程叫做内容分发。

props 传递数据、events 触发事件和 slot 内容分发就构成了 Vue 组件的3个 API 来源，再复杂的组件也是由着3部分构成的。

父组件模板的内容是在父组件作用域内编译，子组件的内容是在子组件作用域内编译。因此，slot 分发的内容，作用域是在父组件上的。

单个 slot。在子组件内使用特殊的`<slot>`元素就可以为这个子组件开启一个 slot（插槽），在父组件模板里，插入在子组件标签内的所有内容将替代子组件的`<slot>`标签及它的内容。

子组件模板内定义`<slot>`元素，如果父组件没有使用 slot 时，默认渲染这段默认内容，否则，父组件的内容进行分发，替换整个`<slot>`插槽。

具名 slot。给`slot`元素指定一个 name 后可以分发多个内容，具名 slot 可以与单个 slot 共存。

如果`<slot>`没有使用 name 特性，它将作为默认 slot 出现，父组件没有使用 slot 的元素与内容都将出现在这里。如果没有指定默认的匿名 slot，父组件内多余的内容片段都将被抛弃，除非父组件的组件标签使用 inline-template 特性，将它们作为内联模板。另一种 `slot` 特性的用法是直接用在一个普通的元素上。

```html
<div class="container">
	<header>
		<slot name="header"></slot>
	</header>
	<main>
		<slot></slot>
	</main>
	<footer>
		<slot name="footer"></slot>
	</footer>
</div>
```



```html
<base-layout>
	<template slot="header">
		<h1>Here might be a page title</h1>
	</template>

	<p>A paragraph for the main content.</p>
	<p>And another one.</p>

	<template slot="footer">
		<p>Here is some contact info</p>
	</template>
</base-layout>
```



作用域插槽。作用域插槽是一种特殊的 slot，使用一个可以复用的模板替换已渲染元素。作用域插槽的使用场景就是父组件既可以复用子组件的 slot，又可以使 slot 内容不一致。

```html
<body>
	<div id="app">
		<child-component>
			<template slot-scope="props">
			<p>来自父组件的内容</p>
			<p>{{ props.msg }}</p>
			</template>
		</child-component>
	</div>
	<script>
		Vue.component('child-component',{
			template:'<div class="container"><slot msg="来自子组件的内容"></slot></div>'
		});
		var app = new Vue({
			el:'#app'
		})
	</script>
</body>
```



作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表每一项。

```html
<child-component :books="bk">
	<!-- 作用域插槽也可以是具名的slot -->
	<template slot="sn" slot-scope="props">
		<li>{{ props.bookName }}</li>
	</template>
</child-component>
```



```javascript
Vue.component('child-component',{
	props:{
		books:{
			type:Array,
			default:function(){
				return [];
			}
		}
	},
	template:'<ul><slot name="sn" v-for="b in books" :book-name="b.name"></slot></ul>'
});
var app = new Vue({
	el:'#app',
	data:{
		bk:[
			{name:'《Vue.js实战》'},
			{name:'javascript高级程序设计'},
			{name:'javascrip语言精粹'}
		]
	}
})
```



访问 slot。Vue.js 2.x 提供了用来访问被 slot 分发的内容的方法`$slots`。每个具名插槽有其相应的属性 (例如：slot="foo"中的内容将会在`vm.$slots.foo`中被找到)。default属性包括了所有没有被包含在具名插槽中的节点。

## 7.5 组件高级用法

递归组件。组件在它的模板内可以递归地调用自己，只要给组件设置name的选项就可以。设置 name 后，在组件模板内就可以递归使用了。不过需要注意的是，必须给一个条件来限制递归数量，否则会抛出错误：max stack size exceeded。

```html
<div id="app">
	<child-component :count="1"></child-component>
</div>
```



```javascript
Vue.component('child-component',{
	name:'child-component',
	props:{
		count:{
			type:Number,
			default:1
		}
	},
	template:'<div class="child"><child-component :count="count+1" v-if="count<6"></child-component></div>'
})
```



内联模板。组件的模板一般都是在 template 选项内定义的，Vue 提供了一个内联模板功能，在使用组件时，给组件标签使用 inline-tempate 特性，组件就会把它的内容当作模板，而不是把它当作内容分发，这让模板更灵活。但是，在父组件声明的数据和在子组件中声明的数据，都可以渲染，如果同名优先使用子组件的数据，这就是内联模板的缺点， 它让作用域比较难理解。

```html
<my-component inline-template>
	<div>
		<p>These are compiled as the component's own template.</p>
		<p>Not parent's transclusion content.</p>
	</div>
</my-component>
```



动态组件。Vue.js 提供了一个特殊的元素`<component>`用来动态地挂载不同的组件，使用 :is 特性来选择要挂载的组件。动态地改变 currentView 的值就可以动态挂载组件了，也可以直接绑定在组件对象上。

```html
<component :is="currentView"></component>
<button @click="handleChangeView('A')">切换到A</button>
<button @click="handleChangeView('B')">切换到B</button>
<button @click="handleChangeView('C')">切换到C</button>
```



```javascript
components:{
	comA:{template:'<div>组件A</div>'},
	comB:{template:'<div>组件B</div>'},
	comC:{template:'<div>组件C</div>'}
},
data:{
	currentView:'comA'
},
methods:{
	handleChangeView:function(com){
		this.currentView = 'com' + com
	}
}
})
	</script>
```



异步组件。当工程足够大，使用组件足够多时，是时候考虑下性能问题了。Vue.js 允许将组件定义为一个工厂函数，动态地解析组件 Vue.js 只在组件需要渲染时触发工厂函数，并且把结果缓存起来，用于后面的再次渲染。

工厂函数接收一个 resolve 回调，在收到从服务器下载的组件定义时调用，也可以调用 reject(reson) 提示加载失败。

```javascript
Vue.component('async-example', function (resolve, reject) {
	setTimeout(function () {
		// 向 `resolve` 回调传递组件定义
		resolve({
			template: '<div>I am async!</div>'
		})
	}, 1000)
})
```



## 7.6 其它

Vue 在观察到数据变化时并不是直接更新 DOM，而是开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。在缓冲时会去除重复数据，从而避免不必要的计算和 DOM 操作。然后，在下一个事件循环 tick 中，Vue 刷新队列并执行实际（已去重的）工作。所以如果用一个 for 循环来动态改变数据100次，其实只会应用最后一次改变，如果没有这种机制，DOM 就要重绘 100次，这固然是一个很大的开销。

Vue 会根据当前浏览器环境优先使用原生的 Promise then 和 MutationObserver，如果都不支持就会采用 setTimeout 代替。

`$nextTick`就是用来指导什么时候 DOM 更新完成的。`$nextTick`将回调延迟到下次DOM更新循环之后执行。在修改数据之后立即使用它，然后等待DOM更新。

理论上，我们应该不用去主动操作 DOM，因为 Vue 的核心思想就是数据驱动 DOM，但在很多业务里，我们避免不了会使用一些第三方库，这些机遇原生 JavaScript 的库都有创建和更新及销毁的完整生命周期，与 Vue 配合使用，就要利用好`$nextTick`。

```javascript
// 修改数据
vm.msg = 'Hello'
// DOM 还没有更新
Vue.nextTick(function () {
	// DOM 更新了
})
```



X-Templates。Vue 提供了另一种定义模板的方式，在一个`<script>`标签里使用 text/x-template 类型，并且指定一个 id，将这个 id 赋给 template。在`<script>`标签里，可以愉快地写 HTML 代码，不用考虑换行问题。不过，Vue 的初衷并不是滥用它，因为它将模板和组件的其他定义隔离了。

```html
<script type="text/x-template" id="hello-world-template">
	<p>Hello hello hello</p>
</script>
```



```javascript
Vue.component('hello-world', {
	template: '#hello-world-template'
})
```



手动挂载实例的方法。创建的实例可以通过 new Vue() 的形式创建出来。在一些非常特殊的情况下，我们需要动态地区创建 Vue 实例。Vue 提供了 Vue.extend 和`$mount`两个方法来手动挂载一个实例。

Vue.extend 是基础 Vue 构造器，创建一个“子类”，参数是包含组件选项的对象。

如果 Vue 实例在实例化时没有收到 el 选项，它就处于“未挂载”状态，没有关联的 DOM 元素，可以使用`$mount()`手动挂载一个未挂载的实例。这个方法返回实例自身，因而可以链式调用其他实例方法。

```html
<div id="mount-div"></div>
<script>
	var MyComponent = Vue.extend({ //Vue.extend是基础构造器，参数是一个包含组件选项的对象
		template:'<div>hello:{{ name }}</div>',
		// 没el:'' 表示没挂载
		data:function(){
			return { name : 'Aresn'}
		}
	})
	new MyComponent().$mount('#mount-div')  //用$mount()手动挂载一个未挂载的实例
	// 还可以这么写：
	// new MyComponent({
	// 	el:'#mount-div'
	// })
	// 或者在文档之外渲染并且随后挂载
	// var com = new MyComponent().$mount();
	// document.getElementById('mount-div').appendChild(com.$el)
</script>
```



## 7.7 实战：两个常用组件的开发

开发一个数字输入框组件。数字输入框是对普通输入框的扩展，用来快捷输入一个标准的数字。数字输入框只能输入数字，而且又两个快捷按钮，可以直接减1或加1。除此之外，还可以设置初始值、最大值、最小值，在数值改变时，触发一个自定义事件来通知父组件。

所有的组件配置都在组件里定义，先在 template 里定义了组件的根节点，因为是独立组件，应该对每个 prop 进行校验。这里根据需求有最大值、最小值和默认值，有3个 prop：max 和 min 都是数字类型，默认值是正无穷大和负无穷大，value 也是数字类型，默认值是0.

父组件的 value 是一个关键的绑定值，所以用了 v-model 这样优雅的方式实现双向绑定，也让 API 看起来很合理。大多数表单类组件都应该有一个 v-model，比如输入框、单选框、多选框、下拉选择器等。

Vue 组件时单向数据流，所以无法从组件内部直接修改 prop、value 的值，解决办法是给组件声明一个 data，默认引用 value 的值，然后在组件内部维护这个 data。

这只解决了初始化时引用父组件 value 的问题，但是如果从父组件修改了 value 值，子组件的值也要一起更新。为了实现这个功能，需要使用 watch 选项来监听某个 prop 或 data 的变化。

练习1：在输入框聚焦时，增加对键盘上下按键的支持，相当于加1和减1.

练习2：增加一个控制步伐的prop——step，比如设置为10，点击加号按钮，一次增加10.

开发一个标签页组件。每个标签页的主题内容肯定是由使用组件的父级控制的，所以这部分是一个 slot，而且 slot 的数量决定了标签切换按钮的数量。同时我们还定义一个子组件 pane，嵌套在标签页组件 tabs 里，我们的业务代码都放在 pane 的 slot 内，而3个 pane 组件作为整体成为 tabs 的 slot。

# 第八章 自定义指令

自定义指令的注册方式和组件很像，也分为全局注册和局部注册，一个指令定义对象可以提供如下几个钩子函数 (均为可选)：有blid、inserted、update、componentUpdated、unbind五种。其中钩子函数有四个参数，el、binding、vnode和oldVnode，binding是一个对象，包含的属性有name、value、oldValue、expression、arg、modifiers六个。

如果指令需要多个值，可以传入一个JavaScript对象字面量。记住，指令函数能够接受所有合法的 JavaScript 表达式

# 第九章 Render函数

Vue.js 2.x与Vue.js1.x最大的区别就在于前者使用了Virtual Dom（虚拟DOM）来更新节点，提升渲染性能。虚拟DOM并不是真正意义上的DOM，而是一个轻量级的JavaScript对象，在状态发生变化时，虚拟DOM会进行Diff运算，来更新只需要被替换的DOM，而不是全部重绘，与DOM操作相比，开销会小很多。
vNode对象通过一些特定的选项描述了真实的DOM结构，每个DOM元素或组件都对应一个VNode对象，VNode主要可以分为如下几类，TextVNode文本节点、ElementVnode普通元素节点、ComponentVnode组件节点、EmptyVNode没有内容的注释节点、CLoneVNode克隆节点，可以是以上任意类型的节点，唯一的区别在于isCloned属性为true

Render函数通过createElement参数来创建Virtual Dom，结构精简了很多。slot的使用场景也是在Render函数中的。createElement函数生成模板，它有3个参数。第一个参数是必选的，可以是一个HTML标签字符串，也可以是一个组件或函数；第二个是可选参数，包含模板相关属性的数据对象，在template中使用，第三个是子节点VNodes，由creatElement()构建而成，也是可选参数。

在以往的template里，我们都是在组件的标签上v-bind:class/v-on:click这样的指令，在Render函数都将其写在了数据对象里。

约束，组件树中的所有VNodes必须是唯一的。如果要重复很多次的元素/组件，可以使用工厂函数来实现。对于含有组件的slot，复用就要复杂一点，需要将slot的每个子节点都要克隆一份。

在Render函数中，不再需要Vue内置的指令，比如v-for、v-if，当然，也没有办法使用它们，无论要实现什么功能，都可以用原生JavaScript。解决办法，if、for用在父组件渲染中而不是模板界面，v-model的替代就是用prop：value和event：input组合使用的一个语法糖，也能在父组件渲染中实现逻辑功能。对于事件修饰符和按键修饰符，基本也需要自己实现，但事件修饰符.capture和.once，Vue提供了特殊的前缀，可以直接写在on的配置里。

函数化组件，Vue.js提供了一个functional的布尔值选项，设置为true可以使组件无状态和无实例，也就是data和this上下文，这样用Render函数返回虚拟节点可以更容易渲染，因为函数化组件是一个函数，渲染开销要小很多。使用函数化组件时，Render函数提供了第二个参数context来提供临时上下文，为了弥补缺少的实例。组件需要的东西都是通过这个上下文来传递的。在添加functional:true之后，锚点标题组件的render函数之间简单更新增加context参数，this.$slots.default更新为context.children，之后this.level更新为context.props.level

JSC是简化的模板，让Render函数更好地书写和阅读，Vue.js提供了插件babel-plugin-transform-vue-jsx来支持JSX语法，它看起来像HTML，但实际是JavaScript的语法扩展，用接近DOM结构的形式来描述一个组件的UI和状态信息。

# 第十章 使用webpack

## 10.1 前端工程化与webpack

webpack的主要使用场景是单页面富应用（SPA，通常是由一个 html 文件和一堆按需加载的 js 组成，它的 html 结构可能会非常简单，所有代码都集成在神奇的 main.js 文件中），将图片/CSS/字体打包成模块，从而处理模块间的依赖关系。

举个简单的例子，平时加载 CSS 大多通过`<link>`标签引入 CSS 文件，而在 webpack 里，直接在一个 .js 文件中导入。import 是 ES2015的语法，这里也可以写成`require('src/styles/index.css')`。在打包时，index.css 会被打包进一个 js 文件里，通过动态创建`<style>`的形式来加载 css 样式，当然也可以进一步配置，在打包编译时把所有的 css 都提取出来，生成一个 css 的文件。

```javascript
import 'src/styles/index.css'
```



export 和 import 是用来导出和导入模块的，一个模块就是一个 js 文件，拥有独立的作用域,里面定义的变量外部是无法获取的。模块导出后，在需要使用模块的文件使用 import 再导入，就可以在这个文件内使用这些模块了。

导入的模块名称都是在 export 的文件中设置的，用户必须预先知道这个名称，如果想自定义名称，则使用 export default 来输出默认的模块。

```javascript
export default {
	version: '1.0.0'
};

export default function (a, b) {
	return a+b;
};

import conf from './config.js'
import Add from './add.js'

console.log(conf);  // { version: '1.0.0'}
console.log(Add(1, 1));  // 2
```




如果使用 npm 安装了一些库，在 webpack 中可以直接导入。

```javascript
import vue from 'vue';
import $ from 'jquery'
```



## 10.2 webpack基础配置

创建目录。使用`npm init`使用 NPM 初始化配置。执行后，会有一系列选项，可以按回车键快速确认，完成后会在 demo 目录生成一个 package.json 的文件。

本地局部安装。使用`npm install webpack --save-dev`安装 webpack，选项会将这作为开发依赖来安装 webpack。安装完成后，在 package.json 中会多一项配置。

接着需要安装 webpack-dev-server，它可以在开发环境中提供很多服务，比如启动一个服务器、热更新、接口代理等，配置起来也很简单。

归根到底，webpack 就是一个 .js 配置文件，架构好或差都体现在这个配置里，随着需求的不断出现，工程配置也是逐渐完善。

在 package.json 的 script 里增加一个快速启动 webpack-dev-server 服务的脚本，当运行`npm run dev`命令时，就会执行对应的命令，其中--config 是指向 webpack-dev-server 读取的配置文件路径，--open 会在执行命令时自动在浏览器打开页面，默认地址是 127.0.0.1:8080，不过 IP 和端口号都是可以配置的。

```javascript
"scripts": {
	 "dev": "webpack-dev-server —-host 172.172.172.1 —-port 8888 —-open --config webpack.config.js",
},
```



webpack 配置中最重要也是必选的两项就是入口 Enrty 和出口 Output，入门的作用是告诉 webpack 从哪里开始寻找依赖，并且编译，出口则用来配置编译后的文件存储位置和文件名。entry 中的 js 文件就是我们配置的单入口，webpack 会从这个文件开始工作。output 中的 path 用来存放打包后文件的输出目录，是必填项，publicPath 指定资源文件引用的目录，filename 用于指定输出文件的名称。

```JavaScript
var path = require('path');
var config = {
		entry: {
				app: './main'
		},
		output: {
				path: path.join(__dirname, './dist'),
				publicPath: '/dist/',
				filename: 'main.js',
		},
};
module.exports = config;
```



在 webpack 的世界里，每个文件都是一个模块，比如 css、js、html、less等。对于不同的模块，需要用不同的加载器 Loaders 来处理，而加载器就是 webpack 的最重要的功能，通过安装不同的加载器可以对各种后缀名的文件进行处理，比如说要写 CSS 样式，就要用到 style-load 和 css-loader。

在 module 对象的 rules 属性中可以指定一系列的 loads，其包含 test 和 use 两个选项，从而让 webpack 在编译过程中遇到 require () 和import 语句导入一个后缀名为 css 的文件时，先转换。use 选项的值可以是数组或字符串，如果是数组，编译顺序就是从后往前。

```javascript
module: {
	rules: [
			{
					test:/\.css$/,
					use: [
							'style-loader',
							'css-loader'
					]
			}
	]
}
```

CSS 是通过 JavaScript 动态创建 (style) 标签来写入的，意味着代码已经编译在了 mian.js 里。但在实际业务中，项目大了样式会很多，都放在 JS 里太占面积，还不能做缓存，这时就要用到 webpack 最后一个重要的概念——插件。

webpack 的插件功能很强大而且还可以定制。通过插件，可以把散落在各地的 css 或提取出来，并生成一个 main.css 文件。最终在 index.html 里通过`<link>`的形式加载它。

## 10.3 单文件组件与vue-loader

在字符串模块 template 选项里拼写字符串 DOM 非常费劲，尤其是用"\"换行。Vue.js是一个渐进式的 JavaScript 框架，在使用 webpack 构建 Vue 项目时，可以使用一种新的构建模式：.vue单文件组件。

.vue 单文件组件就是一个后缀名为 .vue 的文件，是一种新的构建模式，在 webpack 中使用 vue-loader 就可以对 .vue 格式的文件进行处理，该文件一般包含三个部分，即<template>、<script>、<style>。分别是 HTML/CSS/JS/ 模板，如果在 style 标签上使用 scoped 属性则代表只在这个组件有效。也可以使用 CSS 预编译。使用 .vue 文件需要先安装 vue-loader、vue-style-loader 等加载器并做配置，因为要使用 ES6 语法，还需要安装 babel 和 babel-loader 等加载器。在 demo 目录下有一个 .babelrc 的文件，是写入 babel 的配置，webpack 会依赖此配置文件来使用 babel 编译 ES6 代码。这样，每个 .vue 就代表一个组件，组件之间可以互相依赖。

.vue 的组件是没有名称的，在父组件使用时可以对它自定义。写好了组件，就可以在入口 main.js 中使用它了。

```javascript
import Vue from 'vue';
import App from './app/app.vue';

new Vue({
		el: '#app',
		render: h => h(App)  
})
```



ES6语法提示
=>是箭头函数，`render:h=>h(App)`等同于`render:function(h){return h(App)}`也等同于`render:h=>{return h(App)}`。但是，箭头函数里面的this指向与普通函数是不一样的，箭头函数体内的this对象就是定义时所在的对象，而不是使用时所在的对象。

```JavaScript
function Timer () {
	this.id = 1;
	var _this = this;
	setTimeout(function () {
		console.log(this.id);  // undefined
		console.log(_this.id);  // 1
	},1000);

	setTimeout(() => {
		console.log(this.id);  // 1
	},2000)
}

var timer = new Timer();
```

ES6语法提示
`components:{vTitle,vButton}`等同于`component:{vTitle:vTitle,vButton:vButton}`。总结来说就是对象字面量缩写，当对象的 key 和 value 名称一致时，可以缩写成一个。

## 10.4 用于生产环境

安装 url-loader 和 file-loader 来支持图片、字体等文件，其中“?limit=1024”是指如果这个文件小于1kb，就以 base64 的形式加载，不会生出一个文件。

单页面富应用技术，意味着最终只有一个 html 文件，其余都是静态资源，实际部署到生产环境时，一般都会将 html 挂在后端程序下，由后端路由渲染这个页面，将所有静态资源单独部署到CDN，当然也可以和够短程序部署在一起，这样就实现了前后端完全分离。静态资源在大部分场景都有缓存。

# 第十一章 插件

Vue.js 提供了插件机制，可以在全局添加一些功能。它们可以简单到几个方法、属性，也可以很复杂，比如一整套组件库。

注册插件需要一个公开的方法install，它的第一个参数是Vue构造器，第二个参数是一个可选的选项对象。然后通过Vue.use()来使用插件。绝大多数情况下，开发插件主要是通过 NPM 发布后给比人使用的，在自己的项目中可以直接在入门调用以上的方法，无需多一步注册和使用的步骤。

```javascript
MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }

  // 2. 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
    ...
  })

  // 3. 注入组件
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
    ...
  })

  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
}
```



## 11.1 前端路由与vue-router

路由通俗地将，就是网址，就是每次 GET 或者 POST 等请求在服务器端有一个专门的正则配置列表，然后匹配到具体的一条路径，分发到不同的 Controller，进行各种操作，最终将 html 或数据返回给前端，这就完成了一次 IO。

目前大多数的网站都是这种后端路由，也就是多页面的，这样可以让页面在服务器渲染好直接返回给浏览器，不用等待前端加载任何js和css就可以直接显示网页内容，再比如对SEO友好，缺点是后端必须维护和改写模板。

前后端分离的开发模式，后端只提供 API 返回数据，前端通过 Ajax 获取数据后，再用一定的方式渲染到页面里。缺点是首屏渲染需要时间来加载 css 和 js。SPA就是在前后端分离的基础上，加一层前端路由。

前端路由，即由前端来维护一个路由规则，实现有两种，一是利用url的hash，就是常说的描点（#），Javascript 通过 hashChange 事件来监听 url 的改变。另一种就是 HTML5 的 History 模式，它使 url 看起来像普通网站那样，以"/"分割，没有`#`，但页面并没有跳转，不过使用这种模式需要服务端支持，服务器在接收到所有的请求后，都指向同一个 html 文件，不然会出现404。

vue-router 路由不同的页面事实上就是动态加载不同的组件。每个页面对应一个组件，也就是对应一个 .vue 文件。在 main.js 里完成路由的剩下配置，创建一个数组来制定路由匹配列表，每个路由映射一个组件，component 是映射的路由。

ES6语法提示
使用 let 和 const 命令来声明变量，代替了 var。他们的作用域就是块。const 声明后不能再修改，let 则可以修改。

使用了异步路由后，编译出的每个页面的 js 都叫作块。，可通过设置里chunkFilename 字段修改 chunk 命名。

vue-router 有两种跳转页面的方法，第一种是使用内置的`<router-link>`组件，它会被渲染为一个`<a>`标签，to 选项就是一个 prop，指定需要跳转的路径，tag 可以指定渲染成什么标签，使用 replace 不会留下 History 记录，active-class 是跳转成功后给元素增加伪元素。

第二种跳转方法，通过 JavaScript 进行，使用 router 实例的方法。通过点击事件触发`$router.push`方法。`$router`还有 replace 和 go 等方法。

vue-router 提供了导航钩子 beforeEach 和 afterEach，它们会在路由即将改变前和改变后触发。导航钩子有三个参数，to 表示进入的路由对象，from 表示即将离开的路由对象，next 表示调用该方法才能进入下一个钩子。

Vuex 所解决的问题与 bus 类似，它作为 Vue 的一个插件来使用，可以更好地管理和维护整个项目的组件问题。使用 Vuex 会有一定的门槛和复杂度，它的主要使用场景是大型单页应用。它的用法与 vue-router 类似，在 main.js 里，通过 Vue.use() 使用 Vuex。仓库 store 包含了应用的数据（状态）和操作过程，任何组件使用同一 store 的数据发生变化时，对应的组件也会立刻更新。可以通过`$store.state`变量名进行读取。在组件内，来自 store 的数据只能读取，不能手动改变，改变 store 中数据的唯一途径就是显式地提交 mutations。mutations 是 Vuex 的第二个选项，用来直接修改 state 里的数据。在组件内，通过`this.$store.commite`方法来执行 mutations。第二种方法是直接使用包含 type 属性的对象。mutation 里尽量不要异步操作数据。

ES6语法提示
`increment(state,n=1)`等同于`increment(state,n){n=n||1}`

Vuex 还有其他三个选项可以使用，getters、actions、modules。第一个就是用来依赖组件的计算属性；第二个是异步操作业务逻辑，在组件内通过`$store.dispatch`触发；最后一个可以将 store 分割到不同模块，每个 module 拥有自己的 state、getters、mutations、actions，而且可以多层嵌套，在 actions 和 getters 中还可以接受一个参数 rootState 来访问根节点的状态。

ES6语法提示
Promise 是一种异步方案，它有三种状态：Pending（进行中）、Resolve（已完成）、Rejected（已失败）

ES6语法提示
emit(event,...args) 中的 ...args 是函数参数的结构，使用它可以从当前参数（这里是第二个）到最后的参数都获取到。

# 第十二章 iView经典组件解剖

IView是一套基于Vue.js2的开源UI组件库，主要服务于PC界面的中后台产品，深度封装了40多个常用业务组件，比如Input、Checkbox、Select、Table；同时也是一整套的前端解决方案，包括设计规范、基础样式，支持服务端渲染（SSR），同时提供了可视化脚手架，方便快速构建项目工程。

级联选择组件Cascader。接受一个prop:data作为选择面板的数据源，使用v-model可以双向绑定当前选择的项。data中的label是面板显示的内容，value是它对应的值，children是它的子集，可递归。v-model绑定一个数组，每一项对应data里的value。Cascader的核心是用到了组件·递归，使用组件递归必不可少的两个条件是由name选项和在适当的时候结束递归。iView作为独立组件，无法使用bus和vuex，为了实现跨组件通信，iView摸你了Vue1的dispatch和boradcast方法。

折叠面板组件Collapse。组件分为两部分：collapse.vue和panel.vue，前者作为组件容器，接收一个整体的slot，而slot就是由后者组成，并且可以进行折叠面板的嵌套。collage支持v-model来双向绑定当前激活的面板，判断激活的依据是panel的prop：name。

iView内置工具函数。findComponentUpward方法以当前实例为参照点，向上寻找出指定name或几个name中的一个组件实例，找到后立即返回该实例。除了实用的工具函数外，iView内置的自定义指令、混合也可以直接使用。

> 精通CSS：高级Web标准解决方案 第三版   
> [源码](https://github.com/GZ0759/css-mastery-16)

# 第一章 基础知识

## 1.1 组织代码

可维护性。相对来说，CSS 是随着代码量增加而最难保持可维护性的语言之一。

HTML 简史。CSS的初衷是把跟 HTML 混在一起的表现性标记提取出来，使其自成体系，达到结构与表现分离的目的。

渐进增强。平衡向后兼容性与最新的 HTML 和 CSS 特性。

## 1.2 创建结构化、语义化富HTML

语义化标记是优秀 HTML 文档的基础。语义就是以系统方式表示的含义。语义化标记意味着在正确的地方使用正确的元素。

ID 和 class 属性。为元素添加 ID 和 class 属性不一定能给文档增加含义或结构，这两个属性只是一种让其他因素来操作与解析文档的通用手段。CSS 也可以利用这一手段。

HTML5 新增了一批结构化元素。增加这些新元素是为了在 HTML 文档中创建逻辑性区块。除了 main 之外，所有其他新元素都可以在一个文档中多次出现，以便让机器和人更好地理解文档。
- section
- header
- footer
- nav
- article
- aside
- main

div 和 span。在没有合适的语义元素的情况下，div 仍然是给内容分组的一个不错的选择。额外添加的无语义 div 元素对保证代码的清晰和可维护性非常重要。

重新定义的表现性文本元素。今天，`<i>`元素用于下标识与周围内容不一样的内容，一般在排版上会显示为斜体。`<b>`元素的含义和`<i>`几乎一样，只是针对习惯上标记为粗体的内容。这两个元素与`<em>`及`<strong>`的区别在于：它们没有任何强调自己所包含内容的意味。多数情况下，应该选择`<em>`和`<strong>`，因为它们是用来强调及重点强调内容的语义正确的选择。

扩展 HTML 语义。

很多新的 HTML5 元素都考虑到了无障碍访问的场景。比如，如果屏幕阅读器能够理解页面中的 nav 元素，那么它就可以利用这个元素帮助用户定位到相应的内容，或者在必要时返回导航。另一种实现这个目标的方式是利用无障碍富因特网应用（ARIA，accessible rich Internet application），它是对 HTML 规范的补充。ARIA 为我们提供了针对辅助访问设备添加更多语义的手段，方式就是为文档中的不同元素指定其包含什么内容，或者说他们提供什么功能。ARIA 还支持让开发人员指定更复杂的内容片段和界面元素。

微格式。微格式是一组标准的命名约定和标记模式，可用于表示特定的数据类型。

微数据。微数据是跟 HTML5 一起，作为给 HTML 添加结构化数据的另一种方式而推出的。它的目标与微数据非常相似，但在把微数据嵌入内容方面则有所不同。

# 第二章 添加样式

## 2.1 CSS选择符

类型与后代选择符是最基本的选择符。类型应用于选择特定类型的元素，只要写出想要添加样式的元素名即可。类型选择符有时候也被称为元素选择符。后代选择符用于选择某个或某组元素的后代。后代选择符的写法是在两个选择符之间添加空格。

```css
p {
  color: black;
}

blockquote p {
  padding-left: 2em;
}
```

要想更精准地选择目标元素，可以使用 ID 选择符和类选择符。顾名思义，这两个选择符通过对应 ID 和 class 属性的值来选择元素。ID 选择符由井号（#）开通，类选择符由句点（.）开通。

```css
#intro {
  font-weight: bold;
}

.date-posted {
  color: #ccc;
}
```

子选择符与同辈选择符。CSS 提供了高级选择符。第一个高级选择符叫子选择符。与后代选择符会选择一个元素的所有后代不同，子选择符只选择一个元素的直接后代，也就是子元素。有时候可能需要为某个元素相邻的元素添加样式。使用相邻同辈选择符，可以选择位于某个元素后面，并与该元素拥有共同父元素的元素。“>”、“+”在这里被称为组合子，还有一个类似的组合子，那就是一般同辈组合子：`~`，使用一般同辈组合子可以选择指定元素后面的所有相同元素。

注意：相邻同辈选择符和一般同辈选择符都不会选择前面的同辈元素，这主要是跟网页渲染性能有关。通常情况下，浏览器会按照元素在页面中出现的先后顺序给它们应用样式。如果有向前同辈组合子，浏览器就必须先记住相应的选择符，然后对文档进行多轮处理才能彻底应用样式。

```css
#nav > li {
  background: url(folder.png) no-repeat left top;
  padding-left: 20px;
}

h2 + p {
  font-size: 1.4em;
  font-weight: bold;
  color: #777;
}
```

通用选择符。通用选择符可以匹配任何元素，与其他语言中的通配符类似，通用选择符也使用星号（*）表示，以表示可以匹配页面中的所有元素。

```css
* {
  padding: 0;
  margin: 0;
}
```

属性选择符。属性选择符基于元素是否有某个属性或者属性是否有某个值来选择元素。有时候，关心的是属性值是否匹配某个模式，而非某个特定值，通过给属性选择符中的等号前面加上特殊字符，就可以表达出想要匹配的值的形式了。

```css
/* 匹配是否存在某个属性 */
abbr[title] {
border-bottom: 1px dotted #999;
}
 
 /* 匹配特定的属性值 */
input[type="submit"] {
cursor: pointer;
}

/* 匹配以字符开头的属性值 */
a[href^="http:"] 

/* 匹配以字符结尾的属性值 */
img[src$=".jpg"]

/* 匹配包含字符的属性值 */
a[href*="/about/"]

/* 匹配空格分隔的字符串中的属性值 */
a[rel~=next]

/* 匹配开头是特定值或指定值后连着一个短划线 */
a[hreflang|=en]

```

伪元素。有时候想选择的页面区域不是通过元素来表示的，而我们也不想为此给页面添加额外的标记。CSS 为这种情况提供了一些特殊选择符，叫做伪元素。伪元素一般使用双冒号语法，但是一些旧版本浏览器在实现伪元素时支持的是但冒号语法。

```css
.chapter::before {
  content: '”';
  font-size: 15em;
}
```

伪类。有时候想基于文档结构以外的情形来为页面添加样式，比如基于超链接或表单元素的状态。这时候可以使用伪类选择符。伪类选择符的语法是以一个冒号开头，用于选择元素的特定状态或关系。

```css
/* makes all unvisited links blue */
a:link {
color: blue;
}
/* makes all visited links green */
a:visited {
color: green;
}
/* makes links red on mouse hover, keyboard focus */
a:hover,
a:focus {
color: red;
}
/*...and purple when activated. */
a:active {
color: purple;
}
```

结构化伪类。使用结构化伪类能够做很多事，因为在文档中基于相对位置精确选择元素时，它们提供了方便。CSS3 新增了一大批与文档结构有关的伪类。其中最常用的是 nth-child 选择符，可以用来交替地为表格应用样式。nth-last-child 选择符与 nth-child 选择符类似，只不过是从最后一个元素倒序计算。CSS2.1 中有一个选择第一个子元素的伪元素，叫`:first-child`，相当于直观版的`:nth-child(1)`。CSS3 选择符规范又添加了一个选择最后一个子元素的伪类`:last-child`，对应于`nth-last-child(1)`。此外，还有`:only-child`和选择特定类型的唯一子元素`only-of-type`。

表单伪类。还有很多伪类专门用于选择表单元素，这些伪类根据用户与表单控件交互的方式，来反映表单控件的某种状态。

## 2.2 层叠

稍微复杂的样式表中都可能存在两条甚至多条规则同时选择一个元素的情况。CSS 通过一种叫层叠的机制来处理这种冲突。层叠机制的原理是为规则赋予不同的重要程度。同时允许用户使用`!important`标注来覆盖规则，主要是处于无障碍交互的需要。

```css
p {
font-size: 1.5em !important;
color: #666 !important;
}
```

归纳起来，层叠机制的重要性级别从高到低如下所示。

- 标注为`!important`的用户样式
- 标注为`!important`的作者样式
- 作者样式
- 用户样式
- 浏览器或用户代码的默认样式
- 选择符的特殊性

## 2.3 特殊性

为量化规则的特殊性，每种选择符都对应着一个数值。这样，一条规则的特殊性就表示为其每个选择符的累加数值。累加计算是基于位置累加，避免出现高特殊性选择符被一堆低特殊性选择符的累加值所覆盖。

注意：通用选择符（*）的特殊性为0，无论它在规则声明中出现多少次。

利用层叠次序。如果两条规则的特殊性相等，则优先应用后定义的规则。

控制特殊性。理解特殊性是写好 CSS 的关键，而控制特殊性则是大型网站开发总最难处理的问题。利用特殊性，可以先为公用元素设置默认样式，然后再更特殊的元素上覆盖这些样式。

特殊性与调试。任何一款现代浏览器都有内置的开发者工具，都非常清楚地显示应用给特殊元素的样式来自哪条规则。

## 2.4 继承

有些属性，像颜色或字体大小，会被应用它们的元素的后代所继承。继承是很有用的机制，有了它就可以避免给一个元素的所有后代重复应用相同的样式。但是继承的属性值时没有任何特殊性的，连0都说不上，这意味着使用特殊性为0的通用选择符设置的样式都可以覆盖继承的样式。合理利用继承有助于减少选择符的数量，降低复杂度，不同如果有大量的元素继承了各种不同的样式，那么查找样式的来源也会比较麻烦。

## 2.5 为文档应用样式

首先可以把样式放在 style 元素中，直接放在文档的 head 部分。如果样式不多，不愿意因为浏览器额外下载文件而耽误时间，那么就可以使用这种方法。如果样式在外部样式表中，那么有两种方法把它们挂接到网页上，最常用的方式是使用 link 元素，还有一种方法是使用`@import`指令加载外部 CSS 文件。

```html
<!-- 直接放置style -->
<style>
body {
  font-family: Avenir Next, SegoeUI, sans-serif;
  color: grey;
}
</style>

<!-- 使用link加载 -->
<link href="/c/base.css" rel="stylesheet" />


<!-- 使用@import加载 -->
<style>
@import url("/c/modules.css");
</style>
```

性能。选择以什么方式把 CSS 加载到页面中，决定了浏览器显示页面的速度。度量 Web 性能的一个重要指标就是网页内容实际显示在屏幕上需要多久。这个指标有时候也叫“渲染时间”或“上屏时间”。

- 减少 HTTP 请求

线上网页最好把需要加载的 CSS 文件数量控制在1或2个。只用一个 link 元素加载 CSS 文件，然后在其中使用`@import`，并不鞥呢把请求控制为1个，因为这意味着先需要1个请求下载链接的文件，此外还要发送额外的请求取得所有导入的文件。因此，在线上网页中尽量不要使用`@import`。

- 压缩和缓存内容

使用 GZIP 压缩线上资源也非常重要。CSS 压缩的比率很高，因为它的很多属性和值都是重复的。类似的，让 Web 服务器帮助设置一定的 CSS 文件缓存时间也很重要。

- 不让浏览器渲染阻塞 JavaScript

如果在 HTML 文档的`<head>`元素加入了`<script>`元素，浏览器必须先把链接的内容下载下来，然后再向用户显示网页内容。这种情况下的 HTML 和 CSS 解析完全被下载以及执行脚本阻断了，也就是所谓的“渲染阻塞”。渲染阻塞明显会拖慢网站加载速度。为此，主流的做法是在 HTML 页面底部的结束标签`</body>`之前加载 JavaScript。比较现代的做法是在`<head>`中使用`<script>`标签时添加 async 或 defer 属性。

```html
<head>
  <!-- will load asynchronously, but execute immediately when downloaded -->
  <script src="/scripts/core.js" async></script>
  <!-- will load asynchronously, but execute after HTML is done-->
  <script src="/scripts/deferred.js" defer></script>
</head>
```

# 第三章 可视化格式模型

## 3.1 盒模型

盒模型是 CSS 的核心概念，描述了元素如何显示，以及（在一定程度上）如何相互作用、相互影响。页面中的所有元素都被看作一个矩形盒子，这个盒子包含元素的内容、内边距、边框和外边距。

内边距（padding）是内容区周围的控件。给元素应用的背景会作用于元素内容和内边距。因此，内边距通常用于分割内容，使其不致于散布到背景的边界。   
边框（border）会在内边距外侧增加一条框线，这条框线可以是实线、虚线或点划线。   
外框的外侧是外边距（margin）。外边距是围绕在盒子可见部分之外的透明区域，用于在页面中控制元素之间的距离。   

有一个与边框类似的属性，即轮廓线（outline）。这个属性可以在边框盒子外围画出一条线，但这条线不影响盒子的布局，也就是不会影响盒子的宽度和高度。因此，outline 常用于调试复杂布局，或者演示布局效果。

盒子大小。默认情况下，元素盒子的 width 和 height 属性指的是内容盒子，也就是元素可渲染内容区的宽度和高度。这时候添加边距和内边距并不会影响内容盒子的大小，但会导致整个元素盒子变大。通过修改 box-sizing 属性可以改变计算盒子大小的方式。box-sizing 的默认值为 content-box，即内容区。

最大值和最小值。给元素应用 min-height 和 max-height 值很有用。因为这样一来，块级盒子就可以默认自动填充父元素的宽度，但不会收缩到比 min-width 指定的值更窄，或者扩展到比 max-width 指定的值更宽。

## 3.2 可见格式化模型

p、h1 和 article 这些元素是块级元素，作为元素显示为内容块或块级盒子的形式，相对而言，strong、span 和 time 被称为行内元素，因为它们的内容会以行内盒子的形式显示在行内。可以使用 display 属性改变生成的盒子类型。例如通过把 display 属性设置为 block，让 span 变得跟块级元素一样。如果把 display 属性设置为 none，还可以让浏览器不为相应的元素生成盒子。如果不生成盒子，那么元素及其包含的内容就不会显示出来，也不会占用文档中的空间。

CSS 中有几种不同的定位模型，包括浮动、绝对定位和相对定位，除非特别指定，否则所有元素盒子都会在常规文档流中生成，即 position 属性的默认值为 static。常规文档流中元素盒子的位置，由元素在 HTML 中的位置决定。但行内盒子的高度不受其垂直方向上的内边距、边框和外边距的影响。因此，给行内盒子明确设置高度和宽度也不会起作用。

行内盒子是指沿文本流水平排列，也会随文本换行而换行。它们之间的水平间距可以通过水平方向的内边距、边框和外边距来调节。

由一行文本形成的水平盒子叫行盒子（line-box），而行盒子的高度由所包含的行内盒子决定。修改行盒子大小的唯一途径就是修改行高（line-height），或者给它内部的行内盒子设置水平方向的边框、内边距和外边距。

当然，也可以把元素的 display 属性设置为 inline-block。这样设置之后，该元素就会像一个行内盒子一样水平排列。但这个盒子的内部仍然像块级元素一样，能够设置宽度、高度、垂直外边距和内边距。

匿名盒子。HTML 元素可以嵌套，元素盒子当然也可以嵌套。多数盒子都是基于明确定义的元素生成的，不过有一种情况，就算不明确定义也会生成块级盒子。比如下面例子中在 section 这个块级元素的开头加入一些文本。此时，“some text”就算没有定义为块级元素，也会被当成块级元素。这种情况下，这个盒子被称为`匿名块盒子`，因为这个盒子并不与任何特定的元素相关。类似的情况还存在于块级元素内部的文本级行盒子。假设有一个段落中包含三行文本（three lines of text），这三行文本的每一行都构成了一个`匿名行盒子`。除了使用`:first-line`伪元素来添加有限的排班和颜色相关的样式之外，不能直接给`匿名块盒子`或`匿名行盒子`应用样式。

```html
<section>
  <!-- 匿名块盒子 -->
  some text
  <p>Some more text</p>
</section>
```

## 3.3 其他CSS布局模块

# 第四章 网页排版

## 4.1 CSS的基本排版技术

文本颜色（color）。默认情况下，浏览器会把绝大部分文本渲染为黑色。

字体族（font-family）。字体族属性的值是一个备选字体的列表，按优先级从左到右排列。字体列表最后的 serif 和 sans-serif 叫做通用字体族，此外还有 cursive、fantasy 和 monospace 等通用字体族。

字型大小（font-size）与行高（line-height）。几乎所有浏览器中的字型的默认大小都是16像素，除非用户修改过偏好设置。em单位用于字型大小属性时，实际上是一个相应元素继承的字型大小缩放因子。更灵活的方式则是使用 rem 单位，它始终是基于根元素的 em 大小缩放。长度单位还有 mm、cm、in 和 pt 等绝对物理长度，这些主要是给打印样式准备的。

行间距、对齐及行盒子的构造。每行文本都会生成一个行盒子，行盒子还可以进一步拆分成表示行内元素的行内盒子，或者连接两个行内元素的匿名行内盒子。行内盒子中的内容区显示文本，内容区的高度由 font-size 的测量尺度。小写字母“x”的上边界决定了所谓的“x高度”。字形会被摆放在内容区中，每个字形都会在垂直方向上不偏不倚，使得每个行内盒子的底边都默认对齐于靠近底部的共同水平线，这条线叫基线。最后，行高指的是行盒子的总高度，更通俗的叫法是行间距，排版术语叫铅空，就是排字员用来隔开字符行的铅块。除了 line-height，行内盒子也会受到 vertical-align 属性的影响。它的默认值是 baseline ，即子元素的基线与父元素的基线对齐。

文本粗细（font-weight）。可以不用给出变体的名字，而只使用关键字：normal、bold、bolder 和 lighter。也可以直接用数字值，都是 100 的整数倍，最大为 900.

字体样式（font-style）。设置字体样式会从字型中选择斜体显示，前提是存在这个变体，如果不存在，浏览器会通过倾斜体来模拟，但结果同样不会太理想。斜体通常用于表示强调，或者表达一种不同的语气。

大小写变换和小型大写变体。CSS 可以控制英文字母大小写，属性时 text-transform。除了 uppercase 这个值将字母显示为大写，还可以用 lowercase 把所有字母变成小写，用 capitalize 把每个单词的首字母变成大写。CSS 还有一个属性 font-variant，可以通过值 small-caps 把英文文本转换成所谓的“小型大写字母”。

控制字母和单词间距。首先是 word-spacing 属性，功能是控制词间距。类似的，可以通过 letter-spacing 属性来控制字符间的距离。对于小写英文字母的文本来说，人为控制字母间距不是好事。

## 4.2 版心宽度、律动和毛边

接下来讨论一个对阅读体验有着重大影响的因素：行长。用排版的行话说，就是版心宽度。要控制行长，可以通过设定包含文本的段落、标题等元素的宽度来实现。

文本缩进与对齐。默认情况下，文本时左对齐的，文本左对齐有助于眼睛找到下一行，保持阅读节奏。使用相邻组合设置 text-indent 属性可以设置行首缩进。段落的右边参差不齐，这种参差不齐的样式在排版上也有属于，叫做“毛边”（rag）。居中文本非常适合小型用户界面元素（如按钮）或短标题的布局，因为两端参差不齐会影响可读性。

text-align 属性可以接受下列任意一个关键字值：left、right、center 和 justify。CSS Text Level 3规范还额外定义了几个值，包括 start 和 end。这两个逻辑方向关键字与文本书写模式相对应。

连字符。要想使用自动连字符功能，需要保证两点。首先，在页面 html 元素中设置语言代码：`<html lang="en">`。其次，通过 CSS 将相关元素的 hyphens 属性值设为 auto。

多栏文本。为了有效利用资源，可以将文本分成多栏，并且对每栏的宽度加以限制。这里的 columns 属性时 column-count 和column-width 属性的简写形式。如果同时设置了 column-count 和 column-wi，则前者会作为最大栏数，后者会作为最小栏宽。

```css
article {
  max-width: 70em;
  columns: 20em;
  column-gap: 1.5em;
  margin: 0 auto;
}
```

可以让某些元素排列到栏内文本流之外，强制它们伸长以达到跨栏的效果`column-span: all`。

垂直律动与基线网络。在印刷设计中所有分栏的间距是统一的，正文文本都会缩进基线网络。

## 4.3 Web字体

`@font-face`规则。嵌入 Web 字体的关键是`@font-face`规则，通过它可以指定浏览器下载 Web 字体的服务器地址，以及如何在样式表中引用该字体。

```css
@font-face {
  font-family: Vollkorn;
  font-weight: bold;
  src: url("fonts/vollkorn/Vollkorn-Bold.woff") format('woff');
}
h1, h2, h3, h4, h5, h6 {
  font-family: Vollkorn, Georgia, serif;
  font-weight: bold;
}
```

为了解决旧版本浏览器对于字体格式支持的不一致问题，可以在`@font-face`规则中声明多个 src 值，包括`format()`提示。然后，由浏览器来决定到底使用哪种格式。

```css
@font-face {
font-family: Vollkorn;
src: url('fonts/Vollkorn-Regular.eot#?ie') format('embedded-opentype'),
     url('fonts/Vollkorn-Regular.woff2') format('woff2'),
     url('fonts/Vollkorn-Regular.woff') format('woff'),
     url('fonts/Vollkorn-Regular.ttf') format('truetype'),
     url('fonts/Vollkorn-Regular.svg') format('svg');
}
```

字体描述符。字体描述符不是属性，不会改变字体，它们的值只是为了告诉浏览器在什么情况下可以触发使用这个特定字体文件。`@font-face`规则可以接受几个声明，多数是可选的。最常见的列举如下：
- font-family: 必需，字体族的名称。
- src: 必需，url或url列表，用于下载字体。
- font-weight: 可选的字体粗细，默认值为normal。
- font-style: 可选的字体样式，默认值为normal。

Web 字体给网页设计带来了很大的飞跃，但同时也给网页中的实际应用嗲来了一些麻烦。浏览器需要下载额外的字体文件，演唱用户等待的时间。同时，浏览器在渲染这些字体时也会有一些问题。

在下载 Web 字体的时候，浏览器会有两种方式处理相应的文本内容。第一种方式是在字体下完完成钱暂缓显示文本。第二种方式是在字体下载完成前，浏览器先用一种后辈字体显示内容。

## 4.4 高级排版特性

微软和 Adobe 在20世纪90年代开发了 OpenType 字体格式，支持在字体文件中包含字体的额外设定和特性，可以在多数浏览器中显示字距调整、连字、替代数字，以及饰线等装饰性比划。

## 4.5 文本特效

合理使用文本阴影。CSS 的 text-shadow 属性可以用来给文本绘制阴影。对标题或短文本应用阴影，非常适合模拟凸版印刷或者喷涂效果。text-shadow 属性的语法非常直观，需要指定相对于源文本 x 轴和 y 轴的偏移量（可正可负）、模糊距离和颜色值，由空格分隔。除此之外，还可以通过用逗号分隔来给文本添加多组阴影，多组阴影会按先后顺序堆叠。

# 第五章 漂亮的盒子

## 5.1 背景颜色

背景颜色。第二个属性 background 是一个简写属性，通过它可以一次性地设置与背景相关的多个属性，使用这个属性的同时设置了除背景颜色之外的其它值，只不过把其他属性重置为默认值而已。

```css
body {
  background-color: #bada55;
}
/* We could also set the background color using the shorter background property: */
body {
  background: #bada55;
}
```

颜色值与不透明度。可以使用十六进制表示指定的颜色。如果三组数字中每组的两位数字相同，可以简写成三位数字，比如`#aabbcc`可以简写为`#abc`。颜色值也可以用预定义的关键字表示，比如 red、black、teal、goldenrod 或 darkseagreen。

RGB 值可以用另一种方式表示，即`rgb()`函数式表示法。最近，CSS 规范又提供了新的表示颜色的方法：`hsl()`、`rgba()`、`hsla()`。

其中，末尾的 a 表示 alpha，是用于控制透明度的阿尔法通道，用以设置颜色的透明程度。除此之外，CSS 也提供另一种方式来控制透明度，那就是 opacity 属性。前者只让背景颜色变得透明，后者让整个元素都变透明了，包括元素中包含的内容。使用 opacity 把一个元素设置为透明后，将无法再让其子元素变得不那么透明。

## 5.2 背景图片

如果图片从页面中去掉之后，网页本身仍然有意义的，那么该图片就可以当作背景图片。否则，该图片就是内容图片，使用一个 img 元素向页面中插入图片。

简单的背景图片示例。首先设置一个灰蓝色的背景图片颜色，再添加一个背景图片。添加默认背景颜色很重要，以防图片加载失败。背景图片的另一个相关属性 background-repeat 的默认值是 repeat，意思是背景图片要沿着 x 轴和 y 轴重复。这个特性对花纹图案的背景图片非常有用，但对照片可能就不合适了。

```css
.profile-box {
  width: 100%;
  height: 600px;
  background-color: #8Da9cf;
  background-image: url(img/big-cat.jpg);
  background-repeat: no-repeat;
}
```

加载图片。使用`url()`函数式表示法时，可以使用相对路径，如果路径以一个斜杠开头，如`/img/cat.jpg`，则浏览器会在相对于 CSS 文件所在域的顶级目录的 img 子目录下寻找图片。这里也可以使用绝对路径，也可以在加载图片时不指向文件，二十载样式表中直接嵌入数据，这时候要用刀数据URI（data URI），数据URI的值是文件中二进制编码的数据转换而来的长字符串。使用嵌入的数据URI有好处也有坏处，使用它主要是为了减少 HTTP 请求，但与此同时也会增加样式表提及，因此请慎重使用。

```css
.egg {
background-image:
url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAC gAAAAoAQAAAACkhYXAAAAAjElEQVR4AWP…
/* ...and so on, random (?) data for a long time.. */
...4DwIMtzFJs99p9xkOXfsddZ/hlhiY/AYib1vsSbdn+P9vf/1/hv8//oBIIICRz///
r3sPMqHsPcN9MLvn1s6SfIbbUWFl74HkdTB5rWw/w51nN8vzIbrgJDuI/PMTRP7+ByK//68HkeUg8v3//WjkWwj5G0R+
+w5WyV8P1gsxB2EmwhYAgeerNiRVNyEAAAAASUVORK5CYII=);
}
```

图片格式。下面除了 SVG 都是位图格式，位图意味着文件会包含每个像素的数据，拥有内在的维度（宽度和高度），对于细节丰富的图片，比如照片或详细示意图，位图很合适。但很多情况下，真正合适的是 SVG 图形，其文件包含的是如何在屏幕上绘制图形的指令。由于包含的是指令，SVG 土坪可以任意缩放，也可以在任意像素密度的屏幕上清晰呈现。换句话说，SVG 图形永远不会丢失，也不会出现“锯齿”。

- JPEG: 一种位图格式，有损压缩，压缩率越高，损失细节越多，适合照片。不支持透明度设置。
- PNG: 一种位图格式，无损压缩，不适合照片（因为文件会很大），适合图标、插图等小尺寸文件。支持阿尔法透明度设置。
- GIT: 早期的位图格式，与PNG类似，主要用于动图。严格来说，除动图外，GIT 基本已被 PNG 取代。实际上 PNG 也支持动图，只是浏览器支持落后。GIF支持透明度设置，但不支持阿尔法分极，因此边缘会有“锯齿”。
- SVG: 一种矢量图形格式，本身也是一种标记语言。SVG 可以直接嵌入到网页中，也可以作为资源引用；可以作为背景图，也可以作为内容图。
- WebP: 谷歌开的一种新图片格式，结合了 JPEG 的高压缩率和 PNG 的阿尔法透明特性。

## 5.3 背景图片语法

背景位置。背景图片的位置由 background-position 属性控制。该属性既可以使用关键字，也可以使用像素、em或百分比。最简单的情况下，可以只给两个值：一个表示相对于左侧的偏移量，一个表示相对于顶部的偏移量。

使用关键字来对齐背景图片，要在 x 轴上用 left、center 或 right，在 y 轴 上用 top、center 或 bottom。顺序一般都是先 x 轴后 y 轴。

背景图片定位这一限制一直是很多问题的来源。新语法允许给 background-position 添加外边空声明，先写边界关键字，再写长度值。

```css
.link-with-icon {
  padding-right: 2em;
  background-image: url(img/icon.png);
  background-repeat: no-repeat;
  background-position: right 1em top 50%;
}
```

使用另一个 CSS 特性，可以实现与前面示例相同的效果，但支持度可能更高一点，就是`calc()`函数式表示法。

```css
.link-with-icon {
  /* other properties omitted for brevity. */
  background-position: calc(100% - 1em) 50%;
}
```

背景裁剪与原点。默认情况下，背景图片是绘制在元素边框以内的。如果把背景图片定位到边框下方，而边框又被设置为半透明，那么图片边缘就会出现半透明的边框。使用 background-clip 属性可以改变这个行为，这个属性的默认值为`background-clip: border-box`，将其该我I padding-box 就可以把图片裁剪到内边距盒子以内。而 content-box 值则会把图片位于内边距及其之外的部分裁剪掉。

即使 background-clip 属性的值改变了，背景定位默认的远点仍然在代码中声明的内边距盒子（padding-box）的左上角。因此也可以使用 background-origin 属性来控制原点的位置，这个属性也接受盒模型相关的几个值：border-box、padding-box、content-box。

背景附着。背景会附着在指定元素的后面，如果滚动页面，那么背景也会随着元素移动而移动，可以通过 background-attachment 属性改变这种行为。除了 fixed 和默认值 scroll 外，还可以把 background-attachment 设为 local。

背景大小。如果在小屏幕上，图片会被剪切掉，如果屏幕特别大，那么元素边缘可能出现空白。要避免上述情况，不管页面如何缩放，都让内容保持自己的宽高比，就要使用 background-size 属性。

```css
.profile-box {
  background-size: 400px 240px;
}
```

要让图片随元素缩放而缩放，则必须使用百分比值。不过要注意，百分比值并不是相对于图片固有大小，而是相对于容器大小。更好的做法是只给一个维度设置百分比值，另一个维度设置关键字值 auto。

同时，可以把背景大小设置为 contain，这个值可以让浏览器尽可能保持图片最大化，同时不改变图片的宽高比。与前面的例子类似，但浏览器会自动决定哪一边使用 auto 值，哪一边使用100%。

然后，可以将背景大小设置为 cover，意思是图片会缩放以保证覆盖元素的每一个像素，同时不会变形。

背景属性简写。因为两个长度值即可以用于背景位置、又可以用于背景大小，所以两个都需要声明，而且首先声明背景位置，后声明背景大小，值之间以斜杠（/）分割。因为`*-box`关键字即可以用于背景原点，又可以用于背景裁剪，所以有如下规则。
- 如果只存在一个`*-box`关键字，则背景原点和背景裁剪都取这个关键字值。
- 如果存在两个`*-box`关键字，则第一个设置背景原点，第二个设置背景裁剪。

```css
.profile-box {
  /* background: background-position / background-size background-origin background-clip */
  background: url(img/cat.jpg) 50% 50% / cover no-repeat padding-box content-box #bada55;
}
```

## 5.4 多重背景

现在支持一个元素设置多个背景图片，每个背景属性也就有了相应的多值语法，多个值由逗号分隔。多重背景按声明的先后次序自上而下堆叠，最先声明的在最上面，最后声明的在最下面，背景颜色在所有背景图片下面。

```css
.multi-bg {
background-image: url(img/spades.png), url(img/hearts.png),
                  url(img/diamonds.png), url(clubs.png);
  background-position: left top, right top, left bottom, right bottom;
  background-repeat: no-repeat, no-repeat, no-repeat, no-repeat;
  background-color: pink;
}
```

## 5.5 边框与圆角

边框半径：圆角。圆角是一个锦上添花的效果，并不对可用性构成影响。因此，使用标准属性就好。给 border-radius 属性一个长度值，就可以一次性设置盒子四个角的半径。

```css
.box {
  border-radius: 0.5em;
}
/* 顺时针方向一次列出各个角 */
.box {
  border-radius: 0.5em 2em 0.5em 2em;
}
/* 把每个角设置成非对称的 */
.box {
  border-radius: 2em .5em 1em .5em / .5em 2em .5em 1em;
}
.box {
  border-radius: 2em 3em; /* repeated for bottom right and bottom left. */
}
```

创建正圆和胶囊形状。圆角半径可不仅可以使用长度值，还可以使用百分比值。在给 border-radius 指定百分比值时，x 轴和 y 轴分别相对于元素的宽度和高度来计算实际值。如果两个圆角的弧线香蕉，那么两个轴向就会分别缩小半径，知道圆弧不再相交。对于方形元素的对称圆角而言，任何大于50%的值都会得到圆形。而对于圆角半径相同的一个矩形元素而言，可能是一个椭圆形，因为圆角是按照高度或长度碧丽缩小的。

圆角弧线为保证不相交会自动缩小半径。因此，在创建胶囊两头的半圆形时，可以故意指定一个比所需半径大的值，以得到半圆形。

```css
.obrund {
  border-radius: 999em; /* arbitrarily very large length */
}
```

边框图片。border-image 属性支持把一张图片切成九块，浏览器会自动把每一块应用到指定的边框位置，中间的一块回被忽略，不过也可以改变这个行为。也可以告诉浏览器如何覆盖边框，可以拉伸、重复或补白。

## 5.6 盒阴影

CSS 属性 box-shadow 可以给元素添加阴影，而且这个属性浏览器基本都支持。头两个值表示 x 轴和 y 轴的便宜；第三个值表示模糊半径（阴影边界的模糊程度）；最后一个是颜色，使用`rgba()`。而且阴影的形状跟盒子的圆角也是一致的。

扩展半径：调整阴影大小。box-shadow 比 text-shadow 稍微灵活一点，比如可以在模糊半径后面再加一个值，表示扩展半径，用于扩展阴影的大小。这个值默认为0，即阴影与所属元素一样大。增大这个值，阴影相应增大，负值导致阴影缩小。

```css
.larger-shadow {
  box-shadow: 1em 1em .5em .5em rgba(0, 0, 0, 0.3);
}
.smaller-shadow {
  box-shadow: 1em 1em .5em -.5em rgba(0, 0, 0, 0.3);
}
```

box-shadow 的另一个比 text-shadow 更为灵活之处是可以使用 insert 关键字。这个关键字可以为元素应用内阴影，即把元素当成投影表面，可以创造一种背景被“镂空”的效果。

```css
.profile-box {
  box-shadow: inset 0 -.5em .5em rgba(0, 0, 0, 0.3);
}
```

多阴影。与 text-shadow 类似，也可以给一个元素应用多个阴影，以逗号分隔多组值。考虑到 border 只能给元素添加一个边框，可以利用这个技术给元素添加更多“边框”。通过给阴影一个值为0的模糊半径，就可以通过不同的扩展半径来生成多个类似边框的区域。由于阴影不影响布局，这个效果又类似 outline 属性。

```css
.profile-photo {
box-shadow: 0 0 0 10px #1C318D,
            0 0 0 20px #3955C7,
            0 0 0 30px #546DC7,
            0 0 0 40px #7284D8;
}
```

## 5.7 渐变

CSS 提供了一种绘制渐变图的机制，这个机制包括多种渐变方案，可以与任何接受图片的属性联合使用，包括 background-image。

线性渐变。使用`linear-gradient()`函数，沿一条假想线，从元素顶部到底部绘制一个渐变背景。渐变线的方向可以使用关键字 to，再加上一个表示边（top、right、bottom、left）或表示角（top left、top right、bottom left、botto right）的关键字来指定。此外，还可以使用 deg 单位指定渐变线的角度。线性渐变的默认方式是自上而下（to bottom），而0%和100%分别表示第一个和最后一个色标的位置，如果新增色标未指定位置，则在0%-100%范围内取均值。`background-image: linear-gradient(#cfdfee, #8da9cf);`

放射渐变。放射渐变从一个中心点开始向四周扩散，覆盖的范围可以是圆形或椭圆形。`background-image: radial-gradient(circle closest-corner at 20% 30%, #cfdfee, #2c56a1);`

重复渐变。重复渐变函数可以沿渐变直线（或射线）重复某个渐变色标组合，重复次数视其大小（由background-size决定）及允许的大小（元素大小）而定。`repeating-linear-gradient()`是重复的线性渐变，`repeating-radial-gradient()`是重复的放射渐变。

把渐变当作图案。渐变不一定需要很多像素来过渡，它可以是是突然的变化，从而形成锐利的线条或圆环。通过组合线、角、圆、椭圆等简单图形，就可以得到各种各样的图案。

虽然渐变可以替代外部图片，但其本身也可能影响性能，特别是在资源有限的设备上，比如手机，放射性渐变尽量少用为妙。

## 5.8 为嵌入图片和元素添加样式

文档中的图片与其他元素不同，它本身是有像素宽度和高度的，而且宽度和高度的比例固定。在可伸缩的设计中，元素宽度要随浏览器窗口宽度变化而变化，此时也需要 CSS 来控制图片及其他嵌入的元素。

可伸缩的图片模式。`max-width: 100%`意味着图片会随着包含它的容器缩小而缩小，但在容器变大时，它不会大到超过自身的固有尺寸。

控制对象大小的方法。有时候需要根据显示容器设置 img 或其他嵌入对象的大小，这时候可以使用一些最近标准化并被浏览器实现的新属性，比如使用 object-fit 属性，可以像使用 background-size 属性一样，保持元素的宽高比。object-fit 属性的默认值为 fill，意味着图片内容会在必要时拉伸以填充容器，因此可能破坏宽高比。cover、contain 则与 background-size 属性中对应的关键字作用一样，none 则会采用图片固有大小，不管容器多大。

可保持宽高比的容器。对于具有固定宽高比的位图，将高度设置为auto，只改变宽度或者把宽度设置为宽度，只改变高度，都是可以的。但如果是没有固定宽高比的元素，比如 iframe 和 Object 元素、某些情况下的 SVG 内容，就需要特殊处理，才能在可伸缩的同时保持固定宽高比。

减少图片大小。减少图片大小的第一部是优化图片。图片文件中经常包含一些元数据，它们对浏览器显示图片没有用处。

# 第六章 内容布局

## 6.1 使用定位

绝对定位使用案例。绝对定位一般用于遮盖、工具栏和对话盒子等放置在内容上面，这些定位元素可以配合 top、right、bottom、left 属性使用。

使用初始定位。对于下面的例子，每个评论是一个设置在每个提及的段落后面的旁边元素，为了让评论展示在段落最后的左边，需要使用绝对定位。当定位上下文中的偏移量未定义时，绝对定位元素将保留其作为静态元素的位置。如果将位置偏移用 top、right、left、bottom 来定位，那么需要考虑父位置上下文和附近元素的具体数值，不过，可以使用负值 margin 来移动元素。

```css
.comment {
  position: absolute;
  width: 7em;
  margin-left: -9.5em;
  margin-top: -2.5em;
}
```

额外惊喜：用 CSS 创建三角形。

```css
.comment:after {
  position: absolute;
  content: "";
  display: block;
  width: 0;
  height: 0;
  border: 0.5em solid #dcf0ff;
  border-bottom-color: transparent;
  border-right-color: transparent;
  position: absolute;
  right: -1em;
  top: 0.5em;
}
```

使用偏移自动适应尺寸。如果绝对元素没有明确的尺寸，那么它就会变回内容需要的尺寸，当声明位置上下文的相反方向的两个偏移，元素机会伸展到可容纳的尺寸以便适应规则。

```css
.photo-header {
  position: relative;
}
.photo-header-plate {
  position: absolute;
  right: 4em;
  bottom: 4em;
  left: 4em;
  background-color: #fff;
  background-color: rgba(255, 255, 255, 0.7);
  padding: 2em;
}
```

## 6.2 水平布局

## 6.3 Flexbox

Flexbox，也就是 Flexible Box Layout 模块，是 CSS 提供的用于布局的一套新属性。这套属性包括针对容器（弹性容器，flex container）和针对其直接子元素（弹性香，flex item）的两类属性。Flexbox 可以控制弹性项的如下方面：

- 大小，基于内容及空间；
- 流动方向，水平还是垂直，正向还是方向；
- 两个轴向上的对齐与分布；
- 顺序，与源代码中的顺序无关。

容器属性

- flex-flow
  - flex-direction 主轴方向`（flex-direction: row|row-reverse|column|column-reverse）`
  - flex-wrap 换行处理`（flex-wrap: nowrap | wrap | wrap-reverse）`
- justify-content 主轴对齐`（justify-content: flex-start|flex-end|center|space-between|space-around）`
- align-items 辅轴对齐`（align-items: stretch|center|flex-start|flex-end|baseline）`
- align-content 多行对齐`（align-content: stretch|center|flex-start|flex-end|space-between|space-around）`

元素属性

- order 调整顺序
- flex
  - flex-grow 放大比例
  - flex-shrink 缩小比例
  - flex-basis 原始尺寸
- align-self 单独对齐方式

### 6.3.1 浏览器支持与语法

Flexbox 已经得到主流浏览器较新版本的广泛支持。如果要支持 IE10 及更早版本的 Webkit 浏览器，需要在本章标准代码基础上补充提供商前缀属性和一些不同的属性。IE9 及更早版本的 IE 不支持 Flexbox。

### 6.3.2 理解 Flex 方向：主轴和辅轴

区域内的盒子可以沿两个方向排列：默认水平排列（成一行），也可以垂直排列（成一列），这个排列方向成为主轴（main axis）。与主轴垂直的方向为辅轴（cross axis），区域内的盒子可以沿辅轴发生位移或伸缩。

通常，Flexbox 布局中最重要的尺寸就是主轴方向的尺寸：水平布局时的宽度或垂直布局时的高度，这个主轴方向的尺寸叫做主尺寸（main size）。

所有项集中在左侧，是从左到右书写的语言环境下的默认行为。如果把 flex-direction 改成 row-reverse，那么所有项就会集中到右侧，而且变成从右向左排列。

如果不指定大小，Flex 容器内的项目会自动收缩，也就是说，一行中的各项会收缩到各自的最小宽度，或者一列中各项会收缩到各自的最小高度，以恰好可以容纳自身内容为限。

### 6.3.3 对齐与空间

Flexbox 对子项的排列有多种方式，沿主轴的排列叫排布（justification），沿辅轴的排列则叫对齐（alignment）。 

排布对齐。用于指定排布方式的属性是 justify-content，其默认值是 flex-start，表示按照当前文本方向排布，关键词还有 flex-end、center、space-between、space-around。

Flexbox 不允许通过以上这些关键字指定个别项的排布方式。然而，如果指定某一项的外边距值为 auto，而且在容器里哪一侧还有空间，那么该外边距就会扩展占据可用空间。

辅轴对齐。如果增加 Flex 容器自身或其中一项的高度，会发现子项自动就等高了。实际上，控制辅轴对齐的属性 align-items，其默认值就是 stretch，其它关键字时 flex-start、center 和 flex-end。最后，后可以使用 baseline 关键字，将子项中文本的基线与容器基线对齐，效果与行内块元素的默认行为类似。

除了同时对其所有项，还可以在辅轴上指定个别项的对齐方式。使用的是 align-self 属性。align-self 属性重写了容器的 align-items 属性。

垂直和水平居中。Flexbox 中的垂直对齐。在容器里只有一个元素时，只要将容器设置为 flex，再将需要居中的元素的外边距设置为 auto 就行了，因为 Flexbox 中各项的自动外边距会扩展“填充”相应方向的空间。如果 Flex 容器有多个元素，那么就可以使用对齐属性将它们聚拢到水平和垂直中心上，只需要把排布和对齐都设置为 center。

### 6.3.4 可伸缩的尺寸

Flexbox 支持对元素大小的灵活控制。体现在以下三个属性中：flex-basis、flex-grow 和 flex-shrink。这三个属性应用给每个可伸缩项，而不是容器。

- flex-basis。控制项目在主轴方向上，经过修正之前的“首选”大小，可以是长度值、百分比，也可以是关键字 auto。
- flex-grow。一个弹性系数。其值是一个数值，表示剩余空间的一个比值。默认值是 0，表示从 flex-basis 取得尺寸后旧不再扩展。
- flex-shrink。也是一个弹性系数。默认值是 1，表示如果空间不够，所有项都会以自己的首选尺寸为基准等比例收缩。

扩展。只有两个 li，第一部分分得剩余空间的四分之三，第二部分分得四分之一。

```css
.navbar li:first-child {
  flex-grow: 3;
}
.navbar li:last-child {
  flex-grow: 1;
}
```

纯粹按伸缩系数计算大小。假设上面的代码中 flex-basis 的值是 0，那么这一步就不会给项目分配空间。这种情况下，容器内部的全部空间都会留到第二步再分配，就是根据伸缩系数切分，然后将最终尺寸指定给具体的项目。下面两个项目恰好各自占据可分配空间的一半。

```css
.navbar li:first-child {
  flex-basis: 0;
  flex-grow: 1;
}
.navbar li:last-child {
  flex-basis: 0;
  flex-grow: 1;
}
```

接下来可以使用 flex 这个简写属性一次性设置 flex-grow、flex-shrink 和 flex-basis 属性，值以空格分隔`flex: 1 0 0%`。

收缩项目。当项目宽度总和超过容器宽度时，Flexbox 会按照 flex-shrink 属性来决定如何收缩它们。

### 6.3.5 Flexbox 布局

与行内块和浮动类似，Flexbox 也支持内容排布到多行或多列，但具有更强的可控性。

折行与方向。可以将 flex-direction 的值改为 row-reverse，那么所有项就会变成从右上角起从右往左排布，每一项都变成了右对齐。也可以反转垂直排布的方向，让第一行从底部开头，然后向上折行，只要将 flex-wrap 设置成了 wrap-reverse。

多行布局中可伸缩的大小。Flexbox 对多行布局的另一个好处就是，可以利用可伸缩的大小均匀填充每一行。但是如果最后一个项换行了，就会独自占据所有一行，所以可以设置一个 max-width 来限制可伸缩的范围。

对齐所有行。Flexbox 允许相对于一行的 flex-start、center、baseline 和 flex-end 这几个点来对齐项目。而在多行布局中，则可以相对于容器来对齐行或列。align-content 的默认值是 stretch，意思是每一行都会拉伸以填充自己应占的容器高度。align-content 对容器中多行的作用，与 justify-content 对主轴内容排布的作用非常相似。通过 align-content 骇客把多行排布到 flex-start（容器顶部）、flex-end（容器底部）、center（容器中部）、还可以通过 space-between 或 space-around 让多行隔开。

### 6.3.6 列布局与个别排序

使用 Flexbox 的 order 属性，可以完全摆脱项目在源代码中顺序的约束。只要告诉浏览器这个项目排第几就行了。默认情况下，每个项目的 order 值都为 0，意味着按照它们在源代码中的顺序出现。order 的值不一定要连续，而且正、负值都可以，只要是可以比较大小的数值，相应的项就会调整次序。

### 6.3.7 嵌套的 Flexbox 布局

两个文章导读组件及时 Flexbox 行的可伸缩项，也是嵌套的 Flexbox 列。只要在“阅读详情”元素上设置 `margin-top: auto`，就可以把它推到列的底部，让两个组件的元素在视觉上对齐。

```css
.article-teaser-group {
  display: flex;
}

.article-teaser {
  display: flex;
  flex-direction: column;
  max-width: 20em;
}
.article-teaser img {
  width: 100%;
  min-height: 0; 
  order: -1;
}
/* 底部查看更多按钮 */
.article-teaser-more {
  margin-top: auto;
}

```

### 6.3.8 Flexbox 不可用怎么办

首先，因为 Flexbox 只是一种显示模式，所以不理解 flex 关键字的浏览器会忽略它。也就是说，不支持 Flexbox 的浏览器仍然会按照一个常规块元素来显示原来的容器。

其次，给可伸缩项加上 float 声明，或者将其设置为`display: inline-block`，都不会影响 Flexbox 布局。

### 6.3.9 Flexbox 的 bug 与提示

order 属性的值决定了可伸缩项的绘制次序，但这个值可能会影响这些项的叠加次序，与 z-index 类似。

# 第七章 页面布局和网格

## 7.1 布局规划

网格系统是设计师在切分布局时作为参照的一组行和列，行和列之间的空白叫做空距（gutter），行和列值的是网格中的横条和竖条，分别撑满网格的宽度和高度。行与列相交的一个单元格，称为单元（unit）或模块（module）。

类名用于为布局添加样式。随着网站的复杂度提高，某些部分从属于特定的内容层级，就需要尝试“可视化”的命名方式。另一种做法就是把具有共同样式的选择符集中一起。

固定布局，是指页面具有特定的宽度。弹性布局，值布局元素的尺寸使用 em 单位，即使用户缩放文本大小，布局的比例也不会变。六栋布局，也称为“流式布局”，指页面元素会按比例缩放，但元素与元素之间的比率保持不变。这其实是 Web 的默认模式，即块级元素没有设置的宽度，其尺寸随可用空间大小而变化。这种让设计能响应环境的设计方法叫做响应式设计（responsive Web design）。

## 7.2 创建灵活的页面结构

包装元素是页面布局中常用的一个盛放内容的元素，对于流动布局而言，使用百分比来设置一个稍微小于100%的宽度是很常见的，最大宽度则相对于文本大小来设置，单位是 em。

```css
.wrapper {
  width: 95%;
  max-width: 76em;
  margin: 0 auto;
}
```

行容器。通过 overflow 属性创建一个块级格式化上下文，从而实现包含浮动元素的功能，在这里可以设置一个清除的伪元素，因为较大的区块很有可能会有定位内容被摆放到行容器之外，所以使用 overflow 可能对我们不利。

```css
.row:after {
  content: '';
  display: block;
  clear: both;
  height: 0;
}
```

创建列。水平布局浮动时最常见的，也是浏览器支持最好的技术。考虑到将来可能会在不影响列宽度的前提下，直接给列容器添加边框和内边距，还应该设置 box-sizing 属性。

```css
.col {
  float: left;
  box-sizing: border-box;
}

.col-1of4 {
  width: 25%;
}
.col-1of2 {
  width: 50%;
}
/* ...etc */
```

利用宽度的类来指定列宽，缺点是过分强调某种布局。如果将来需要根据屏幕大小动态调整布局，那么这种命名方式就不太适合。可以给结合点换个名字，不使用特定的宽度或者比率，让它更加普遍。用音乐来比喻，可以创建一条规则，让行容器在正常情况下包含四个宽度相等的部分（quartet，四重奏）。然后使用通用选择符，直接针对行容器的子元素，同时可以降低这条通用规则的特殊性。

```css
.row-quartet > * {
  width: 25%;
}
```

```html
<div class="row row-quartet">
  <div class="col"></div>
  <div class="col"></div>
  <div class="col"></div>
  <div class="col"></div>
</div>
<div class="row row-quartet">
  <div class="col my-special-column"></div>
  <div class="col"></div>
  <div class="col"></div>
</div>
```

```css
.my-special-column {
  width: 50%;
}
```

流式空距。在流动布局中，空距可以是百分比，也可以是相对于字体大小的固定宽度。不管采用哪种方式，列元素两边的宽度都应该相等。如果希望给列添加背景颜色或图片时它们也保持间距，那就应该以外边距作为空距。

抵消最外侧的空距。没有特定宽度的非浮动块级元素，会在左、右负外边距都设置的情况下扩展其宽度。由于我们使用了一个独立的元素作为行来分隔内容，那就可以直接给每一行的左、右两侧都应用一个等于空距一般宽度的负外边距。

设置空距的替代方案。要想进一步简化列宽的计算，可以利用 box-sizing 属性，并使用内边距来设置空距。

增强列：包装与等高。用行内块包装行与列，使用 Flexbox 实现等高的列。

```css
.flexbox .row {
  display: flex;
}

.flexbox .col {
  display: flex;
  flex-direction: column;
}
.flexbox .col > * {
  flex: 1;
}

.flexwrap .row-wrapping {
  display: flex;
  flex-wrap: wrap;
}
```

作为网页布局通用工具的 Flexbox。由于 Flexbox 本身会忽略可伸缩项的浮动和显示属性，使用它能够轻松打磨基于浮动的布局。现代 Flexbox 性能一般都比浮动优越，但早期 Flexbox 的实现性能不太好，因此在相应的旧版本浏览器中使用时要注意。另外，Flexbox 会随着其中内容的加载而重新计算尺寸，因此页面首次加载时会跳一下。

## 7.3 二维布局：CSS Grid Layout

使用 Grid Layout 模块，可以抛开之前用到的很多辅助控制元素，从而大幅精简 HTML 标记。与此同时，这个模块也把基于元素本身来设置水平和垂直维度的负担，转移到了在页面中表示网格的一个包含元素上。

- 被设置为`display: grid`的元素叫网格容器（grid container）
- 容器进一步被网格线（grid line）划分为不同的区域，叫网格但愿（grid cell）
- 网格线之间的水平或垂直路径叫网格轨道（grid track）。具体来说，水平方向的网格轨道叫网格行（grid row），垂直方向的网格轨道叫网格列（grid column）
- 由相邻网格单元组合起来的矩形区块叫网格区（grid area）
- 网格容器的直接子元素叫网格项（grid item），网格项可以放在网格区内。

定义行和列。创建网格需要告诉浏览器网格行与网格列的数量和行为。

```css
.wrapper {
  display: grid;
  grid-template-rows: 300px 300px;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
```

添加网格项。添加网格项要以其起止处的网格线作为参考。

自动网格定位。自动定位机制是 Grid Layout 中默认的，不会改变网格项的源代码次序。所有网格项自动从第一行第一个可用的网格单元开始，逐列填充。一行填满后，网格会自动开启一行并继续填充。

网格模板区。通过“命名模板区”特性，能够以可视化方式来指定如何排布项目。 

# 第八章 响应式 Web 设计与 CSS

# 第九章 表单和数据表

# 第十章 变换、过渡与动画

## 10.1 概述

CSS 变换用于在空间中移动物体，而 CSS 过渡和 CSS 关键帧动画用于控制元素随时间推移而变化。在实战中人们通常把它们当成互补的技术来用，加个动画就意味着每秒要改变其外观 60 次，变换可以让浏览器根据对变化的描述进行非常高效的计算。过渡和关键帧动画可以让我们以巧妙的方式把这些变化转换成动画效果。

关于浏览器支持。变换、过渡和关键帧动画的规范仍然在制定中，但是大多数特性已经在常用浏览器中实现了，除了 IE8 和 Opera Mini。IE9 只支持变换的二维子集，而且要使用 -ms 前缀，并不支持关键帧动画和过渡。在 Webkit 和 Blink 系的浏览器中，变换、过渡和关键帧动画相关的属性都要加 -webkit- 前缀，旧版本的 FIrefox 还需要加 -moz- 前缀。

## 10.2 二维变换

CSS 变换支持在页面中平移、旋转、变形和缩放元素。此外，还可以增加第三维。

从技术角度说，变换改变的是元素所在的坐标系统。给元素应用变换，会为元素最初所在的空间创建所谓的局部坐标系统。局部坐标系统就是畸变场，用于转换元素的像素。

```css
.box {
  transform: rotate(45deg);
}
```

如上，页面上元素原来的位置仍然保留了像素空间，但元素上的所有点都被畸变场给变换到了新位置。如果给变换后的元素再应用一个外边距，那么因为元素所在的整个据表坐标都会被旋转，不会出现跟未旋转时一样的效果。旋转后的元素也不会妨碍页面其他部分的布局，就好像根本没有变换过一样。

注意，变换会影响页面的溢出。如果变换后的元素超出设置了 overflow 属性的元素，导致出现了滚动条，则变换后的元素会影响可滚动区域。

变换原点。默认情况下，变换是以元素边框盒子的中心作为原点的。控制原点的属性叫transform-origin。可以给transform-origin设1~3个值，分别代表x、y和z轴坐标。如果只给了一个值，则第二个默认是关键字center，与background- position类似。

注意，给SVG元素应用变换，有些地方不一样。比如，transform-origin属性的默认值是元素左上角，而不是元素中心点。

平移。平移就是元素移动到新位置。可以沿一个轴平移，使用`translateX()`或者`translateY()`；也可以同时沿两个轴平移，使用`translate()`。使用`translate()`函数时，要给它传入两个坐标值，分别代表x轴和y轴平移的距离。这两个值可以是任何长度值，像素、em或百分比都可以。但要注意，百分比这时候是相对于元素自身大小，而不是包含块的大小。

```css
.box {
  /* equivalent to transform: translateX(100%); */
  transform: translate(100%, 0);
}
```

多重变换。可以同时应用多重变换。多重变换的值以空格分隔的列表形式提供给transform属性，按照声明的顺序依次应用。

```css
.rules li {
  counter-increment: rulecount;
  position: relative;
}

.rules li:before {
  content: '§ ' counter(rulecount);
  position: absolute;
  transform-origin: 100% 100%;
  transform: translate(-100%, -100%) rotate(-90deg);
}
```

修改变换。声明多个变换以后，如果想增加新变换，不能直接在原来的基础上添加，而要重新声明整套变换。

```css
.thing {
  transform: translate(0, 100px);
}

.thing:hover {
  /* 警告：这条声明会删除平移效果 */
  /* transform: rotate(45deg); */
  /* 先平移，再旋转 */
  transform: translate(0, 100px) rotate(45deg);
}
```

缩放和变形。现在已经介绍了`translate()`和`rotate()`这两个二维变换函数，剩下的两个是`scale()`和`skew()`。这两个函数都有对应x轴和y轴的变体：scaleX()、scaleY()、skewX()和skewY()。

`scale()`函数很简单，它的参数是没有单位的数值，可以是一个，也可以是两个。如果只传给它一个数值，就表示同时在x轴和y轴上缩放。比如传入数值2，就会同时沿x轴和y轴放大一倍。传入数值1则意味着不会发生任何改变。

```css
.doubled {
  transform: scale(2);
  /* 等于transform: scale(2, 2); */
  /* 也等于transform: scaleX(2) scaleY(2); */
}
/* 只沿一个方向缩放，会导致元素被压扁或拉长。 */
.squashed-text {
  transform: scaleX(0.5);
  /* 等于transform: scale(0.5, 1); */
}
```

变形是指水平或垂直方向平行的边发生相对位移，或偏移一定角度。很多人分不清x轴变形和y轴变形。x轴变形可以理解为水平线在元素变形后仍然保持水平，而垂直线则会发生倾斜。关键在于你想让哪个轴向的边发生相对位移。

```css
.rules li {
  transform: skewX(15deg);
}

.rules li:nth-child(even) {
  transform: skewX(-15deg);
}
```

二维矩阵变换。对浏览器而言，所有这些变换都归于一个数学结构：变换矩阵。我们可以通过`matrix()`这个低级函数直接操纵变换矩阵的值，值一共有6个。

```css
.box {
  width: 100px;
  height: 100px;
  transform: matrix(1.41, 1.41, -1.16, 1.66, 70.7, 70.7);
  /* 等于 transform: rotate(45deg) translate(100px, 0) scale(2) skewX(10deg); */
}
```

变换与性能。浏览器在计算CSS效果时，会在某些情况下多花一些时间。比如，如果修改文本大小，那么生成的行盒子可能会随着文本折行而变化，而元素本身也会变高。元素变高会把下方的元素向页面下方推挤，这样一来又会迫使浏览器进一步重新计算布局。

在使用CSS变换时，相应的计算只会影响相关元素的坐标系统，既不会改变元素内部的布局，又不会影响外部的其他元素。而且，这时的计算基本上可以独立于页面上的其他计算（比如运行脚本或布局其他元素），因为变换不会影响它们。多数浏览器也都会尽量安排图形处理器（如果有的话）来做这些计算，毕竟图形处理器是专门设计来做这种数学计算的。换句话说，变换从性能角度讲是很好的。如果你想实现的效果可以用变换来做，那么变换的性能一定更好。连续多重变换的性能更佳，比如实现某个元素的动画或过渡效果。

变换也有一些副作用。
- 有些浏览器会为变换的元素切换抗锯齿方法，渲染模式会在最终变换应用前完成切换。
- 应用给元素的任何变换都会创建一个堆叠上下文。这意味着在使用变换时，要注意z-index。这是因为变换后的元素有自己的堆叠上下文。换句话说，子元素上的z-index值再大，也不会出现在父元素上方。
- 对于固定定位，变换后的元素也会创建一个新的包含块。如果发生变换的元素中有一个元素使用了`positon: fixed`，那么它会将发生变换的元素当成自己的视口。

## 10.3 过渡

过渡是一种动画，可以从一种状态过渡到另一种状态，比如按钮从常规状态变成被按下的状态。正常情况下，这种变化是瞬间完成的，至少浏览器会尽快实现这种状态变换。在点击或按下按钮时，浏览器会计算页面的新外观，然后在几毫秒之内完成重绘。而应用过渡时，我们要告诉浏览器完成类似变换要花多长时间，然后浏览器再计算在此期间屏幕上该显示哪些过渡状态。

过渡会自动双向运行，因此只要状态一反转，反向动画就会运行。

```css
button {
  transition: all 150ms;
}
button:active {
  transform: translateY(.25em);
}
```

transition 属性是一个简写形式，可以一次设置多个属性。设置过渡的持续时间，以及告诉浏览器在两个状态间切换时动画所有属性。

- transition-property 过渡效果的 CSS 属性的名称
- transition-duration 完成过渡效果需要的时间
- transition-timing-function 速度效果的速度曲线
- transition-delay 过渡效果何时开始

```css
button {
  transition-property: all;
  transition-duration: .15s;
}

/* 如果是特定属性变化而不是全部 */
button {
  transition: box-shadow .15s, transform .15s;
}

/* 另一种方式 */
button {
  transition-property: transform, box-shadow;
  transition-duration: .15s;
}
```

过渡计时函数。默认情况下，过渡变化的速度并不是每一帧都相同，这种速度的变化在动画属于中叫缓动，能让动画效果更自然和流畅。CSS 通过相应的数学函数控制这些变化，而这些函数由 `transition-timing-function`属性来指定。还有一些关键字代表不同类型的缓动函数。
- ease
- liner
- ease-in
- ease-out
- ease-in-out

```css
button {
transition: all .25 ease-in;
/* ...or we could set transition-timing-function: ease-in; */
}
```

在底层，控制速度变化的数学函数基于三次贝塞尔函数生成，每个关键字都是这些函数带特定参数的简写形式，在 CSS 变换中可以使用`cubic-bezier()`函数作为缓动值。同时，还可以指定过渡中每一步的状态，这非常适合创建订个动画。这里的`tiansition-timing-function`指定为`steps(6, start)`意思就是把过渡过程切分为六个步骤，在么跑一次开始时改变属性。默认情况下，`steps(6)`会在每一步结束时改变属性，但也可以通过传入`start`或`end`作为第二个参数来明确指定。

```css
.hello-box {
  width: 200px;
  height: 200px;
  transition: background-position 1s steps(6, start);
  background: url(steps-animation.png) no-repeat 0 -1200px;
}
.hello-box:hover {
  background-position: 0 0;
}
```

使用不同的正向和反向过渡。有时候，会希望某个方向的过渡快一些，而反方向的过渡慢一些，为此可以定义不同的过渡属性集合：一个针对非悬停状态，另一个针对悬停状态。

```css
.hello {
  transition: background-position 0s steps(6);
}
.hello:hover {
  transition-duration: .6s;
}
```

“黏着”过渡。另一个方法是根本不让过渡方向，为了“黏着”过渡，可以指定一个非常大的持续时间。

```css
.hello {
  transition: background-position 9999999999s steps(6);
}
.hello:hover {
  transition-duration: 0.6s;
}
```

延迟过渡。通常，过渡会随状态变化立即发生，比如类名被 JavaScript 修改或按钮被按下。但是可以通过 `transition-delay` 属性来推迟过渡的发生。延迟时间也可以是负值，可以一开始就直接跳到过渡的中段。

过渡的能与不能。多数情况下，涉及长度和颜色的都是可以的，这取决于是否有计算值的中间状态。比如100px到200px，红到蓝，但 display 属性的两个值 block 和 none 就没有中间状态。当然，这个规则本身其实也有例外的。

- 可插值。有些属性虽然没有明确的中间值，却可以实现动画。比如 z-index 和 column-count 只接受整数值，浏览器会自动插入整数值。visibility 属性也可以实现过渡动画，浏览器在过渡经过中点后突变为两个终点值中的一个。
- 过渡到内容高度。对于有些可以实现动画的属性比如 height，只能在数值之间过渡，不能使用像 auto 这样的关键字。

```css
.js .expando-list {
  overflow: hidden;
  transition: all .25s ease-in-out;
  max-height: 0;
  opacity: 0;
}
.js .is-expanded .expando-list {
  max-height: 24em;
  opacity: 1;
}
```

## 10.4 CSS关键帧动画

CSS过渡是一种隐式动画。换句话说，我们给浏览器指定两个状态，让浏览器在元素从一个状态过渡到另一个状态的过程中，给指定的属性添加动画效果。有时候，动画的范围不仅限于两个状态，或者要实现动画的属性一开始也不一定存在。CSS Animations规范引入了关键帧的概念来帮我们实现这一类动画。此外，关键帧动画还支持对动画时间及方式的控制。

动画与生命的幻象。CSS 动画的语法有点古怪，需要使用`@keyframes`规则来定义并命名一个关键帧序列，然后再通过`animation-*`属性将该序列连接到一个或多个规则。

```css
/* 将关键帧序列命名为roll */
@keyframes roll {
/* from是0%的别名 */
from {
  transform: translateX(-100%);
  animation-timing-function: ease-in-out;
}
20% {
  transform: translateX(-100%) skewX(15deg);
}
28% {
  transform: translateX(-100%) skewX(0deg);
  animation-timing-function: ease-out;
}
45% {
  transform: translateX(-100%) skewX(-5deg) rotate(20deg) scaleY(1.1);
  animation-timing-function: ease-in-out;
}
50% {
  transform: translateX(-100%) rotate(45deg) scaleY(1.1);
  animation-timing-function: ease-in;
}
60% {
  transform: translateX(-100%) rotate(90deg);
}
65% {
  transform: translateX(-100%) rotate(90deg) skewY(10deg);
}
70% {
  transform: translateX(-100%) rotate(90deg) skewY(0deg);
}
/* to是100%的别名 */
to {
  transform: translateX(-100%) rotate(90deg);
}
}
```

将关键帧块连接到元素。关键帧动画也有相应的属性控制持续时间、延迟和计时函数，而且可控制的方面更多一些。

```css
.box-inner {
  animation-name: roll;
  animation-duration: 1.5s;
  animation-delay: 1s;
  animation-iteration-count: 3;
  animation-timing-function: linear;
  transform-origin: bottom right;
  /* 相当于 */
  animation: roll 1.5s 1s 3 linear;
  transform-origin: bottom right;
}
```

可以让方块原地旋转，但希望方块能从视口外面进入并移动到其最终位置的话，还需要附加到前一个动画里实现。因为想让动画从某个值开始，到初始值结束，所以省略了 to 关键帧。最后一个关键字 backwards，设置的是动画序列的 animation-fill-mode 属性，告诉浏览器在动画运行之前或之后如何处理动画。默认情况下，第一个关键帧中的属性在动画运行之前不会被应用，如果指定了关键字 backwards，那相应的属性就会反向填充，即第一个关键帧中的属性会立即应用，即使动画有延迟或一开始被暂停。关键字 forward 表示应用最后一个关键帧的计算样式。both 表示同时应用正向和反向填充。

```css
@keyframes shift {
  from {
    transform: translateX(-300%);
  }
}
.box-outer {
  display: inline-block;
  animation: shift 4.5s 1s steps(3, start) backwards;
}
```

```html
<h1 class="logo">
<!-- This is the box we are animating -->
<span class="box-outer"><span class="box-inner"></span></span>
<span class="logo-box">Box</span><span class="logo-model">model</span>
</h1>
```

曲线动画。通常，元素在两点间的位移动画都是走直线的，通过多使用一些关键帧，每一帧稍微改变一点方向，可以实现元素沿曲线运行，但更好的办法是以特殊方式组合旋转和平移。这里没有专门的属性来设置延迟，因此让从 70% 到 100% 这段时间的状态保持相同。同时可以通过 animation-play-state 的值设置为 paused 让动画暂停，该属性默认值为 running。

```css
@keyframes jump {
  from {
    transform: rotate(0) translateX(-170px) rotate(0) scale(1);
  }
  70%, 100% {
    transform: rotate(175deg) translateX(-170px) rotate(-175deg) scale(.5);
  }
}

.file-icon {
  animation: jump 2s ease-in-out infinite;
}
```

## 10.5 三维变换

三维空间中，概念与二维变化一样，只不过要多考虑一个维度，也就是z轴。三维变换允许我们控制坐标系统，旋转、变形、缩放元素，以及向前或向后移动元素。

透视简介。提到三维，就意味着要在三个轴向上变换。其中 x 轴和 y 轴跟以前一样，而 z 轴表示的是用户到屏幕的方向。屏幕的表面通常被称为 z 平面，也就是 z 轴默认的起点位置。要定义 perspective 透视，先得确定用户距离这个元素有多远。离得越近变化越大，离得越远变化越小。默认的距离是无穷远。因此不会发生明显的变化。可以通过 perspective-origin 属性来修改消失点的位置。在父元素上设置 perspective 属性，额可以让其中所有元素的三维变换共享同样的透视关系。要设置个别元素的透视，可以使用 `perspective()` 函数。

```css
body {
  perspective: 800px;
}

.box {
  margin: auto;
  border: 2px solid;
  width: 100px;
  height: 100px;
  transform: rotateY(60deg);
}
```

创建三维部件。让用户界面的一部分隐藏在元素的背面，点击按钮这个元素会翻转180度，显示背面的面板。

```html
<div class="flip-wrapper menu-wrapper">
  <div class="flip-a menu">
    <h1 class="menu-heading">Top menu choices</h1>
    <ol class="menu-list">
      <li>Capricciosa</li>
      <!-- ...and so on, all 10 choices -->
    </ol>
  </div>
  <div class="flip-b menu-settings">
  <!-- the form on the back of the widget goes here. -->
  </div>
</div>
```

首先，在 body 元素上设置透视，并让包装元素成为其后代的定位上下文。然后再针对包装元素的 transform 属性来添加过渡。

```css
.csstransforms3d body {
  perspective: 1000px;
}
.csstransforms3d .flip-wrapper {
  position: relative;
  transition: transform .25s ease-in-out;
}
```

接下来让背面对应的元素绝对定位，以便它占据跟前面一样大的空间，同时将其围绕 y 轴翻转 180 度。同时还需要在两面被翻时两面都不可见，以免互相干扰。

```css
.csstransforms3d .flip-b {
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  margin: 0;
  transform: rotateY(-180deg);
}
.csstransforms3d .flip-b,
.csstransforms3d .flip-a {
  backface-visibility: hidden;
}
```

默认情况下，任何应用给父元素的三维变换都会让子元素上的三维变换失效，并使其变平。通过将包装元素上的 transform-style 属性设置为 preserve-3d，创建一个三维上下文，让子元素的变换与父元素在同一个三维空间中。

```css
.csstransforms3d .flip-wrapper {
  position: relative;
  transition: all .25s ease-in-out;
  transform-style: preserve-3d; /* default is flat */
}
```

最后一步，在用户点击按钮时，通过 JavaScript 切换包装元素上的类名，添加 is-flipped 类会触发整个部件沿 y 轴旋转180度。

```CSS
.csstransforms3d .flip-wrapper.is-flipped {
  transform: rotateY(180deg);
}
```

高级三维变换。

`rotate3d()`。除了`rorateX()`、`rorateY()`和`rorateZ()`（以及二维版本`rorate()`）这几个单维旋转函数之外，还有一个`rorate3d()`函数，这个函数可以围绕穿越三维空间的任意一条线翻转元素。这个函数接受四个参数：前三个参数分别表示 x 轴、y 轴和 z 轴的向量坐标，最后一个是角度。其中坐标定义了一条线，作为翻转环绕的轴。

```css
.box {
  transform: rotate3d(1, 1, 1, 45deg);
}
```

与二维矩阵变换类似，也有一个 `matrix3d()` 函数可以组合多个轴向上的平移、缩放、变形和旋转。三维矩阵变换接受十六个参数，以便计算最终呈现在坐标系中的对象。

# 第十一章 高级特效

# 第十二章 品控与流程

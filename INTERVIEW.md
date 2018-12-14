# 一、YNKG

## **link和@import的区别。**

从属关系区别。@import是 CSS 提供的语法规则，只有导入样式表的作用；link是HTML提供的标签，不仅可以加载 CSS 文件，还可以定义 RSS、rel 连接属性等。

加载顺序区别。加载页面时，link标签引入的 CSS 被同时加载；@import引入的 CSS 将在页面加载完毕后被加载。

兼容性区别。@import是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；link标签作为 HTML 元素，不存在兼容性问题。

DOM可控性区别。可以通过 JS 操作 DOM ，插入link标签来改变样式；由于 DOM 方法是基于文档的，无法使用@import的方式插入样式。

权重区别。link引入的样式权重大于@import引入的样式。



## **HTML5有哪些新特性，移除了哪些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分HTML和HTML5?**

语义化标签、增强型表单、视频和音频标准、Canvas绘图、拖放API、Web Worker、Web Storage、WebSocket。

移除的元素有纯表现的元素（basefont,big,center,font,s,strike,tt,u）、对可用性产生负面影响的元素（frame,frameset,noframes）

IE8/IE7/IE6支持通过document.createElement方法产生的标签， 可以利用这一特性让这些浏览器支持HTML5新标签， 浏览器支持新标签后，还需要添加标签默认的样式。 当然也可以直接使用成熟的框架、比如html5shim; 

```html
<!--[if lt IE 9]> 
<script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script> 
<![endif]-->
```

HTML5文档类型声明：<!doctype html>。DOCTYPE声明的方式是区分HTML和HTML5标志的一个重要因素，此外，还可以根据新增的结构、功能元素来加以区分。



## **position为relative和为absolute的相对定位元素。**

对于其他元素，如果该元素的 position 是 relative 或者 static，则“包含块” 由其最近的块容器祖先盒的 content box 边界形成。

如果元素 position:fixed，则“包含块”是“初始包含块”。根元素（很多场景下可以看成是<html>）被称为“初始包含块”，其尺寸等同于浏览器可视窗口的大小。

如果元素 position:absolute，则“包含块”由最近的 position 不为 static 的祖先元素建立。



## **离线存储的方式和原理。**

HTML5 提供了两种在客户端存储数据的新方法：

- localStorage - 没有时间限制的数据存储。
- sessionStorage - 针对一个 session 的数据存储。



## **创建Ajax的步骤。**

step1. 创建XMLHttpRequest对象，也就是创建一个异步调用对象；  

step2. 创建一个新的HTTP请求，并指定改HTTP请求的方法、URL以及验证信息；  

step3. 设置响应HTTP状态变化的函数；  

step4. 发送HTTP请求；  

step5. 获取异步调用返回的数据；  

step6. 使用javascript和DOM实现局部刷新；

```javascript
<script type="text/javascript">
    window.onload = function(){
        //第一步：创建xhr对象
        //xhr是一个对象；里面可以放很多东西，数据；
        var xhr = null;
        if(window.XMLHttpRequest){//标准浏览器
            xhr = new XMLHttpRequest();//创建一个对象
        }else{//早期的IE浏览器
            xhr = new ActiveXObject('Microsoft.XMLHTTP');//参数是规定的；
        }
        console.log("状态q"+xhr.readyState);//0
        //第二步：准备发送请求-配置发送请求的一些行为
        //open即打开链接，第一个参数是以什么方式；第二个是往哪儿发送请求，第三个可以不写，默认true,表示异步，false表示同步；；
        xhr.open('get','03form.php',true);
        console.log("状态w"+xhr.readyState);//1

        //第三步：执行发送的动作
        //send也可以写在前面，推荐写在后面；写null是兼容问题；
        xhr.send(null);
        console.log("状态e"+xhr.readyState);//1

        //第四步：指定一些回调函数，也属于事件函数；不触发不执行，触发条件是xhr.readyState;z这个值有0-4，共5个状态，是由浏览器控制的；
        xhr.onreadystatechange = function(){
            if(xhr.readyState == 4){//4指服务器返回的数据可以使用；
                if(xhr.status == 200){ //判断已经成功的获取了数据；200表示hTTP请求成功；404表示找不到页面；503表示服务器端有语法错误；
                    var data = xhr.responseText;//json，文本，主角；
                    // var data1 = xhr.responseXML;
                }
            }
            // console.log("状态t"+xhr.readyState);//2表示已经发送完成；

            // console.log(1234);
        }

        // console.log(456);
        console.log("状态r"+xhr.readyState);//1


    }
    </script>
```



## **GET请求和POST请求。**

（1）使用Get请求时,参数在URL中显示,而使用Post请求,则不会显示出来； 

（2）Post传输的数据量大，可以达到2M，而Get方法由于受到URL长度的限制,只能传递大约1024字节. 

（3）Get请求请求需注意缓存问题,Post请求不需担心这个问题； 

（4）Post请求必须设置Content-Type值为application/x-form-www-urlencoded； 

（5）发送请求时,因为Get请求的参数都在url里,所以send函数发送的参数为null,而Post请求在使用send方法时,却需赋予其参数； 

（6）GET方式请求的数据会被浏览器缓存起来，因此其他人就可以从浏览器的历史记录中读取到这些数据，例如账号和密码等。在某种情况下，GET方式会带来严重的安全问题。而POST方式相对来说就可以避免这些问题。 



## **angular和react使用的TypeScript语法**

简单笼统地说，TypeScript是JavaScript的超集，是微软开发的一种脚本语言。和JavaScript一样，是现在项目开发中热门的脚本语言。

TypeScript是一种强类型语言，可编译成JavaScript，包含了JavaScript的所有元素，可以载入JavaScript代码运行，扩展了JavaScript的语法。相比JavaScript，TypeScript是真面向对象，增加了静态类型，类，模块，接口和类型注解。

TypeScript的编译器很智能，会进行冲突检测，举个例子，一个enum的数据，你初始化一个数据，再用一个switch语句，编译器会判定其他选项为无效的，会编译错误，直接指出错误原因，这在JavaScript是不会指出错误的。       




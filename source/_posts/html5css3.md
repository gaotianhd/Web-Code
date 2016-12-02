---
title: html5/css3面试题
---
##### Doctype作用？严格模式与混杂模式如何区分？它们有何意义?
1. <!Doctype>声明位于文档中的最前面的位置，处于<html>标签之前，次标签可以告知浏览器文档使用哪种HTML或XHTML规范
2. 严格模式的排版和JS运作模式是以该浏览器支持的最高标准运行。
3. 混杂模式，页面以宽松的向后兼容的方式显示。模拟老式浏览器的行为以防止以防止站点无法工作。
4. 触发严格模式很简单，就是正常的建立网页，声明正确的DTD，便是严格模式。DOCTYPE不存在或格式不正确会导致文档以混杂模式呈现。

##### HTML5 为什么只需要写<!DOCTYPE HTML> ？
HTML5 不基于SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为（让浏览器按照它们应该的方式来运行）而HTML4.01基于SGML，所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。

##### 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？
行内元素：a 标签 ， em 强调 ， strong 粗体强调 ， i 斜体 ， img 图片 ， b 粗体 ， span 标签 ， label 标签 ， select 下拉菜单。
块级元素：div，dl，dt，dd，ul，li，ol，p，h1-h6，table，form。
空元素：br 换行，hr 水平线。

##### 页面导入样式时，使用link和@import有什么区别？
1. link属于XHTML标签，而@import是CSS提供的;
2. 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
3. import只在IE5以上才能识别，而link是XHTML标签，无兼容问题;
4. link方式的样式的权重 高于@import的权重.

##### 介绍一下你对浏览器内核的理解？

##### 常见的浏览器内核有哪些？
1. IE浏览器的内核Trident
2. Mozilla的Gecko
3. Chrome的Blink（WebKit的分支）
4. Opera内核原为Presto，现为Blink

##### html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？
* HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。

* 绘画 canvas  
  用于媒介回放的 video 和 audio 元素
  本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
  sessionStorage 的数据在浏览器关闭后自动删除

  语意化更好的内容元素，比如 article、footer、header、nav、section
  表单控件，calendar、date、time、email、url、search  
  新的技术webworker, websockt, Geolocation

* 移除的元素

纯表现的元素：basefont，big，center，font, s，strike，tt，u；

对可用性产生负面影响的元素：frame，frameset，noframes；

支持HTML5新标签：

* IE8/IE7/IE6支持通过document.createElement方法产生的标签，
  可以利用这一特性让这些浏览器支持HTML5新标签，

  浏览器支持新标签后，还需要添加标签默认的样式：

* 当然最好的方式是直接使用成熟的框架、使用最多的是html5shim框架
```
   <!--[if lt IE 9]>
   <script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script>
   <![endif]-->
```
如何区分： DOCTYPE声明\新增的结构元素\功能元素

##### 简述一下你对HTML语义化的理解？
用正确的标签做正确的事情！
html语义化就是让页面的内容结构化，便于对浏览器、搜索引擎解析；
在没有样式CCS情况下也以一种文档格式显示，并且是容易阅读的。
搜索引擎的爬虫依赖于标记来确定上下文和各个关键字的权重，利于 SEO。
使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解。

##### HTML5的离线储存怎么使用，工作原理能不能解释一下？
localStorage    长期存储数据，浏览器关闭后数据不丢失；
sessionStorage  数据在浏览器关闭后自动删除。

##### 浏览器是怎么对HTML5的离线储存资源进行管理和加载的呢？

##### 请描述一下 cookies，sessionStorage 和 localStorage 的区别？
cookie在浏览器和服务器间来回传递。 sessionStorage和localStorage不会
sessionStorage和localStorage的存储空间更大；
sessionStorage和localStorage有更多丰富易用的接口；
sessionStorage和localStorage各自独立的存储空间；

##### iframe有那些缺点？
iframe会阻塞主页面的Onload事件；
iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript
动态给iframe添加src属性值，这样可以可以绕开以上两个问题。

##### Label的作用是什么？是怎么用的？（加 for 或 包裹）
可以使标签内的区域指向label标签for属性指代的对象的事件。
```
无label标签:<input id="ye" type="checkbox" /> 普通文本
有label标签:<label for="ye"><input id="ye" type="checkbox" />我是标签文本</label>
```
有label标签的这一段，点击标签中的文本，可使多选框聚焦。

##### HTML5的form如何关闭自动完成功能？
autocomplete 设置整个表单是否开启自动完成 on|off

##### 如何实现浏览器内多个标签页之间的通信? (阿里)
方法一：使用localStorage
使用localStorage.setItem(key,value);添加内容
使用storage事件监听添加、修改、删除的动作   
```
window.addEventListener("storage",function(event){  
        $("#name").val(event.key+”=”+event.newValue);  
});  
```
方法二、使用cookie+setInterval
HTML代码
```
<inputidinputid="name"><input type="button" id="btnOK"value="发送">  
```
JS代码-页面1
```
$(function(){  
       $("#btnOK").click(function(){  
           varname=$("#name").val();  
           document.cookie="name="+name;  
       });  
   });
```
JS代码-页面2
```
//获取Cookie天的内容  
function getKey(key) {  
    return JSON.parse("{\""+ document.cookie.replace(/;\s+/gim,"\",\"").replace(/=/gim, "\":\"") +"\"}")[key];  
}  
//每隔1秒获取Cookie的内容  
setInterval(function(){  
    console.log(getKey("name"));  
 },1000);  
```
##### webSocket如何兼容低浏览器？(阿里)

##### 页面可见性（Page Visibility API） 可以有哪些用途？
页面可见性： 就是对于用户来说，页面是显示还是隐藏, 所谓显示的页面，就是我们正在看的页面；隐藏的页面，就是我们没有看的页面。 因为，我们一次可以打开好多标签页面来回切换着，始终只有一个页面在我们眼前，其他页面就是隐藏的，还有一种就是，把浏览器最小化，所有的页面就都不可见了。

API 很简单，document.hidden 就返回一个布尔值，如果是true, 表示页面可见，false 则表示，页面隐藏。  不同页面之间来回切换，触发visibilitychange事件。 还有一个document.visibilityState, 表示页面所处的状态，取值：visible, hidden 等四个。
```
document.addEventListener("visibilitychange", function(){
    if(document.hidden){
        document.title ="hidden";
    }else {
        document.title = "visibile";
    }
})
```
我们打开这个页面，然后再打开另一个页面，来回点击这两个页面，当我们看到这个页面时，标题显示visiable ,当我们看另一个页面时，标题显示hidden;动画，视频，音频都可以在页面显示时打开，在页面隐藏时关闭。

##### 如何在页面上实现一个圆形的可点击区域？
1. border-radius（H5）
```
<style>  
 .disc{  
     width:100px;  
     height:100px;  
     background-color:dimgray;  
     border-radius: 50%;  
     cursor: pointer;  
     position: absolute;  
     left:50px;  
     top:50px;    
     line-height: 100px;  
     text-align: center;  
     color: white;  
 }  
</style>  
<div class="disc">智学无忧</div>  
```
2. 纯js实现
需要求一个点在不在圆上简单算法、获取鼠标坐标等等
两点之间的距离计算公式  
上面公式对于JavaScript代码如下：
|AB|=Math.abs(Math.sqrt(Math.pow(X2-X1),2)+Math.pow(Y2-Y1,2)))
Math.abs()求绝对值
Math.pow(底数,指数)
Math.sqrt()求平方根
示例：
假设圆心为（100,100）,半径为50，在圆内点击弹出相应的信息，在圆外显示不在圆内的信息
```
document.onclick=function(e){  
    var r=50;//圆的半径  
var x1=100,y1=100,x2= e.clientX;y2= e.clientY;  
//计算鼠标点的位置与圆心的距离  
    var len=Math.abs(Math.sqrt(Math.pow(x2-x1,2)+Math.pow(y2-y1,2)));  
    if(len<=50){  
        console.log("内")  
    }else{  
        console.log("外")  
    }  
 }  
 ```
##### 实现不使用 border 画出1px高的线，在不同浏览器的Quirksmode和CSSCompat模式下都能保持同一效果。

##### 网页验证码是干嘛的，是为了解决什么安全问题？
防止恶意注册和暴力破解 所谓恶意注册和暴力破解都是用软件进行的。 人工注册再快，也需要一项一项输入资料，速度很慢，对服务器基本没有影响。如果没有验证码可以使用软件注册的话，可以同时运行成千上万个线程，一次能注册成千上万个用户，让服务器的数据库很快变得臃肿不堪，运行效率下降。

##### title与h1的区别、b与strong的区别、i与em的区别？
从网站角度看，title更重于网站信息。title可以直接告诉搜索引擎和用户这个网站是关于什么主题和内容的。
从文章角度看，h1则是用于概括文章主题。用户进入内容页，想看到的当然就是文章的内容，h1文章标题就是最重要的。文章标题最好只有一个，多个h1会导致搜索引擎不知道这个页面哪个标题内容最重要，导致淡化这个页面的标题和关键词，起不到突出主题的效果。
区别：
h1突出文章主题，面对用户，更突出其视觉效果，突出网站标题或关键字用title。一篇文章，一个页面最好只用一个h1，多个h1会稀释主题。一个网站可以有多个title,最好一个单页用一个title，以便突出网站页面主体信息，从seo看，title权重比h1高，适用性比h1广。标记了h1的文字页面给予的权重会比页面内其他权重高很多。一个好的网站是h1和title并存，既突出h1文章主题，又突出网站主题和关键字。达到双重优化网站的效果。

<b>为了加粗而加粗，<strong>为了标明重点而加粗，也可以用其它方式来强调，比如下划线，比如字体加大，比如红色，等等，可以通过css来改变strong的具体表现。

I是Italic(斜体)，而em是emphasize(强调)。
##### 介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？
CSS盒子模型：由四个属性组成的外边距(margin)、内边距(padding)、边界(border)、内容区(width和height);

标准的CSS盒子模型和低端IE CSS盒子模型不同：宽高不一样

标准的css盒子模型宽高就是内容区宽高；

低端IE css盒子模型宽高 内边距﹢边界﹢内容区；

##### CSS选择符有哪些？哪些属性可以继承？
通配选择符* { sRules }

类型选择符E { sRules }  
```
td { font-size:14px; width:120px; }
```
属性选择符
```
E [ attr ] { sRules }
E [ attr = value ] { sRules }
E [ attr ~= value ] { sRules }
E [ attr |= value ] { sRules }  
h[title] { color: blue; }/* 所有具有title属性的h对象*/
span[class=demo] { color: red; }  
div[speed="fast"][dorun="no"] { color: red; }
a[rel~="copyright"] { color:black; }
```

包含选择符E1 E2 { sRules }
```
table td { font-size:14px; }
```

子对象选择符E1 > E2 { sRules }
```
div ul>li p { font-size:14px; }
```

ID选择符 #ID { sRules }

类选择符 E.className { sRules }

选择符分组  
E1 , E2 , E3 { sRules }

伪类及伪对象选择符  
```
E : Pseudo-Classes { sRules }  
( Pseudo-Classes )[:link :hover :active :visited :focus :first-child :first :left :right :lang]
E : Pseudo-Elements { sRules }  
( Pseudo-Elements )[:first-letter :first-line :before :after]
```
可以继承的有：
**font-size font-family color**
不可继承的一般有：
**border padding margin background-color width height 等**

##### CSS优先级算法如何计算？
行内style > id > class > 标签 > 继承 > 浏览器默认属性

##### CSS3新增伪类有那些？
:first-of-type    p:first-of-type    选择属于其父元素的首个 p 元素的每个 p 元素。     
:last-of-type    p:last-of-type    选择属于其父元素的最后 p 元素的每个 p 元素。
:only-of-type    p:only-of-type    选择属于其父元素唯一的 p 元素的每个 p 元素。
:only-child    p:only-child    选择属于其父元素的唯一子元素的每个 p 元素。
:nth-child(n)    p:nth-child(2)    选择属于其父元素的第二个子元素的每个 p 元素。
:nth-last-child(n)    p:nth-last-child(2)    同上，从最后一个子元素开始计数。
:nth-of-type(n)    p:nth-of-type(2)    选择属于其父元素第二个 p 元素的每个 p 元素。   
:nth-last-of-type(n)    p:nth-last-of-type(2)    同上，但是从最后一个子元素开始计数。     
:last-child    p:last-child    选择属于其父元素最后一个子元素每个 p 元素。  
:root    :root    选择文档的根元素。   
:empty    p:empty    选择没有子元素的每个 p 元素（包括文本节点）。  
:target    #news:target    选择当前活动的 #news 元素。
:enabled    input:enabled    选择每个启用的 input 元素。  
:disabled    input:disabled    选择每个禁用的 input 元素  
:checked    input:checked    选择每个被选中的 input 元素。  
:not(selector)    :not(p)    选择非 p 元素的每个元素。
::selection    ::selection    选择被用户选取的元素部分。

##### 如何居中div？如何居中一个浮动元素？如何让绝对定位的div居中？
居中div：margin: 0 auto
居中浮动元素：外面套一个div设置margin:0 auto;
如何让设置绝对定位的div居中：position:absolute;top:50%;left:50%;margin-top:-盒子高度;margin-left:盒子宽度

##### display有哪些值？说明他们的作用。
none：设置元素不显示，并且不占页面空间。
block：设置为块级元素，可以设置宽高。
inline：设置为行内元素。
inline-block：设置为行内块元素。

##### position的值relative和absolute定位原点是？
relative：相对于这个元素原先的位置偏移。
absolute：如果父元素设置为relative则相对于父元素。不设置相对于body。

##### CSS3有哪些新特性？
CSS3有哪些新特性：
    新增各种CSS选择器  （: not(.input)：所有 class 不是“input”的节点）
    圆角           （border-radius:8px）
    多列布局        （multi-column layout）
    阴影和反射        （Shadow\Reflect）
    文字特效      （text-shadow、）
    文字渲染      （Text-decoration）
    线性渐变      （gradient）
    旋转          （transform）
    增加了旋转,缩放,定位,倾斜,动画，多背景
    transform:\scale(0.85,0.90)\ translate(0px,-30px)\ skew(-9deg,0deg)\Animation

##### 请解释一下CSS3的Flexbox（弹性盒布局模型）,以及适用场景？

##### 用纯CSS创建一个三角形的原理是什么？

##### 一个满屏 品 字布局 如何设计?

##### 经常遇到的浏览器的兼容性有哪些？原因，解决方法是什么，常用hack的技巧 ？

##### li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法？

##### 为什么要初始化CSS样式?

##### absolute的containing block计算方式跟正常流有什么不同？

##### CSS里的visibility属性有个collapse属性值是干嘛用的？在不同浏览器下以后什么区别？

##### position跟display、margin collapse、overflow、float这些特性相互叠加后会怎么样？

##### 对BFC规范(块级格式化上下文：block formatting context)的理解？

##### CSS权重优先级是如何计算的？

##### 请解释一下为什么会出现浮动和什么时候需要清除浮动？清除浮动的方式

##### 移动端的布局用过媒体查询吗？

##### 使用 CSS 预处理器吗？喜欢那个？

##### CSS优化、提高性能的方法有哪些？

##### 浏览器是怎样解析CSS选择器的？

##### 在网页中的应该使用奇数还是偶数的字体？为什么呢？

##### margin和padding分别适合什么场景使用？

##### 抽离样式模块怎么写，说出思路，有无实践经验？[阿里航旅的面试题]

##### 元素竖向的百分比设定是相对于容器的高度吗？

##### 全屏滚动的原理是什么？用到了CSS的那些属性？

##### 什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE？

##### 视差滚动效果，如何给每页做不同的动画？（回到顶部，向下滑动要再次出现，和只出现一次分别怎么做？）

##### ::before 和 :after中双冒号和单冒号 有什么区别？解释一下这2个伪元素的作用。

##### 如何修改chrome记住密码后自动填充表单的黄色背景 ？

##### 你对line-height是如何理解的？

##### 设置元素浮动后，该元素的display值是多少？（自动变成display:block）

##### 怎么让Chrome支持小于12px 的文字？

##### 让页面里的字体变清晰，变细用CSS怎么做？（-webkit-font-smoothing: antialiased;）

##### font-style属性可以让它赋值为“oblique” oblique是什么意思？

##### position:fixed;在android下无效怎么处理？

##### 如果需要手动写动画，你认为最小时间间隔是多久，为什么？（阿里）

##### display:inline-block 什么时候会显示间隙？(携程)

##### overflow: scroll时不能平滑滚动的问题怎么处理？

##### 有一个高度自适应的div，里面有两个div，一个高度100px，希望另一个填满剩下的高度。

##### png、jpg、gif 这些图片格式解释一下，分别什么时候用。有没有了解过webp？

##### 什么是Cookie 隔离？（或者说：请求资源的时候不要让它带cookie怎么做）

##### style标签写在body后与body前有什么区别？

##### 什么是CSS 预处理器 / 后处理器？

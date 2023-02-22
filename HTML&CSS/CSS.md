## CSS

#### css3有哪些新特性？

------

- rgba和透明度

- background-image、background-origin（原点位置的背景相对区域）、background-size、background-repeat
- word-wrap：break-word（对长的不可分割的单词换行）
- 文字阴影 text-shadow、盒阴影box-shadow
- font-face属性，定义自己的字体
- border-radius
- 边框图片border-image
- 媒体查询：定义多套css，当游览器尺寸发生变化时采用不同的属性

#### style标签写在body后与body有什么区别？

-----

1.写在body标签前有利于游览器逐步渲染：resources downloading --> cssDOM + DOM --> Render Tree -->layout -->paint

2.写在body标签后：由于游览器以逐行方式对HTML文档进行解析，当解析到写在尾部的样式表（外联或写在style标签）会导致游览器停止之前的渲染，等待加载且解析样式完成后重新渲染；在windows的IE下可能出现样式失效导致的页面闪烁问题。

#### CSS选择器及优先级

-----

1.选择器：

- id选择器（#myid）
- 类选择器（.myclass）
- 属性选择器（a[rel = "external"]）
- 伪类选择器（a:hover, li:nth-child()）
- 标签选择器（div，h1，p）
- 伪元素选择器（p::first-line）
- 相邻选择器（h1 + p）
- 子选择器（ul > li）
- 后代选择器（li a）
- 通配符选择器（*）

2.优先级

- !important
- 内联样式（1000）
- ID选择器（0100）
- 类选择器/ 属性选择/ 伪类选择器（0010）
- 标签选择器 / 伪元素选择器（0001）
- 关系选择器 / 通配符选择器 （0000）

带！important 标记的样式属性优先级最高；样式表的来源相同时：!important > 行内样式 > ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 游览器默认属性

#### rgba() 和 opacity 设置透明度的区别是什么？

-----

两者均能实现透明效果。

区别：opacity作用于元素，以及元素内的所有内容的透明度；rgba() 只作用于元素的颜色或其背景色，设置rgba()透明的元素的子元素不会继承透明效果。

#### 游览器是如何解析CSS选择器等？

-----

从右向左解析的。

若从左向右匹配，发现不符合规则，需要回溯，会损失很多性能。若从右向左匹配，先找到所有的最后节点，对于每一个节点，向上寻找其父节点直到查找至根元素或满足条件的匹配规则，则结束这个分支的遍历。

在css解析完毕后，需将解析结果css规则树和DOM tree一起进行分析建立Render Tree，最终用来绘图。

#### display:none 和 visibility:hidden 两者的区别

-----

1.display:none 隐藏后不占用文档流；而visibility:hidden 会占用文档流

2.visibility具有继承性，给父元素设置visibility:hidden，子元素也会继承该属性，但如果给子元素设置一个"visibility: visible"，则子元素又会显示出来了

3.visibility: hidden 不会影响计数器的计数，虽然隐藏了，但计数器依然运行着。

4.css3中transition支持visibility属性，但不支持display。因为transition可以延迟执行。配合visibility使用纯css延时显示效果可以提高用户体验

5.display：none会引起回流（重拍）和重绘；visibility：hidden会引起重绘。

#### 简述transform，transition，animation的作用

----

1.transform：元素静态样式，本身不会呈现动画效果。可对元素rotate、扭曲skew、缩放scale、移动translate、矩形变形matrix。常配合transition和animation使用

2.transition：样式过渡。一种效果逐渐改变为另一种效果，是一个合写属性。

```css
{
    transition:transition-property transition-duration transition-timing-function transition-delay;
    /*过渡属性名称、过渡动画所需的时间、加速度曲线（ease-in、ease-out这类）、过渡效果开始之前需要等待的时间*/
}
```

3.animation: 动画，由`@keyframes` 来描述每一帧的样式。

区别：

- transform 仅表示元素的静态样式，常配合transition和animation使用
- transition通常和hover等事件配合使用；animation是自发的，立即播放。
- animation可以设置循环次数，可以设置每一帧的样式和时间，transition只能设置头尾。
- transition可以与js配合使用，js设定要变化的样式，transition负责动画效果。

#### line-height 如何继承？

-----

- 父元素的line-height是具体数值，则子元素line-height 继承该值。
- 父元素的line-height是比例值，如‘2’，则子元素line-height继承该比例
- 父元素的line-height是百分比，则子元素line-height继承的是父元素的font-size * 百分比计算出来的值

#### 如何让chrome支持10px的文字

-----

1.font-size：12px；-webkit-transform：scale（0.84）；

2.font-size：20px；-webkit-transform：scale（0.5）；

#### position属性的值有哪些？

-----

1.static：默认定位。元素出现在正常的文档流中（忽略top，bottom，left，right 或 z-index声明）

2.relative：相对定位。如果对一个元素进行相对定位，将出现在它所在的位置上。然后，可以通过设置垂直或水平位置，使其“相对于”它的起点进行移动。使用相对定位时，无论是否移动，元素仍然占据原来的空间；移动元素会导致其覆盖其他元素。

3.absolute：绝对定位；元素的位置相对于最近的已定位的父元素，如果元素没有已定位的父元素，则相对于根元素（即 html 元素）定位。绝对定位的元素会脱离文档流，不占据空间，会与其他元素重叠

4.fixed：固定定位。元素的位置相对于浏览器窗口是固定位置，即使窗口滚动它也不会移动。固定定位的元素会脱离文档流，不占据空间，会与其他元素重叠

5.sticky：粘性定位。可以被认为是相对定位和固定定位的混合，元素在跨越特定阈值前为相对定位，之后为固定定位。top，right，buttom，left，必须指定这四个阈值中的一个，才可以使粘性定位生效，否则行为与其相对定位相同。

6.inherit：规定应该从父元素继承position属性的值

#### css盒模型？

-----

- 标准盒模型：width指content部分的宽度，总宽度 = width + border（左右） + padding（左右） + margin（左右）；高度同理。
- 怪异盒模型（IE盒模型），width指content + border（左右） + padding （左右）三部分的高度，所以总宽度是width + margin（左右）；高度同理

- **标准盒模型：**width、height的计算范围只包含content
- **IE盒模型：**width、height的计算范围包含content、padding、border

通过`box-sizing`进行设置

- **box-sizing: content-box**标准盒模型（默认）
- **box-sizing: border-box**IE盒模型（怪异盒模型）

#### box-sizing属性

------

1.content-box，对应标准盒模型。

2.border-box，IE盒模型；

3.inherit，继承父元素的box-sizing值。

#### BFC（块级格式上下文）

-----

1.概念：BFC（Block Formatting Context），块级格式上下文。BFC是css布局的一个概念。是一个独立的渲染区域，规定了内部box如何布局，且这个区域内的子元素不会影响到外面的元素。

2.布局规则：

- 内部的 box 会在垂直方向一个接一个的放置(换行)
- box 垂直方向的距离由 margin 决定，同一个 BFC 相邻的 box 的 margin 会发生重叠
- 每个 box 的 margin 左边，与包含块的左 border 相接触（对于从左往右的格式化，否则相反）
- BFC 的区域不会与 float box 重叠
- BFC 是一个独立容器，容器内的子元素不会影响到外面的元素
- 计算 BFC 高度时，浮动元素也参与计算高度

3.如何创建 BFC ？

- 根元素，即 html 元素
- float 值不为 none
- position 值为 absolute 或 fixed
- display 的值为 inline-block、tabl-cell、table-caption
- overflow 的值不为 visible

4.BFC 的使用场景

- 去除边距重叠问题
- 清除浮动（让父元素的高度包含子浮动元素）
- 阻止元素被浮动元素覆盖

#### 让一个元素水平/垂直居中

-----

1.水平居中

- 行内元素：text-align：center
- 对于确定宽度的块级元素
  - width和margin实现：margin：0 auto；
  - 绝对定位和margin-left实现：margin-left:(父width - 子width)/ 2;（前提是父元素相对定位）

- 对于宽度未知的块级元素
  - table标签配合margin左右auto实现
  - inline-block实现：display: inline-block; text-align: center;
  - 绝对定位50%和transform实现，translateX可以移动本身元素的-50%
  - flex布局：justify-content：center

2.垂直居中

- 纯文字类，设置line-height等于height
- 子绝父相，子元素通过margin实现自适应居中。
- 子绝父相，通过位移transform实现
- flex布局，align-items： center
- table布局，父级通过转换为表格形式，子级设置vertical-align实现。

#### flex布局

-----

弹性布局。由container（容器）以及item（项目）组成。通常可用于 水平/垂直居中，两栏，三栏布局等场景。

flex属性，是flex-grow，flex-shrink和flex-basis的简写，默认值为0 1 auto。该属性有两个快捷值：auto（1 1 auto）和none （0 0 auto）

- flex-grow：定义item（项目）的放大比例，默认为0，即如果存在剩余空间，也不放大。如果所有项目的flex-grow属性都为1，则它们将等分为剩余空间（如果有的话）。如果一个item的flex-grow属性为2，其他item（项目）都为1，则前者占据的剩余空间将比其他项多一倍。
- flex-shrink：项目的缩小比例，默认为1，即如果空间不足，项目将缩小。如果所有item（项目）的这个属性都是1，当空间不足的时候，都将等比例缩小。如果一个item（项目）为0，其他为1，则空间不足时，前者缩小。
- flex-basis：定义了在分配多余空间之前，项目占据的主轴空间。游览器会根据该属性，计算主轴是否有多余空间。默认值为auto，即item（项目）本来大小。当设置为0时，会根据内容撑开。也可以设为跟width，height属性一样的值（350px），则项目将占据固定空间。

常用的属性值

- flex: 1 --> flex: 1 1 0%
- flex: 2 --> flex: 2 1 0%

- flex: auto --> flex: 1 1 auto
- flex: none --> flex: 0 0 auto【常用于固定尺寸不伸缩】

----有补充的项目，这点不够，flex-direction都没

#### 清除浮动

-----

1. 直接把 `<div style="clear: both;"></div>`作为最后一个子标签

- 优点：通俗易懂，容易掌握；
- 缺点：会添加较多无意义的空标签，有违结构与表现的分离，在后期维护中将是噩梦

2. .clearfix { overflow: hidden; zoom: 1; }

- 优点：不存在结构和语义化问题，代码量极少
- 缺点：内容增多时容易造成不自动换行，导致内容被隐藏掉，无法显示需要溢出的元素

3. 建立伪类选择器

   ```css
   .fix:after {
       content: '';/* 设置添加子元素的内容为空 */
       display:block;
       height: 0;
       visibility: hidden;
       clear: both;
   }
   ```
#### css中优雅降级和渐进增强有什么区别？

-----

css3流出来的一个概念。低级游览器不支持css3，但css3的效果又很好，所以高级点的游览器用css3，低级的只要保证最基本的功能。二者最关键的区别是它们所侧重的内容，以及这种不同所造成的工作流程的差异。

- 优雅降级：一开始就构建完整的功能，然后针对游览器测试和修复。
- 渐进增强：一开始就针对低版本游览器进行构建页面，完成基本的功能，然后再针对高级游览器进行效果、交互、追加功能以达到更好的体验。

#### img的alt和title的异同？实现图片懒加载的原理

-----

- alt是图片加载失败时显示在网页上的替代文字；title是鼠标放在图片上面时显示的文字，是对图片的进一步描述和说明。
- alt是img的必要属性，title不是。
- 对于网站SEO优先来说，搜索引擎对图片意思的判断，主要是靠alt属性，所以在图片alt属性中以简要文字说明，包含关键字，也算一种优化

懒加载原理：

先设置图片的data-set属性值（其他也行，只要不发送请求就行，作用就是为了存取值）为图片的路径，由于不是src属性，所以没有请求。然后计算页面的scrollTop的高度和游览器的高度之和，如果图片距页面顶端距离小于两者之和，说明图片得加载出来了，这时将data-set属性替换成src属性就行。

#### css sprites（雪碧图/精灵图）

-----

把小图片整合到一张图片文件中的，利用background-image，background-repeat，background-position的组合进行背景定位；

优点：减少图片体积；减少http请求次数

缺点：维护比较麻烦，不能随便改变大小，会失真模糊

#### 什么是字体图标

-----

特殊的字体，显示的跟图片一样，不会变形，加载速度快，就跟文字一样，随意通过css来控制它的大小和颜色，比较方便

#### 主流游览器内核私有属性 css前缀

-----

- mozilla(firefox、flock等): -moz
- webkit 内核(safari、chrome等): -webkit
- opera 内核(opera浏览器): -o
- trident 内核(ie 浏览器): -ms

#### css实现节流

-----

原理：精准控制一个动画，假设有一个动画控制按钮从**禁用->可点击**的变化，每次点击时让这个动画重新执行一遍，在执行的过程中，一直处于禁用状态，就达到了节流的效果。

局限性：仅限点击行为，很多时候，节流可能会用在滚动事件或键盘事件上

```html
<style>
    body {
  		display: grid;
  		place-content: center;
 		height: 100vh;
  		margin: 0;
  		gap: 15px;
  		background: #f1f1f1;
  		user-select: none;
	}
	button {
  		user-select: none;
	}
	.throttle {
  		animation: throttle 2s step-end forwards;
	}
	.throttle:active {
  		animation: none;
	}
	@keyframes throttle {
  		from {
    		pointer-events: none;
    		opacity: .5;
  		}
  		to {
    		pointer-events: all;
    		opacity: 1;
  		}
	}
</style>    
<h4>打开控制台查看</h4>
<button onclick="console.log('保存1')">我是“普通”保存</button>
<button class="throttle" onclick="console.log('保存2')">我是“节流”保存</button>
```

#### vw/vh/px/em/rem/vmin/vmax的区别

-----

- vw和vh：相对于视口的大小，100vh等于视口的高度，100vw等于视口的砍断
- px：固定大小
- em：大小相对于父元素的font-size：20px； 1em = 20px；
- rem：大小相对于html的font-size
- vmin：大小等于最小的视口；100vmin等于最小的视口
- vmax：大小等于最大的视口；

#### 移动端适配方案

----

- rem  vue 项目可以使用一些插件，例如postcss-pxtorem、 flexable.js等 

- 百分比
- vw/vh，vmin/vmax
- flex
- 栅格布局

自适应布局方式

- **媒体查询@media：**通过监听不同的窗口宽度，展示不同的效果
- **rem：**监听不同窗口宽度，为根节点设置对应font-size，并进而使所有使用到rem的样式得到变化（rem的“r”就是“root”）

#### 可继承和不可继承的属性

----

##### 不可继承

- display
- width、height、margin、padding、border
- background、background-color
- position、top、right、left、bottom

##### 可继承

- font-size、font-weight、font-family
- line-height、text-align、color
- visibility
- cursor

#### block和inline的区别？

----

- block：独占一行，可设置宽高、margin、padding
- inline：不独占一行，不可设置宽高，可设置水平margin、padding但不能设置垂直方向margin、padding

#### 隐藏元素的方式

-----

- display：none：直接小时
- visibility：hidden 不可见，但占着位置
- opacity：0 透明不可见，但占位置
- position：absolute 绝对定位并移出可见范围
- z-index：-999 将层级设置在底部，让他不被看见

#### transition和animation的区别

----

- **transition：**过度样式，需要被动触发
- **animation：**动画样式，不需要被动触发，可以自动触发，可结合@keyframe进行多个关键帧的动画

#### link和@import区别

----

- link可以加载css、rss；@import 只能加载css
- link在页面载入时同时加载；@import在页面载入后再加载
- link无兼容问题；@import是新语法，低版本游览器不兼容
- link标签可被js操作dom去除；@import不行

#### 伪元素和伪类

----

- **伪元素：**顾名思义，假的元素，只会显示其内容，但是并不会在dom树中找到他

```css
p::before {content:"林三心";}
p::after {content:"林三心";}
p::first-line {background:red;}
p::first-letter {font-size:30px;}
```

- **伪类：**将一些效果加到节点上，例如鼠标点击，悬浮等

```css
a:hover {color: #FF00FF}
p:first-child {color: red}
```

#### CSS提升性能方式

----

1、css代码压缩

2、link代替@import

3、避免多层选择器

4、动画CPU加速

#### 为何使用less、sass

-----

他们是css预处理器，使用他们的变量、继承、运算、函数等功能，大大提高样式编写效率，大多数打包工具最后都会将他们解析为原始css样式代码

#### postCss是啥

----

后置css，将解析后的css样式代码进行处理，提高其在各个浏览器的兼容性，常用做法是为每个浏览器样式添加特有前缀

#### 单行、多行溢出省略号

- 单行

```css
overflow: hidden; 
text-overflow: ellipsis;
white-space: nowrap;
```

- 多行

```css
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
```

#### 两栏布局

-----

两栏布局指的是，左边固定右边自适应

- **float浮动**

```css
.outer {
  height: 100px;
}
.left {
  float: left;
  width: 200px;
  background: tomato;
}
.right {
  margin-left: 200px;
  width: auto;
  background: gold;
}
```

- **flex布局**

```css
.outer {
  display: flex;
  height: 100px;
}
.left {
  width: 200px;
  background: tomato;
}
.right {
  flex: 1;
  background: gold;
}
```

#### 三栏布局

----

三栏布局指的是左右固定，中间自适应

直接用flex布局

```
.outer {
  display: flex;
  height: 100px;
}

.left {
  width: 100px;
  background: tomato;
}

.right {
  width: 100px;
  background: gold;
}

.center {
  flex: 1;
  background: lightgreen;
}
```

#### 实现一个三角形？

-----

宽度设置0，四个border中，透明掉三个，剩一个显示，就是三角形了

扇形则多设置一个border-radius；

```css
div {
    width: 0;
    height: 0;
    border: 100px solid transparent;
    border-bottom-color: red;
}
```


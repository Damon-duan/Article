一、	设计注意事项：
1、	适配和缩略：

<p>
weex 是如何适配不同大小屏幕的？这个问题涉及到组件在页面上的最终展现。如果你扩展的组件有自定义的属性或者样式，涉及到尺寸大小的，需要非常注意这一块。每个组件在被创建之前，会由 ComponentManager 将当前屏幕的 scale 值注入组件的 data (在除了 constructor 以外的任何组件方法中都可以通过 this.data.scale 访问到)中。那么这个 scale 到底是什么？

weex 中的设计尺寸是 __750px__，也就是说 weex 认为所有屏幕的宽度都是归一化的 __750px__. 当真实屏幕不是 750px，weex 会自动将设计尺寸映射到真实尺寸中去，这个 scale 就是这种映射的比例。它的计算公式是 当前屏幕尺寸 / 750.

所以在扩展组件的时候，如果用户传入一个尺寸值，比如说 375，这个值是相对于 750 的设计尺寸来说的。你只需要将这个值乘以 scale, 就是适配当前屏幕的真实尺寸：value = 375 * this.data.scale. 它应该占据真实屏幕一半的大小。
</p>
 
二、	开发注意事项：
> 样式类：

1、	设定边框，border目前不支持类似这样border: 1px solid #ff0000;的组合写法。

2、	设定背景色，background目前不支持类似这样background: red的写法，需要修改为具体的background-color: red。

3、	weex支持position定位，relative|absolute|fixed|sticky，默认值为relative。

4、	weex目前不支持z-index设置元素层级关系，但靠后的元素层级更高，因此，对于层级高的元素，可将其排列在最后面。

5、	如果定位元素超过容器边界，在Android下，超出部分将不可见，原因在于Android端元素overflow默认值为hidden。

6、	weex支持线性渐变linear-gradient，不支持radial-gradient径向渐变。

7、	weex中box-shadow仅仅支持iOS。

8、	目前<image>组件无法定义一个或几个角的border-radius，只对iOS有效，对Android无效。

9、	Weex中，flexbox是默认并且唯一的布局模型，每个元素都默认拥有了display: felx属性。

10、	文本样式的属性color;font-size;font-weight;font-style;font-family;text-decoration;text-aline;text-overflow(内容超长时的省略样式)；lines(指定文本行数)。

> 组件类：

1、	<a>组件：

A.	weex中a组件定义了指向weex页面打包后的一个js地址。

B.	组件中无法添加文本，需要在其中加上text组件才能添加文本。

C.	此组件支持除了自己外的所有weex组件作为子组件。

D.	支持所有通用样式。

E.	请不要为<a>组件添加click事件。

2、	<web>组件：

A.	<web>组件用于在页面中嵌入一张网页；src用以指定资源地址。

B.	不支持任何字组件。

C.	pagestart，<web>组件开始加载时执行。

D.	pagefinsh，<web>组件完成加载时执行。

E.	error，<web>组件加载错误时执行。

3、	<webview>组件:

一系列<web>组件的操作接口。可以通过调用this.$refs.el来获取元素的引用。

4、	API：

A.	goBack(webElement)，加载历史记录里的前一个资源地址。

B.	goFroward(webElement)，加载历史记录里的下一个资源地址。

C.	reload(webElement)，刷新当前页面。

6、	<list>组件：

A.	<list>组件是垂直列表功能的核心组件。

B.	<cell>组件，列表中的子项，类似于html中的ul和li的关系。

C.	<header>达到屏幕顶部是，吸附在屏幕顶部。

D.	<refresh>给列表添加下拉刷新的功能。

E.	loadmore事件，loadmoreoffset{number}触发事件所需的距离。

F.	scroll事件列表滚动时触发的该事件。

G.	offset-accuracy{number}控制scroll事件发生的频率（默认10px）。

7、	<refresh>组件：

A.	用于提供下拉加载功能。<scroller>和<list>的子组件，只有被这两个组件包含时才能正常展示。

B.	refresh事件，当list组件和scroller组件被下拉时触发。

C.	pullingdown，当list组件和scroller组件被下拉时触发，可以从事件的参数对象中获取dy，pullingDistance，viewHeight，type。

三、	weex和vue的区别：

1、	布局不同，float浮动，weex不支持，推荐使用flex，flex移动端支持;

2、	不支持百分比布局;

3、	不支持综合式写法（border: 1px solid #ccc）;

Border-width: 1px;

Border-style: solid;

Border-color: #bbb;

4、	weex支持px和wx长度单位，vue不支持wx长度单位;

5、	weex不支持img标签，使用的何小程序一样image标签（且weex中image需要设置宽高，才有效果）宽750px，相当于宽100%，高1350px 相当于高100%；

6、	weex不支持router-link,只能使用编程式导航this.$router.push({name: ‘listpage’});

7、	路由的三个模式：hash,history,abstract只有abstract是可以使用的，其他使用都会出现问题；

8、	Weex不建议使用vue-router，过多使用会出现性能问题；

9、	使用wuex注意，vue是单页应用。Weex是多页应用，不在同一个页面获取不到之前的vuex中的数据；

10、	Weex默认使用的盒模型是box-sizing: border-box;padding是不会撑开盒子的；



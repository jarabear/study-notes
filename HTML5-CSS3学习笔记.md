**1. 内联元素**可以设置margin, padding, border, 不过margin和padding只在左右方向上起作用，对input等替换元素在上下方向上依然有效。[关于为何img、input等内联元素可以设置宽、高（替换元素）](http://blog.csdn.net/jlds123/article/details/8647448#comments)。br为行内元素；hr为块级元素。

**2. 样式的四种插入方式：**
`<style type="text/css">
body {backgroud-color: #FF0000}
..
</style>`
or
`<body style="backgroud-color:red;..">`
or
`<link type="text/css" rel="stylesheet" href="index.css"
in index.css file:
body {
backgroud-color: red;
}`
or
`<style type="text/css" media="screen">
@import url("http://www.taobao.com/home/css/global/v2.0.css?t=20070518.css");
</style>`
[link和@import方法的区别1](http://www.daqianduan.com/2417.html),  [区别参考2](https://zhidao.baidu.com/question/1047402758024877979.html)
<br/>

**3.1** 请始终将正斜杠添加到子文件夹。假如这样书写链接：href="http://www.w3school.com.cn/html"，就会向服务器产生两次 HTTP 请求。这是因为服务器会添加正斜杠到这个地址，然后创建一个新的请求，就像这样：href="http://www.w3school.com.cn/html/"
**3.2** [电子邮件链接mailto的用法](http://www.5icool.org/a/201003/308.html)
eg: `<a href="mailto:aaa@mail.com;xxx@mail.com?cc=yyy@mail.com&bcc=zzz@mail.com&subject=party%20invitation&body=pls%20come%20to%20the%20party%20held%20on%20this%20Sat">宴会邀请</a>`
注意用此功能需要本地安装邮件客户端，如果本地已安装，它无法实现跳转自动打开邮件客户端。可以去控制面板-->程序-->默认程序-->设置关联,找到mailto，更改程序，设置成你本地的邮件客户端，就好了。
![这里写图片描述](http://img.blog.csdn.net/20161021152842150)

**4. CSS浮动(float,clear)通俗讲解**

4.1 [float影响div块级元素讲解链接](http://www.html5cn.org/article-5948-1.html)   设置float属性后，脱离正常流。正常流和正常流连在一起，注意div是块级元素，从上到下排列。浮动元素与浮动元素连在一起，若都设置为left，第二个元素跟在第一个的右边。假如某个div元素A是浮动的，如果A元素上一个元素也是浮动的，那么A元素会跟随在上一个元素的后边(如果一行放不下这两个元素，那么A元素会被挤到下一行)；如果A元素上一个元素是标准流中的元素，那么A的相对垂直位置不会改变，也就是说A的顶部总是和上一个元素的底部对齐。
4.2 对于CSS的清除浮动(clear)，一定要牢记：这个规则只能影响使用清除的元素本身，不能影响其他元素，想要谁移动，clear用在谁身上。若浮动元素的父元素不做任何设置，即使设置了父元素的宽高，其里面都没内容，是瘪的。。因为子元素设置了float，把父元素撑起来的方法：[对overflow与zoom”清除浮动”的一些认识](http://www.zhangxinxu.com/wordpress/2010/01/%E5%AF%B9overflow%E4%B8%8Ezoom%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8%E7%9A%84%E4%B8%80%E4%BA%9B%E8%AE%A4%E8%AF%86/) 
清除浮动把父元素撑起来，除了上面链接里面介绍的(**float,absolute,inline-block,overflow,zoom(zoom只对IE6/7有效**)，还有以下两种方法：给在父元素内紧跟着浮动元素的非浮动元素加上clear；或是给父元素的after伪元素(::after)加上clearfix样式(若父元素内只有2个浮动元素），见
方法二：
样式部分：
`          .clearfix::after{       /*在类名为“clearfix”的元素内最后面加入内容，不是元素后面加*/
            content:"";           /*content内容可以为字符串或是图片。。url()*/
            height:0;             /*高度为0*/
            display:block;       /*加入的这个元素转换为块级元素。*/
            clear:both;          /*清除两边浮动*/
            visibility: hidden; /*隐藏，仍占据空间，只是看不到*/
        }
        .clearfix{
            *zoom:1;             /*针对IE6，IE6不支持after伪元素。height:1%; 可能也可以*/
        } `
body部分：
`<div class="container clearfix">
    <div class="div1">div1</div>
    <div class="div2">div2</div>
    <!--<div class="div3">div3</div>-->
</div>`
方法一：直接给div3加上clear:both

4.3 float: left/right可实现文字环绕，加上position:absolute/fixed后失效。float实现文字环绕而非覆盖文字的原因：[文字围绕浮动之谜](http://blog.sina.com.cn/s/blog_83958bc601010y81.html) （浮动占据行框空间（文字图片），不占据块空间。当加上position:absolute/fixed后，彻底浮起来脱离了，行框空间也不占据了，文字环绕的效果消失,变成文字被覆盖了。当float后面的非浮动元素设置display:inline-block/inline后，其“没有”块空间，故浮动元素不会占据其任何空间，其与浮动元素紧挨着，没有被覆盖任何空间）
4.4 margin和padding对于float元素依然起作用，可用来调节float元素的位置。浮动元素会压缩其size到其里面包含东西（文字/图片）的大小，即其宽度会自适应其内容，position为absolute和display为inline-block也是这样.float在position为absolute或是display:none时失效，但是其size仍为其内容的size。
4.5 当一个内联元素浮动的时候会变成一个"块级"元素（相当于inline-block;），只占据行框，可以给其添加width,padding,margin， 这些也占据行框。
<br/>
**5. Position的relative和absolute属性值详解**

[十步图解CSS的position](http://blog.jobbole.com/49320/)
[postion为absolute用于文字阴影效果和自适应布局（图片旁有文字）](http://www.zhangxinxu.com/wordpress/2010/01/absolute%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E7%9A%84%E9%9D%9E%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E7%94%A8%E6%B3%95/)
如果你给某个元素指定了postion的值为“relative”，那么你就可以通过“T-R-B-L”(也就是top,right,bottom,left)来设置元素的定位值。如果你给元素指定了absolute，整个元素就会漂出文档流，而且元素自身的物理空间也同时消失了。关于绝对定位元素(absolute)的定位是相对于包含块(containing block)来说的，关于包含块的定义：
[w3c中containing block的规定](https://www.w3.org/wiki/CSS_absolute_and_fixed_positioning#Containing_blocks)
![这里写图片描述](http://img.blog.csdn.net/20161101160829037)
<br/>
6. [浅谈CSS3中单位**px,em,rem**的区别与优劣](http://wenku.baidu.com/link?url=DpolT1okOoEdwzI99PE3nEwBitkl1GGrM8fI9V4ffWED5OWmTMQKuquJoae_0tT45jz5ywE4O3brHHSVOz5_rkOtLaS56NFjosdqOyPZNMK) em相对父元素的字体大小，rem相对根元素html的字体大小。
7. 代码: `<code>`; 代码块:`<pre>`; 变量:`<var`;输入:`<kbd>`;输出:`<samp>`
8. **双飞翼布局、圣杯布局**（三列布局）
下边代码可以实现三列布局，但是left和right始终占据middle的padding：
![运用了box-sizing](http://img.blog.csdn.net/20161106134546335)
圣杯布局：把container的内容移到中间
**5. 表格属性**

5.1 在给table加边框时，html中直接`<table border="1">`,也会给th，td加上边框，这里的border规定表格边框的宽度，值越大，表格的边框越粗，但是th,td的边框不会变粗。在css样式中，要写`table, th, td {border: 1px solid black}`或其他border-width, border-style和border-color，如果不规定border-style(默认值为none)，即使有width，边框也不会出现。
5.2 在html中，运用col标签的aligh属性使每列居左中或右，对于th标签无效，td可以。在css样式中，是设置td/th的text-aligh属性。
5.3 在html中，可以运用table的cellpadding和cellspacing来规定内容和边框以及边框之间的距离。在css样式中，是用td/th的padding和table的border-spacing。
5.4 对于空单元格边框是否出现，html中用是否加`&nbsp;`，在css样式中用table的empty-cells属性值为show或是hide, 如html中使用了`&nbsp;`, css中的empty-cells为hide无效，会有边框；`&nbsp;`空单元格，其属于占位且不可见。也可以使用tr或td的visibility属性设置为collapse或hidden来使有内容或无内容的单元格消失不见。
【设置table的border-spacing和empty-cells时，注意其border-collapse属性值需为separate，不能为collapse】
<br/>
**6. 表单的get和post方法**比较：
![这里写图片描述](http://img.blog.csdn.net/20160311131251510)
<br/>
**7. localstorage/sessionstorage**操作

[html5的localstorage和sessionstorage](http://www.cnblogs.com/yuzhongwusan/archive/2011/12/19/2293347.html)

【不但可以用自身的setItem,getItem等方便存取，也可以像普通对象一样用点(.)操作符，及[]的方式进行数据存储。提供的key()和length可以方便的实现存储的数据遍历】
`var str = "<table border='1px'>";
    for(var j=0; j<sessionStorage.length; j++) {
        var tm = sessionStorage.key(j);
        var val = sessionStorage.getItem(tm);
        str += "<tr><td>"+tm+"</td><td>"+val+"</td></tr>";
    }
    str += "</table>";`
    <br/>
**8. 切图工具fireworks和PS比较：** http://www.jb51.net/photoshop/23509.html
<br/>
**9. [JS操作JSON总结]**(http://www.cnblogs.com/worfdream/articles/1956449.html)
{
"employees": [
{ "firstName":"Bill" , "lastName":"Gates" },
{ "firstName":"George" , "lastName":"Bush" },
{ "firstName":"Thomas" , "lastName":"Carter" }
]
}
<br/>
**10.  text-decoration-style**: dotted 等浏览器不支持

[text-decoration新特性表现](http://www.zhangxinxu.com/wordpress/2015/06/know-css-text-decoration-style-color-ship/)
<br/>
**11. background的css属性**
11.1 [如何用雪碧图中的一个小图标](http://zhidao.baidu.com/link?url=wbB0M9FHXmsM8gUCMequGj2qD94kRSBD0lCs0o26r66LyeJY7SyojdHvTbc0fQpYptG4ui7d9r7MjxEb-96nra) (用background-position)
11.2 backgound-color属性不能继承，默认值transparent. 
`background:url("1.jpg") fixed center no-repeat;`
    等同于
    `/*background-image: url("1.jpg");*/
    /*background-attachment: fixed;*/
    /*background-position: center;*/
    /*background-repeat:no-repeat;*/
}`
11.3
`div{
    width:80%;
    height: 500px;
    border:50px solid black;
    padding:100px;
    background:url("1.jpg") ;
    background-size:100% 100%;
    background-repeat:no-repeat;
    /*background-image: url("1.jpg");*/
    /*background-attachment: fixed;*/
    /*background-position: left top;*/
    /*background-origin: content-box;*/
}`
如果以百分比规定尺寸**background-size**，那么尺寸相对于父元素的宽度和高度，这里为div。size为100% 100%时最好，自适应box的大小。size为cover和contain时，见[background-size为cover和contain](http://sentsin.com/web/939.html) 
11.3 **background-origin**与**background-clip**的区别：origin是从border，padding和content开始100% 100%的显示，每次都是完整的图片。clip是开始图片100% 100%的显示，border会把在border外的部分去掉;padding会把在border的部分cut掉；当clip的值为content-box时，会把在border和padding的部分cut掉，只留下图片在content-box的部分。
11.4 **background-position**当其为left或right时，表示left center/right center，在最左/右边的中间，left top 10px表示贴着左边离上面10px（left 0 top 10px)。center center/50% 50%，背景图的中心与图像的中心重合，在离上下左右都居中的位置。图片默认情况下其左上角与承载其的元素左上角重合，设置background-position:-10px 10px,即把背景图片往左移10px,下移10px。背景图片比承载其的元素大，只显示背景图片在元素内部的画面。
<br/>
**12. width, height, padding, margin, border**
**box-sizing**: border-box(width包括了border和padding和content），为**怪异盒模型**，一般用于mobile开发，先规定外面的“鞋盒”width和height，再调节至里面的“鞋子”，从外到里。box-sizing；content-box(width只包括content），设置size从里到外，一般用于pc。[MDN中关于box-sizing的定义](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)
![content-box,padding-box,border-box,margin-box各范围图](http://img.blog.csdn.net/20161102220019389)
正常盒子与怪异盒模型code：
![这里写图片描述](http://img.blog.csdn.net/20161102222956223)

用css设置网页布局时，注意width 和 height 指的是内容区域的宽度和高度（即默认为content-box）。增加内边距、边框和外边距不会影响内容区域的尺寸，但是会增加元素框的总尺寸。可以用`border:1px solid black`把每个div框起来，利于了解当前div位置，给每个元素加属性时，想清楚该属性到底加在父元素上还是子元素上。
<br/>
**13. 弹性盒模型**（display:-webkit-box(旧)  &  display:flex(新)）
见[伸缩盒子（旧、新）](http://www.css88.com/book/css/properties/flex/flex.htm)
[**align-items**与**align-content**的区别（主要前者针对每个item在其所在单行中的表现；后者针对所有多行中的items在整个container里面的表现）](http://stackoverflow.com/questions/31250174/css-flexbox-difference-between-align-items-and-align-content)
<br/>
**13. CSS设置图片位置的方法**
若设置图像居中，可以在img外面加一个div，设置div的margin:0 auto;让div中的img居中也可设置div的text-align:center。或是设置img的display:block; margin: 0 auto。使图像居于任何位置，可以设置img的position:relative; 再通过left, top, bottom或是right来设置到各边的距离。使图像居左或居右，可以设置img的float: left/right或text-align:left/right或position的left:0/right:0。可以使用margin来调整img的位置，对其直接用margin:0 auto无用。
<br/>
**14. line-height 属性设置行间的距离**
在大多数浏览器中默认行高大约是 110% 到 120% or 20px or 1。 不允许使用负值。用%，number(0.5/2)和length(px)赋值。
<br/>
15. **display：block**；比较常用于a和span这两个标签，因为他们不是块级元素，定义display：block或inline-block属性后，定义width、height等和长宽相关的css属性才会生效。此元素将显示为块级元素，前后会带有换行符（给链接a加方框）[a标签display为block和inline-block的说明](http://yangjunwei.com/a/554.html#respond)
[关于如何去除inline-block之间的间距--资料1](http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/)
[关于如何去除inline-block之间的间距--资料2](https://css-tricks.com/fighting-the-space-between-inline-block-elements/)
<br/>
**16. 列表中ol和ul的样式**（ul,ol,li都是块级元素）

16.1 ol的属性常用的有以下几个，注意reversed和start都是针对数字，直接用于html中。在css样式文件中用link-style-type，值有decimal，lower-alpha，lower-roman等可供选择
![ol的属性](http://img.blog.csdn.net/20160307023929816)
16.2 ul一般用css样式去规定其type，属性为link-style-type，值有square，circle，disc，也可用图片link-style-imag![这里写图片描述](http://img.blog.csdn.net/20160420194028476)e
[在ul和li中定义css属性的区别](http://zhidao.baidu.com/link?url=6b3GRKqZoJtL45Lm83E18Urd58k7XfwYsjETYYzZBi274_voQNmbI6IHdpmojZMXckcZe5kADEDaYpgA4nmaxa)
16.3 **用ul和li制作下拉菜单**：
16.4 如下情况，li中的a(display:block）不会居中，ul中的li前面有空白节点。

<br/>
17. css中**vertical-align**可能的值：
![这里写图片描述](http://img.blog.csdn.net/20160307140422572)
vertical-align只用于内联元素或是table-cell元素 
`.img1{vertical-aligh: text-top;}
  .img2{vertical-aligh: text-bottom;}`
  或直接在html中，`<img align="top"/"middle"/"bottom"/"left"/"right" src="" />` 下图1是aligh为top, 2中align是bottom。(这种不推荐使用）
![这里写图片描述](http://img.blog.csdn.net/20160307140758889)
<br/>
18. 使段落的**首字母浮动于左侧**
**span**标签用于组合文档中的行内元素，单独改变span标签内的文字的样式。用`<p><span>T</span>his is a text.</p>`
`span
{
float:left;                                          
width:0.7em;
font-size:400%;
line-height:80%;
}`
![这里写图片描述](http://img.blog.csdn.net/20160307223210524)
<br/>
**19. display：none和visibility：hidden的区别**
none表示完全没有了，株连子孙后代, 当前的位置会被后面的元素补上来。hidden仅仅是隐藏, 但是它的位置会保留，虽然子孙后代也看不到了，但是如果子孙里面加一个visibility: visible，即可见。
[css元素隐藏方法及none和hidden的区别](http://www.zhangxinxu.com/wordpress/2012/02/css-overflow-hidden-visibility-hidden-disabled-use/)
<br/>
20. 创建水平菜单的方法：http://www.w3school.com.cn/tiy/t.asp?f=csse_float5                  ????方法总结，应该还有
<br/>
21.1 不同于**类选择器，ID 选择器**不能结合使用，因为 ID 属性不允许有以空格分隔的词列表。如`<p class="urgent"..><p class="important"..><p class="urgent important"..>` 第三个p可以用前2个的样式，还可以设置.urgent.important的style，这个只能第三个p使用。id选择器没有这种用法。
21.2 属性选择器各符号参考：
![这里写图片描述](http://img.blog.csdn.net/20170206150246486?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hhbmdiZWFy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
<br/>
22. CSS（**cascading stylesheet**层叠样式表 [cascade的概念](http://www.nowamagic.net/librarys/veda/detail/1531) ）是为了解决内容与表现分离的问题。cascade, 层叠级联，为每个样式规则指派权重，若元素应用了多个样式规则，那么将优先使用权重最高的的样式，当然，样式也可继承，如color等属性。css选择器是从右向左进行匹配的，注意 css选择器如何书写更高效，可参见[html/css书写规范](http://nec.netease.com/standard)。
若一个元素有多个class，在css样式表中后定义的会覆盖前面的。见[css 关于同一个类里多个类名的优先级](http://www.zhihu.com/question/28976590) 及 [同一元素应用多个class的优先级的测试（关于!important）](http://blog.csdn.net/szwangdf/article/details/4219878)
下图为选择器与权重关系图：
!important(10000) > inline style{}(1000) > id选择器(100) > 类/属性选择器(10) > 标签(1) > *(0)
![css选择器权重](http://img.blog.csdn.net/20160308160946361)
</br>
23. z-index position opacity http://www.wufangbo.com/z-index-na-xie-shi/   z-index只能在定位元素上生效（position为relative或是absolute）。   position是不是一定要设置成absolute才出现到最前面。试试transform: rotate(30deg).. transition: ...
24. 水平导航栏 w3school(css)  照片墙 照片透明
25. 当width:0;height:0，设置border时，会出现border合并,border两两合并。用CSS制作三角形（箭头）的方式：
`
<style type="text/css">
        .div1{
            margin-left:100px;
            margin-top:100px;
            width:0;
            height:0;
            border:30px solid red;
            /*border-width:20px 50px;*/
            /*border-left-color:black;*/
            /*border-right-color:red;*/
            /*border-top-color:yellow;*/
            /*border-bottom-color:blue;*/
            /*border-style: solid;*/
        }
        .dengyao {
            border-top: 0;
            border-left-color: transparent;
            border-right-color: transparent;
        }
        .zhijiao{
            /*border-left-color: transparent;*/
            /*border-bottom-color: transparent;*/
            border-left:0;
            border-top:0;
            border-right-color: transparent;
        }
        .xiejiao{
            border-top: 20px solid transparent;
            border-bottom: 20px solid transparent;
            border-right: 8px solid black;
            -webkit-transform-origin: top right;
            -webkit-transform: rotate(-60deg);
        }
    </style>
 `
![这里写图片描述](http://img.blog.csdn.net/20160407223944095)
</br>
26.1 查看html/css/js的各元素属性或方法在各版本浏览器中的兼容支持情况，可在网站[can i use](http://caniuse.com/#index)中查看。
26.2 IE与html5新特性兼容性问题：可在head标签中加入：
![这里写图片描述](http://img.blog.csdn.net/20160419120609724)
或把html5.js下载下来，直接拖到project下，再加src。
26.3 同一css属性在不同浏览器中显示效果不一样，可用css hack解决，但尽量少用。[关于css hack](http://blog.csdn.net/freshlover/article/details/12132801)
[CSS Hack大全-教你如何区分出IE6-IE10、FireFox、Chrome、Opera](http://www.jb51.net/article/50116.htm)
css hack例子：
.content .test {
    width: 200px;
    height: 200px;
    background: #f60; /*all*/
    background: #06f9; /*IE*/
    *background: #666; /*IE6,7*/
    _background: #ccc; /*IE6*/
}

/* webkit and opera */
@media all and (min-width:0){
    .content .test {
        background: #0f0;
    }
}

/* webkit */
@media screen and (-webkit-min-device-pixel-ratio:0) {
    .content .test {
        background: #ff0;
    }
}

/*FireFox*/
@-moz-document url-prefix() {
    .content .test {
        background: #f0f;
    }
}

/*IE9+*/
@media all and (min-width:0) {
    .content .test{
        background: #f009;
        }
}

/*IE10+*/
@media screen and (-ms-high-contrast: active), (-ms-high-contrast: none) {
    .content .test {
        background: #0ff;
    }
} 
26.4 测试IE各版本兼容性，可以打开自带的最高版本IE浏览器，点开F12开发者工具，右下角有个选择不同版本的文档模式的图标，从而进行测试。
26.5 有些浏览器版本对css3的新属性不兼容，加前缀-webkit（Safari、Chrome）或是-moz（firefox）等，IE浏览器可用ie-css3.htc或是filter。具体见：[IE中的CSS3不完全兼容方案](http://www.cnblogs.com/platero/archive/2010/08.html) 和[ie-css3.htc介绍](http://www.zhangxinxu.com/wordpress/2010/04/%E8%AE%A9ie6ie7ie8%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81css3%E5%B1%9E%E6%80%A7/)
26.6 解决IE7/8 background-size不兼容问题: 
behavior: url(backgroundsize.min.htc);
</br>
 27.  [引入css样式用link还是@import](http://blog.163.com/z_h_chen/blog/static/160134987201041605514402/)
</br>
 28. &copy; : `&copy;`    &yen; : `&yen;` [符号实体参考](http://www.w3school.com.cn/tags/html_ref_entities.html)
 </br>
 29. input与input之间或是input与button之间有小空隙，可以不换行把它们写在一行，可去掉空隙。如：
`<form>
	<input type="text" name="searchkey" /><button>百度一下</button>
 </form>`
 ![这里写图片描述](http://img.blog.csdn.net/20160420000007579)
可直接在css样式中定义input的长宽。加上vertical-align:middle使input与button在同一水平线上（？？？），注意它们的宽高。
![这里写图片描述](http://img.blog.csdn.net/20160420000311873)
</br>
 30. [html中meta标签属性介绍](http://www.haorooms.com/post/html_meta_ds)
[meta中name为viewport的深入理解](http://www.cnblogs.com/2050/p/3877280.html)
 **33.    雪碧图**(http://www.cnblogs.com/scgw/archive/2011/03/19/1988908.html)
 34. 给同一个元素加**多个box-shadow**用逗号隔开。让一个元素的边框四周和边框里面的四周都有shadow: eg:box-shadow: 0px 0px 10px rgba(153,153,153,0.8), 0px 0px 10px rgba(0,0,0,0.5) inset; <br/>
 35. border-radius对设置了collapse的元素无效（？？不确定）：
![这里写图片描述](http://img.blog.csdn.net/20160421152745724)
[border-radius介绍](http://www.kuqin.com/shuoit/20141014/342620.html)
![这里写图片描述](http://img.blog.csdn.net/20160421175323985)
th/td设置border-radius后，table呈现出效果，但当border-collapse被设置为collapse后，chrome/firefox的border-radius失效，IE/Edge仍然有效。当td/th加上display为inline-block后，chrome/firefox中radius有效，但是border与它周围元素border重叠，效果看起来也不太好。table设置border-radius后，有效果，设置collapse后，效果在所有浏览器中都消失，加上display:block后，效果重现。
 36. div水平垂直居中：a. 父元素position:relative;子元素`position:absolute;top:0;bottom:0;left:0;right:0;margin:auto;` b. 父元素：`display:-webkit-box;-webkit-box-pack:center; -webkit-box-align:center;` c. 父元素: `display:flex; justify-content:center; align-items:center;` d. 父元素position:relative;子元素`position:absolute; top:50%; left:50%; margin-top:子元素1/2*height; margin-left:1/2*width; ` e. 一个block中只有一行文字，使其垂直居中，使其`line-height:height；`f. 使一block中子元素垂直居中,设置这个父元素`display:table-cell;设置子元素且需是内联元素 vertical-align:middle;` (**vertical-align** only applies to inline elements and table-cell elements)。
 http://blog.csdn.net/wolinxuebin/article/details/7615098
 37. 
 38. [input不同的type对应的重要的属性](http://blog.sina.com.cn/s/blog_67923a080100ibjq.
 39. 直接写`<option value>`，当用户不选中男女时，submit后会有请选择性别的提示。
`<select name="sex" required>
           <option value>请选择...</option>
           <option value="male">男</option>
           <option value="female">女</option>
      </select>`
    </br>
    40. RGBA是定义一个颜色的红绿蓝值和这个颜色的透明度。opacity是定义一个元素的透明度，这个元素里面的内容也会跟着变透明。
    41. transform执行顺序从右至左：`-webkit-transform:  translateX(-80px) rotateY(90deg);`先rotate后translate。
    42. [IE6/IE7下：inline-block解决方案](http://www.cnblogs.com/leejersey/archive/2012/07/11/2586506.html)
    43. [Maximum and Minimum Height and Width in Internet Explorer](https://perishablepress.com/maximum-and-minimum-height-and-width-in-internet-explorer/)          http://www.cnblogs.com/nidilzhang/archive/2009/12/25/1632289.html                                               http://www.cnblogs.com/pigtail/archive/2012/06/28/2568646.html
<br/>
44. DTD文档模型，一般PC端用XHTML 1.0 transitional:
`<!DOCTYPE html
        PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
        "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">`
 移动端（web app/h5页面）用html5:
 `<!doctype html>
<html>` [极客学院1.3 19:19]
[如何选择用transitional还是stric的DTD文档模型](http://www.cnblogs.com/weisenz/archive/2012/03/31/2426627.html)
`<!DOCTYPE>` 声明不是 HTML 标签；它是指示 web 浏览器关于页面使用哪个 HTML 版本进行编写的指令。
在 HTML 4.01 中，<!DOCTYPE> 声明引用 DTD，因为 HTML 4.01 基于 SGML。DTD 规定了标记语言的规则，这样浏览器才能正确地呈现内容。
HTML5 不基于 SGML，所以不需要引用 DTD。
[关于SGML XML HTML5的关系](http://www.oschina.net/news/56236/40-important-html-5-interview-questions-with-answers)
[w3c中关于doctype的介绍](http://www.w3school.com.cn/tags/tag_doctype.asp)
[关于DTD的介绍（定义合法的文档构建模块，规定有哪些元素、属性，它们又是什么类型，等等）](http://www.phpstudy.net/e/dtd/dtd_intro.html)
<br/>
45. [关于浏览器window、document、html、body高度的探究](http://www.tuicool.com/articles/aUNNVnr)
[对html与body的一些研究与理解](http://www.zhangxinxu.com/wordpress/2009/09/%E5%AF%B9html%E4%B8%8Ebody%E7%9A%84%E4%B8%80%E4%BA%9B%E7%A0%94%E7%A9%B6%E4%B8%8E%E7%90%86%E8%A7%A3/)
46. [关于`<?xml version="1.0" encoding="UTF-8"?>`](http://blog.sina.com.cn/s/blog_9645cd2f010151x3.html)
47. 给tag的title或是bookmark中的title前面加上icon用link标签引入favicon：http://www.jb51.net/web/16645.html
 `<link href="favicon.ico" rel="icon" type="image/x-icon" /> 
<link href="favicon.ico" rel="shortcut icon" type="image/x-icon" /> 
`
48. 用rgba,2个相同颜色透明度的叠加在一起，叠加部分颜色加深
![这里写图片描述](http://img.blog.csdn.net/20170124122851084?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hhbmdiZWFy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170124123058319?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hhbmdiZWFy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
<br/>
49. [浏览器加载、解析、渲染的过程](http://blog.csdn.net/XIAOZHUXMEN/article/details/52014901?locationNum=1&fps=1)。  [关于高性能HTML5](http://m.blog.csdn.net/article/details?id=50354060)
<br/>
**50. 切图**
50.1 图片在网页中显示的两种方式，用img标签或是用background-image。一般用img标签来引入logo或是海报之类的，方便搜索引擎找到图像，抓取数据。一般切图切logo时，需要背景透明，如果边缘模糊有渐变色有阴影，保存为png, 若边缘清晰单色对其要求不高或是想保存动态图，可用gif，gif格式的size小一些，gif可用于做后台统计日志。颜色比较丰富的大图一般存储为jpg格式的，不选择png（size太大了）。切图出来的图片大小一般为200k--500k之间，太小了效果不好太大了加载慢。存储大图保存为jpg格式的时候选用存储为web所用格式可压缩图片，还可以降低品质，出来的文件大小会更小一些。
![这里写图片描述](http://img.blog.csdn.net/20170208002226214?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hhbmdiZWFy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
50.2 svg转font face
<br/>
51. **关于margin对background-color的影响**
如果div1中有一个子元素div2，div1设有background-color，设置div2的margin,其margin部分不会显示background-color，可以设置div1的overflow为hidden或是设置div1的padding。即当一个元素div2包含在另一个元素div1中时（假设div1没设置padding或border与div2的margin隔开），他们的顶和或底的margin会发生叠加。见[overflow触发BFC，BFC阻止折叠](https://segmentfault.com/q/1010000000613827)
<br/>
52. 当鼠标滑过一个元素，想让其呈现变暗的效果，可以在其内最后加上一个i元素，设置i的样式如下：
```
a:hover i{
    opacity: .2;
}
a i{
    /*display:inline-block;*/
    position: absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    background-color: #000;
    opacity: 0;
    transition: opacity .8s;
    -webkit-transition: opacity .8s;
}```
<br/>
**53.** 通常，**transition**要想实现动画通常需要由hover伪类来触发，否则在页面加载的时候它已经运动完毕，保持运动的末态，这并不是我们想要的。**animation**不一样，它拥有更多的表现形式，使其看起来像与生俱来，天生就会动一样。[CSS3中的Transition过度与Animation动画属性使用要点](http://www.jb51.net/css/461831.html)
用css3 imation实现类似图片轮播：(animation中从50%开始设置，可以延长每次动画循环之间的间隔）
```<div class="div1">
	<div class="div2">
		<a></a>
		<a></a>
		<a></a>
		<a></a>
		<a></a>
		<a></a>
	</div>
</div>	```
css样式部分：
```.div1{
    width:1000px;
    height:200px;
    overflow:hidden;
}
.div1 .div2{
    width:2000px;
    height:200px;
    box-sizing: border-box;
    animation: infomove 6s ease 2s infinite;
}
@keyframes infomove {
    50%  {margin-left:0; }
    100%  {margin-left:-1000px;}
}
@-webkit-keyframes infomove {
    50%  {margin-left:0; }
    100%  {margin-left:-1000px;}
}
.div1 .div2 a{
    float:left;
    width:320px;
    height:200px;
    margin-right:20px;
    text-decoration: none;
}```
<br/>
54. bootstrap中的aria-hidden="true"就是使可以读屏的设备读不出加了这个属性值的元素的内容。aria-label属性值就是想要读屏设备读出来的内容。见 [aria-label及aria-labelledby应用](http://accessibilityunion.org/archives/808)
![这里写图片描述](http://img.blog.csdn.net/20170216104429613?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hhbmdiZWFy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)   

关于aria-各属性及其属性值/role各属性值含义，见[WAI-ARIA无障碍网页应用属性完全展示](http://www.zhangxinxu.com/wordpress/2012/03/wai-aria-%E6%97%A0%E9%9A%9C%E7%A2%8D%E9%98%85%E8%AF%BB/) 或  [w3c中关于WAI-ARIA各属性的介绍](https://www.w3.org/TR/2011/CR-wai-aria-20110118/states_and_properties#attrs_widgets)
<br/>
55. css中可以继承的属性
所有元素可继承：  visibility和cursor。
内联元素可继承：
letter-spacing、word-spacing、white-space、line-height、color、font、 font-family、font-size、font-style、font-variant、font-weight、text- decoration、text-transform、direction。
块状元素可继承：text-indent和text-align。
列表元素可继承：list-style、list-style-type、list-style-position、list-style-image。
表格元素可继承：border-collapse。
56. 浏览器内核：IE(IE内核) firefox(Gecko) chrome(WebKit) safari(WebKit) opera(Presto)
57. 标准盒模型中，box-sizing:content-box，即width/height为内容的宽高；怪异盒模型中，box-sizing:border-box, 即width/height为内容宽高+padding+border。
[ 浏览器标准模式和怪异模式](http://blog.csdn.net/xujie_0311/article/details/42044277)
58. HTML语义化就是根据内容选择合适的对应的html标签,便于开发者/机器/搜索引擎读取抓取信息。网站性能优化: [你如何对网站的文件和资源进行优化？](http://blog.csdn.net/xujie_0311/article/details/42418797)
<br/>
59. IE6常见bug解决方法<br/>
60. 当一个子元素的大小超过父元素的content大小（width/height)时，在父元素的padding部分也有显示，即子元素不是仅显示在父元素的content area。当设置父元素的overflow为hidden/scroll时，也是隐藏/截断子元素超出父元素padding edge的部分，scrollbar显示在border内padding外部分。[见此应用](http://zihua.li/2013/12/keep-height-relevant-to-width-using-css/)<br/>
61. 响应式布局设计中，如百度搜索框，想要button固定宽度，搜索框随着屏幕伸缩而占据剩余空间，可以在它们外面包一个div，设置其margin:0 16px; display:-webkit-box;（旧伸缩盒子），然后设置搜索框-webkit-box-flex:1，这样可以使屏幕不论如何伸缩，外面的div两端始终有16px的与屏幕边缘的间距，且搜索盒子占据除button的剩余所有空间(box-flex:1起作用)。还可以通过设置div的overflow为hidden & position:relative，搜索框的width:100%;position:absolute;right:width of btn来实现。<br/>
62. [CSS实现单行、多行文本溢出显示省略号（…）](http://www.daqianduan.com/6179.html)  <br/>
63. 可单独给input或其他表单元素的placeholder和selection（被选中部分）设置样式，用伪对象选择符::placeholder和::selection。需要注意的是，除了Firefox是 :-moz-placeholder，其他浏览器都是使用 ::[prefix]input-placeholder。详情参见 [伪对象选择符](http://www.css88.com/book/css/selectors/pseudo-element/placeholder.htm)。<br/>
64. 

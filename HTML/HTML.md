# HTML

## 1.meta的用法和结构

```html
<!--<meta>标记在<head>部分-->
<meta charset="" http-equiv="" name="" content=""/><!-- 这样是一些字符写在一起 -->
<meta charser="UTF-8"/><!--定义字符编码集-->
<meta name="keywords" content=""/><!--为搜索引擎关键字-->
<meta name="description" content="  "/> <!--定义对于网页的基本描述 -->
<meta name="revised" content="  "/><!--定义网页的最新版本-->
<meta http-equiv="refresh" content="5（注释，秒）"[url="资源路径"]/>
<!--不写url表示每隔5秒钟刷新一次页面否者跳到资源网址-->
```



## 2. HTML 5 标记

**注意：**

- **除了a标签以外的所有内联都不能嵌套块级标签**

- **块级标签可以嵌套任意标签,但是p不能嵌套块级标签**,因为嵌套关系如果P标签里面是块级就会把P标签分割成

  两个独立的P标签,那个块级元素就在中间



### 2.1 文本标签

< pre >< pre />标记保留了文本的所有换行与空

< hr >< /hr >用法与< br >相同，只是在中间加了一条水平线

< cite>< /cite>标记用来定义作品的标题，从视觉效果上表现为斜体形式

< ins>< /ins>表示插入的文本

< dle>< /dle>表示删除的文本

< sub>< /sub>定义下标文本    < sup>< /sup>定义上标文本 

 **例子：**如上标文本可用来表示方程的平方等，下标用做方程的下标																	 

< ruby >< /ruby >标记需要定义被旁注的文本 

**1.< rt></ rt> 定义文本的注音或解释 （用做直接出现在文字上方）**

**2.< rp>< /rp>出现在文字右方（用做<rt>不被浏览器支持的时候）**



**文本可编辑属性contenteditable**

该属性可以让在页面不能够被编辑的内容变成像表单一样的可编辑状态

需要在对应的标签上加上contenteditable=true的属性



### 2.2 列表

< ul >< /ul >表示无序列表

< ol >< /ol >表示有序列表  

**1.< ol reversed="reversed">能够使序号倒序 **

**2.ol标签有一个type属性,默认是让序号为阿拉伯数字,如果要是其他的样子需要用其他的字母作为值type="a"就是让序号为abcd这种,也可以写罗马数字这种**

< dl >< /dl >表示描述列表

**1.< dt>< /dt>标记定义列表项（相当于标题）**

**2.< dd>< /dd>标记用于描述列表中的项目**

```html
<dl>
<dt>  </dt>
<dd>  </dd>
<dd> </dd><!--可以自动换行-->
<dt>  </dt>
<dd>  </dd>
<dt>  </dt>
<dd>  </dd>
</dl>
```



## 3.媒体

### 3.1 图片标签

```html
<img src="" alt="" height="" width="">
<!--如果只设置了宽和高的一个,另外一个会等比例自动变化-->
```

**注意：如果只填了高或者宽，则另一个会按照比例变化**



### 3.2 视频标签

```html
<video src=" " controls="controls" autoplay="autoplay" width=" " height="" preload=" " loop="loop">您的浏览器不支持video(当浏览器不支持video时显示)</video>
<!--这是视频的标准语法,标签中的文字是当视频加载不成功的时候才会显示出来的-->
<!--
	controls="controls"添加浏览器为视频设置的默认控件 
	autoplay="autoplay"设置网页中视频加载就绪后自动播放 
	loop="loop"设置媒介文件循坏播放
	preload="auto/none/meta" 值:1.none表示不加载任何视频 2.meta表示只加载元数据（长度，尺寸等 3.auto表示让浏览器自己决定怎么做（如果引用了autoplay属性，则忽略该属性）
	poster='' ‘’指定加载视频时要显示的图像，接受所需图像的URL(如果引用了autoplay属性，则忽略该属性)	使用方法：<video poster="网址"></video>
	muted="muted" 设置是否静音
-->
```



### 3.3 音频标签

```
<audio src="url" controls="controls" autoplay="autoplay" preload=" " loop="loop">浏览器不支持audio</audio>
```



### 3.4 兼容写法

#### 3.4.1 source兼容

**< source>< source>可以连接不同的媒体文件**

```html
<video width="" height="" controls="controls">
 	<source src=" " type="">
 	<source src=" " type="">
	<source src=""  type="">           
	浏览器不支持video元素
</video>
<!--
	type用于指定视频类型 一般有三种格式所以type一般为video/webm video/mp4 video/ogg
	兼容原因：由于全球五大浏览器只支持各自的视频，source能在其中挑选出一个符合该浏览器格式的视频播放
-->
```



#### 3.4.2 插入flash文件

用embed标签实现对flash文件的插入

```html
<embed src="url" width="" height="" type="">
```



#### 3.4.3 figure标签

```html
<figure></figure><!--规定独立的流内容，将其从网页中移除不会对其他内容产生影响-->
<figcaption></figcaption><!--代表<figure>中的一个标题或相关解释-->
```



#### 3.4.4 详情和概要标签

```html
<details>
<summary>概要信息</summary>
	详情信息
</details>
<!--
当写入信息时只会在网页上显示概要信息，并且在之后又下拉式箭头，能把详情信息下拉展开出来，默认是折叠显示
-->
```



#### 3.4.5 marquee标签

**注意marquee标签不是W3C官方支持的标签，在官方无线查询，但是各大浏览器都支持且效果很好**

**作用：跑马灯，相当于弹幕**

< marquee>< /marquee>

**属性：**

direction=""控制方向 默认从右往左 right从左往右 up:从下往上 down

scrollamounts=“”设置滚动速度 值越大滚动越快 1 2 3

loop=""设置滚动次数1代表只滚动一次 默认是-1也就是无限

behavior=""设置滚动类型slide设置滚到边界就不滚动了alternate让弹幕到边界就不断弹回



## 4.超链接

### 4.1 a标签

```html
<a href="" target="" hreflang="" title=""></a>
<!--
	href是要跳转页面的链接地址
	注意:<a href="">会刷新当前网页<a hred="#">不会刷新当前网页但会到网页最顶端,可以通过设置锚点的形	式跳转到指定页面的指定位置(锚点由ID写入)
	target设置打开目标窗口的方式 如：_blank:打开一个新的窗口加载
	hreflang规定目标URL的基准语言
	title用做提示信息(可以作为简写后的补充说明)
-->
```



### 4.2 map标签

```html
<!--<map>用来创建图像映射，与<img>元素相关联 图像映射是指一个图像建立多个链接，在图像上定义多个区域，每个区域链接不同的地址-->
<img src="" usemap="#图像映射名称">
<map name="图像映射名称">
<area shape="形状（circle rect poly四边形等)" coords="坐标" href="" title="">
<area shape="形状（circle rect poly四边形等)" coords="坐标" href="" title="">
</map>
```



### 4.3 base标签

```html
<base hred="url" target="">
<!--
位于<head>部分，用于浏览器不使用当前文档的url而使用<base>定义的，这样会影响到后面的元素
-->
```



### 4.4 iframe标签

```html
<iframe src="" frameborder="0" scrollinig="no">
    你的浏览器不支持,请使用高版本浏览器
</iframe>
<!--
	iframe标签能在原本页面中再内嵌入一个页面,这个标签的显示模式是inline-block,可以横排显示
	src中写要嵌入页面的域名
	frameborder属性就是这个标签的边框,有两个值，分别为0和1,0就是没有边框,而1是有边框,基本上都是设置为0,同时这个边框和边框线border是不冲突的,可以同时设置
	scrolling是控制这个标签周围是否出现滚动条,有三个值yes,no和auto,默认是yes,一般都是设置no来和页面契合
	注意:这个标签因为是在一个页面中再次嵌入多个页面,所以加载速度会很慢,还有很多安全性问题,尽量减少使用
-->
```

```html
<!--iframe标签可以和a标签搭配使用-->
<a href="http:\\www.tmall.com" target="tmall">跳转到天猫</a>
<iframe src="http:\\www.baidu.com" frameborder="0" scrollinig="no" name="tmall">
你的浏览器不支持,请使用高版本浏览器
</iframe>
<!--
	通过a标签的target绑定iframe标签的name值让点击a标签的时候不是自己网页发生跳转而是iframe中的页面发生跳转,从百度跳转到天猫
-->
```



## 5.表格 

**表格是网页制作的元老级别标签,这个标签以前用做制作网页主体,所以有许多独属于表格的属性和用法**

```html
<!--<table></table>设置表格
<caption></caption>为表格题目，每个表格只能设置一个题目
<tr></tr>表示一行开始
<td></td>表示每一个单元格
<th></th>为将内容居中并以粗体显示，常用于表头的单元格-->
<table border="1">
<caption></caption>
<thead>
<tr>
<th></th>
</tr>  
</thead>
<tbody>
<tr>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</tbody>
<thead>
<tr>
<td></td>
</tr>    
</thead>
</table>
<!--
	<table border="1">说明这个表格为带表格边框的表格,但这个属性尽量不要用,能用样式解决就不要用属性
	<td colspan="数字"></td>跨列,表示列合并中间的数字表示要合并的单元格列合并
	<td rowspan="数字">跨行,表示行合并，行和列的合并可以一起用
	如果在合并的时候出现了合并列有数据的情况可以直接把那一列删除,因为已经合并了列那一列也不需要了
	表格可以嵌套表格
	<thead></thead>表示表格的头
	<tbody></tbody>表示表格的身体
	<tfoot></tfoot>表示表格的结尾
	上面三个属性是对表格结构上的解析,在写代码的时候可以不写上面三个,正常情况下不会影响表格格局，但是可以用CSS改变表格样式,浏览器解析的时候会自动为表格加上上面的tbody属性,所以在控制js进行选择的时候要注意,建议都加上
-->

<!--
	对于table中内容的解释:
	1.tbody里面的单元格默认根据内容百分比平方table的宽度和高度
	2.行和列的宽度高度取决于当前行列中最宽最高的单元格
	3.th内容上下左右居中对齐
	4.td内容上下居中左对齐
	5.给所有td固定宽高时会将表格完全等分,也可以单独给某一个td设置
	6.td不支持margin,并且padding会很奇怪
	7.table到td之间不能嵌套任何元素,td和th中可以有任何元素
-->
```

**table样式**

```css
table{
    border:1px solid #000；
    border-spacing:0;
    border-collapse:collapse;
}
/*
	在不设置属性border="1"的时候通过上面设置css样式来让表格有边框
	border-spacing属性时边框直接的距离,浏览器默认会为table旁加上这个属性并且有值,这个时候就会看见每个表格的边框都时分离并且有一段距离的,而把这个属性设置为0时就会让距离为0,但是会出现边框合并变粗的问题
	border-collapse属性决定是否将表格边框合并默认的值时spareate分开,当使用上面的collapse属性时则会合并表格边框,而且用了这个属性之后border-spacing属性的距离会失效,因为边框已经合并了就没有边框距离可言了
*/
```

**表格的特性**

- 表格有一个属于自己的表现形式就是display:table

- 表格的子标签td,th等的表现形式为display:table-cell意味表格细胞

- 独占一行

- 不给宽高的时候,高度和宽度由内容撑开,不向块级标签一样宽度默认是百分之百

- 支持margin属性并且支持margin:0 auto这样的形式,但是对padding属性的支持效果特别奇怪,只有当不写其余和高度宽度等相关属性的时候才生

  效,所以可以说是不支持padding属性的



## 6.表单

### 6.1 form标签

```html
<form name="" method="" action="url">
</form>
<!--
	name表示表单的名称
	method表示提交信息的方式,取值为post和get，默认为get，区别是get提交快但有限制，post提交慢无限制,get是通过网址传递，name信息写在网址后面，有缓存post不通过网址，要在控制台中查看,没有缓存
	action用来指定处理表单数据的程序文件所在的位置
-->
```



### 6.2 input标签

```html
 <input autpfocus="autofocus name="名称" type="类型" readonly="readonly" （只读）size="文本长度" maxlength="最大可输入字符" value="默认值">
 <!--input
	autpfocus="autofocus"属性表示文本输入字段被设置为当页面加载时获得焦点
    type属性一般不可省，如果省略则默认为text类型
                         
    type="password"为密码框
    type="submit"为提交按钮
    type="reset"为重置按钮，重置文本信息
    type="button"为普通按钮(点击按钮没有任何操作，需要用js)
   	type="radio"为单选框，用于创建单选选项，两个单选框的name属性值必须是一样的 同时可设计checked="checked"设置默认选择
	type="checkbox"为多选框，同上
	type="url"为输入路径值，自动进行验证，不合法会有提示语句
    type="email"与url相同
    type="color" 可以在选择颜色框里任意选择颜色
    type="file"用于可以点击传入文件
    type="hidden"定义一个用户看不见的input框
	type="date/month/week/time/datetime/datatime-local"date为选择日，月，年 month为选择月，年 week为选择周，年 time选择时间 datetime输入时间后，会验证是否符合格式(只有这个是输入) datetime-local选择时间.日期.月,年(本地时间）
    <input type="image" src="" width="" height="">形成一个图像域也是图像按钮
 	<input type="range或number" name="名称" min="" max="" step="步长" value="初始值">step为数字间的间隔，上方依旧为选择数字，调整数字时只能调整以step为间隔的数字,range与number只是在数字的选择形式上有差别   
                                                                           
 	size表示input表单的宽度
	vaule表示文本框里的值，如果赋值，打开网页就直接出现相关的值，如果没有则默认打开文本框为空
	readonly为只读属性，将使得文本框既不能输入也不能编辑
	placeholder="提示文本"可以在文本区域显示一段提示语句，光标地位时语句就会消失
	required="required"检测输入的数据是否为空，如果为空不能提交并显示错误
	pattern="正则表达式"用于验证input的输入是否符合规则，不符合规则就提交不了，具体的正则表达式查表
    formacti在sumbit按钮里定义提交地址,这样就不用在form的action中获取提交地址了
-->
```

```css
/*像input标签这样能够输入内容的独有的css属性*/
input:focus{
    outline:red dashed 2px;/*这个是input的外边框,和border的属性值色设置刚好相反*/
    outline-offset:5px/*边框偏移量,设置边框在偏移input外边框多远的距离显示*/
}
/*	
	focus表示只有当聚焦在input
	一般这个ouline属属性需要用时都是设置为none
*/
input::-webkit-input-placeholder{
	color:red;/*选中palceholder */
}
```



### 6.3 textarea标签

```html
    <textarea rows="行数" cols="列数">
    </textarea>
	<!--
		<textarea>为多行文本区域,用于需要大量文字的地方
	-->
```



### 6.4 select标签

```html
<!--
	<select></select>为选择菜单，能够下拉进行选择
-->
<select multiple="multiple">
<option value="列表中的值">说明</option>
<option value="列表中的值">说明</option>
<option value="列表中的值">说明</option>
</select> <!--里面不要加label-->
<!--
	可以加上optgroup进行分组<optgroup lable="分组名"></outgroup>
	selected="selected"属性，则该选项就被默认选中
	multiple="multiple" multiple属性可以多选，多选是按住ctrl
-->
```

**拓展标签datalist(不推荐使用)**

```html
<!--<datalist></datalist>这个标签也可以建立列表，效果和select类似,但不能单独使用，必须和一个可输入文本框类型一起配合使用,并且可以用label属性
用法如下：
-->
<input type="" list="要绑定的datalist的id" name="名称">
<datalist id="datalist的id">

<option label="列表项的说明" value="列表项的值"></option>

<option label="列表项的说明" value="列表项的值"></option>

<option label="列表项的说明" value="列表项的值"></option>

</datalist>
<!--
	datalist标记只能用于标记区域范围
	label属性设置列表项的标记
	在设置option时必须设置value
-->
```



### 6.5 label标签

```html
<label for="控件id名称">
<!--
	如果你在label标签内点击文本，就会触发此控件，这个标签也可以用做包裹input框
-->
```



### 6.6 fieldset标签

```html
<fieldset>
<legend>控件组的标题</legend>

。。。。

。。。。

</fieldset><!--将表单周围围起来-->
<!--
    <fieldset></fieldset>对表单内部控件进行分组，还会在周围生成边框线
    <legend></legend>用做标记标题使用
-->
```
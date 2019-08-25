# 	CSS

## 1.外部链接

```css
@charset "UTF-8";
/*在外部的css中最上方应该写上上面的代码*/
```

```css
<link href="" rel="stylesheet" type="text/css">
/*
    这是链入外部样式表
    rel用来定义链接的文件与HTML之间的关系，rel="stylesheet"指在页面中使用这个外部链接表
    type指定文件类型 type="text/css"指文件的类型是样式表文本
*/
<link href="" rel="icon">
/*
	这种是用来设置logo图标,该图标就是title前面的小图形
	href就是要引入的小图片的域名
*/
```

```css
<style type="text/css">
	@import url("");
</style>
/*
	导入外部样式表@import url("");用在style里，用法和link很相似，但是实际上相当于是内部链接
*/
```



## 2.类和ID及css选择器

### 2.1 类

<标记名 class="">



### 2.2 ID

<标记名 id="">id具有唯一性



### 2.3 css选择器

**选择器的权重大小:ID>类>标签名>通配符**

在选择器中的属性出现相同的时候,**优先考虑ID选择器,然后是类选择器,标签选择器,最后才考虐通配符选择器**

在都有这些选择器的时候再看每一个选择器的数量,从ID到标签选择器,数量多的属性生效,如果数量完全相同,那么写

在下方的生效

**注意:**1.如果css是写在行内的,无视所有选择器直接拿到权重

​	2.相同选择器数量的情况下内部样式的权重大于外部样式的权重,但是如果外部用的是ID选择器内部是类选择

​	器,那么外部的属性生效

​	3.可以使用!important将权重提升到最高,甚至超过行内样式,但是尽量减少important的使用,因为过多的使用	

​	容易造成对于样式设置的混乱

- ##### 标签选择器

  **通过标签直接对所有该标签元素进行样式的修改**

- ##### 通配符选择器

  **能选择所有的标签元素，权重很低,在开发中不推荐使用**

  用法:*{}

- ##### 后代元素选择器

  **选择当前标签下的所有相匹配的后代,不一定是子代,可以是孙子代,中间用空格隔开**

  如:.类 ul li p这样的选择器就是选择当前类下的ul标签中的所有li标签中的所有p标签

- ##### 子元素选择器

  **基本与后代元素选择器的用法相同,不过只能够选择子代,不能选择孙子代,中间用>符合隔开**

  如:.类>li>a等就是对必须是li下面的a元素才起作用

- ##### 兄弟选择器

  - **相邻兄弟选择器,匹配前面一个选择器后紧跟的同级选择器,中间用+符号隔开**	

    如:a+p{}表示匹配在前一个a元素的后面紧跟着的一个p元素

  - **一般兄弟选择器,匹配前一个元素后面的所有同级的某一个元素,中间用~符号隔开**

     如:a~p{}表示匹配在前一个a元素的所有后面p的元素

- ##### 伪元素选择器

  - **创建伪元素before/after**

    **伪元素必须依赖某一个标签才能产生**

    **如下：**

    ```css
    div::before{/*或者是after*/
    content:"";/*这个属性必须写,没有就赋空值*/
    display:inline-block;/*伪元素的默认表现形式是inline*/
    height:;
    width:;
    background-color：
    }
    /*
    	div::before/after是在div的内容前面加上一个元素，这个元素依然在该元素里面
    */
    ```

    注：

    - 伪元素可以传入图片,一种是通过background-image属性传入,一种是在content处用url()函数来传入图片

    - 可以通过在content处写上attr()函数来获取伪元素所在环境的标签上的属性

      ```css
      div::before{
          content:attr(data-test);/*通过这种方法可以把下方div中data-test属性的值取出*/
      }
      /*
      	注意:在H5的规范中自定以属性前面最好加上data-的标识
      */
      ```

      ```html
      <div data-test="123">
      </div>
      ```

    - 可以通过伪元素来清除浮动

      ```css
      .clear::after{
          content:"";
          display:block;
          clear:both;
      }
      /*
      	只需要给因为子元素浮动而造成本身高度塌陷的父元素.clear这个类就能够清除浮动效果
      */
      ```

  - **伪类选择器**

    **注:伪类可以对链接在不同状态下定义不同的样式效果，伪类是CSS已经定义了的**

    **如：**

    ```css
    E:empty{
    }//匹配内容为空的标签(空格也算作内容)
    E:first-letter{
    }//匹配对象的第一个字符
    E:first-line{
    }//匹配对象内的第一个行
    E:selection{
    Background-color:black;
    color:white;
    }//设置对象被选择时的样式,上面使得被选中时呈现黑底白色
    E:hover{
    }//匹配对象被鼠标悬浮时的样式
    /*伪类一般是对a标签进行控制的,下面几种伪类选择器一般都是对a标签生效*/
    a:link{
    }//匹配a标签没触发时的状态
    a:visited{
    }//匹配a标签被点击后的状态
    a:hover{
    }//匹配a标签被悬浮时的状态的状态
    a:active{
    }//匹配a标签被点击但是鼠标还没放开时的激活状态
    /*记忆方法:love hate*/
    /*
    	如果四个标签一起出现,必须要严格的顺序要求,link visited hover activ
    	如果默认与被访问过一样，可以进行缩写直接a{}代替，但是hover和active如果需要必须写上
    */
    ```

- ##### 属性选择器

  **属性选择器选择时一般都是通过[]将要进行筛选的属性括起来**

  **如**

  ```css
  a[herf]{
  }//表示任何带有href的a标记
  a[herf="http://www.baidu.com"]{
  }//表示将指向百度的链接a
  
  p[class=cc]{
  }
  /*
  	找到所有p标签中class属性等于cc的,这个的用法最常见于区分input属性type=password
  */
  ```

  - **前缀和后缀和包含匹配**

    ```css
    /*前缀使用[^=]*/
    [id^="user"]{
    }
    /*
    	则所有有id并且前缀为user的元素都可以被设置
    	如:<p id="userName">李振</p>等可被设置
    */
    
    /*后缀使用[$=]*/
    [id$="Name"]{
    }
    /*
    	则所有有id并且后缀为Name的元素都可以被设置
    */
    
    /*包含使用[*=]*/
    [id*="test"]{
    }
    /*
    	则所有有id并且id中包含有test的元素都可以被设置
    */
    ```

- ##### 交并集选择器

  - **交集选择器需要两个选择器紧挨着写,并且如果有标签选择器标签需要在最前面写**
  - **并集选择器则是用逗号(,)隔开就可以,这样就会选中所有的被,隔开的选择器**

- ##### 序选择器

  - **:first-child选中同级别第一个**

    如:p:first-child这样用相当于和p做交集选出同级别第一个元素后与p做交集

  - **:first-of-type选中同级别同类型的第一个元素**

  - **:last-child选中同级别最后一个标签，不区分类型**

  - **:last-of-type选中同级别同类型的最后一个**

  - **:nth-child(n)选中同级别中的第n个标签，不区分类型** 

    **拓展用法:**odd代表奇数行	even代表偶数行 

    ​		用xn+y这样的语法进行选择:x和y是用户自定义的,而0是一个计数器，从0开始递增

  - **:nth-of-type(n)选中同级别中、同类型的第n个标签，用法同上**

  - **:nth-lastchild(n)选中同级别的第倒数第几个标签**

  - **:only-child选中只有一个子元素的父元素带的子元素**

  - **:only-of-type选中父元素中唯一类型的标签**

  - **:not(tagname)选中不是指定标签的所有标签**

**注意:CSS样式的继承性:**

​	只有以color/font-/text-/line开头的属性才可以继承

​	**特例**:a标签的下划线和颜色不能继承，h标签的字体大小不能继承

​	**计算权重时只对被直接选中的元素进行选择**



## 3.文本样式

### ​3.1 字体样式

**1.字体类型font-family**

**font-family属性表示用哪一个字体**,这个属性后面写需要的字体,字体可以写多个,但总是从第一个开始用,如果用户

没有第一个字体样式.那么会依次用后面的字体

**如何用外部引用的字体:**

```css
	@font-face {
    font-family:"字体名称";
    src:"字体文件在服务器上的相对或绝对路径";
	}
	/*
		设置之后就能在font-family里面设置自己需要的字体了
	*/
```

​		

**2.字体尺寸font-size**

**font-size属性表示字体大小,**这个属性一般用的单位为px,也可以用一般表示大小的单词来直接表示大小,这个大小

是由浏览器加上的,如:xx-small x-small等绝对尺寸,smaller和larger等相对尺寸以及百分比尺寸

​	

**3.字体粗细font-weight**

**font-weight属性改变字体粗细**,可以写具体的数字也可以写表示大小的单词由浏览器加上

- 100-900表示字体粗细的数字值

- normal表示正常字体粗细，相当于数字值400 


- bold表示粗体，相当于数字值700,同时这个值也是最常用的,一般不用数字值,而且这个属性一般也只用作让字

  体加粗

- bolder lighter定义比继承者值更重和更轻的值 ,这是相对值


​	

**4.字体风格font-style**

**font-style属性表示字体风格**,一般都用作让字体倾斜

- nomal表示正常字体


- italic和oblique都是表示斜体,但是实质上还是有差别,一般都用italic




**5.字体显示小型大写字符font-variant**

**font-variant属性定义小写字母是否显示为小型大写字母**，默认的值为normal

**用法:font-variant:small-caps;**

**字体混合属性用法:**font:font-style font-variant font-weight font-size font-family 中间可以少,但是必须按照

这个顺序排列

​	

**6.字体颜色color**

color属性表示字体颜色,支持英文单词,16进制颜色和rgb颜色

**注意:**

- **rgb()和rgba()后者最后一个是透明度**

- **color:transparent为颜色透明**	

  

### ​3.2 行高line-height	

**line-height属性设置行高**，行高和字体高度不同,但是行高默认会随着字体大小的变化而变化,而撑开盒子高

度就是靠的行高

**注意:通过设置行高和内容高度相等可以使得单行文本垂直居中**

**多行文本垂直居中方法:**

**方法一：使用插入 table  (包括tbody、tr、td)标签，或者父元素使用display:table和子元素使用**

**display:table-cell属性来模拟表格，同时设置子元素vertical-align:middle**

```html
<div class="span_box bg_box">
	<span class="words_span">
		方法一：父元素使用display:table和子元素使用display:table-cell属性来模拟表格，子元素设置vertical-align:middle即可垂直居中
	</span>
</div>

```

```css
.bg_box {
	width: 300px;
	height: 300px;
	margin-top: 20px;
	background-color: #BBBBBB;
}
/*方法一*/
.span_box {
	display: table;
}
.words_span {
	display: table-cell;
	vertical-align: middle;
}

```



**方法二：对子元素设置display:inline-block属性，使其转化成行内块元素，模拟成单行文本。元素设置对应的height和line-height。对子元素设置vertical-align:middle属性，使其基线对齐添加line-height属性，覆盖继承自父元素的行高。缺点：文本的高度不能超过外部盒子的高度**

```html
<div class="p_box bg_box">
	<p class="words_p">
		方法二：对子元素设置display:inline-block属性，使其转化成行内块元素，模拟成单行文本。父元素设置对应的height和line-height。对子元素设置vertical-align:middle属性，使其基线对齐。添加line-height属性，覆盖继承自父元素的行高。缺点：文本的高度不能超过外部盒子的高度。
	</p>
</div>

```

```css
.bg_box {
	width: 300px;
	height: 300px;
	margin-top: 20px;
	background-color: #BBBBBB;
}
/*方法二*/
.p_box {
	line-height: 300px;
}
.words_p {
	display: inline-block;
	line-height: 20px;  /*单独给子元素设置行高，覆盖父级元素的行高*/
	vertical-align: middle;  /*基线居中对齐*/
}

```



**方法三:脱离文档流的居中方式，把内部div设置宽高之后，再设置top为50%，使用负边距调整，将margin-to**

**设置为负的高度的一半就可以垂直居中了。缺点:需要计算出多行文字固定的高度。高度一旦改变，负边距也要**

**调整。**		​	

```html
<div class="wrapper bg_box">
	<div class="content_box">
		方法三：脱离文档流的居中方式，把内部div设置宽高之后，再设置top为50%，使用负边距调整，将margin-top设置为负的高度的一半就可以垂直居中了。缺点：需要计算出多行文字固定的高度。高度一旦改变，负边距也要调整。</div>
</div>

```

```css
.bg_box {
	width: 300px;
	height: 300px;
	margin-top: 20px;
	background-color: #BBBBBB;
}
/*方法三*/
.wrapper {
	position: relative;
	overflow: hidden;
}
.content_box {
	position: absolute;
	top: 50%;
	width: 300px;
	height: 127px; /*本页面中这么多文字的高度，文本篇幅改变，高度也会变*/
    margin-top: -63.5px;  /*height的一半*/
}

```

#### 			  	

### 			  	3.3 文字上下对齐vertical-align

**vertical-align属性设置内联元素的上下对齐方式,**在父元素设置此样式时,会对inline-block和inline类型的子元素都

有用,这个样式一般用做图片和文字的对齐,因为内联元素的独特的对齐方式,所以设置图文对齐的方法一般都是图形

和文字都设置vertical-align:middle来使得图文对齐

**verticl-align的值:**

| 值          | **描述**                                                     |
| ----------- | ------------------------------------------------------------ |
| baseline    | 默认。元素放置在父元素的基线上。                             |
| sub         | 垂直对齐文本的下标。                                         |
| super       | 垂直对齐文本的上标                                           |
| top         | 把元素的顶端与行中最高元素的顶端对齐                         |
| text-top    | 把元素的顶端与父元素字体的顶端对齐                           |
| middle      | 把此元素放置在父元素的中部。                                 |
| bottom      | 把元素的顶端与行中最低的元素的顶端对齐。                     |
| text-bottom | 把元素的底端与父元素字体的底端对齐。                         |
| length      | 定义固定的值                                                 |
| %           | 使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。 |
| inherit     | 规定应该从父元素继承 vertical-align 属性的值。               |

**内联元素的对齐方式:**

所有的内联元素都是通过文字基线对齐方式来实现对齐的,默认情况下是以小写x的两个下角为边界进行图文对齐,

图片不会超过x的下角所在水平线的位置,vertical-align的默认值是baseline,元素是放在父元素的基线上的

​

**不知道盒子高度的情况下将文本垂直居中**

```css
/*设置伪元素使文本居中*/
p:before{
	display:inline-block;
    content:"";
    height:100%;
    vertical-align:middle;
}
```



### 3.4 文本水平对齐text-align

**text-align属性设置水平对齐方式**

值:left左对齐 right右对齐 center居中对齐 justify两端对齐 

​

### 3.5 文本换行white-space

**white-space属性可以对文本的换行操作进行控制,**默认值是nomal,设置文本到达父元素的边界就自动换行

**white-space:nowrap设置文本不换行**,意思是文本到达边界依然会继续的由左向右平铺,该属性只要都是设置这个

值,除此之外还有其他的值,如:white-space:pre不换行又保留多个空格 white-space:pre-wrap保留空白字符但是换

行 white-space: pre-line保留换行符 white-space:inherit继承父元素的该属性

**实现文本超出显示省略号:**

需要三个属性的组合

- white-space:nowrap不换行
- text-overflow:ellipsis文本超出显示省略号
- overflow:hidden超出隐藏



### ​3.6 单词换行word-break

**word-break属性可以控制单词或字符进行换行,**默认值为normal,使浏览器遵循默认的换行规则

word-break:break-all强制换行,允许在单词内部换行,如果只写一个因为单词但是太长用了这个属性就会强制把单词断开

word-break:keep:all,只允许在半角空格或连字符处换行。



### ​3.7 文本缩进text-indent

**text-indent属性控制文本在一个父元素最前方字符的缩进距离**,该属性可以通过调试来让文本显示在父元素的一

些特殊位置,可以把它用做于padding-left的作用类似，默认值是not specified,也就是不首行缩进

**设置的值一般都是固定的缩放距离**，比如:text-indent:2em 首行缩进2个字符。也可以是百分比，定义基于父元素

宽度的百分比的缩进



### ​3.8 控制单词形式text-transform

**text-transform属性控制英文单词大小写转换**

**值:**none 无转换 capitalize将每个单词的第一个字母大写 uppercase将所有字母大写 lowercase将所有字母小写 

ful-width全角



### 		3.9 字符与单词间距

**一行文字如果中间没有空格隔开就是组成部分就是一个个字符,这一行文字就是一个单词,而如果有空格隔开,根据**

**空格的数量来判断单词的数量**

**letter-spacing用来控制字符间的间距**,默认值为normal,具体也是通过写固定的值来实现字符间距

**注:**汉字也是被认为是一个个的字符,因为中间没有用空格隔开

**word-spacing为单词间距**,该间距是每一个用空格隔开英文单词的间距.只有用空格隔开的词才会认为是一个单词,

单词内部间距不变



### 		3.10 文本修饰text-decoration

**text-decoration属性用于修改文本的样式**,是一个复合属性,该属性也是由一些小的属性的组合属性,但是在运

用时都是直接用这个属性

**子属性:**		

**1.text-decoration-line设置文本修饰的样式线条**,一般标签的默认值为none关闭修饰,而a标签的默认值是

underline 下划线文本,所以一般a标签都需要通过设置text-decoration:none来修改a标签的样式,其它值还有

overline上划线  line-through贯穿线(也叫作删除线)

**2.text-decoration-style设置文本修饰的风格,值:**

solid实线 double双线 dotted点线 dashed虚线 wave波浪线

**3.text-decoration-color设置文本修饰颜色**,用来指定文本装饰线条的颜色,如果不写这个属性这是默认用的字体颜

色color



### ​3.11 文本阴影text-shadow

**text-shadow属性设置文本周围是否出现阴影,**默认值为none 无阴影  

**设置阴影test-shadow:长度1 长度二  长度三  颜色**  (长度三和颜色为可选属性) 

- 长度1设置水平偏移值，可以为负值，正值表示阴影在右，负值在左 

- 长度2设置垂直偏移量 正值在下负值在上

- 长度三用来设置文本的阴影模糊值，不允许用负值

- 颜色用来设置阴影的颜色

**注:可以写多组阴影,每一组阴影中间用逗号隔开**

​

### 3.12 文本书写模式writing-mode

**writing-mode 属性定义了文本在水平或垂直方向上如何排布** 

**语法:**

```css
writing-mode: horizontal-tb | vertical-rl | vertical-lr | sideways-rl | sideways-lr;
```

**值：**

- **horizontal-tb：**水平方向自上而下的书写方式。**即 left-right-top-bottom**
- **vertical-rl：**垂直方向自右而左的书写方式。**即 top-bottom-right-left**
- **vertical-lr：**垂直方向内内容从上到下，水平方向从左到右
- **sideways-rl**：内容垂直方向从上到下排列
- **sideways-lr：**内容垂直方向从下到上排列	



### 		3.13 列表属性list-style

**list-style是复合的列表属性,**包含list-style-image,list-style-position和list-style-type等

- **list-style-image属性用来设置对象的列表项是否图像作为项目符号**

  list-style-image:url()来指定图片的域名,还有一个none值意为不指定图片符号

- **list-style-position属性用来设置对象的列表序号的位置** 

  list-style-positon:outside（inside）ouside为默认值,让列表符号与文字分离开,inside使得列表序号在文本内,

  可以通过控制文本来控制

- **list-style-type属性为设置对象的列表项所使用的项目符号**

  list-style-type:disc实心圆 circle空心圆 square实心方块 decimal阿拉伯数字 lower-alpha小写英文字母 

  upper-alpha大写英文字母  none 为不用项目符号

  列表复合属性语法:

  list-style:list-style-image  list-style-position  list-style-type 按照这个顺序进行书写 如果list-style-image

  这个属性有值并且生效,那么list-style-type属性将不会生效,如果这个属性使none那么list-style-type属性生



### 3.14 文本溢出text-overflow

**text-overflow 属性规定当文本溢出包含元素时发生的事情**,默认值是clip,修剪文本,将文本剪裁掉

**值:**

| 值       | 描述                               |
| -------- | ---------------------------------- |
| clip     | 修剪文本                           |
| ellipsis | 显示省略符号来代表被修剪的文本     |
| *string* | 使用给定的字符串来代表被修剪的文本 |



### 	      3.15 内容溢出overflow

**overflow属性定义溢出元素内容区的内容会如何处理,**默认值是visible，超出内容不会被修剪，会呈现在元素框之

外

**值:**

| 值      | 描述                                                     |
| ------- | -------------------------------------------------------- |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
| scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
| inherit | 规定应该从父元素继承 overflow 属性的值。                 |
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |



## 4.盒子模型

**注意:因为不同的浏览器会为盒子自动添加不同的内外边距.如果要让网页在每一个浏览器上都显示一样的效果.需要**

**在写代码前把默认的边距全部清空,在平时可以用通配符选择器来清空,但是写正规项目的时候不要这样写,因为通配**

**符选择器的权重太低了**

**HTML盒模型由4个元素组成:content padding border margin(按由内到外的顺序)**

**盒子大小的计算方法:**content(内容区)+padding(内边距)+border(边框)+margin(外边距)

### 4.1 margin的注意点

- 两个紧挨的兄弟盒子同时设margin,左右两边的margin会叠加,而上下的margin会重叠,margin值更大的盒子的

margin会生效

- 当两个盒子是父子关系时,设置子盒子左右的margin值会让子盒子在父盒子里面进行偏移,如果设置上下的

  margin值则会有margin重合的现象,让父盒子也跟着子盒子一起移动,给父盒子添加border属性可以避免这种

  现象

  

### 4.2 box-sizing

**box-sizeing属性能改变盒子大小的计算方法**

```css
box-sizing：content-box|border-box;
```

默认值是content-box,默认情况下width和height只改变内容区

**border-box能让盒子变为怪异盒模型,**怪异盒模型的计算方式和普通内容盒模型的计算方式不同,怪异盒模型是把

padding和border的宽度一起算入width和height中,当宽度和高度固定时,变化padding和border值会自动的将内

容区域缩小



### 4.3 边框border

#### 4.3.1 一般边框

**border属性是一个复合属性,可以按照border-width border-style border-color的顺序设置边框**

border:宽度 样式 颜色

- **border-width属性,定义边框厚度**

  | 值       | 描述                           |
  | -------- | ------------------------------ |
  | thin     | 定义细的边框。                 |
  | medium   | 默认。定义中等的边框。         |
  | thick    | 定义粗的边框。                 |
  | *length* | 允许自定义边框的宽度。         |
  | inherit  | 规定应该从父元素继承边框宽度。 |

  **通常情况下边框的宽度都是直接写固定的值**

- **border-style属性定义边框的样式**

  | 值     | 描述                                                         |
  | ------ | ------------------------------------------------------------ |
  | none   | 定义无边框。除非用border-image边框才能使边框生效             |
  | hidden | 与 "none" 相同。不过应用于表时除外，对于表，hidden 用于解决边框冲突。 |
  | dotted | 定义点状边框。在大多数浏览器中呈现为实线。                   |
  | dashed | 定义虚线。在大多数浏览器中呈现为实线。                       |
  | solid  | 定义实线。                                                   |
  | double | 定义双线。双线的宽度等于 border-width 的值。                 |
  | groove | 定义 3D 凹槽边框。其效果取决于 border-color 的值。           |
  | ridge  | 定义 3D 垄状边框。其效果取决于 border-color 的值。           |
  | inset  | 定义 3D inset 边框。其效果取决于 border-color 的值。         |
  | outset | 定义 3D outset 边框。其效果取决于 border-color 的值。        |
  | nherit | 规定应该从父元素继承边框样式。                               |

- **border-color属性,定义边框线的颜色**

  **注意:**

  以上的三个属性也全部都是复合属性,都是上下左右四个方向的复合属性结构,只写一个值是上下左右四个面全

  部都是那个值,可以通过如:border-top-width这样的写法来单独设置某一个面,也可以写四个值来分别显示四个

  面的结果 ,一样的遵循上右下左的格式

  

#### 4.3.2 圆角边框

**border-radius属性决定是否边角出现圆角**,这个属性也是个复合属性,决定左上 右上 右下 左下四个方向的圆角大

小,也可以只写一个来代表所有方向,这个属性和border属性无关,即使没有边框也可以作用

**注意:这个属性经常用做显示胶囊状的盒子,同时如果盒子的宽高相等,再将这个属性设置为50%，就会出现圆形的盒**

**子**



#### 4.3.3 图像边框

**border-image属性设置是否用图像边框.**启用的这个属性border-style设置的边框将会无效,同时这个属性也是个复

合属性

```css
border-image:border-image-sorce border-image-slice/border-image-width/*(/号不是或者的意思,如果要写必须写/）*/ border-image-outside border-image-repeat
/*具体用法如下*/
border-image:url() slice/width outside repeat;
```

- **border-image-sorce属性用于指定要用于绘制边框的图像的位置**,该属性默认值是none,如果要传入图片和写

  背景图片的用法是一样的用url("")来传入图片的地址

- **border-image-slice属性控制边框图像地切片方式**,也是指定四个位置

  ```css
  border-image-slice: number|%|fill;
  ```

  这个属性的值可以是具体的数值也可以是百分比,如果后面再加fill表示保留着中间部分，相当于盒子作为了背

  景图片

  **注意:** 此属性指定顶部 ，右，底部，左边缘的图像向内偏移，分为九个区域：四个角，四边和中间。图像中间部分将被丢弃（完全透明的处理），除非填写关键字。如果省略第四个数字/百分比，它和第二个相同的。如果也省略了第三个，它和第一个是相同的。如果也省略了第二个，它和第一个是相同的。

- **border-image-width属性用于指定使用多厚的边框来承载被裁剪后的图像**

  ```css
  border-image-width: number|%|auto;
  ```

  该属性的值可以是具体的数值也可以是百分比,默认是1,该属性同样也是包含上右下左的复合属性,该属性有一

  个值是auto,如果指定了，宽度是相应的image-slice的内在宽度或高度

- **border-image-outside属性指定边框图像向外扩展所定义的数值**,默认值是0的长度,这个值可以用具体的带单

  位的数值和用纯数字来作为值,纯数字代表border-width的倍数

- **border-image-repeat属性用于指定边框图像地填充方式** ,默认值为stretch,拉伸图像来填充区域

  ```css
  border-image-repeat: stretch|repeat|round|initial|inherit;
  ```

  | 值      | 描述                                                         |
  | ------- | ------------------------------------------------------------ |
  | stretch | 默认值。拉伸图像来填充区域                                   |
  | repeat  | 平铺（repeated）图像来填充区域。                             |
  | round   | 类似 repeat 值。如果无法完整平铺所有图像，则对图像进行缩放以适应区域。 |
  | space   | 类似 repeat 值。如果无法完整平铺所有图像，扩展空间会分布在图像周围 |
  | initial | 将此属性设置为默认值。                                       |
  | inherit | 从父元素中继承该属性。                                       |

**注:**border-collapse属性指定是否用做合成单一的边框,这个属性一般都是给表格使用的,默认值是separate分开,可



### 4.4 盒子阴影

**box-shadow属性为是否显示盒子阴影**,默认值是none没有阴影,这个属性的用法text-shadow用法基本一致

有阴影box-shadow:水平偏移量 垂直偏移量 阴影模糊半径 阴影扩展半径 阴影颜色 可以在最后加上inset表示阴影

是否是内阴影,默认是外阴影outset,阴影扩展是在原本阴影的上下左右再继续添加阴影,如果没有写阴影颜色,阴影颜

色由盒子内容的颜色决定**所以简写只用写前面3个**



### 4.5 表现形式

#### 4.5.1 display

**display属性,这个属性能从根本上改变盒子的存在方式,**常用的值有 none inline block inline-block以及flex(弹性盒

子)



#### 4.5.2 visiblity

**visiblity属性,这个属性指定是否显示一个元素生成的元素框,**但是无论怎么设置原本的盒子都不会脱离文档流,依然

会占据其本来的空间,默认值是visible可见的

| 值       | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| visible  | 默认值。元素是可见的。                                       |
| hidden   | 元素是不可见的。                                             |
| collapse | 当在表格元素中使用时，此值可删除一行或一列，但是它不会影响表格的布局。被行或列占据的空间会留给其他内容使用。如果此值被用在其他的元素上，会呈现为 "hidden"。 |
| inherit  | 规定应该从父元素继承 visibility 属性的值。                   |



#### 4.5.3 opacity

**opacity属性设置一个元素的透明度**,值为0到1,0为完全透明,1为正常显示,默认值是1

**注意:**这个属性在ie8以及ie8以下的浏览器不起作用,但是ie浏览器有自己设置透明度的方式

​	filter:alpha(opacity=num)这个属性能代替opacity在ie浏览器中生效,但是值是0到100,对应opacity的0到1

​	如要要兼容ie8浏览器,可以两个属性都写上,应为其他浏览器不支持下方的属性,该属性相当于没有写

**让盒子在页面中消失:**

- display:none能让一个盒子脱离文档流在一个页面中完全消失并且不可被选中

- visiblity:hidden能让一个盒子在页面中隐藏但是还是占据了原本的位置,并没有脱离文档流

- opacity:0能让一个盒子的透明度为0从而达到消失的效果,但是这个消失的效果并不好,因为还能够选中文字,当

  然也并没有脱离文档流



### 4.6 弹性盒子

**弹性盒子由display申明,通过设置 display 属性的值为 flex 或 inline-flex将其定义为弹性容器**,弹性容器内包含了

一个或多个弹性子元素

**设置弹性盒子时需要用到的属性:**

| 属性            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| display         | 指定 HTML 元素盒子类型。                                     |
| flex-direction  | 指定了弹性容器中子元素的排列方式                             |
| justify-content | 设置弹性盒子元素在主轴（横轴）方向上的对齐方式。             |
| align-items     | 设置弹性盒子元素在侧轴（纵轴）方向上的对齐方式。             |
| flex-wrap       | 设置弹性盒子的子元素超出父容器时是否换行。                   |
| align-content   | 修改 flex-wrap 属性的行为，类似align-items, 但不是设置子元素对齐，而是设置行对齐 |
| flex-flow       | flex-direction 和 flex-wrap 的简写                           |
| order           | 设置弹性盒子的子元素排列顺序。                               |
| align-self      | 在弹性子元素上使用。覆盖容器的 align-items 属性。            |
| flex            | 设置弹性盒子的子元素如何分配空间,下面三个属性的简写。        |
| flex-grow       | 定义项目的放大比例                                           |
| flex-shrink     | 定义项目的缩小比例                                           |
| flex-basis      | 定义项目对空间的分配行为                                     |



#### 4.6.1.flex-direction

**flex-direction属性定义弹性盒子中项目的主轴方向**,弹性盒模型将盒子中的内容通过主轴和侧轴进行显示,主轴默

认是水平从左向右的,侧轴默认垂直从上到下

| 值             | 描述                                           |
| -------------- | ---------------------------------------------- |
| row            | 默认值。灵活的项目将水平显示，正如一个行一样。 |
| row-reverse    | 与 row 相同，但是以相反的顺序。                |
| column         | 灵活的项目将垂直显示，正如一个列一样。         |
| column-reverse | 与 column 相同，但是以相反的顺序。             |
| initial        | 设置该属性为它的默认值。                       |
| inherit        | 从父元素继承该属性。                           |



#### 4.6.2.flex-wrap

**flex-wrap属性规定了flex盒子在主轴上单行超出是否换行**,默认值是nowrap不换行

| 值           | 描述                                                     |
| ------------ | -------------------------------------------------------- |
| nowrap       | 默认值。规定灵活的项目不拆行或不拆列。                   |
| wrap         | 规定灵活的项目在必要的时候拆行或拆列。                   |
| wrap-reverse | 规定灵活的项目在必要的时候拆行或拆列，但是以相反的顺序。 |
| initial      | 设置该属性为它的默认值。                                 |
| initial      | 设置该属性为它的默认值。                                 |

**注意:**

在弹性盒子中因为flex-wrap的默认值是nowrap,如果设置了子元素的宽高超出了弹性盒子的总范围的时候会对子

元素的大小进行弹性压缩,让超出的子元素等比例缩小到一行中,**但是如果子元素里面有文字的时候**,最多子元素只能

被压缩到文字的大小,因为文字不能被弹性压缩,只能通过font-size来设置,被压缩到文字大小后就会超出弹性盒子



**注:上面两个属性用flex-flow代替的时候flex-direction写在前面,flex-wrap写在后面**



#### 4.6.3 justify-content

**justify-content 用于设置或检索弹性盒子元素在主轴方向上的对齐方式**，默认值是flex-start,让项目位于弹性盒子

的开头

| 值            | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| flex-start    | 默认值。项目位于容器的开头                                   |
| flex-end      | 项目位于容器的结尾。                                         |
| center        | 项目位于容器的中心。                                         |
| space-between | 项目位于各行之间留有空白的容器内。先将盒子的一行的两端占据,再将剩下的空间平分 |
| space-around  | 项目位于各行之前、之间、之后都留有空白的容器内。将盒子的一行完全平分 |
| initial       | 设置该属性为它的默认值                                       |
| inherit       | 从父元素继承该属性                                           |



#### 4.6.4 align-items

**align-items属性定义了项目在侧轴上的对齐方式(algin-items主要支持单行项目),**默认值是stretch,当子元素没有在侧轴方向定义宽或高的具体长度或者长度为auto时拉伸子元素的宽度或高度到整个侧轴的长度

| 值         | 描述                                           |
| ---------- | ---------------------------------------------- |
| stretch    | 默认。 拉伸元件以适应容器。                    |
| center     | 中心元素在容器内。                             |
| flex-start | 位置元素在容器的开头。                         |
| flex-end   | 位置元素在容器的末端。                         |
| baseline   | 位置元素在容器的文本基线。将子元素类的文字对齐 |
| initial    | 设置为默认值。                                 |
| inherit    | 从其父元素继承此属性。                         |

**注意:如果align-items的值表示默认值stretch而且子元素又没有给具体的宽高,它的大小由里面的内容撑开**



#### 4.6.5 align-content

**align-content定义了多行子元素在侧轴上的对齐方式,**默认值是strtch,效果和align-items的相同,只不过是多行子

元素进行平分整个侧轴长度

| 值            | 描述                                             |
| ------------- | ------------------------------------------------ |
| stretch       | 默认值。项目被拉伸以适应容器。                   |
| center        | 项目位于容器的中心。                             |
| flex-start    | 项目位于容器的开头。                             |
| flex-end      | 项目位于容器的结尾。                             |
| space-between | 项目位于各行之间留有空白的容器内。               |
| space-around  | 项目位于各行之前、之间、之后都留有空白的容器内。 |
| initial       | 设置该属性为它的默认值                           |
| inherit       | 从父元素继承该属性。                             |



**align-items和align-content的异同:**

- align-items和align-content有相同的功能，不过不同点是align-items是用来让每一个单行的容器居中而不是

  让整个容器居中

- 所以对于只有一行的flex元素，align-content是没有效果的，只有用align-items才能达到预期的效果

- 如果是多行flex元素,至于使用align-content才能让所有的flex元素在侧轴方向上面有类似justify-content在主

  轴上的居中效果,而使用align-items时两个flex元素间总是有间隙



**上面是针对于整个容器的属性,现在是针对项目上的属性**

#### 4.6.6 order

**order属性设置项目中的子元素在它那一行中主轴的排列顺序,**默认是每一个子元素都是0,数值越小排序越靠前

**注:该属性接受负值,且接受的值是整数**



#### 4.6.7 flex-grow

**flex-grow属性设置项目中的子元素在主轴上还有剩余的空间的时候通过设置一个数字来获取对于剩余空间的占比**,

默认值是0

**具体用法**:会将所有子元素的flex-grow属性的数字加起来然后用各自的flex-grow的数字除以这个数就是每个子元素

再分配得剩余空间大小的量

**公式:该元素分得剩余空间=该元素的flex-grow值/所有flex-grow值之和*剩余空间大小**

**注:该属性不接受负值,允许接受浮点数**



#### 4.6.8 flex-shrink

flex-shrink属性设置项目中的子元素在主轴上空间不足(这里flex-wrap没有设置为wrap)的时候,通过设置一个数字

进行对自身比例的缩小从而使得flex-shrink为0的子元素保持其本身设置的大小,该属性的默认值也是0

**具体用法**:会将所有子元素的flex-shrink属性的数字加起来然后用各自的flex-shrink的数字除以这个数就是每个子元素对于超出弹性盒子容积空间占比要缩小的量

**公式:该元素缩小空间=该元素的flex-shrink值/所有flex-shink值之和*超出弹性盒子空间大小**

**注:该属性不接受负值,允许接受浮点数**



#### 4.6.9 flex-basis

flex-basis属性定义项目对空间分配行为时,项目的实际大小(这个属性会和width和heigth等属性起冲突,在弹性盒子

中有这个属性width或heigth将会无效),同时只要用这个属性设置了固定的大小,该元素就不会受到扩大或者收缩的

影响

**合法值："auto"、"inherit" 或一个后跟 "%"、"px"、"em" 或任何其他长度单位的数字**

**注意:**

- **如果该元素占据的那一行占满了弹性盒子长度,那么后面的元素会自动到下一行**
- **有这个属性的元素的最大长度不能超过弹性盒子长度**



**注意:上面三个属性用flex代替的时候用flex-grow flex-shrink flex-basis 的顺序,默认值为0 1 auto** 



#### 4.6.10 align-self

**align-self 属性定义flex子项单独在侧轴方向上的对齐方式**,默认值是auto，继承父元素的align-items属性的值

| 值         | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| auto       | 默认值。元素继承了它的父容器的 align-items 属性。如果没有父容器则为 "stretch"。 |
| stretch    | 元素被拉伸以适应容器。                                       |
| center     | 元素位于容器的中心。                                         |
| flex-start | 元素位于容器的开头。                                         |
| flex-end   | 元素位于容器的结尾。                                         |
| baseline   | 元素位于容器的基线上。                                       |
| initial    | 设置该属性为它的默认值                                       |
| inherit    | 从父元素继承该属性                                           |



### 4.7 BFC环境

**全称叫block formatting context块级格式化上下文,具有流体特性** 

**作用:**元素内部的标签无论发生什么情况都不会影响其本身的位置的值的计算

**开启BFC环境的方式:**

- overflow值不为visible都能开启
- float的值不为none
- position的值为绝对和固定定位
- display的值为inline-block,flex,inline-flex等

**常见用法:**

- 阻止兄弟元素上下外边距的合并问题,具体可用一个大盒子包裹下方的盒子,然后让大盒子overflow:hidden开启

  BFC环境

- 防止因子元素浮动而导致的父元素高度塌陷问题

- 用作双栏式的页面布局,左边是固定宽度,右边是自适应的宽度,这种布局需要给左边的导航栏开启BFC,让左边的

  内容被隔离出来,类似于float,但是又占着原来的位置,左右两个部分相当于是inline-block的表现形式

  ```css
  top{
      overflow:hidden;
      width:300px;
      height:200px;
      background:red
  }
  bottom{
      height:200px;
      background:green
  }
  ```

  ```html
  <div class="top">    
  </div>
  <div class="bottom">
  </div>
  ```

  

## 5.背景

**background设置背景属性,该属性是一个复合属性**

```css
background:background-color|background-image|background-repeat|background-attachment|background-position/background-size/*(如果如果要写大小必须写位置,不然会报错,同时格式也是这样写的)*/
/*下面是一般写法*/
background:color url repeat position/size/*(如果同时写了位置和和大小必须用/隔开)*/      
```

### 5.1 background-color

**background-color属性设置元素的背景颜色**，默认值是transparent透明色

| 值          | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| color_name  | 规定颜色值为颜色名称的背景颜色（比如 red）                   |
| hex_number  | 规定颜色值为十六进制值的背景颜色（比如 #ff0000）             |
| rgb_number  | 规定颜色值为 rgb 代码的背景颜色（比如 rgb(255,0,0)。也可以使用rgba,最后一个控制背景颜色的透明度 |
| transparent | 默认。背景颜色为透明                                         |
| inherit     | 规定应该从父元素继承 background-color 属性的设置             |

**元素背景的范围**

background-color 属性为元素设置一种纯色。这种颜色会填充元素的内容、内边距和边框区域,扩展到元素边框的外边界(但不包括外边距)。如果边框有透明部分(如虚线边框),会透过这些透明部分显示出背景色。



**渐变色背景(这里是线性渐变,还有一种径向渐变)**(IE基本不支持渐变色,要作渐变色最好还是用图片)

**渐变颜色由颜色渐变函数linear-gradient()设置**,第一个参数是要渐变的方向,如果不写就是默认方向,后面可以跟上

任意颜色参数，同时**在两个颜色参数中间**(一定要是中间)还可以跟上一个百分比的参数表示两个颜色间过渡区域的

占比

**渐变色有方向性,如果写了两种颜色默认是从上向下平方渐变,渐变的方向只能是左右,上下和对角方向或者指定一**

**个度数**

为了创建一个线性渐变,需要设置一个起始点和一个方向（指定为一个角度）的渐变效果,还需要定义终止色,,所以

必须最少指定至少两种，当然也会可以指定更多的颜色去创建更复杂的渐变效果。

| 值             | 描述             |
| -------------- | ---------------- |
| to top         | 从下往上渐变     |
| to bottom      | 从上往下渐变     |
| to left        | 从右往左渐变     |
| to right       | 从左往右渐变     |
| to top left    | 从右下往左上渐变 |
| to top right   | 从左上往右下渐变 |
| to bottom left | 从右上往左下渐变 |
| to bottom left | 从左上往右下渐变 |

```css
background-image:linear-gradient(to top,yellow,white);/*表示颜色由下往上从黄色渐变为白色*/
background-image:linear-gradient(to top left,yellow,80%,white);/*表示颜色从右下网左上从80%处开始渐变*/
background-image:linear-gradient(45deg,yellow,white)/*表示在45度方向方向开始渐变*/
```

**注意:渐变函数linear-gradient()占用的是background-image属性**



### 5.2 background-image

**background-image属性设置背景图像,**默认值是none,如要要设置背景图片和border-image一样用url("")中间写图

片的地址来设置



### 5.3 background-repeat

**background-repeat属性设置背景图片是否重复,**默认值为repeat,意为水平和垂直方向都要重复,其它值有

no-repeat只展示一次背景图片 repeat-x只在水平方向重复 repeat-y只在垂直方向重复



### 5.4 background-attachment

**background-attachment属性设置背景图像是否固定或者随着页面的其余部分滚动,**默认值是scroll，背景图片会随

着页面其余部分滚动而滚动,如果要让背景图片不移动,设置次属性为fixed



### 5.5 background-position

**background-positon设置背景图像的起始位置,**默认的起始值是0% 0%,也就是在盒子的左上角位置

这个属性的值可以是具体的像素数值,也可以是百分比,还可以用一些表示方位的关键词来写

- 如果是用数值或者百分比来写,有两个值,第一个值是水平位置,第二个值是垂直位置,如果只规定了一个值,那么

  第二个值默认是50%,可以数值和百分号一起使用

- 如果用关键词来写,具体的关键词有top left right bottom center,也是第一个值表示水平位置,第二个词表示垂

  直位置，如果只写了一个值,那么第二个值是center



### 5.6 background-size

**background-size属性规定背景图像的尺寸**,默认值是auto

**background-size:长度/百分比/auto/cover/contain** 

auto真实大小 cover将背景图像等比例缩放到完全覆盖容器,直到将容器填满,不会让图片变形,有可能超出容器 contain将图像等比拉伸，直到高度或者宽度将容器填满且始终包含在容器内

**background-size实际上是控制两个地方,宽和高,如果只写一个就是指控制宽,而高自动为auto**

| 值         | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| length     | 设置背景图像的高度和宽度。第一个值设置宽度，第二个值设置高度。如果只设置一个值，则第二个值会被设置为 "auto"。 |
| percentage | 以父元素的百分比来设置背景图像的宽度和高度。第一个值设置宽度，第二个值设置高度。如果只设置一个值，则第二个值会被设置为 "auto"。 |
| cover      | 把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。背景图像的某些部分也许无法显示在背景定位区域中。 |
| contain    | 把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。 |



### 5.7 background-origin

**background-origin 属性规定background-position属性相对于什么位置来定位,**默认值是padding-box,从padding

位置开始当做背景参考原点

| 值          | 描述                           |
| ----------- | ------------------------------ |
| padding-box | 背景图像相对于内边距框来定位。 |
| border-box  | 背景图像相对于边框盒来定位。   |
| content-box | 背景图像相对于内容框来定位。   |



###  5.8 background-clip

**background-clips属性背景的绘制区域,**默认值是border-box,在border作为绘制背景图片的区域,其实背景图片的

border区域一直有背景图,但是如果border的样式是实现的话就看不见图片,如果是点线等中空的线条的话就能看见

| 值          | 描述                   |
| ----------- | ---------------------- |
| border-box  | 背景被裁剪到边框盒。   |
| padding-box | 背景被裁剪到内边距框。 |
| content-box | 背景被裁剪到内容框。   |



### 5.9 多背景图片

可以多张背景图片一起组合作为背景图,每一个背景图中间用逗号隔开

语法:

```css
background:color url repeat position/size，
			color url repeat position/size，
			color url repeat position/size；
```

 

## 5.10 filter

**filter 属性定义了元素(通常是< img>)的可视效果**

```css
filter: none | blur() | brightness() | contrast() | drop-shadow() | grayscale() | hue-rotate() | invert() | opacity() | saturate() | sepia() | url();
```

**注意:** 

- 滤镜通常使用百分比 (如：75%), 也可以使用小数来表示 (如：0.75)
- 使用空格可以分隔多个滤镜
- 一般可以使用blur()来设置毛玻璃效果

| Filter                                             | 描述                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| none                                               | 默认值，没有效果。                                           |
| blur(*px*)                                         | 给图像设置高斯模糊。"radius"一值设定高斯函数的标准差，或者是屏幕上以多少像素融在一起， 所以值越大越模糊；  如果没有设定值，则默认是0；这个参数可设置css长度值，但不接受百分比值。 |
| brightness(*%*)                                    | 给图片应用一种线性乘法，使其看起来更亮或更暗。如果值是0%，图像会全黑。值是100%，则图像无变化。其他的值对应线性乘数效果。值超过100%也是可以的，图像会比原来更亮。如果没有设定值，默认是1。 |
| contrast(*%*)                                      | 调整图像的对比度。值是0%的话，图像会全黑。值是100%，图像不变。值可以超过100%，意味着会运用更低的对比。若没有设置值，默认是1。 |
| drop-shadow(*h-shadow v-shadow blur spread color*) | 给图像设置一个阴影效果。阴影是合成在图像下面，可以有模糊度的，可以以特定颜色画出的遮罩图的偏移版本。 函数接受<shadow>(在CSS3背景中定义)类型的值，除了"inset"关键字是不允许的 |
| grayscale(*%*)                                     | 将图像转换为灰度图像。值定义转换的比例。值为100%则完全转为灰度图像，值为0%图像无变化。值在0%到100%之间，则是效果的线性乘子。若未设置，值默认是0； |
| hue-rotate(*deg*)                                  | 给图像应用色相旋转。"angle"一值设定图像会被调整的色环角度值。值为0deg，则图像无变化。若值未设置，默认值是0deg。该值虽然没有最大值，超过360deg的值相当于又绕一圈。 |
| invert(*%*)                                        | 反转输入图像。值定义转换的比例。100%的价值是完全反转。值为0%则图像无变化。值在0%和100%之间，则是效果的线性乘子。 若值未设置，值默认是0。 |
| opacity(*%*)                                       | 转化图像的透明程度。值定义转换的比例。值为0%则是完全透明，值为100%则图像无变化。值在0%和100%之间，则是效果的线性乘子，也相当于图像样本乘以数量。 若值未设置，值默认是1。该函数与已有的opacity属性很相似，不同之处在于通过filter，一些浏览器为了提升性能会提供硬件加速。 |
| saturate(*%*)                                      | 转换图像饱和度。值定义转换的比例。值为0%则是完全不饱和，值为100%则图像无变化。其他值，则是效果的线性乘子。超过100%的值是允许的，则有更高的饱和度。 若值未设置，值默认是1。 |
| sepia(*%*)                                         | 将图像转换为深褐色。值定义转换的比例。值为100%则完全是深褐色的，值为0%图像无变化。值在0%到100%之间，则是效果的线性乘子。若未设置，值默认是0； |
| url()                                              | URL函数接受一个XML文件，该文件设置了 一个SVG滤镜，且可以包含一个锚点来指定一个具体的滤镜元素。例如：`filter: url(svg-url#element-id)` |
| initial                                            | 设置属性为默认值                                             |
| inherit                                            | 从父元素继承该属性                                           |



## 6.CSS盒子布局和定位

### 6.1 position

**position定位属性指定一个元素的定位方法的类型**,默认值是static静态定位,遵循正常的文档流

| 值       | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| absolute | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位，一个绝对定位元素会忽略父元素的padding属性,元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| fixed    | 生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| relative | 生成相对定位的元素，相对于其正常位置进行定位,依然会占据原本文档流的位置 |
| static   | 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
| sticky   | 粘性定位，该定位基于用户滚动的位置。它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。 |
| inherit  | 规定应该从父元素继承 position 属性的值。                     |
| initial  | 设置该属性为默认值                                           |

**在上述值中:只有absolute,fixed和sticky在超出滚动距离的时候才会脱离文档流,脱离文档流的盒子不会影响到**

**其他盒子的表现形式**

**注意:**

- 在使用top,left,right,bottom这些方位词时,原则上是只写top和bottom，left和right中的一个,另外一个默认是

  auto，如果都写了那么相互的拉伸力就会相互抵消,从而达不到想要的效果

- 绝对定位(absolute)默认是相对于body来定位的,如果要让子元素相对于父元素进行定位,必须在父元素上同样

  设置定位属性(静态定位除外),如果父元素没有设置定位,那么就会以最近的定位元素为参考点,以此类推最会会

  以body为参考点，不会因为是 某个元素的子元素就以它为参考点

- 绝对定位如果是以body作为参考点,那么是以网页的首屏作为参考点,就是刚刚进来时看见的页面,而不是以整个

  网页的宽度和高度作为参考点

- .默认情况下定位流的元素会盖住标准流的元素
-  默认情况下定位流的元素后面编写的会盖住前面编写的
- 所有绝对定位的盒子的表现形式都会变为block



**通过绝对定位让子元素在父元素里面水平垂直居中**

```css
.father{
    position:relative;
    width:200px;
    height:200px;
}
.child{
    position:absolute;
    top:0;
    bottom:0;
    left:0;
    right:0;
    margin:auto;
    width:100px;
    height:100px;
}
```

```html
<div class="father">
    <div class="child"></div>
</div>
```



### 6.2 z-index

**z-index分层属性设置元素的堆叠顺序**,使得拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面,默认

的值为auto,元素在当前层叠上下文中的层叠级别是0.不会创建新的局部上下文

| 值       | 描述                                    |
| -------- | --------------------------------------- |
| auto     | 默认。堆叠顺序与父元素相等。            |
| *number* | 设置元素的堆叠顺序。                    |
| inherit  | 规定应该从父元素继承 z-index 属性的值。 |

**注意:**

- z-index有从父现象，如果父元素没有设置z-index，那么子元素谁的层级大谁就显示，如果父元素设置的z-index，那么子元素的z-index就会失效，而会跟随父元素的z-index

- z-index 仅能在定位元素上生效,在static定位上不生效

- 所有元素默认值是auto,也是0层,正常情况下相同层级的元素后面的可以覆盖掉前面的,设置z-index的值为数值,

  数值越大层级越高,可以有负值,负值越大层级越小



### 6.3 float

**float属性定义元素在哪个方向浮动,**默认值是none,元素不浮动

| 值      | 描述                                                 |
| ------- | ---------------------------------------------------- |
| left    | 元素向左浮动。                                       |
| right   | 元素向右浮动。                                       |
| none    | 默认值。元素不浮动，并会显示在其在文本中出现的位置。 |
| inherit | 规定应该从父元素继承 float 属性的值。                |

**注意：**

- 浮动的元素会脱离正常的文档流,如果是子元素在父元素里面浮动,父元素不设置高可能会造成高度塌陷的问题

- 浮动的元素可以与浮动的元素进行操作,支持margin等值

- 浮动流和定位流选择其中一个使用,最好不要同时使用

- 假如在一行之上只有极少的空间可供浮动元素，那么这个元素会跳至下一行，这个过程会持续到某一行拥有足

  够的空间为止

  

### 6.4 clear

clear属性定义了元素的哪边上不允许出现浮动元素,这个属性常用做清除浮动,默认值为none，允许出现浮动元素

| 值      | 描述                                  |
| ------- | ------------------------------------- |
| left    | 在左侧不允许浮动元素。                |
| right   | 在右侧不允许浮动元素。                |
| both    | 在左右两侧均不允许浮动元素。          |
| none    | 默认值。允许浮动元素出现在两侧。      |
| inherit | 规定应该从父元素继承 clear 属性的值。 |

**注意：**

- **因为不允许出现浮动元素,但是被设置了浮动的元素已经变成块级元素,所以依然会得到块级元素的换行特性**
- **如果上一层是float元素，用了clear属性的元素的margin属性将会失效**



**清除浮动的六种方法**

- 给上一个父元素盒子设置高度

- clear清除浮动

- 外墙法清除浮动:在两个盒子中间额外添加一个块级元素，并且设置clear:both

  **注意:**只有第二个元素能用margin-top,第一个元素不能用margin-bottom来隔开距离,所以一般隔开距离的方式

  是添加中间的div的高度

- 内墙法清除浮动:在第一个盒子最后一个元素后面添加一个块级元素并且设置clear:both

  **注意:**该方法第一第二个盒子都可以使用margin属性

- 为第一个盒子添加伪元素(类似内墙法),但不占用内存空间,推荐使用

  ```css
  box1::after{
  content:"";
  display:block;
  height:0;
  visiblity:hidden;
  clear:both;
  }
  ```

- 在前一个盒子里面使用overflow:hidden,给这个盒子添加BFC环境

  **拓展:**也可以利用oveflow:hidden来让外面的盒子不设置边框的情况下里面的盒子使用margin-top的时候不会

  将外面的盒子一起顶下来 



**不定宽度的块状元素有三种方法居中**

- 加入 table 标签

  ```html
  <table>
      <tbody>
          <tr>
              <td></td> 
          </tr>
      </tbody>
  </table>
  ```

-  设置 display: inline 方法:与第一种类似，显示类型设为 行内元素，进行不定宽元素的属性设置

- 设置 position:relative 和 left:50%;利用 相对定位 的方式，将元素向左偏移 50% ，即达到居中的目的

  如:给父元素设置float:left;position:relative;left:50%;给子元素设置position:relative;left:-50% 



## 7.过渡模块

**transition过渡属性规定某一个要变化的属性从初始值到变化值的延迟效果,**该属性是一个复合属性

**复合属性语法:**

```css
transition:transition-property transition-duration transition-timing-function transition-delay 
```

**过渡效果的要素：**

- 必须有属性变化
- 必须告诉系统哪个属性需要执行过渡效果
- 必须告诉系统过渡效效果持续时长

上面的语法依次为:transition:过渡属性 过渡时长 过渡速度 延迟时间。可以省略后面两个参数，因为前面两个已经

有了过渡的三要素

**如果想给多个属性添加过渡效果,需要用逗号隔开**

transition:过渡属性 过渡时长 过渡速度 延迟时间,过渡属性 过渡时长 过渡速度 延迟时间;

### 7.1 过渡属性

**transition-property属性设置要过渡的属性,**默认值是all,也就是所有变化的属性,需要哪些元素的哪些属性发生改

变,就在transition里面设置哪个属性,如果所有的属性要变化的时间,速度和延迟时间等都写了并且相同,可以直接写

all代替

**如果有多个属性要过渡:transition-property:属性1,属性2;**



### 7.2 过渡时间

**transition-duration属性设置要实现过渡效果所用的时间,**单位为s或者ms,如果不设置时间默认是0

**如果有多个属性要过渡:transition-duration:时间1,时间2;**



### 7.3 过渡速度

**transition-timing-function属性设置在相同的时间内过渡是通过怎样的方式进行的,**默认的值是ease先慢后快

| 值                            | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| linear                        | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。 |
| ease                          | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。 |
| ease-in                       | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。  |
| ease-out                      | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。  |
| ease-in-out                   | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。 |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。 |

**注:过渡速度一般都用的匀速linear,如果想用一些特别的速度模式,用cubil-bezier(贝塞尔曲线)来写出自己想要**

**的运动模式,这个一般可以直接在百度上搜索然后调试出来**



### 7.4 过渡延迟

**transition-delay属性设置过渡的延迟时间**,告诉系统延迟多少秒之后才开始进行过渡动画，可以通过设置这个来

达到多个盒子实现跑火车的效果

**不支持过渡的属性:display:none**

让一个元素消失的过渡一般用透明opacity来做,但是这个属性设置之后可以选中文本.所以一般配合

visibility:hidden来使用,至于为什么直接用这个属性来实现消失的过渡效果,因为这个属性的过渡效果很奇怪,元素

其他过渡完成后才突然消失,虽然不像display:none一样直接开始就消失,但是也没有过渡效果产生

**注意:**

- 一般过渡效果都是具体的值来变化的,这样才最适合,当一个元素设定absolute时,如果不设置初始位置，那么默

  认的初始位置是上下左右全为auto而不是0,所以如果不设置初始位置,过渡变化absolute时不会支持过渡属性

- 要写过渡元素时，先不要管过渡属性，而是先写基本界面，修改我们认为需要修改的属性，最后再反过来修改

  过渡元素

  

## 8.动画模块

 **animation动画属性规定元素呈现自动运行的过渡效果,**该属性是一个复合属性

**复合属性语法:**animation:动画名称 动画时长 动画运动速度 延迟时间 执行次数 往返动画

```css
animation: animation-name animation-duration animation-timing-function animation-delay animation-iteration-count animation-direction;
```

**动画三要素**

- 告诉系统执行哪个动画用animation-name:run(名称随便取)确定动画名称
- 告诉系统我们需要创建一个名称叫做run的动画
- 告诉系统动画持续的时长如:animation-duration:3s;

### 8.1 动画名称

**animation-name属性为@keyframes(关键帧)动画规定名称**,只有通过这个名称才能将元素与动画联系起来

**创建一个关键帧:**

```css
/*第一种方法,只能进行两次动画帧的修改*/
@keyframes run{
from{
margin-left:0;
	}
to{
margin-left:500px;
	}
}
/*第二种方法,能进行多次动画帧的修改*/
@keyframes run{
0%{
}
1%{
}
/*
 中间可以用0%到100%的任意值，想指定多少节点就指定多少个
 注意:0%是初始帧,100%是最后一帧，与animation-fill-mode属性相关
 比如:   
 */
@keyframes run{ 
0%{
left:0;
top:0;
}
25%{
left:300px;
top:0;
}
50%{
left:300px;
top:300px;
}
75%{
left:0;
top:300px;
}
100%{
left:0;
top:0;
}	 
}
 /*
  注意:可以用多元素选择器,如果不同的帧有相同的样式可以用,隔开一起设置
    比如:50%处和75处都是一样的
    50%,75%{  
			}
  */
```



### 8.2 动画时长

**animation-duration属性定义动画完成一个周期所需要的时间,**单位为s或者ms



### 8.3 动画运动速度

**animation-timing-function属性规定动画的速度曲线,**默认值是ease,先慢后快

| 值                            | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| linear                        | 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。 |
| ease                          | 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。 |
| ease-in                       | 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。  |
| ease-out                      | 规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。  |
| ease-in-out                   | 规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。 |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。 |



### 8.4 动画延迟

**animation-delay 属性定义动画何时开始,**单位为s或者ms,这个动画延迟是触发动画后离正式进行动画的时间差

**注意:这个属性的值允许负值,如:-2s使动画马上开始,但跳过 2 秒进入动画,而过渡不能有负值。**



### 8.5 动画播放次数

**animation-iteration-count属性定义动画的播放次数,**默认值是1,代表动画值进行一次

| 值       | 描述                     |
| -------- | ------------------------ |
| *n*      | 定义动画播放次数的数值。 |
| infinite | 规定动画应该无限次播放。 |



### 8.6 动画进行方向

**animation-direction属性是否应该轮流反向播放动画**,默认值是normal.动画正常播放

| 值                | 描述                     |
| ----------------- | ------------------------ |
| normal            | 默认值。动画按正常播放   |
| reverse           | 动画反向播放             |
| alternate         | 动画先正向播放再反向播放 |
| alternate-reverse | 动画先反向播放再正向播放 |



### 8.7 动画状态

animation-play-state属性告诉系统当前动画的进行状态,默认值为running执行动画

| 值      | 描述                                         |
| ------- | -------------------------------------------- |
| paused  | 指定暂停动画,通常用做hover效果等悬停暂停效果 |
| running | 指定正在运行的动画                           |

**注:动画有三个状态:等待状态 执行状态 结束状态**



### 8.8 动画应用样式

animation-fill-mode属性规定当动画不播放时(当动画完成时，或当动画有一个延迟未开始播放时),要应用到元素

的样式，默认值是none,不应用任何样式

| 值        | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| none      | 默认值。动画在动画执行之前和之后不会应用任何样式到目标元素。 |
| forwards  | 在动画结束后,动画会回到本身属性设置的地方,这个属性让元素保持最后一帧100%是的位置和样式 |
| backwards | 在动画开始前(在动画延迟时间里或者动画本身的原始状态和第一帧的状态不一样时),应用第一帧0%的样式 |
| both      | 相当于同时应用了forwards和backwards                          |



**过渡动画与动画之间的异同**

**1.不同点**

过渡必须认为的触发才会执行动画  动画不需要人为的就可以执行

**2.相同点**

过渡和动画都是用来给元素添加动画的,过渡和动画都是系统新增的一些属性,过渡和动画都需要满足三要素才会有

动画效果



## 9.2D/3D转换模块

### 9.1 transform

**transform属性意为变化,通过这个属性可以对一个盒子进行2D\3D形式的变化**

#### 9.1.1 rotate

rotate旋转,这个值控制盒子是否进行2D旋转,可以接收一个参数里面的值需要带单位,支持负值

**deg度数 turn圈 rad弧度 grad梯度**   比如:rotate(45deg)就是顺时针旋转45度,rotate(45deg)就是逆时针旋转45度

**旋转中心**

**默认情况下旋转的中心是元素自身的中心点**,如正方形就是两条对角线的交界处

如果要改变旋转的中心点,可以通过形变中心点属性transform-origin来修改参考点

**transform-origin:水平 垂直**,里面的坐标是相对于自身元素的 

比如transform-origin:0 0代表元素自身的左上角

**取值的三种形式:**

- 具体像素,只有3DZ轴旋转的时候才支持,2D旋转不支持具体像素

- 百分比,比如默认的是宽度和高度的50%处 写作 50% 50%
- 特殊关键字,如left right top bottom center等

**默认情况下所有元素围绕Z轴旋转**，也就是transform:rotateZ(),想围绕那个轴旋转,只用在rotate后添加想要旋转的

轴即可,但是如果要向绕X和Y轴旋转，虽然在2D的情况下也能用这个值,但是看出来很怪异,所以在2D情况下一般都

是遵循默认情况



#### 9.1.2 skew

skew倾斜,让元素产生2D扭曲,这个值可以接收2个参数,第一个是在x轴上的倾斜,第二个参数是y轴,值的单位是deg,

控制x轴倾斜视角上呈现的是左右向两边拉动,高度变小,y轴视角上时上下向两边拉动,宽度变小,最终使元素呈现扭

曲的样式,最后可能会扭曲消失

```css
transform:skew(45deg,45deg);
/*
	上方这种写就会让整个元素消失不见
*/
```



#### 9.1.3 translate

translate位移,这个值可以接受两个参数(x,y),其中x和y分别代表x轴和y轴,意为水平和垂直方向，如果只写一个参

数,只会对x轴起作用

```css
transform:translate(100px,100px);
```

**也可以单独的只设置x轴或者y轴方向上的位移**

- translateX,这个值只接收一个参数,就是水平方向上的位移

  ```css
  transform:translateX(100px);
  ```

- translateY,这个值只接收一个参数,就是垂直方向上的位移

  ```css
  transform:translateY(100px);
  ```

- translateZ，同上, 定义3D之后才用translateZ,这时translateX等就不是2D面上的偏移了,而是在3D的坐标轴上

  偏移

  ```css
  transform:translateZ(100px);
  ```

**注意:**

- 因为可以在transform中写入不同的2D变化,中间只需要用空格隔开,所以如果旋转了再平移,平移的坐标系就会

  发生改变,平移的坐标系应为旋转后会修改坐标系

  **所以如果要多个一起写,translate放在最前面**

- 位移后实际上还是占据了原来的位置,这一点和相对定位类似,但是比相对定位简单一点

- 这个的值可以是具体的值也可以是百分比,这个百分比是相对自身长度或高度的百分比,不是父元素的

- 位移的方向计算方式和background-position一样,都是以右和下为正方向,上和左为负方向

```css
/*在不知道父元素宽高的情况下如果让子元素在父元素里面绝对居中*/
.father{
    position:relative;
}
.child{
    position:absolute;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%);
}
```



#### 9.1.4 scale

scale缩放,将一个元素缩放为原来的多少倍数,可以接受两个参数,第一个参数是对水平方向进行缩放,第二个参数是对垂

直方向进行缩放,默认是scale(1,1),值为0到正无穷大的数字,值为0到1是缩小(值为0是没有缩放),值为1到正无穷是放

大,如果水平和垂直的缩放一样,可以只写一个参数

```css
transform:scale(2,1);
```

注意:正常情况下scale中传入的参数为负值应该是没有用的,但是在这里传入负值的样子会向电磁感应一样,如果传入的是-1会以上方为中心绕x轴一圈反向展示



### 9.2 3D转换模块

#### 9.2.1 perspective

**perspective为透视属性,定义 3D 元素距视图的距离,**单位为px,默认值是none,与单位为0相同,该属性允许用户改变 

3D 元素查看 3D 元素的视图。

**注意:**

- **当为元素定义 perspective 属性时,其子元素会获得透视效果,而不是元素本身**。在需要呈现效果元素的父元素或祖先元素中加一个perspective属性,就能呈现近大远小效果,给人带来3D的视觉感受

  **像素的数值是元素距离视图的距离**,若为10px则效果特别明显，若为10000px则基本没变化，因为距离太远，具体的值需要自己进行调试

  **注意:这个属性不兼容所有浏览器,如果用chrome需要用-webkit-perspective**

- perspective 属性只影响 3D 转换元素
- 这个元素最好与perspective-origin 属性一起使用,这样就能改变3D元素的底部位置



#### 9.2.2 perspective-origin

**perspective-origin属性定义 3D 元素所基于的x轴和y轴,**该属性允许用户改变3D元素的底部位置,,和

transform-origin属性一样都是有两个参数x轴和y轴,默认值为50% 50%,也就是x和y轴的中间位置

**取值的三种形式:**

- 具体像素,因为本来就是3D状态,支持具体的像素值

- 百分比,比如默认的是宽度和高度的50%处 写作 50% 50%
- 特殊关键字,如left right top bottom center等



#### 9.2.3 transform-style

transform-style属性规定如何在 3D 空间中呈现被嵌套的元素,默认值为flat,子元素将不保留其 3D 位置。

| 值          | 描述                       |
| ----------- | -------------------------- |
| flat        | 子元素将不保留其 3D 位置。 |
| preserve-3d | 子元素将保留其 3D 位置。   |

**注意：**该属性必须与transform属性一起使用



#### 9.2.4 backface-visibility

backface-visibility属性定义当元素不面向屏幕时是否可见,默认值是visible,显示背面是可见的,该属性通常用在旋转时不想要看见元素背面

| 值      | 描述             |
| ------- | ---------------- |
| visible | 背面是可见的。   |
| hidden  | 背面是不可见的。 |



**企业开发的正方体竖直轮转** 

**按照上 后 下 前的顺序做每一个边(因为左右两个边其实可以不呈现出来)**,假如父元素的厚度为200px

transform:rotateX(90deg/180deg/270deg/360deg)(里面的角度分别对应了上后下前的顺序) translateZ(100px)

(里面的数字是父元素的一半厚度，因为默认这些子元素是在父元素的中间）

最后只用给这个正方体的父元素设置3D视角就可以了

**扩展:**要做长方体只用把上 后 下 前四个面用transform:scale扩大(只扩大长宽,不扩大正方体的厚度),左右再多移动

100px



## 10.媒体查询

**@media 查询可以针对不同的媒体类型或者不同的屏幕尺寸设置不同的样式**,@media常用于响应式页面的布局

@media在对媒体类型设置的时候一般都是screen,用于电脑屏幕,平板电脑,智能手机等，同时在设计screen尺寸样

式的时候也是一般通过max-width和min-width来设置

**max-width可以理解为宽度小于等于最大宽度的时候样式才生效,min-width可以理解为宽度大于等于最小宽度**

**的时候样式才生效**

```css
@media screen and (min-width:300px) and (max-width:799px){
    body{
        background:red;
    }
}
@media screen and (min-width:800px) and (max-width:799px){
    body{
        background:green;
    }
}
@media screen and (min-width:1200px){
    body{
        background:blue;
    }
}
/*
	一般设置响应式的时候都是只设置最小或者最大宽度,并两个都设置,所以在这里后面的max-width可以舍去,效	  果还是一样的
*/
```

**注意:**

在都设置的是最小宽度的时候,min-width按照从小到大的顺序写,在都设置的是最大宽度的时候,max-width按照从

大到小的顺序写,这样写是为了后面写的到了合适的时候会覆盖前面的内容,如果写反了就会只有一种样式

### 10.1 引入资源

**媒体查询还可以根据不同的大小引入不同的资源文件**

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <link rel="stylesheet" href="a.css" modia="screen and (min-width:320px)"/>
        <link rel="stylesheet" href="b.css" modia="screen and (min-width:640px)"/>
        <title>Document</title>
    </head>
    <body>
        <div>
            <!-- coding... -->
        </div>
    </body>
</html>
```



## 11.布局思路

- **圣杯布局**

  一个大盒子要设置最小宽度min-width，大盒子中间写在最上面，宽度为100%,大盒子设置左右padding分别留出左右部分的位置大小，然后左右中三者都要浮动，然后左右设置margin跑到上中间里，通过相对定位达到响应式布局

- **双飞翼布局**

  也是一个大盒子设置最小宽度，中间部分外面多了一层设置宽度为100%，然后中间部分设置margin刚好是左右的宽度，左右部分和中间部分外面都要浮动，然后通过外边距设置调到上方

**注:**只是简单写了下布局的思路,如果要具体了解请参考其他资料


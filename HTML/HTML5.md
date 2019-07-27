# HTML5

## 1.拖拽

**拖拽事件需要在需要拖拽的元素上设置`draggable=true`来让该元素可以被拖拽**

### 1.1 主要事件

- **dragstart**:当用户开始拖动元素或者拖动选中文本时触发，应用在被拖拽元素上
- **drag**:当元素或者选中的文本被拖动时触发（每几百毫秒触发一次），应用在被拖拽元素上
- **dragend**:当拖动操作结束时触发（通过释放鼠标按钮或者点击转义键），应用在被拖拽元素上
- **dragenter**:当一个被拖动的元素或者选中的文本进入一个有效的放置目标时触发，应用在目标元素上
- **dragover**:当元素或者选中的文本被拖动到有效放置区域上方时触发（每几百毫秒触发一次），应用在目标元素上
- **dragleave**：当拖动元素或者选中的文本离开有效的放置区域时触发，应用在目标元素上
- **drop**:当元素或选中的文本在有效区域放置时触发，应用在目标元素上
- **dragexit**:当元素不再是拖动操作的直接选择元素时触发(很少使用)



### 1.2 注意事项及兼容问题

- **ondrop**事件不能调用,因为HTML元素默认是阻止放的操作的,想让这个事件被调用,必须要在正在拖拽的时候**ondragover**时阻止默认事件

- 注意火狐浏览器在拖拽时需要要携带数据,在其它浏览器你只需要在HTML元素上加上`draggable=true`即可 ,但因为火狐要求被拖动元素必须包含数据

  ```js
  box.ondragstart=function(e){ 
      e.dataTransfer.setData("index",1); 
      /*
      在进行拖放操作时,e.dataTransfer对象用来保存被拖动的数据。它可以保存一项或多项数据、一种或者多	  种数据类型,这个对象在所有的拖动事件属性dataTransfer 都是可用的，但是不能单独创建
      */
  } 
  //但是火狐会默认将携带的页面,所以需要阻止事件冒泡事件和默认事件
  document.ondragover = function (e) {
      e.preventDefault();//阻止默认事件
      e.stopPropagation();//阻止冒泡
      //上面两者一起用就可以了
      return false;
  }
  document.ondrop = function (e) {
      e.preventDefault();
      e.stopPropagation();
      return false;
  }
  ```

  

### 1.3 案例-将外部文件拖入盒子中

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      html,
      body {
        overflow: hidden;
        width: 100%;
        height: 100%;
        background-color: pink;
      }
      #box {
        overflow: hidden;
        margin: 300px auto;
        width: 1066px;
        height: 150px;
        background: #fff;
      }
    </style>
  </head>
  <body>
    <div id="box"></div>
    <script>
      let box = document.getElementById("box");
      box.ondrop = function(e) {
        let fileReader = new FileReader();//创建一个可读文档对象
        fileReader.readAsDataURL(e.dataTransfer.files[0]);//读取拖拽的文件
        fileReader.onload = function(e) {//可读文档对象创建后可以进行一系列操作
          let img = new Image();//创建一个img文档对象
          //let img = document.createElement("img");也可以使用该方法
          img.src = fileReader.result;//该数据是base64位的,特别长,所以可以使用Blob进行
            /*
            	const blob=new Blob([e.dataTransfer.files[0]]);
            	img.src=window.URL.createObjectURL(blob);
            	window.URL.createObjectURL()能将一个blob对象转换为可以使用的URL对象
            */
          img.style.cssText = "height:100%;width:auto";
          box.appendChild(img);
        };
        e.preventDefault();//必须要阻止默认事件才能在盒子中展示,否则会在浏览器中打开该图片
        e.stopPropagation();
      };
      box.ondragover = function(e) {
        e.preventDefault();//必须要阻止默认事件才能在盒子中展示,否则会在浏览器中打开该图片
        e.stopPropagation();
      };
    </script>
  </body>
</html>
```

**MIME类型文件**
MIME (Multipurpose Internet Mail Extensions) 是描述消息内容类型的因特网标准。MIME消息能包含文本、图像、音频、视频以及其他应用程序专用的数据,类型大约有191种类型,不同的应用程序支持不同的 MIME 类型



**base64类型文件**

base64是网络上最常见的用于传输8Bit字节码的编码方式之一,base64就是一种基于64个可打印字符来表示二进制数据的方法,base64编码是从二进制到字符的过程中,可用于在HTTP环境下传递较长的标识信息



**FileReader文件对象**

FileReader API 用于读取文件，即把文件内容读入内存，是一种异步文件读取机制，它的参数是 File 对象或 Blob 对象。对于不同类型的文件，FileReader 提供不同的方法读取文件

```js
//创建读取文件的对象
var reader = new FileReader();	

readAsDataURL(Blob|File);
//读取文件并将文件以数据URI的形式保存在result属性中，返回一个基于 Base64 编码的 data-uri 对象
reader.readAsDataURL(files[0]);


readAsText(Blob|File, opt_encoding);
/*
返回文本字符串。默认情况下，文本编码格式是 UTF-8，可以通过可选的格式参数，指定其他编码格式的文本
以纯文本形式读取文件，将读取到的文本保存在result属性中，第二个参数用于指定编码类型，可选的
*/
reader.readAsText( files[0],encoding );


readAsBinaryString(Blob|File)//IE可能不支持
/*
返回二进制字符串，该字符串每个字节包含一个 0 到 255 之间的整数。（已废弃）
读取文件并将一个字符保存在result属性中，字符串的每个字符表示一字节
*/
reader.readAsBinaryString(files);


readAsArrayBuffer(Blob|File);//IE可能不支持
/*返回一个 ArrayBuffer 对象
读取文件并将一个包含文件内容的ArrayBuffer保存在result属性中
*/
reader.readAsArrayBuffer(files)
```



### 1.4 dataTransfer

- **setData(format,data):**设置拖拽元素的信息
  - format:系统默认格式为`text/plain`(也可以直接写text)、`text/html`、`text/xml`、`text/uri-list`(也可以自定义format，把format-data当key-value键值对使用)

    ```js
    //兼容写法
    //获取URL
    var url = dataTransfer.getData('url') || dataTransfer.getData('text/uri-list')
    //获取文本
    var url = dataTransfer.getData('Text') || dataTransfer.getData('text/plain')
    ```

  - data:保存在拖拽元素中的数据
- **getData(format)**:获取拖拽元素的信息,可以通过该方式获取选中拖入的文字,然后将文字拖到投放区

  - format:和setData里的format遥相呼应，才能取到相应的值
- **clearData():**清除拖拽信息

**dataTransfer的常用属性**

- **effectAllowed:**设置拖拽时应带有的样式类型

  **注:**应该在dragstart事件中设置此属性，以便为拖动源设置所需的拖动效果,在dragenter 和dragover 事件处理程序中,该属性将设置为在dragstart 事件期间分配的任何值,因此,可以使用effectAllowed来确定允许哪个效果

  **值:**

  - **none**,此项表示不允许放下
  - **copy**,源项目的复制项可能会出现在新位置。
  - **copyLink**,允许 *copy* 或者 *link* 操作
  - **copyMove**,允许 *copy* 或者 *move* 操作
  - **link**,可以在新地方建立与源的链接
  - **linkMove**,允许 *link* 或者 *move* 操作
  - **move**,一个项目可能被移动到新位置
  - **all**,允许所有的操作
  - **uninitialized**,效果没有设置时的默认值，则等同于*all*

- **dropEffect:**设置拖拽元素被放下时的样式

- **files**:内含一系列文件信息，常用于将文件从桌面拖向浏览器



## 2.Blob

**file对象的父类型是Blob对象,Blob对象代表了一段二进制数据,提供了一系列操作接口**

**生成 Blob 对象有两种方法**

- 使用 Blob 构造函数
- 另一种是对现有的 Blob 对象使用 slice 方法切出一部分

**Blob对象有两个只读属性**

- **size:**二进制数据的大小,单位为字节(文件上传时,可以在前端判断文件大小是否合适)
- **type:**二进制数据的 MIME 类型,全部为小写,如果类型未知,则该值为空字符串(文件上传时可以在前端判断文件类型是否合适)
  + **gbk编码:**数字字母 一字节 1KB= 1024字节 一个中文汉字是2字节
  + **UTF-8编码:**数字字母 一字节 1KB= 1024字节 一个中文汉字是3字节

### 2.1 Bolb构造函数

**Blob 构造函数接受两个参数,但是这两个参数都不是必需的**

- 一个包含实际数据的数组
- 数据的类型

```js
var arr = ["hello", "world"]
var Blob = new Blob(arr, { "type" : "text/xml" })
console.log(Blob)
```



### 2.2 Bolb对象的slice方法

**Blob对象的slice方法,将二进制数据按照字节分块,并且返回一个新的Blob对象,只读取文件的一部分可以节省时间,非常适合只关注数据中某个特定部分(如文件文件)的情况**

```js
var arr = ["hello", "world"]
var Blob = new Blob(arr, { "type" : "text/xml" })
var newBlob = Blob.slice(0, 5);//用在分片文件,后台接收把这些片段检验并组合一个文件
console.log(newBlob);
```

```js
var reader = new FileReader()
var blob = blogSlice( e.dataTransfer.files[0] , 0 , 20 )
reader.readAsText( blob )
if( blob ){
   	reader.onload = function(){
		box.innerHTML = this.result
	}
}else{
  	alert('no data')
}


function blogSlice( blob,start,end ){
    if( blog.slice ){
		return blob.slice(start,end)
    }else if( blob.webkitSlice ){
      	return blob.webkitSlice(start,end)
    }else if( blob.mozSlice ){
    	return blob.mozSlice(start,end)         
    }else{
      	return null
    }
}
```



### 2.3 对象URL

**对象URL也被称为blob URL,指的是引用保存在File或Blob中数据的URL,使用对象URL的好处是没必要把内容读取到JS中,而直接使用文件内容,能生成一个链接,例如`img.src = URL`**



**创建对象URL**

用`window.URL.createObjectURL( blob )`方法,并传入flle或Blob对象,对二进制数据生成一个 临时的URL,这个URL 可以放置于任何通常可以放置 URL 的地方,**比如img标签的src属性**

```js
function createObject(blob){
  	if( window.URL ){
      	return window.URL.createObjectURL(blob)
  	}else if( window.webkitURL ){
      	return window.webkitURL.createObjectURL(blob)
  	}else{
      	return null
  	}
}
//createObject函数的返回值是一个字符串，指向一块内存地址。因为这个字符串是URL，所以在DOM中也能使用
var reader = new FileReader()
var url = createObject( files[0] )
if( url ){
  	if( /image/.test(files[0].type) ){
     	box.innerHTML = '<img src="'+url+'" />'
    }else{
      	alert( 'no img' )
    }
}else{
  	alert( 'no data' )
}
/*
	直接把对象URL放在img标签中，就省去了把数据先读取js中，另一方面img标签则会找到相应的内存地址，直接	  读取数据并将图像显示到页面中
*/
```

**删除对象URL**

出于一些特殊的需要,也可以使URL失效,调用`window.URL.revokeObjectURL(url)` 方法,使 URL失效



### 2.4 案例-使用Bolb和对象URL下载文件

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <script>
      let txt = "Hello World";
      create("Coloring.txt", txt);
      function create(fileName, content) {
        let blob = new Blob([content]);//一个bolb对象
        let link = document.createElement("a");//创建一个a连接
        link.download = fileName;//从浏览器下载文件的事件
        link.innerHTML = content;//写入文件的内容
        link.href = window.URL.createObjectURL(blob);//将blob转换为对象URL
        link.click();//执行这段代码不用点击也能自动打开a标签
      }
    </script>
  </body>
</html>
```



### 2.5 加密

#### 2.5.1 encodeURI和decodeURI

- encodeURI()用做将字符串转变为URL格式的编码
- decodeURI()用做将URL格式变化转变为普通的字符串



#### 2.5.2 btoa和atob

- btoa()用做将传入的字符串进行加密

  **注意:**被转化为base64位的编码格式,不能对普通的中文编码进行加密,最好先转换为encodeURI编码

- atob()用做将btoa()加密的字符串解密



## 3.attribute和property

- **HTML标签的预定义和自定义属性统称为attribute**
- **JS原生对象的直接属性统称property**

### 3.1 布尔值属性与非布尔值属性

**property的属性值为布尔类型的统称为布尔值属性,属性值为非布尔值类型的统称为非布尔值属性**

**attribute与property的同步关系**

- **非布尔值属性:**实时同步
- **布尔值属性:**
  - property永远不会同步attribute
  - 在没有动过property的情况下,attribute会同步property,在动过property的情况下,attribute不会同步property

**注意:**

- 用户操作的是property
- 浏览器认的是property



## 4.Canvas

### 4.1 什么是Canvas?

**`canvas`是HTML5新增元素,可用于通过使用JS绘制图形,可以使用`canvas`标签定义一个`canvas`元素**

**注意:**

- 使用`canvas`标签时建议成对出现,不要使用闭合形式
- `canvas`元素具有默认宽高,`width:300px;height:150px`



**替换内容**

有些浏览器不支持使用`canvas`,可以在`canvas`标签中提供需要替换的内容,支持`canvas`的浏览器会正常显示`canvas`,不会显示其中的内容,而不支持`canvas`的浏览器会显示替代内容

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
   	  <canvas><span>您当前的浏览器版本太低,请更新到最新版本</span></canvas>
      <script>
      	  let canvas=document.querySeletor("canvas");
          if(canvas){//如果能正常获取
           	  let ctx=canvas.getContext("2d");//2d方式获取上下文内容   
          }
      </script>
  </body>
</html>

```



### 4.2 画布API

- `canvas`标签只有width和height两个属性,并且这两个属性都是可选的,如果没有设置height和width,`canvas`会初始化宽度为300px和高度150px
- `canvas`标签有一个方法**getContext()**用于获取上下文

**画布中的width与height**

- HTML属性中设置的高宽只会影响画布本身的宽高而不会影响其中内容的宽高
- CSS属性中设置的高宽不但会影响画布本身的高宽,还会使得画布中的内容等比例缩放(缩放参照于画布默认的尺寸)



### 4.3 绘制矩形

**`canvas`提供了三种方法绘制矩形**

- **fillRect(x,y,width,height)**,绘制一个填充的矩形(填充色默认为黑色)
- **strokeRect(x,y,width,height)**,绘制一个矩形的边框(默认的边框颜色为1px实心黑色)
- **clearRect(x,y,width,height)**,清除指定的矩形区域,让清除部分完全透明

**注:**

- x与y指定了在`canvas`画布上所绘制的矩形的左上角(相对于原点)的坐标,width与height设置矩形的尺寸,如果存在边框,边框会在width与height上占据一个边框的宽度
- 里面所有的参数都不带任何单位

```js
let canvas=document.querySeletor("canvas");
let ctx=canvas.getContext("2d");
ctx.fillRect(0,0,100,100);
ctx.strokeRect(100,100,100,100);
ctx.clearRect(100,100,100,100);
```



**使用strokeRect时边框出现的像素渲染问题**

**按理说如果是使用的默认的边框边框的宽度应该是1px,但是`canvas`在渲染边框时,边框的宽度是平均在偏移位置的两侧进行渲染的**

```js
let canvas=document.querySeletor("canvas");
let context=canvas.getContext("2d");
context.strokeRect(10,10,50,50);
//边框会渲染在10.5与9.5之间,但是浏览器不会让一个像素只占一半,所以边框会被渲染在11与9之间,边框宽度为2
context.strokeRect(10.5,10.5,50,50);//如果这样写的话边框就会渲染在10与11之间,向上取整
```

#### 4.3.1 添加样式颜色

- **fillStyle**,设置矩形的填充颜色
- **strokeStyle**,设置图形轮廓的颜色,默认情况下填充的颜色都是黑色
- **lineWidth**,设置当前绘制线条的颜色,值必须为正数,默认是1,如果值是0,负数,infinity和NaN时会被忽略

```js
let canvas=document.querySeletor("canvas");
let ctx=canvas.getContext("2d");
ctx.fillStyle="pink";
ctx.stokeStyle="red";
ctx.lineWidth=50;
ctx.fillRect(0,0,100,100);
ctx.strokeRect(100,100,100,100);
ctx.clearRect(100,100,100,100);
```

**注意:**

- `canvas`内部的样式是同步布置的,所以绘制的调用先后会对最后的结果产生很大的影响,如果想最开始就有样式的话就需要先在前面定义了样式后再进行绘制,同时后面绘制的图形的层级比前面的高,所以会覆盖住前面的图形
- 填充图形的样式不会作用给线条图形,线条图形也不会影响到填充图形,并且l**ineWidth**属性也只会作用给线条图形



#### 4.3.2 设置线条接合样式

**lineJoin能设置线条与线条之间接合处的样式**,默认是`miter`直角

**值:**

- round:圆角
- bevel:斜角
- miter:直角

```js
let canvas=document.querySeletor("canvas");
let ctx=canvas.getContext("2d");
ctx.lineJoin="round";
ctx.strokeRect(100,100,100,100);
```



### 4.4 绘制路径

图形的基本元素是路径,路径是通过不同颜色和宽度的线段或曲线相连而成的不同形状的集合

**绘制路径的步骤**

- 先创建路径起始点
- 再使用画图命令绘画路径
- 把路径封闭
- 一旦路径生成,就能通过描边或填充路径区域来渲染图形



#### 4.4.1 路径方法

- **beginPath()**,新建一条路径,生成之后图形绘制命令被指向到路径上准备生成路径,生成路径的第一步就是先使用**beginPath()**。本质上,路径是由很多子路径构成,这些子路径都是在一个列表里,所有的子路径(线、弧形等)构成图形。而每次调用该方法后列表都会被清空重置,我们就可以重新绘制新的图形

- **moveTo(x,y)**,将触笔移动到指定的坐标x以及y上,当`canvas`初始化或者**beginPath()**调用后,通常会使用**moveTo()**设置起点

- **lineTo(x,y)**,绘制一条从当前位置到指定x以及y位置的直线

- **closePath()**,闭合路径之后图形绘制命令又重新指向到上下文,该方法并不是必须要调用,该方法会通过绘制一条从当前点到开始点的直线来闭合图形,如果图形已经闭合了,该方法不会有任何作用

  **注:**当调用**fill()**函数后所有没有闭合的形状都会自动闭合,不需要该函数,而如果调用**stroke()**函数就不会自动闭合路径

- **stroke()**,通过线条来绘制图形轮廓,该函数不会自动调用**closePath()**

- **fill()**,通过填充路径的内容区域生成实心的图形,该函数会自动调用**closePath()**

- **rect(x,y,width,height)**,绘制一个左上角(x,y),宽高为width和height的矩形,当该方法执行的时候,**moveTo()**会自动设置坐标参数为(0,0),也就是说当前笔触会自动充值坐标

- **lineCap**,lineCap是**Canvas 2D API**指定如何绘制每一条线代末端的属性,该属性默认值是butt,线段末端以方形结束

  **值:**

  + butt:线段末端以方形结束
  + round:线段末端以圆形结束
  + square:线段末端以方形结束,但是增加了一个宽度和线段相同,高度是线段厚度一半的矩形区域

- **save()**,save()是**Canvas 2D API**通过将当前样式状态放入样式栈中,保存`canvas`全部状态的方法

  保存到样式栈中额绘制状态由下面几个部分组成:

  + 当前的变换矩阵
  + 当前的剪切区域
  + 当前的虚线列表
  + strokeStyle,fillStyle,lineWidth,lineCap,lineJoin等当前的值

- **restore()**,restore()是**Canvas 2D API**通过在绘图状态栈中弹出顶端的状态,可以将`canvas`恢复到最近的保存状态的中,如果没有保存该方法不做任何改变

**注意:**

- **save()和restore()方法需要成对出现使用**
- **save()和restore()是控制样式的设置,而beiginPath()是关于路径的设置**



#### 4.4.2 路径容器与样式容器

- **路径容器:**每次调用关于路径的API时,都会往路径容器中添加路径做登记,调用beginPath()时,情况整个路径容器
- **样式容器:**每次调用样式API时,都会往样式容器里做登记,调用save()的时候,将样式容器里的状态记入样式栈,调用restore()的时候,将样式栈的栈顶状态弹出到样式容器里进行覆盖
- **样式栈:**调用save()的时候,将样式容器里的状态记入样式栈,调用restore()的时候,将样式栈的栈顶状态弹出到样式容器里进行覆盖



#### 4.4.3 绘制圆形

**角度与弧度的转化表达式:`radians=(Math.PI/180)*degrees`**

- **arc(x,y,radius,startAngle,endAngle,anticlokwise)**,该方法用于画一个以(x,y)为圆心的以radius为半径的圆弧(圆),从startAngle开始到endAngle结束,按照anticlockwise给定的方向(该值为一个布尔值,默认为false顺时针方向)来生成

  - x,y为绘制圆弧所在圆上的圆心坐标,相对于原点
  - radius为半径
  - startAngle和endAngle参数用**弧度**定义了开始以及结束位置的弧度,都是以x轴为基准参数
  - anticlockwise为一个布尔值,当值为true时代表逆时针方向,为false时代表顺时针方向

- **arcTo(x1,y1,x2,y2,radius)**,根据给定的控制点和半径画一段圆弧

  **注意:**使用该方法其实需要三个控制点,**第一个控制点是第一次使用moveTo()时给定的地方,**从哪个地方其向(x1,y1)方向进行画圆弧,也就是说**该圆弧必定经过moveTo()设定的第一个点和(x2,y2),但不一定经过(x1,y1),这个坐标只是为了控制圆弧的方向**,而**圆弧的半径设置就是用一个半径为radius的圆往两个控制点夹角进行移动,直到刚好能够卡住两条夹线**

- **quadraticCurveTo(cp1x,cp1y,x,y)**,该方法用于绘制二次贝塞尔曲线,**(cp1x,cp1y)为一个控制点,(x,y)为结束点,起始点为moveTo()刚抬起指定的点**

- **bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y)**,该方法用于绘制三次贝塞尔曲线,**(cp1x,cp1y)为一个控制点,(cp2x,cp2y)为第二个控制点,(x,y)为结束点,起始点为moveTo()刚抬起指定的点**



### 4.5 变换

- **translate(x,y)**,该方法用于移动`canvas`的坐标原点(默认是左上角),该方法接收两个参数,**x为水平偏移量,y为垂直偏移量**

  **注意:**在`canvas`中translate()方法造成的结果是累加的

- **rotate(angle)**,该方法只接收一个参数旋转的角度(angle),**该旋转方向为顺时针方向**,以**弧度**为单位的值,用于旋转整个`canvas`画布

  **注意:**旋转的中心点始终为`canvas`的原点,也就是说内部的所有图形都是围绕原点转圈,如果要改变它需要使用translate()方法,并且在`canvas`中translate()方法也是累加的

- **scale(x,y)**,该方法接收两个参数,x和y分别代表横轴和纵轴的缩放因子,这两个值都必须为正值,当值小于1.0代表缩小,大于1.0代表放大

  **注意:**scale()方法中的缩小和放大是相对于整个`canvas`画布的像素数目,放大总个数会减少,单个像素的实际物理尺寸变大,而缩小总个数会增多,单个像素的实际物理尺寸减小,对图像、位图进行缩小或放大,并且在`canvas`中scale()方法是累乘的

```js
let canvas=document.querySeletor("canvas");
let ctx=canvas.getContext("2d");
ctx.translate(50,50);
ctx.rotate(Math.PI*90/180);
ctx.scale(.5,.5);
```



### 4.6 图片操作

#### 4.6.1 插入图片

**drawImage(image,x,y,width,height)**方法用于在`canvas`中插入图片

**参数:**

- image为`image`对象或者`canvas`对象
- x,y是插入的图片在目标`canvas`里的起始坐标
- width和height是用来控制插入的图片在`canvas`中应该显示的大小

**注意:**在`canvas`中插入图片需要使用**image对象**,并且必须要等图片加载完成后才能进行操作,所以需要在**image对象**执行onload事件后再执行`canvas`插入图片的函数

```js
let canvas=document.querySeletor("canvas");
let ctx=canvas.getContext("2d");
let img=new Image();
img.src="1.jpg";
img.onload=function(){
    ctx.drawImage(this,100,100,img.width,img.height);
}
```



#### 4.6.2 设置背景

**createPattern(image,repetition)**方法用于在`canvas`中设置背景

**参数:**

- image为`image`对象等图像源
- repetition为背景在`canvas`中平铺方式,值为"repeat","repeat-x","repeat-y"和"no-repeat"

**注意:**该方法会返回一个对象,一般情况下我们都会将该方法返回的对象作为fillStyle的值进行填充

```js
let canvas=document.querySeletor("canvas");
let ctx=canvas.getContext("2d");
let img=new Image();
img.src="1.jpg";
img.onload=function(){
    let pattern=ctx.createPattern(img,"no-repeat");
    ctx.fillStyle=pattern;
    ctx.fillRect(0,0,300,300);//在绘制的矩形中进行背景的填充
}
```



### 4.7 设置渐变

- **线性渐变**

  + **createLinearGradient(x1,y1,x2,y2)**方法设置线性渐变的终点与起点,(x1,y1)表示起点,(x2,y2)表示终点

    **注意:**该方法会返回一个对象gradient,通过该对象能对线性渐变的颜色等进行具体的设置,同时该对象一般赋值给fillStyle属性

  + **gradient.addColorStop(position,color)**

    **参数:**

    + **positon,**该参数的值为一个0.0到1.0之间的数值,表示渐变中颜色所在的相对位置,表示从哪个比例开始进行颜色的渐变
    + **color**,该参数必须是一个有效的CSS颜色值,如十六进制数或rgb等

    **注意:**该方法该可以调用多次,加入的颜色会存入栈中,根据所在位置的不同在调用时会显示不用的渐变颜色

  ```js
  let canvas=document.querySeletor("canvas");
  let ctx=canvas.getContext("2d");
  let gradient=ctx.createLinearGradient(0,0,300,300);
  gradient.addColorStop(0,"red");
  gradient.addColorStop(0.5,"red");
  gradient.addColorStop(1,"green");
  ctx.fillStyle=gradient;
  ctx.fillRect(0,0,300,300);
  ```

- **径向渐变**

  - **createRadialGradient(x1,y1,r2,x2,y2,r2)**方式设置径向渐变的终点圆和起点圆,**前三个参数表示以(x1,y1)为圆点,r1为半径的圆。后三个参数表示以(x2,y2)为圆点,r2为半径的圆**

  - **gradient.addColorStop(position,color)**

    **参数:**

    - **positon,**该参数的值为一个0.0到1.0之间的数值,表示渐变中颜色所在的相对位置,表示从哪个比例开始进行颜色的渐变
    - **color**,该参数必须是一个有效的CSS颜色值,如十六进制数或rgb等

    **注意:**该方法该可以调用多次,加入的颜色会存入栈中,根据所在位置的不同在调用时会显示不用的渐变颜色

  ```js
  let canvas=document.querySeletor("canvas");
  let ctx=canvas.getContext("2d");
  let gradient=ctx.createRadialGradient(150,150,50,150,150,100);
  //在两个圆之间的区域会形成渐变
  gradient.addColorStop(0,"red");
  gradient.addColorStop(0.5,"red");
  gradient.addColorStop(1,"green");
  ctx.fillStyle=gradient;
  ctx.fillRect(0,0,300,300);
  ```




### 4.8 绘制文本

**在`canvas`中可以使用两种方法来绘制文本**

- **fillText(text,x,y)**,text为要写入的文本,(x,y)位置表示填充指定的文本
- **strokeText(text,x,y)**,text为要写入的文本,(x,y)位置表示填充指定的文本



#### 4.8.1 文本样式

- **font = "font-size font-family"** ,当前我们用来绘制文本的样式,该属性和CSS中的font属性的语法相同,默认的字体为`font="10px sans-serif"`

  **注意:**当要使用font属性时,必须要大小和字体同时存在,缺一不可

  ```js
  let canvas = document.querySelector("canvas");
  let ctx = canvas.getContext("2d");
  ctx.strokeText("canvas", 100, 100);
  ctx.fillText("canvas", 100, 100);
  ```

- **textAlign="value"**,文本对齐属性,可选的值为**right,left,center,start,end**,其中**start和end分别对应left和right**,默认值是**left**

  **注意:**这里的文本对齐和CSS中的文本对齐不一样,这里的left和right都是相对于fillText()和strokeText()指定的位置时的偏差,默认是最坐标的文字靠近指定点的x,而center与right这些指定就是将写在一起的文字的中部和最右边靠近指定点

- **textBaseline="value"**,该属性用于描述文本时文本基线的属性

  **值:**

  + **alphabetic**,默认值,文本基线是普通的字母基线
  + **top**文本基线是 em 方框的顶端,em方框是包裹整个默认文字基线的方框
  + **hanging**,文本基线是悬挂基线
  + **middle**,文本基线是 em 方框的正中
  +  **ideographic**,文本基线是表意基线
  + **bottom**,文本基线是 em 方框的底端

- **measureText(text)**,该方法返回一个TextMetrics对象,包含了关于文本尺寸的信息(例如文本的宽度),参数是需要测试文本信息的字符串

```js
let canvas = document.querySelector("canvas");
let ctx = canvas.getContext("2d");

ctx.fillStyle="red";//文字的颜色也是通过该样式来改变的
ctx.strokeStyle="blue";
ctx.font="50px sans-serif";
ctx.textAlign="center";
ctx.textBaseline="middle";
//注意在绘制之前要先设置样式
ctx.strokeText("canvas", 100, 100);
ctx.fillText("canvas", 100, 100);
console.log(ctx.measureText("canvas").width);
```



#### 4.8.2 文本水平垂直居中

```js
let canvas = document.querySelector("canvas");
let ctx = canvas.getContext("2d");
ctx.font="50px sans-serif";
ctx.textBaseline="middle";
let width=ctx.measureText("canvas").width;
ctx.fillText("canvas",(canvas.width-width)/2,(canvas.height-50)/2);
```



#### 4.8.3 文本阴影

- **shadowOffsetX="value"**,该属性用来设置阴影在X轴的延伸距离,默认是0

- **shadowOffsetY="value"**,该属性用来设置阴影在Y轴的延伸距离,默认是0

- **shadowBlur="value"**,该属性用于设置阴影的模糊度,其数值并不跟像素挂钩,也不受变换矩阵的影响,默认为0

- **shadowColor="color"**,用于设置阴影颜色,该颜色的值为标准的CSS颜色值

  **默认是全透明的黑色:`rgba(0,0,0,0)`**

```js
let canvas = document.querySelector("canvas");
let ctx = canvas.getContext("2d");
ctx.shadowOffsetX = "10";
ctx.shadowOffsetXY = "-10";
ctx.shadowColor = "red";
ctx.shadowBlur = "5";
ctx.fillText("canvas",100,100);
```

**注意:**因为默认的颜色是透明的黑色,所以如果想要有阴影必须要设置阴影的颜色



### 4.9 像素操作

**在`canvas`中,我们可以直接通过ImageData对象操纵像素数据,直接读取或将数据数组写入该对象中**

- **getImageData(sx,sy,sw,sh)**,该方法用做获取一个ImageData对象,代表了画布区域的对象数据

  **参数:**

  + **sw:**将要被提取的图像数据矩阵区域的左上角x坐标
  + **sy:**将要被提取的图像数据矩阵区域的左上角y坐标
  + **sw**:将要被提取的图像数据矩形区域的宽度
  + **sh:**将要被提取的图像数据矩形区域的高度

  ```js
  let canvas = document.querySelector("canvas");
  let ctx = canvas.getContext("2d");
  ctx.fillStyle = "rgba(255,192,203,1)";
  ctx.fillRect(0, 0, 100, 100);
  let imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  ```

- **ImageData对象**,该对象中存储着`canvas`对象真实的像素数据,包含以下几个属性:

  - **width:**图片宽度,单位为px

  - **height:**图片高度,单位为px

  - **data:**Uint8ClampeArray类型的一维数组,包含着rgba格式的整形数据,范围在0到255之间(包括255)

    **注:**该属性保存了每一个像素所占的rgba值,数组中的每四个数据就是一个像素的完整rgba值,并且值都是0到255,由黑色到白色,**注意这里的a(透明度)的值也是0到255,和CSS中的透明度的算法不同**

- **putImageData(ImageData,dx,dy)**,该方法用于对场景进行像素数据的写入

  **参数:**

  + **ImageData:**为一个ImageData对象,该对象就是要写入的像素数据
  + **dx:**要绘制图形的x坐标
  + **dy:**要绘制图形的y坐标

  ```js
  let canvas = document.querySelector("canvas");
  let ctx = canvas.getContext("2d");
  ctx.fillStyle = "rgba(255,192,203,1)";
  ctx.fillRect(0, 0, 100, 100);
  let imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  ctx.putImageData(imgData,100,100);
  ```

- **createImageData(width,height)**,该方法用于直接创建一个ImageData对象

  **参数:**

  + **width:**新对象的宽度
  + **height:**新对象的高度

  **注意:**默认情况下创建出来的ImageData对象是完全透明的,**颜色为rgba(0,0,0,0)**

  ```js
  let canvas = document.querySelector("canvas");
  let ctx = canvas.getContext("2d");
  let imgData=ctx.createImageData(100,100);
  for(let i=0;i<imgData.data.lenght;i++){
      imgData.data[4*i+3]=255;//该索引可以取得每一个像素的透明度,同时设置为255不透明
  }
  ctx.putImageData(imgData,100,100);
  ```




**获取单像素颜色**

```js
//通过传入imgdata和需要获取的像素点得到颜色
function getPxInfo(imgdata,x,y){
    let color=[];
    let data=imgdata.data;
    let w=imgdata.width;
    let h=imgdata.height;
    //x代表多少行,y代表多少列
    color[0]=data[(y*w+x)*4];
    color[1]=data[(y*w+x)*4+1];
    color[2]=data[(y*w+x)*4+2];
    color[3]=data[(y*w+x)*4+3];
    return color;
}
```

```js
let canvas = document.querySelector("canvas");
let ctx = canvas.getContext("2d");
ctx.fillStyle = "rgba(255,192,203,1)";
ctx.fillRect(0, 0, 100, 100);
let imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
let color=getPxInfo(imgdata,20,20);
```



**设置单像素颜色**

```js
function setPxInfo(imgdata,x,y,color){
    //color为传入的一个代表0到255的四个成员的数组
    let data=imgdata.data;
    let w=imgdata.width;
    let h=imgdata.height;
	data[(y*w+x)*4]=color[0];
    data[(y*w+x)*4+1]=color[1];
    data[(y*w+x)*4+2]=color[2];
    data[(y*w+x)*4+3]=color[3];
}
```

```js
let canvas = document.querySelector("canvas");
let ctx = canvas.getContext("2d");
ctx.fillStyle = "rgba(255,192,203,1)";
ctx.fillRect(0, 0, 100, 100);
let imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
for(let i=0;i<imgData.width;i++){
    setPxInfo(imgData,i,50,[0,0,0,255]);//通过循环可以设置一行的像素
}
ctx.putImageData(imgData, 0, 0);//要想让画布重新渲染,必须要使用该函数将已经改变了的imgData传入
```



### 4.10 合成

#### 4.10.1 全局透明度

**globalAlpha="value"**,该属性设置或返回绘图的当前`alpha`或透明值,这个属性影响到`canvas`里所有图形的透明度,**有效的值范围是0.0(完全透明)到1.0(完全不透明)**,默认值为1.0



#### 4.10.2 覆盖合成

**globalCompositeOperation="value"**,设置或返回新图像如何绘制到已有的图像上

**可用的值:**

+ **source-over**,**在目标图像上显示源图像**,新的图像层级比较高,该值为默认值
+ **source-atop**,在目标图像顶部显示源图像,源图像位于目标图像之外的部分是不可见的,也就是**砍掉溢出的源图像部分**
+ **source-in**,在目标图像中显示源图像,只有目标图像内的源图像部分会显示,目标图像是透明的,也就是**只留下源图像与目标图像重复的部分(源图像的那部分)**
+ **source-out**,在目标图像之外显示源图像。只会显示目标图像之外源图像部分,目标图像是透明的。也就是**只留下源图像超出目标的部分**
+ **destination-over**,在源图像上方显示目标图像,目标图像的层级较高
+ **destination-atop**,在源图像顶部显示目标图像。源图像之外的目标图像部分不会被显示。也就是**砍掉溢出的目标图像部分**
+ **destination-in**,在源图像中显示目标图像。只有源图像内的目标图像部分会被显示，源图像是透明的。也就是**只留下源图像与目标图像重复的部分(目标图像的那部分)**
+ **destination-out**,在源图像外显示目标图像。只有源图像外的目标图像部分会被显示，源图像是透明的。也就是**只留下超出源图像的部分**
+ **lighter**,显示源图像和目标图像
+ **copy** ,显示源图像。忽略目标图像
+ **xor**,使用异或操作对源图像与目标图像进行组合

**注意:**

- **source:**新的图像(源)
- **destination:**已经绘制过的图像(目标)
- **globalCompositeOperation属性应该在第一个图像绘制后第二个图像回之前使用**



### 4.11 导出与事件操作

- **将画布导出为图片**

  火狐、谷歌浏览器右键菜单可直接导出为图片,但是如果为手机端可以使用内置的API方法导出

  + **canvas.toBlob()**,把canvas图片数据转换成 blob对象
  + **canvas.toDataURL(),**默认导出data:png,为base64编码的二进制URL
  + **canvas.toDataURL('image/jpeg'),**导出data:jpg,为base64编码的二进制URL

  **注意:**这些API由画布本身调用

- **事件操作**

  **ctx.isPointInPath(x,y)**,判断在当前路径是否包含有检测点,就是通过检测clientX和clientY来进行判断是否触发到了画布上的路径,x代表检测点的x坐标,y代表检测点的y坐标,两者都是相对于画布本身来说的

  **注意:**此方法只作用于最新画出的`canvas`路径图像

  ```js
  let canvas = document.querySelector("canvas");
  let ctx = canvas.getContext("2d");
  ctx.arc(100,100,50,0,360*Math.PI/180);
  ctx.fill()
  ctx.beginPath();//如果没有这个那么下面的代码可以在两个圆中都实现效果
  ctx.arc(200,200,50,0,360*Math.PI/180);
  ctx.fill();
  canvas.onclick=function(e){
      e=e||window.event;
      let x=e.clientX-canvas.offsetLeft;
      let y=e.clientY-canvas.offsetTop;
      if(ctx.isPointInPath(x,y)){
          alert(123);//只有第二个圆才会触发,因为是新画的
      }
  }
  ```

  

## 5.SVG

### 5.1 SVG与Canvas的区别

- **SVG**

  SVG 是一种使用 XML 描述 2D 图形的语言,基于XML,这意味着SVG内部的DOM中的每个元素都是可用的,可以为某个元素附加 JavaScript 事件处理器,在SVG中,每个被绘制的图形均被视为对象。**如果 SVG 对象的属性发生变化,浏览器能够自动重现该图形**

  **svg的特征**

  - 不依赖分辨率
  - 支持事件处理器
  - 最适合带有大型渲染区域的应用程序(比如谷歌地图)
  - 复杂度高会减慢渲染速度（任何过度使用DOM的应用都不快）
  - 不适合游戏应用

- **Canvas**

  Canvas通过 JavaScript 来绘制 2D 图形,C**anvas是逐像素进行渲染的**,在canvas 中,一旦图形被绘制完成,它就不会继续得到浏览器的关注。**如果其位置发生变化,那么整个场景也需要重新绘制,包括任何或许已被图形覆盖的对象**

  **canvas的特征**

  - 依赖分辨率
  - 不支持事件处理器
  - 弱的文本渲染能力
  - 能够以 .png 或 .jpg 格式保存结果图像
  - 最适合图像密集型的游戏，其中的许多对象会被频繁重绘



### 5.2 引入SVG

**svg是基于xml技术实现的,使用svg可以通过三种方式:**

**其中:**xmlns后面的字符串为命名空间,用来区分不同的代码功能

- **svg文件**

  ```xml
   <?xml version="1.1" encoding="utf-8"?>
  <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"          "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">    
  <svg xmlns="http://www.w3.org/2000/svg">
  	<circle cx="100" cy="100" r="40" fill="red"></circle>
  </svg> 
  ```

- **通过图片、背景、框架进行引入svg文件**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    	 html,
       body {
          height: 100%;
          overflow: hidden;
        }  
    </style>
  </head>
  <body>
      <!--图片引入-->
  	<img src="1.svg">
      <!--背景引入-->
      <div style="height:200px;width:200px;background:url('1.svg')"></div>
  	<!--框架引入-->
      <iframe src="1.svg"></iframe>
  </body>
  </html>
  ```

- **直接在html页面中引入svg**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        html,
        body {
            height: 100%;
            overflow: hidden;
        }  
    </style>
  </head>
  <body>
      <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      	<circle cx="100" cy="100" r="40" fill="red"></circle>
      </svg>
  </body>
  </html>
  ```



### 5.3 基本图形

**所有的svg标签(图层)会有层级关系,后续的标签如果与前面的标签重合就会覆盖点前面的标签**

#### 5.3.1 圆

**svg中的圆通过circle标签进行绘制**

- **圆心坐标(cx,cy):**写在标签上代表圆心离`<svg></svg>标签`左上角的距离坐标,单位为像素(不必填写)

- **圆心半径(r):**写在标签上代表从(cx,cy)坐标开始向四周扩展的圆的半径

- **fill:**通过填充方式画圆,填充的样式为圆的颜色,默认的填充颜色为黑色,不论是否填写了fill属性

- **stroke:**通过画线的方式画圆,画线方式为圆的边框颜色

- **stroke-width:**设置画线方式的线条宽度,单位为像素(不必填写)

- **stroke-dasharray:**将实线改为虚线形式绘画,可以提供短划线与缺口之间的长度,通过设置该属性手动设置长度如果提供了奇数个值,则这个值的数列重复一次,从而变成偶数个值

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
      <style>
        html,
        body {
            height: 100%;
            overflow: hidden;
        }  
    </style>
    </head>
    <body>
      <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
        <circle cx="100" cy="100" r="40" fill="transparent" stroke="red" stroke-width="5" stroke-dasharray="5 5 10 6"></circle>
      </svg>
    </body>
  </html>
  ```

- **stroke-opacity:**设置线条的透明度

- **fill-opacity:**设置填充样式的透明度

**注:**如果只想要通过画线的方式画圆,需要将fill设置为transparent(透明)或者是none(没有填充色)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
          height: 100%;
          overflow: hidden;
      }  
  </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <circle cx="100" cy="100" r="40" fill="transparent" stroke="red" stroke-width="5"></circle>
    </svg>
  </body>
</html>

```

**通过style属性设置**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
          height: 100%;
          overflow: hidden;
      }  
  </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <circle cx="100" cy="100" r="40" style="fill:transparent;stroke:red;stroke-width:5"></circle>
    </svg>
  </body>
</html>
<!--
	style中只能写样式属性,不对写cx,cy这样的定位属性和r这样的大小属性,也就是能对所有图形的公共属性设置
-->
```



#### 5.3.2 椭圆

**svg中椭圆通过ellipse标签进行绘制**

- **椭圆中心坐标(cx,cy)**
- **rx:**定义椭圆的水平半径
- **ry:**定义椭圆的垂直半径 

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <ellipse cx="100" cy="100" rx="100" ry="50" style="fill:none;stroke:red;stroke-width:5"></ellipse>
    </svg>
  </body>
</html>
```



#### 5.3.3 矩形

**svg中的矩形通过rect标签进行绘制**

- **width:**矩形的宽
- **height:**矩形的高
- **(x,y):**矩形相对于`<svg></svg>标签`的坐标
- **(rx,ry):**设置矩形圆角,rx和ry分别为在四个角画离x和y相应距离的圆或椭圆,然后截取矩形超出的角。默认为0,如果只写rx或者只写ry那么另一个也会默认也为那个值,会用一个圆去进行切割矩形

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
     <rect width="200" height="200" x="100" y="100" fill="red" fill-opacity="0.5" rx="30" ry="20"></rect>
    </svg>
  </body>
</html>
```



#### 5.3.4 线条

**svg中的线条通过line标签进行绘制**

- **(x1,y1):**设置线条的起点
- **(x2,y2):**设置线条的终点

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <line x1="50" y1="50" x2="200" y2="200" stroke="black" stroke-width="5" stroke-  opacity="0.2"></line>
    </svg>
  </body>
</html>
```



#### 5.3.5 折线

**svg中的折线用polyline标签进行绘制**

- **points:**设置折线个各个点坐标,通过`points="x1 y1 x2 y2 ..."`或`points="x1,y1,x2,y2...."`进行设置

  **注:**这两种方式也可以混合使用

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <polyline points="50 50 200 300 230 300 250 200 50 50" fill="none" stroke="black" stroke-width="5"></polyline><!--折线是不闭合的,如果要闭合需要最后一个点为其实点-->
      <polyline points="50 50,200 300,230 300,250 200"></polyline>
    </svg>
  </body>
</html>
```



#### 5.3.6 多边形

**svg中的折线用polygon标签进行绘制**

- **points:**设置折线个各个点坐标,通过`points="x1 y1 x2 y2 ..."`或`points="x1,y1,x2,y2...."`进行设置

   **注:**这两种方式也可以混合使用

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <polyline points="50 50 200 300 230 300 250 200" fill="none" stroke="black" stroke-width="5"></polyline><!--多边形是闭合的,这与折线有区别,不必连接起点-->
    </svg>
  </body>
</html>
```



#### 5.3.7 路径

**svg中可以用path标签进行对一整个路径进行绘制**

- **d:**该属性用与设置路径,需要与路径命令进行配合,命令的语法`d="M0 0L100 100"`,命令后紧跟着路径坐标,路径坐标的x和y坐标用空格隔开

  **路径命令:**

  + **M命令:**设置路径的起始坐标

  + **L命令:**设置路径的结束坐标

  + **Z命令:**闭合路径,该命令放在前一个坐标的最后,用作闭合前面的路径

  + **H命令:**绘制水平线,后面只跟着一个x坐标,对应的点会与上一个点的y坐标相同,该坐标的x也是代表着离`<svg></svg>标签`左上角的距离

  + **V命令:**绘制垂直线,后面只跟着一个y坐标,对应的点会与上一个点的x坐标相同,该坐标的y也是代表着离`<svg></svg>标签`左上角的距离

  + **C,S,Q,T命令:**绘制贝塞尔曲线 
    + **C命令:**绘制三次贝塞尔曲线(x1,y1,x2,y2,x,y)
      + 控制点一(x1,y)
      + 控制点二 (x2,y2)
      + 结束点(x,y)
    + **S命令:**绘制平滑贝塞尔曲线(自动对称一个控制点)(x1,y1,x,y)
      + 控制点(x1,,y1)
      + 结束点(x,y)
    + **Q命令:**绘制二次贝塞尔曲线(x1,y1,x,y)
      + 控制点(x1,y1)
      + 结束点(x,y)
    + **T命令:**绘制一次贝塞尔曲线(x,y)
      + 结束点(x,y) 

  **注:**这些命令有大小写两种方式.**大写为绝对坐标(具体的坐标位置)**,小**写为相对坐标(相对于上一个坐标点的具体长度)** 

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
      <style>
        html,
        body {
          height: 100%;
          overflow: hidden;
        }
      </style>
    </head>
    <body>
      <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
        <path d="M50 100L200 200L100 100ZM300 100H400" fill="none" stroke="black"></path>
      </svg>
    </body>
  </html>
  ```

  + **A命令:**通过`d="Mx1 y1Arx ry x-axis-rotation large-arc-flag sweep-flag x2 y2"`,表示绘制一个椭圆圆弧经过(x1,y1),(x2,y2)点

    **注:**A命令需要与M命名等一起配合使用确定圆弧的起始点

    **A命令标识:**

    + **rx:**椭圆横轴半径
    + **ry:**椭圆竖轴半径
    + **x-axis-rotation:**椭圆横轴相对于CanvasX轴的偏移角度
    + **large-arc-flag:**在前面三个参数确定的情况下,满足当前点(x1,y1)到指定点(x2,y2)位置条件的圆弧总是有四条,此值**取0表示绘制小弧度,取值1表示绘制大弧度**
    + **sweep-flag:**在前三个参数确定的情况下,满足当前点(x1,y1)到指定点(x2,y2)位置条件的圆弧总是有四条,去掉通过上面**large-arc-flag**标识后还有两个,**sweep-flag取值0表示绘制逆时针方向的圆弧,取值1表示绘制顺时针方向的圆弧**

    ```html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
        <style>
          html,
          body {
            height: 100%;
            overflow: hidden;
          }
        </style>
      </head>
      <body>
        <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
          <!--绘制一段弧形圆弧-->
          <path d="M150 150A100 100 0 0 1 250 150L225 175A50 50 0 0 0 175 175Z" fill="none" stroke="black" stroke-width="5"></path>
        </svg>
      </body>
    </html>
    ```



### 5.4 其余标签

#### 5.4.1 g标签

**g标签是一个容器(分组)标签,专门用来组合元素的,同时可以用来设置元素公共属性**,注意是公共属性**(换句话说就能能写在style中的属性**),不是只有某种图像自身才有的属性,自身才有的属性需要设置给自身的标签

**注:**可以使用所有元素的共用属性`transform="translate(0,0)"`用作移动整个组合元素

```html
<!--画一个靶心-->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <g stroke="red" stroke-width="5" fill="transparent" transform="translate(200,200)">
        <circle cx="100" cy="50" r="40" fill="transparent"></circle>
        <circle r="30"></circle>
        <circle r="20"></circle>
        <circle r="10"></circle>
      </g>
      <!--
	   通过transform可以移动整个p标签和内部的元素,cx,cy是能够单独控制对应的圆相对移动后的p标签的移动
	   如果内部元素与g标签中属性冲突,内部元素的属性会起作用 
	  -->
    </svg>
  </body>
</html>
```



#### 5.4.2 text标签

**text标签时svg中专门用来写入文本的标签**

- **(x,y):**文字标签相对于`<svg></svg>标签`的坐标,移动或文字的最左边靠这(x,y)坐标

- **font-size:**设置文字大小,单位为像素

- **text-anchor:**设置文本的对齐方式

  **值:**

  + **start:**文字开头对齐(x,y)坐标
  + **middle:**文字中部对齐(x,y)坐标
  + **end:**文字结尾对齐(x,y)坐标

**注意:**文字的颜色不能通过style中的color来设置,svg中的文字默认是fill属性来绘制的,可以通过改变fill属性的颜色来改变文字颜色,同时如果给文字加上stroke属性,会在文字周围再加上围绕的线条

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <text x="200" y="200" font-size="20" text-anchor="middle" fill="blue" stroke="red">SVG</text>
    </svg>
  </body>
</html>
```



#### 5.4.3 imgae标签

**image标签用于在svg中添加图片**

- **width:**图片的宽
- **height:**图片的高
- **(x,y):**图片相对于`<svg></svg>标签`的坐标
- **xlink:href:**该属性写入图片的链接

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <image x="200" y="200" width="100" height="100" xlink:href="1.jpg"></image>
    </svg>
  </body>
</html>
```



#### 5.4.4 defs与symbol标签

**这两个标签专门用来装需要引入的元素容器,内部的元素使用ID作为唯一标识的url,通过锚点方式引入内部元素url在`<defs></defs>或<symbol></symbol>`标签外部显示,通常与use标签等搭配使用**

**注:**

- **defs和symbol标签内部的原始元素不会在页面上显示出来,只有通过外部引用才能显示**
- 引用的元素有内部可以写对应的样式,在引用时如果引用的标签没有做任何的操作,那么就会使用内部的样式**,如果与引用标签内部规定的样式相冲突,原来样式就会无效**
- 在使用通用样式时(如fill),会有层级的限制,如果直接在内部标签上那个设置fill等样式,外部引用时设置fill将不会起作用,如果设置在内部的g标签中,外部的样式就会起作用

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
      <defs>
        <circle id="circle" cx="100" cy="100" r="50" fill="blue"></circle>
      </defs>
      <use xlink:href="#circle" x="120" y="150" fill="red"></use>
    <!--因为use写了x和y,所以内部圆的cx和cy将无效,引入use中fill的层级没有内部圆的高,所以圆为蓝色-->
    </svg>
  </body>
</html>

```



#### 5.4.5 filter标签

**svg滤镜只能定义在`<defs>`元素中,使用`<filter>`标签用来定义svg滤镜,`<filter>`标签使用必需的ID属性来定义向图形应用哪个滤镜**

**注:**其余图形需要使用该滤镜可以通过该属性上内置的filter属性进行设置,设置方式`filter="url(#引用ID名)"`

##### 5.4.5.1 模糊效果

**通过`<feGaussianBlur>`元素定义模糊效果**

- **in="SourceGraphic":**定义了由整个图像创建效果为SourceGraphic
- **stdDeviation:**定义模糊量

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%">
       <defs>
        <filter id="f1" x="0" y="0">
          <feGaussianBlur in="SourceGraphic" stdDeviation="15" />
        </filter>
      </defs>
      <rect width="90" height="90" stroke="green" stroke-width="3"
      fill="yellow" filter="url(#f1)"></rect>
    </svg>
  </body>
</html>
```



##### 5.4.5.2 阴影

**通过`<feOffset>`实现滤镜移动,通过`<feGaussianBlur>`实现阴影模糊,通过`<feColorMatrix>`为阴影周围上色**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg">
      <defs>
        <filter id="f1" x="0" y="0" width="200%" height="200%">
          <feOffset result="offOut" in="SourceGraphic" dx="20" dy="20" />
          <feColorMatrix result="matrixOut" in="offOut" type="matrix"
          values="0.2 0 0 0 0 0 0.2 0 0 0 0 0 0.2 0 0 0 0 0 1 0" />
          <feGaussianBlur result="blurOut" in="matrixOut" stdDeviation="10" />
          <feBlend in="SourceGraphic" in2="blurOut" mode="normal" />
        </filter>
      </defs>
      <rect width="90" height="90" stroke="green" stroke-width="3"
      fill="yellow" filter="url(#f1)" />
	</svg>
  </body>
</html>
```



#### 5.4.6 use标签

**该标签可以通过使用url引用一个`<g>或<svg>`或其他具有一个唯一的ID属性和重复的图形元素,复制的是原始的元素,因此文件中的原始存在只是一个参考,并且原始影响到所有副本的任何改变**

- **x:**克隆元素的左上角的x轴
- **y:**克隆元素的左上角的y轴
- **width:**克隆元素的宽度
- **height:**克隆元素的高度
- **xlink:href:**通过url的方式引用克隆元素,引入方式`xlink:href="#引用ID名"`



### 5.5 渐变

**SVG渐变主要有两种类型**

- Linear
- Radial

#### 5.5.1 stop标签

**`<stop>`标签用来设置渐变停止的位置**

- **offset:**偏移的停止量,参考值为0%到100%或0.0到1.0
- **stop-color:**这个stop内的颜色
- **stop-opacity:**这个stop类的不透明度,参考值为0到1



#### 5.5.2 线性渐变

**`<linearGradient>`标签用于定义线性渐变,**线性渐变可以定义为水平,垂直或角渐变

- 当y1和y2相等,而x1和x2不同时,可创建水平渐变
- 当x1和x2相等,而y1和y2不同时,可创建垂直渐变
- 当x1和x2不同,且y1和y2不同时,可创建角形渐变

**注意:`<linearGradient>`标签必须嵌套在`<defs>`的内部**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg">
      <defs>
         <!--一个水平椭圆渐变-->
        <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" style="stop-color:rgb(255,255,0);stop-opacity:1" />
          <stop offset="50%" style="stop-color:rgb(0,255,0);stop-opacity:1" />
          <stop offset="100%" style="stop-color:rgb(255,0,0);stop-opacity:1" />
        </linearGradient>
      </defs>
      <ellipse cx="200" cy="70" rx="85" ry="55" fill="url(#grad1)" />
    </svg>
  </body>
</html>

```



#### 5.5.3 放射性渐变

**`<radialGradient>`元素用于定义放射性渐变**

**注意:`<radialGradient>`标签必须嵌套在`<defs>`的内部**

- **渐变中心点(cx,cy):**参考值为数字或%,默认是50％
- **r:**渐变的半径,参考值为数字或%,默认50％
- **fx:**渐变的焦点,参考值为数字或%,默认0％
- **fy:**渐变的焦点,参考值为数字或%,默认0％

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
        <defs>
            <radialGradient id="grad1" cx="50%" cy="50%" r="50%" fx="50%" fy="50%">
                <stop offset="0%" style="stop-color:rgb(255,255,255);
                                         stop-opacity:0" />
                <stop offset="100%" style="stop-color:rgb(0,0,255);stop-opacity:1" />
            </radialGradient>
        </defs>
        <ellipse cx="200" cy="70" rx="85" ry="55" fill="url(#grad1)" />
    </svg>
  </body>
</html>

```



### 5.6 使用JS创建SVG

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
      div {
        height: 100%;
        width: 100%;
      }
    </style>
  </head>
  <body>
    <div></div>
    <script>
      window.onload = () => {
        let parent = document.getElementsByTagName("div")[0];
        let svgNS = "http://www.w3.org/2000/svg";
        let SVG = document.createElementNS(svgNS, "svg"); //创建带有命名空间的标签
        SVG.setAttribute("xmlns", svgNS);
        SVG.setAttribute("width", "100%");
        SVG.setAttribute("height", "100%");
       	parent.appendChild(SVG);
        //通过这种方式创建了一个svg标签,但是很麻烦
      };
    </script>
  </body>
</html>

```

**功能函数**

```js
function createTag(tag,Attr){
    //传入一个Attr对象
    let svgNS="http://www.w3.org/2000/svg"";
    let oTag=document.createElementNS(svgNS,tag);
    for(let attr in Attr){
        let value=Attr[attr]+"";//可以传入数值转换为字符串
        oTag.setAttribute(attr,value);
    }
    return oTag;
}
```



### 5.7 SVG动画

#### 5.6.1 旋转

**通过`transform:rotate(angle x y)`属性可以实现svg内置的旋转效果(不要写在style里面)**

- **angle:**旋转角度
- **(x,y):**旋转中心,这个旋转中心可以是任何位置,一般都是图形的自身旋转中心点



#### 5.6.2 animate

**`<animate>`标签可以用来嵌入到其他标签元素中,使得被嵌入的标签有动画效果**

- **attributeName:**要变化的元素属性名称

  - 可以是元素直接暴露的属性,如x和y等svg图像的专有属性
  - 可以是CSS属性,如width,height等

- **from,to,by,values**

  - **from:**动画的起始值(如果与元素的默认值一样可省略此值)
  - **to:**动画的结束值
  - **by:**动画结束值(相对变化的值)
  - **values:**用分号分隔的一个或多个值,可以看成是动画的多个关键值点

- **begin:**动画开始时间,可为具体的时间值,也可以是其他条件触发

  oﬀset-value | syncbase-value | event-value | repeat-value | accessKey-value | media-markervalue | wallclock-sync-value | "indeﬁnite" 

- **dur:**动画过渡的时间 ,参考值为具体时间或indefinite(无限时间即无具体意义)
- **repeatCount, repeatDur**
  - **repeatCount:**表示动画执行次数,可以是合法数值或者”indefinite“
  - **repeatDur:**定义重复动画的总时间,可以是普通时间值或者indeﬁnite

**注意:**如果想要动画结束后停留在最后的位置,需要加上`fill="freeze"`属性

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" height="100%" width="100%">
      <rect width="100" height="100" x="100" y="100" fill="deeppink">
        <animate attributeName="x" begin="0s" by="400" dur="3s" fill="freeze" repeatCount="2"></animate><!--by="400"同to="500"-->
      </rect>
    </svg>
  </body>
</html>

```



#### 5.6.3 animateTransform 

**`<animateTransform>`标签专门用来对transform属性进行设置动画效果**,通过type属性选择需要变换的类型

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" height="100%" width="100%">
      <rect width="100" height="100" x="100" y="100" fill="deeppink">
        <animateTransform attributeName="transform" type="translate" begin="0s" to="500,500" dur="3s" fill="freeze" repeatCount="2"></animateTransform>
      </rect>
    </svg>
  </body>
</html>

```



**注:**

- 一个元素类可以写多个动画标签,但是如果相互冲突会执行最下面的动画(指的是都是控制相同的属性),如果是不同的属性会同时执行
- 可以在begin属性中设置一些限制条件来控制动画的运行

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" height="100%" width="100%">
      <rect width="100" height="100" x="100" y="100" fill="deeppink">
        <animateTransform
          id="f1"
          attributeName="transform"
          type="translate"
          begin="0s"
          to="500,500"
          dur="3s"
          fill="freeze"
          repeatCount="2"
        ></animateTransform>
          <!--下面的f1.end表示在f2运动执行完成后才执行该运动-->
        <animateTransform
          attributeName="transform"
          type="rotate"
          begin="f1.end"	
          to="360 100 100"
          dur="3"
          fill="freeze"
          repeatCount="2"
        ></animateTransform>
      </rect>
    </svg>
  </body>
</html>
```



## 6.Audio与Video

### 6.1 容器

大多数人认为视频文件就是`.avi .mp4`等,但事实上这些仅仅只是容器的格式,只决定了怎么将视频存储起来,而不关心储存的内容,类似于压缩文件格式`.zip .rar`等



**视频与音频容器**

- **视频容器包含了音频轨道、视频轨道、和其他的一些元数据,在视频播放的时候,音频轨道和视频滚到是绑定在一起的,元数据包含了视频的封面、标题、子标题、字幕等相关信息**
- **音频容器包含了音频轨道**



**主流的文件格式**

- **视频**
  - **MPEG-4**:通常以`.mp4`为扩展名
  - **Flash视频**:通常以`.fiv`为扩展名
  - **Ogg**:通常以`.Ogv`为扩展名
  - **WebM**:通常以`.webm`为扩展名
  - **音视频交错**:通常以`.avi`为扩展名
- **音频**
  - **MPEG-3**:通常以`.mp3`为扩展名
  - **Acc音频**:通常以`.acc`为扩展名
  - **Ogg音频**:通常以`.Ogg`为扩展名

**注:**可以使用转码器进行转码改变文件格式



### 6.2 兼容

通过与`<source></source>`相结合可以实现在不同浏览器下的兼容写法

```html
<video controls>
    <source src="test.mp4" type="video/mp4"></source>
    <source src="test.ogv" type="video/ogg"></source>
    <source src="test.webm" type="video/webm"></source>
	当前浏览器不支持视频播放,点击下载<a href="test.webm">下载视频</a>
</video>

<audio controls>
    <source src="test.mp3" type="audio/mpeg"></source>
    <source src="test.aac" type='audio/aac;codecs="aac"'></source>
    <source src="test.ogg" type='audio/ogg;codecs="vorbis"'></source>
</audio>

<!--
	type是用于浏览器进行辨认,通过type浏览器能够快速辨认自身是否支持该文件,不会浪费时间一个个下载,所以	  type一定要书写正确,后面的codecs是告诉浏览器需要使用的解码器
-->
```



### 6.3 音视频的attribute

- **视频**

  + **width:**视频显示区域的宽度,单位是CSS像素

  + **height:**视频展示区域的高度,单位是CSS像素

  + **poster:**一个海报帧的url,用于在用户播放或者跳帧之前展示

  + **src:**要嵌到页面的视频url

  + **controls:**显示或隐藏用户控制界面

  + **autoplay:**媒体是否循环播放

  + **muted:**是否静音

  + **preload:**该属性旨在告诉浏览器作者认为达到最佳的用户体验的方式是什么

    **值:**

    + **none:**提示作者认为用户不需要查看该视频,服务器也想要最小化访问流量,换句话说就是提示浏览器该视频不需要缓存
    + **metadata:**提示尽管作者认为用户不需要查看该视频,但是还是需要提取元数据(如长度等)
    + **auto**:用户需要这个视频优先加载,换句话说就是提示如果需要的话可以下载整个视频,即使用户不一定会使用它

    **注意:**如果值为空的字符串也是代指auto

- **音频**

  - **src:**要嵌到页面的视频url

  - **controls:**显示或隐藏用户控制界面

  - **autoplay:**媒体是否循环播放

  - **muted:**是否静音

  - **preload:**该属性旨在告诉浏览器作者认为达到最佳的用户体验的方式是什么

    **值:**

    - **none:**提示作者认为用户不需要查看该视频,服务器也想要最小化访问流量,换句话说就是提示浏览器该视频不需要缓存
    - **metadata:**提示尽管作者认为用户不需要查看该视频,但是还是需要提取元数据(如长度等)
    - **auto**:用户需要这个视频优先加载,换句话说就是提示如果需要的话可以下载整个视频,即使用户不一定会使用它

    **注意:**如果值为空的字符串也是代指auto



### 6.4 音视频的property

**音视频相关的JS属性**

- **duration:**媒体总时间(只读)

- **currentTime:**开始播放到现在所用的时间(可读写)

- **muted:**是否静音(可读写,相比于volume优先级更高)

- **volume:**`0.0到1.0`的音量相对值(可读写)

  **注意:**muted与volume不会同步,所以在写muted为true的时候需要把volume也为设置0,做到两者同步

- **paused:**媒体是否暂停(只读)

- **ended:**媒体是否播放完毕(只读)

- **error:**媒体发生错误的时候

- **currentSrc:**以字符串形式返回媒体地址(只读)

**视频多出的JS属性**

- **poster:**视频播放前的预览图片(可读写)
- **width:**设置视频标签的宽
- **height:**设置视频标签的高
- **videoWidth:**视频的实际宽(就是只有视频图像的那部分)
- **videoHeight:**视频的实际高

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  	<video src="test.mp4"></video>
  	<audio src="test.mp3"></audio>
  	<script>
    	let video=document.querySelector("video");
    	let audio=document.querySelector("aduio");
        //只写一下必要的属性
        video.muted=true;
        video.volume=0;
    	audio.muted=true;
        audio.volume=0;
        video.addEventListener("loadeddata",()=>{
            conosole.log(video.videoWidth);
            conosole.log(video.videoHeight);
        })
    </script>
</body>
</html>
```

**更多属性**

| 属性                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| audioTracks         | 返回表示可用音频轨道的 AudioTrackList 对象。                 |
| autoplay            | 设置或返回是否在加载完成后随即播放音频/视频。                |
| buffered            | 返回表示音频/视频已缓冲部分的 TimeRanges 对象。              |
| controller          | 返回表示音频/视频当前媒体控制器的 MediaController 对象。     |
| controls            | 设置或返回音频/视频是否显示控件（比如播放/暂停等）。         |
| crossOrigin         | 设置或返回音频/视频的 CORS 设置。                            |
| currentSrc          | 返回当前音频/视频的 URL。                                    |
| currentTime         | 设置或返回音频/视频中的当前播放位置（以秒计）。              |
| defaultMuted        | 设置或返回音频/视频默认是否静音。                            |
| defaultPlaybackRate | 设置或返回音频/视频的默认播放速度。                          |
| duration            | 返回当前音频/视频的长度（以秒计）。                          |
| ended               | 返回音频/视频的播放是否已结束。                              |
| error               | 返回表示音频/视频错误状态的 MediaError 对象。                |
| loop                | 设置或返回音频/视频是否应在结束时重新播放。                  |
| mediaGroup          | 设置或返回音频/视频所属的组合（用于连接多个音频/视频元素）。 |
| muted               | 设置或返回音频/视频是否静音。                                |
| networkState        | 返回音频/视频的当前网络状态。                                |
| paused              | 设置或返回音频/视频是否暂停。                                |
| playbackRate        | 设置或返回音频/视频播放的速度。                              |
| played              | 返回表示音频/视频已播放部分的 TimeRanges 对象。              |
| preload             | 设置或返回音频/视频是否应该在页面加载后进行加载。            |
| readyState          | 返回音频/视频当前的就绪状态。                                |
| seekable            | 返回表示音频/视频可寻址部分的 TimeRanges 对象。              |
| seeking             | 返回用户是否正在音频/视频中进行查找。                        |
| src                 | 设置或返回音频/视频元素的当前来源。                          |
| startDate           | 返回表示当前时间偏移的 Date 对象。                           |
| textTracks          | 返回表示可用文本轨道的 TextTrackList 对象。                  |
| videoTracks         | 返回表示可用视频轨道的 VideoTrackList 对象。                 |
| volume              | 设置或返回音频/视频的音量。                                  |

## 



### 6.5 音视频相关事件及函数

- **音视频方法**

    | 方法                                                         | 描述                                      |
    | ------------------------------------------------------------ | ----------------------------------------- |
    | addTextTrack() | 向音频/视频添加新的文本轨道。             |
    | canPlayType() | 检测浏览器是否能播放指定的音频/视频类型。 |
    | load()      | 重新加载音频/视频元素。                   |
    | play()     | 开始播放音频/视频。                       |
    | pause()     | 暂停当前播放的音频/视频。                 |

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
    </head>
    <body>
      	<video src="test.mp4"></video>
        <audio>
            <source src="test.mp3" type="audio/mpeg"></source>
            <source src="test.aac" type='audio/aac;codecs="aac"'></source>
            <source src="test.ogg" type='audio/ogg;codecs="vorbis"'></source>
        </audio>
      	<script>
        	let video=document.querySelector("video");
        	let audio=document.querySelector("audio");
        	let source=document.querySelectorAll("source");
    		//默认是关闭的
            video.play();//这样打开就会自动播放
            setInterval(()=>{
                video.pause();//5s后自动停止播放
            },5000);
            video.src="test2.mp4";//如果是直接写在音视频标签上的链接改变时不需要load()方法
            source[0].src="test2.mp3";
            audio.load();
            //如果是写在source标签上的链接在改变时需要使用该方法且是音视频标签使用才能改变链接
        </script>
    </body>
    </html>
    ```

- **音视频事件**

    | 事件                                                         | 描述                                               |
    | ------------------------------------------------------------ | -------------------------------------------------- |
    | abort     | 当音频/视频的加载已放弃时触发。                    |
    | canplay | 当浏览器可以开始播放音频/视频时触发。              |
    | canplaythrough | 当浏览器可在不因缓冲而停顿的情况下进行播放时触发。 |
    | durationchange | 当音频/视频的时长已更改时触发。                    |
    | emptied                                                      | 当目前的播放列表为空时触发。                       |
    | ended    | 当目前的播放列表已结束时触发。                     |
    | error     | 当在音频/视频加载期间发生错误时触发。              |
    | loadeddata | 当浏览器已加载音频/视频的当前帧时触发。            |
    | loadedmetadata | 当浏览器已加载音频/视频的元数据时触发。            |
    | loadstart | 当浏览器开始查找音频/视频时触发。                  |
    | pause   | 当音频/视频已暂停时触发。                          |
    | play       | 当音频/视频已开始或不再暂停时触发。                |
    | playing | 当音频/视频在因缓冲而暂停或停止后已就绪时触发。    |
    | progress | 当浏览器正在下载音频/视频时触发。                  |
    | ratechange | 当音频/视频的播放速度已更改时触发。                |
    | seeked   | 当用户已移动/跳跃到音频/视频中的新位置时触发。     |
    | seeking | 当用户开始移动/跳跃到音频/视频中的新位置时触发。   |
    | stalled | 当浏览器尝试获取媒体数据，但数据不可用时触发。     |
    | suspend | 当浏览器刻意不获取媒体数据时触发。                 |
    | timeupdate | 当目前的播放位置已更改时触发。                     |
    | volumechange | 当音量已更改时触发。                               |
    | waiting | 当视频由于需要缓冲下一帧而停止时触发。             |



### 6.6 视频与canvas结合

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
 	canvas {
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
        background-color: gray;
        border: 1px solid #000;
      }   
  </style>
</head>
<body>
    <!--放映机-->
  	<video src="test.mp4" width="0" height="0"></video>
    <!--荧幕-->
  	<canvas width="300" height="300"></canvas>
  	<script>
  		let video=document.getElementByTagNames("video")[0];
        let canvas = document.querySelector("canvas");
        let ctx=canvas.getContext("2d");
        video.addEventListener("loadeddata",()=>{
            setInterval(()=>{
                ctx.drawImage(video,0,0,canvas.width,canvas.height);
            }});
        })
        
    </script>   
</body>
</html>
```



## 7.表单验证

**表单验证时有一个`validity`对象,通过该对象可以查看验证是否通过,内部拥有八种验证,如果八种验证都通过则返回true(是验证通过,不是每一项代表的布尔值,每一项代表的布尔值不通过会变为true),一旦验证失败返回false,**验证事件通过`node.addEventListener("invalid",funciotn(){},false)`进行绑定,**其中node为用户写入数据的input标签**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
    请输入邮箱<br>
	<input type="email"/>
    <script>
    	let input=document.querySlector("input");
        input.addEventListener("invalid",function(){
        	console.log(this.validity);
        },false)
    </script>      
</body>
</html>
```



**八种验证**

- **valueMissing:**输入值为空时返回true

- **typeMismatch:**控件值与预期类型不匹配返回true

- **patternMismatch:**输入值不满足pattern正则返回true

- **tooLong:**超过maxLeng最大值限制返回true

- **rangeUnderflow:**验证的range最小值返回true

- **rangeOverflow:**验证的range最大值返回true

- **stepMismatch:**验证range的当前值是否符合min、max及step的规则返回true

- **customError:**不符合用户的自定义验证返回true

  **注:**自定义验证可以通过`node.setCustonValidity("输入格式有误")`传递给用户



**关闭表单验证:**通过formnovalidate属性可以关闭表单的验证



## 8.地理信息

### 8.1 位置信息来源

- IP地址
- GPS全球定位系统
- Wi-Fi无线网络
- 基站

#### 8.2 navigator.geolocation 

 **navigator.geolocation用于获取设备的当前位置** 

**navigator.geolocation的三个方法：**

- getCurrentPosition()
-  watchPosition()
- clearWatch()

####  8.2.1 getCurrentPosition

**使用方法:`navigator.geolocation.getCurrentPosition(successCallback,[errorCallback], [positionOptions])`;**

- **successCallback:**successCallback返回一个地理数据对象**position**作为参数,该对象有属性**timestamp**和**coords**。**timestamp表示该地理数据创建时间(时间戳),可以通过`new Date(position.timestamp)`得到,coords包括另外七个属性：**

  + coords.latitude:估计纬度
    + coords.longitude:估计经度
    + coords.altitude:估计高度
    + coords.accuracy:所提供的以米为单位的经度和纬度估计的精确度
    + coords.altitudeAccuracy:所提供的以米为单位的高度估计的精确度
    + coords.heading:宿主设备当前移动的角度方向，相对于正北方向顺时针计算
    + coords.speed:以米每秒为单位的设备的当前对地速度

- **errorCallback:**errorCallback返回一个错误数据对象**error**作为参数,该对象有属性**code**和**message**

  - **code :**表示失败原因,会返回失败4种编号.
    + **0:**不包括其他错误编号中的错误
    + **1:**用户拒绝浏览器获取位置信息
    + **2:**尝试获取用户信息,但失败了
    + **3:**设置了timeout值,获取位置超时了
  - message:错误的提示内容 

- **positionOptions:** positionOptions用来设置positionOptions来更精细的执行定位,positionOptions拥有三个属性:

  + **enableHighAccuracy:**值为true或者false(默认),是否返回更详细更准确的结构,默认为false不启用,选择true则启用,但是会导致较长的响应时间及增加功耗,这种情况更多的用在移动设备上
  + **timeout:**设备位置获取操作的超时时间设定(不包括获取用户权限时间),单位为毫秒,如果在设定的timeout时间内未能获取位置定位,则会执行errorCallback()返回code(3)。如果未设定timeout,那么timeout默认为无穷大,如果timeout为负数,则默认timeout为0

  + **maximumAge:**设定位置缓存时间,以毫秒为单位,如果不设置该值,该值默认为0,如果设定负数,则默认为0。该值为0时,位置定位时会重新获取一个新的位置对象。该值大于0时,即从上一次获取位置时开始,缓存位置对象,如果再次获取位置时间不超过maximumAge,则返回缓存中的位置,如果超出maximumAge,则重新获取一个新的位置

  ```js
  navigator.geolocation.getCurrentPosition(position=>{
      console.log(position.coords.latitude);
      console.log(position.coords.longitude);
      console.log(position.coords.altitude);
      console.log(position.coords.accuracy);
      console.log(position.coords.altitudeAccuracy);
      console.log(position.coords.heading);
      console.log(position.coords.speed);
      console.log(new Date(position.timestamp));
  },err=>{
  	console.log(err.code,err.message)
  },{
      enableHighAccuracy:false,
      timeout:5000,
      maximumAge:1000
  })
  ```

  

#### 8.2.2 watchPosition

该方法功能与getCurrentPosition()类似,参数也是相同的三个参数,不过该方法会在检查到位置发生改变后才会触发,所以只对移动设备有用,并且该函数会触发多次

**使用方法:`navigator.geolocation.watchPosition(successCallback, [errorCallback] , [positionOptions])**`



#### 8.2.3 clearWatch

**该方法用于配合watchPosition()使用,用于停止watchPosition()轮询**

watchPosition()需要定义一个watchID,如`let watchID = watchPosition(...)，`,通过clearWatch(watchID)来停止watchPosition()，使用方法类似setInterval



## 9.Worker与EventSource

### 9.1 Worker

**Woreker是H5中提出的可以让JS实现多线程的对象,通过向其中传入后台处理的JS地址就能让JS通过多线程处理代码,提高效率**

- **postMessage:**传输数据给后台
- **onmessage事件:**接收到前台和后台传输数据的事件
- **importScripts("其余JS文件"):**该函数能够引入一个JS文件并执行其中代码

```js
//index.js
const worker=new Worker("worker.js);
worker.postMessage(10000);
worker.onmessage=function(e){
    console.log(e.data);//从worker.js接收的数据
}
```

```js
//worker.js
importScripts("test.js");//打印test
self.onmessage=function(e){//self是固定写法,代表从父级接收到的信息
    console.log(e.data);//e.data就是接收到的信息
    self.postMessage(1000);//通过该方法将数据传输出去
}
```

```js
//test.js
console.log("test");
```



### 9.2 EventSource

**EventSource对象能够获取到sse服务器推送的消息, 同时也是使用其onmessage事件监听接收消息**

```js
const source=new EventSource("sse.php");//连接后端
source.onmessage=function(e){
    console.log(e.data);//打印从后台获取的信息
}
```



## 10.离线存储和跨文档请求

### 10.1 离线存储

- **服务器设置头信息 :** `AddType text/cache-manifest .manifest`
- **html标签加 :** `manifest=“xxxxx.manifest”`
- **写manifest文件 :  离线的清单列表**
  ​	先写 :  CACHE MANIFEST
  ​	FALLBACK :  第一个网络地址没获取到，就走第二个缓存的
  ​	NETWORK ：无论缓存中存在与否，均从网络获取	



### 10.2 跨文档请求

#### 10.2.1 同域跨文档

- **iframe内页：**
  + **父页面操作子页面**:`iframe.contentWindow`,iframe为需要操作的iframe标签
  + **子页面操作父页面:**`window.top(找到最顶级的父页面)/parent(第一父页面)`
- **新窗口页：**
  - **父页面操作子页面:**window.open
  - **子页面操作父页面:**window.opener



#### 10.2.2 不同域跨文档

**通过postMessage(“发送的数据”,”接收的域”)发送跨域信息,再通过对window绑定message事件进行监听**
**​message事件:**

- **e.origin:**发送数据来源的域
- **e.data:**发送的数据,通过判断发送的数据来执行相应的需求



## 11.移动端事件

### 11.1 基础事件

- **PC端事件**
  + **onclick:**鼠标点击触发
  + **onmousedown:**鼠标按下触发
  + **onmousemove:**鼠标移动触发
  + **onmouseup:**鼠标抬起触发
- **移动端触屏事件**
  - **ontouchstart:**手指按下触发
  - **ontouchmove:**手指移动触发
  - **ontouchend:**手指抬起触发



### 11.2 注意事项

- 通过on的方式添加touch事件在谷歌模拟器下无效

- 鼠标事件在移动端可以使用，但有300毫秒的延迟

  **点透问题:**点击了页面之后,浏览器会记录点击下去的坐标300毫秒之后,在该坐标找到现在的元素,执行该事件,所以如果期间进行了移动可能会无效

  **解决办法:**

  + 阻止默认事件,但在部分安卓机不支持
  + 不用a标签做页面跳转,用window.location.href做跳转,比如移动端淘宝
  + 在移动端不用鼠标事件

- **防止误触问题:**用JS做判断,手指移动就不跳转,没有移动,说明是点击,则跳转



### 11.3 获取手指信息

**所有的手指信息都是在触屏事件的event对象中得到**

- **e.touches:**当前屏幕上的手指列表
- **e.targetTouches:**当前元素上的手指列表
- **e.changedTouches:**触发当前事件的手指列表
- **e.changedTouches.length:**获取手指的个数 
- **e.changedTouches[0].pageX|pageY:**获取X和Y坐标 

 **注意:**在touchend事件的时候想要获取手指列表,只能用**e.changedTouches**,因为手指抬起也就没有touches和targetTouches了,只能用changedTouches



## 12.移动端适配

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!--
	name="viewport"视图窗口 
	content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no"
		width=device-width 设备宽度
		initial-scale=1.0  100px初始化的比例值
		maximum-scale=1.0  最大的比例值
		user-scalable=no   用户自定义缩放
	-->
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body> 
</body>
</html>
```

**适配方案**

- 固定高度，宽度百分比适配-布局非常均匀 适合百分比布局 
- 固定宽度，改变缩放比例适配-什么情况都可以
- rem适配
- PC端和移动端 分开开发
- 响应式开发(很麻烦)
- Bootstrap

**单位**

- em:根据元素自身的字体大小计算元素自身
- rem:根据html的字体大小计算其他元素尺寸



### 12.1 固定高度,宽度百分比

- 根据设置的大小去设置高度,单位可以用px、百分比、auto 
- 常用Flex布局
- 百分比宽度



### 12.2 固定宽度,改变缩放比例

- 设计图的宽度就是网页显示的宽度
- 改变视口的缩放比例
- 页面宽度固定死

```js
(function(){
    /*
    	var html=document.querySelector("html);
    	var width=html.getBoundingClientRect().width;
    */
    var width=window.screen.width;
    var fixedW=320;//设计稿的宽度
    var scale=width/fixedW;
    var meta=document.createElement("meta");
    var metaAttr={
        name:"viewport",
        content:`width=${fixedW},initial-scale=${scale},minimum-scale=${scale},
				 maximum-scale=${scale},user-scalable=no`
    }
    for(var key in metaAttr){
        meta[key]=metaAttr[key];
    }
    document.head.appendChild(meta);
})
```



### 12.3 rem适配

- 根据屏幕的分辨率动态设置html的文字大小,达到等比缩放的功能
- 保证html最终算出来的字体大小,不能小于12px
- 在不同的移动端显示不同的元素比例效果
- 把设计图的宽度分成多少分之一,根据实际情况

```js
(function(){
    var html=document.querySelector("html");
    var timer;
    function changeRem(){
        var width=html.getBoundingClientRect().with;//设备宽度
        if(width>540){
            width=540;
        }
        var scale=width/10;
        html.style.fontSize=scale+"px";   
    }
    function Time(){
        clearTimeout(timer);
        timer=setTimeout(changeRem,50);
    }
    //页面尺寸发生变化时.重新计算文字大小,执行changeRem函数
    window.addEventListener("resize",function(){
		Time();
    });
    //页面加载的时候,如果是调用缓存的话,这就再执行changeRem函数
    window.addEventLisetner("pageshow",function(e){
        if(e.persisted){//缓存
            Time();
        }
    })
    
})()
```



### 12.4 像素比适配

- 物理像素:手机屏幕分辨率

- 独立像素 指css像素的屏幕宽度

- 像素比 = 物理像素 / css 

- 获取设备的像素比:window.devicePixelRatio

```html
<meta name="viewport" content="initial-scale=1/dpr,maximum-scale=1/dpr,user-scalable=0,minimum-scale=1/dpr">
<!--
	dpr==1:
	物理像素==独立像素缩放比为1,直接布局,和上一种方法一样,1rem=设备宽度/10
	dpr==2:
	物理像素==2*独立像素,1rem=2*设备宽度/10,布局出来的宽度为设备宽度的两倍,所以在meta里缩小
-->
```

```css
/*文字适配*/
/*给html标签设置data-dpr属性 data-dpr=dpr*/
[data-dpr=1] body{
    font-size:12px;
}
[data-dpr=2] body{
    font-size:24px;
}
[data-dpr=3] body{
    font-size:36px;
}
```

```js
if(!dpr&&!scale){
    var isAndroid=window.navigator.appVersion.match(/anroid/gi);
    var isIPhone=window.navigator.appVersion.match(/iphone/gi);
    var devicePixelRatio=window.devicePixelRatio;
    if(isIPhone){
        //IOS下,对于2和3屏,用二倍的方案,其余用一倍
        if(devicePixelRatio>=3&&(!dpr||dpr>=3)){
            dpr=3;
        }else if(devicePixelRatio>=2&&(!dpr||dpr>=2)){
            dpr=2;
        }else{
            dpr=1;
        }
    }else{//其余设备
        dpr=1;
    }
    scale=1/dpr;//缩放比例
}
//动态生成meta
var metaEl=document.createElement("meta");
metaEl.setAttribute("name","viewport");
metaEl,setAttribute("content",`width=${fixedW},initial-scale=${scale},minimum-scale=${scale},maximum-scale=${scale},user-scalable=no`);
document.documentElement.firstElementChild.appendChild(metaEl);
```

**更多优化方案:**http://www.cnblogs.com/wujindong/p/5442275.html



### 12.5 横竖屏切换

- window.orientation判断选择角度

- **方向:**竖屏0、横屏90或-90
- 根据横屏幕的切换执行不同的事情
- **横竖屏切换监听事件:**orientationchange

```js
var width=(window.orientation==90||window.orientation==-90)?window.screen.height:window.screen.width;
var fixedW=660;
var meta=document.createElement("meta");
meta.setAttribute("name","viewport");
meta,setAttribute("content",`width=${fixedW},initial-scale=${scale},minimum-scale=${scale},maximum-scale=${scale},user-scalable=no`);
document.head.appendChild(meta);
window.addEventListener("orientationchange",function(){
    setRem();//横竖屏切换
    setTimeout(function(){
        window.location.reload();
    },600);
}));
window.addEventListener("resize",function(){
    setRem();//电脑端
})

setRem();
function setRem(){
    var html=document.querySelector("html");
    var width=html.getBoundingClientRect().width;
    html.style.fontSize=width/16+"px";
}
```

























































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































​	
























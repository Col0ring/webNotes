# 元数据处理



## **序言**

随着WebSocket、WebAudio、Ajax2等广泛应用，前端方面只要是处理大数据或者想提高数据处理性能，那一定是少不了 ArrayBuffer对象

同时在浏览器当中处理二进制数据的需求也在不断的增加，有时需要字节数组、8位、16位、32位整数型数组，所以对于JS中处理二进制迟早学习比较好

现今世界上几乎所有的计算机体系结构都是以字节（byte）为二进制数据的基本单位，所以二进制常常以字节数组的形式存在于程序当中

众所周知，JS是弱类型语言，并且JS设计之初似乎根本没想过要处理二进制的东西，对于字节的概念可以说是非常非常的模糊。如果要表达字节数组，那么似乎只能用一个普通数组来表示

那么H5的诞生及标准的发布，对技术的革新起了非常大的作用，深入地研究H5，会渐渐发现，很多时候都会对二进制数据进行处理，结合JS的ArrayBuffer和 Typed Array去获取及处理音频数据、XHR2上传或下载二进制内容等等

## arrayBuffer

`ArrayBuffer`表示二进制数据的原始缓冲区，该缓冲区用于存储各种类型化数组的数据。是最基础的原始数据容器，无法直接读取或写入， 需要通过其他方式来读写。 但可根据需要将其传递到类型化数组或 DataView 对象来解释原始缓冲区。

也就是说他是一个二进制数据的原始缓冲区，虽然 JavaScript 是弱类型语言，但是他本身是对数据的类型和大小都有限制的，我们`需要通过某种数据结构将缓冲区的内容有序的读取出来或写进去`

```css
例如：
Int8Array             8位有符号整数
Uint8Array            8位无符号整数
Uint8ClampedArray	  同上，像素操作
Int16Array            
Uint16Array
Int32Array
Uint32Array
Float32Array
Float64Array

以上是Typed Array类型化数组，类型化数组类型表示可编制索引和操纵的 ArrayBuffer 对象 的各种视图。 所有数组类型的长度均固定。而DataView视图对象对数据的操作更加细致

```

`ArrayBuffer`是一个固定长度的字节序列，通过`new ArrayBuffer(length)`来得到一片空间，内部实现与数组应该是不一样（内存分配和布局与Array不一样），`ArrayBuffer`是连续内存，因此对于高密度的访问（如音频数据）操作而言它比JS中的Array速度会快很多

`ArrayBuffer`是不能直接被访问的，因此需要借助Typed Array

Typed Array的背后是一个`ArrayBuffer`，也就是说，事实上的数据是存在`ArrayBuffer`里面的，而Typed Array只是给你提供了一个某种类型的读写接口

总结一句话:	Typed Array不直接存放任何数据，所有对Typed Array进行读写的操作，最终都会落实到它背后所持有的`ArrayBuffer`的身上。 `ArrayBuffer`才是真正的元始数据字节，而Typed Array只是一个操作窗口/操作视图（View）



## 获取二进制数据



常见的在网页里获取二进制数据有三种:

> -[x] XMLHttpRequest2
> -[x] File
> -[x] Blob

​	**通过XMLHttpRequest 2**

​	`XHR2` 的接口跟 `XHR` 几乎是一样的，当制定`xhr.responseType = 'arraybuffer'`以后，在成功获取数据的回调里就可以通过`xhr.response`来得到请求结果的`ArrayBuffer`了，然后就可以按照你的意愿来构造各种Typed Array进行访问。

`responseType`还可以有`blob`取值，可以用`xhr.response`获得[Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)对象。



​	**通过File和Blob**

​	在H5的表单File控件中，通过files对象来获取一个`FileList` ，此列表对象中的每一个都是files对象

​	也可以通过H5的拖拽drop事件捕获到files对象或blob对象

​	`File`继承了`Blob`，并提供了`name`  ， `lastModifiedDate`， `size` ，`type` 等基础元数据

​	`Blob` 与`ArrayBuffer`的区别是除了 原始字节 以外它还提供了mime type作为元数据

​	它们都可以借助 FileReader将`Blob`读取为更为实用的数据类型去使用

```css
readAsArrayBuffer()
readAsBinaryString()
readAsDataURL()
readAsText()
```



## 各种类型



上节课中，我们讲过在火狐下拖拽元素需要用setData函数设置键值对，同时用getData函数可以获取key的value值，那么IE定义了 text和url 这两种有效的数据类型，可以获取本网页上文本和图片路径

> e.dataTransfer.getData('url')	获取 url

> e.dataTransfer.getData('Text')		获取文本

H5对此也支持，并扩展了各种 `MIME` 类型，这两种类型会被映射为 'text/plain'和'text/uri-list'

所以可以兼容一下:

```js
兼容
var dataTransfer = e.dataTransfer

获取 URL
var url = dataTransfer.getData('url') || dataTransfer.getData('text/uri-list')

获取 文本
var url = dataTransfer.getData('Text') || dataTransfer.getData('text/plain')
```



```css
MIME 类型
MIME (Multipurpose Internet Mail Extensions) 是描述消息内容类型的因特网标准。
MIME 消息能包含文本、图像、音频、视频以及其他应用程序专用的数据。

不同的应用程序支持不同的 MIME 类型。
MIME 类型大约有191种类型，是的没错，191种^.^
```

```css
Base64 类型
Base64是网络上最常见的用于传输8Bit字节码的编码方式之一，Base64就是一种基于64个可打印字符来表示二进制数据的方法
Base64编码是从二进制到字符的过程中，可用于在HTTP环境下传递较长的标识信息
```



**FileReader文件对象**

```css
FileReader API 用于读取文件，即把文件内容读入内存，是一种异步文件读取机制，它的参数是 File 对象或 Blob 对象。对于不同类型的文件，FileReader 提供不同的方法读取文件。
```

```css
创建读取文件的对象
var reader = new FileReader()	

readAsDataURL(Blob|File) 
读取文件并将文件以数据URI的形式保存在result属性中，返回一个基于 Base64 编码的 data-uri 对象
reader.readAsDataURL(files[0])

readAsText(Blob|File, opt_encoding)
返回文本字符串。默认情况下，文本编码格式是 UTF-8，可以通过可选的格式参数，指定其他编码格式的文本
以纯文本形式读取文件，将读取到的文本保存在result属性中，第二个参数用于指定编码类型，可选的
reader.readAsText( files[0],encoding )

readAsBinaryString(Blob|File)	IE可能不支持
返回二进制字符串，该字符串每个字节包含一个 0 到 255 之间的整数。（已废弃）
读取文件并将一个字符保存在result属性中，字符串的每个字符表示一字节
reader.readAsBinaryString(files)

readAsArrayBuffer(Blob|File)	IE可能不支持
返回一个 ArrayBuffer 对象
读取文件并将一个包含文件内容的ArrayBuffer保存在result属性中
reader.readAsArrayBuffer(files)
```



## 具体用法



**FileList** 对象 

​		当用户通过 ***file 控件*** 选取文件后，这个控件的 this.files 属性值就是 FileList 对象。是个***类数组***，带上multiple 属性用户可选取多个文件，否则只能选择一个元素。

```js
<input type='file' multiple id='oInput'/>
<script>
    oInput.onchange = function() {
      	console.log(this.files)
    };
</script>
```

​		拖拽也可以获取  FileList 对象

```js
oInput.ondrop = function(e) {
    e.stopPropagation()
    e.preventDefault()
    var files = e.dataTransfer.files
    console.log(files)
  };
```

​		*在表单选择文件或者拖拽文件中，用户通过事件触发，只能被动地读取FileList 文件列表*



**Blob** 对象 

​		file对象的父类型是Blob对象， Blob对象代表了一段二进制数据，提供了一系列操作接口

​		生成 Blob 对象有两种方法：

> - [x] 使用 Blob 构造函数
> - [x] 另一种是对现有的 Blob 对象使用 slice 方法切出一部分

```js
Blob 构造函数，接受两个参数。
第一个参数是一个包含实际数据的数组
第二个参数是数据的类型
这两个参数都不是必需的

var arr = ["hello", "world"]
var Blob = new Blob(arr, { "type" : "text/xml" })
console.log(Blob)
```

```js
Blob 对象的 slice 方法，将二进制数据按照字节分块，返回一个新的 Blob 对象
var arr = ["hello", "world"]
var Blob = new Blob(arr, { "type" : "text/xml" })
var newBlob = Blob.slice(0, 5) ;  // 用在分片文件 ，后台接收把这些片段检验并组合一个文件
console.log(newBlob)

Blob 对象有两个只读属性：
size：二进制数据的大小，单位为字节。（文件上传时	可以在前端判断文件大小是否合适）
type：二进制数据的 MIME 类型，全部为小写，如果类型未知，则该值为空字符串。（文件上传时可以在前端判断文件类型是否合适）
gbk编码：	数字字母 一字节 1KB= 1024字节 一中文汉字是 2字节
UTF-8编码： 数字字母 一字节 1KB= 1024字节 一个中文汉字是 3字节

```



​	**Silce** *读取部分内容*

​		有时候我们读取一部分而不是全部内容，Filereader对象支持一个slice( )方法，在火狐中用mozSlice( )，在chrome中webkitSilde( )

```css
	> silce( start,end )
	>
	> start 开始索引，默认为0
	>
	> end	截取结束索引（不包括end）
	> contentType	新Blob的MIME类型，默认为空字符串
```

​		这个方法返回一个Blob实例，Blog是File类型的父类型，Blog类型有一个size属性和一个type属性，它也支持slice方法，以便进一步切割数据，通过FileReader也可以从Blob中读取数据

```js
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

​	blob类型读取FileReader的20B内容 

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

```

​	只读取文件的一部分可以节省时间，非常适合只关注数据中某个特定部分（如文件文件）的情况



​	**对象URL**	

​		对象URL也被称为 blob URL，指的是引用保存在File或Blob中数据的URL，使用对象URL的好处是没必要把内容读取到js中，而直接使用文件内容，能生成一个链接，例如 Img的src = URL

​		创建对象URL，用 window.URL.createObjectURL( blob )方法，并传入flle或Blob对象，对二进制数据生成一个 临时的URL，这个 URL 可以放置于任何通常可以放置 URL 的地方，比如 img 标签的 src 属性，出于一些特殊的需要，也可以使URL失效，调用 URL.revokeObjectURL( url ) 方法，使 URL 失效

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
```

​	createObject函数的返回值是一个字符串，指向一块***内存地址***。因为这个字符串是URL，所 以在DOM中也能使用，例如下用法 

```js
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
```

​	直接把对象URL放在img标签中，就省去了把数据先读取js中，另一方面img标签则会找到相应的内存地址，直接读取数据并将图像显示到页面中












































# Node.js

## 1.认识Node.js

- **Node.js既不是一门语言,也不是一个库或框架,而只是一个构建在Chrome的V8引擎上的一个JS的运行环境**,可以解析和执行JS代码,在以前只能在浏览器中解析执行JS代码,而现在可以完全脱离浏览器来运行

- **在Node.js的中的JS没用DOM和BOM,只有ECMAScript和额外为JS提供的一些服务器级别的API**
- **Node.js使用事件驱动,异步模型,具有轻量高效的特点**
- **Node.js使用npm进行包管理,绝大多数JS相关的包都存放在npm上,**这样做的目的是为了让开发人员更方便去下载使用

**node与浏览器的异同**

- 架构一样,都是基于事件驱动的异步架构
- 浏览器主要是通过事件驱动来进行服务器交互
- node主要是通过事件驱动来服务(I/O),进行用户间的数据交互



## 2.node的作用

### 2.1 执行JS脚本文件

**在命令行的父目录中使用node 文件名的方法解析和运行JS脚本文件**

**注意:**JS的文件名不能node.js,不然不会执行该脚本,而是会直接打开该文件



### 2.2 Buffer

**Buffer是一种数据结构,里面专门存放二进制数据,而为了方便将里面的数据显示,所以用了十六进制来进行显示**

```js
let str="123";
let buf=Buffer.from(str);//字符串转Buffer数据
console.log(buf);
console.log(buf.length);//表示字节长度,一个英文字节为1,中文字节为3

let str2=Buffer.toString(buf);//Buffer数据转字符串
console.log(str2);//123
```



## 3.模块系统

**在node中有三种模块:**

- 具体的核心模块
- 用户自己编写的文件模块
- 第三方模块

### 3.1 node如何实现模块化

node实现模块化的方法很简单，就是闭包，每个模块在运行时默认都是在最外层放了一个模块包装函数（Module Wrapper Function）中，node通过调用这个封装过的函数实现的模块化。

```js
(function (exports, require, module, __filename, __dirname) {

}
```

**每个模块中的代码都是包裹在一个立即执行函数中的,同时立即执行函数都有5个形参**

- exports:专门用来暴露模块数据的一个对象
- require:专门用来引入外部模块的函数
- module:内置的模块系统对象，具体介绍见下

- __filename:当前文件的绝对路径
- __dirname:当前文件夹的路径

**注意:**

- 在node文件操作中,相对路径并不可靠,因为node中的文件路径被设计为相对于执行node命令所处的路径,所以推荐使用path.join加上`__dirname`和`__filenmae`把路径转换为绝对路径

- 而模块中的路径表示与文件操作中的不一致,用相对路径就好了

  

### 3.2 全局对象global

与浏览器端的`window`对象一样，**`global`也是作为一个全局对象存在的**，该对象中包含了许多node应用内置的属性和方法（比如我们使用的`console`对象其实在node环境中是挂载到global对象上的）。

**可以见到，由node实现模块化的情况来看，所有的代码最后都集合在一起，而global是作为最外层和模块同作用域的顶级对象来存在的。**

通过在global对象挂载属性可以在其他模块进行访问,通过不声明直接命名的方式可以实现

```js
let a = 1
global.a = a // 这样在另外的模块中也可以同global.a使用，但是不推荐这样做，或污染全局环境
```

相比起全局对象，我们更希望通过挂在到exports对象上来实现

**注意：**

- 即使是使用`var`声明的变量也不会默认挂载到全局对象上，这点和浏览器端的`window`对象有区别，需要手动挂载	

    ```js
    console.log(global)
    var message = ''
    console.log(global.message) // undefined
    global.message = message // 挂载到全局对象上，所有模块都能访问
    ```

- 挂载到global对象上的属性同window对象上的一样，可以不加顶级对象直接使用挂载的属性（前提是有属性挂载过，否则会报错）

  ```js
  global.a = 1
  console.log(a) // 1
  ```

  

### 3.3 模块对象module

`module`是node中内置的模块系统对象，该对象内部拥有几个与模块相关的属性。

```js
console.log(module)
/*
  Module {
      id: '.',
      exports: {},
      parent: null,
      filename: 'E:\\demo\\学习项目\\node\\2.module-object.js',
      loaded: false,
      children: [],
      paths:
       [ 'E:\\demo\\学习项目\\node\\node_modules',
         'E:\\demo\\学习项目\\node_modules',
         'E:\\demo\\node_modules',
         'E:\\node_modules' 
      ] 
     }
*/
```

**属性：**

- **id:**当前模块的唯一标识，是以node执行的入口js文件和项目工程为参照的绝对路径，默认入口的id为`.`，其余文件的id为其绝对路径
- **filename:**当前文件的绝对路径，其实默认是和id的值是一样的
- **parent和children:**相对而言的属性，一个模块被另一个模块引用，另一个模块就是该模块的父模块，相对的，该模块是另一模块的子模块
- **loaded:**当前模块是否加载完成
- **path:**node解析策略构成的一个数组
- **exports:**与全局的exports共用一个内存，但是在导出模块的时候该属性下的内存起主导作用



### 3.4 核心模块

node为JS提供了很多服务器级别的API,这些API绝大多数都被包装到了一个具体的核心模块中了,例如文件操作的`fs`模块,`http`服务构建的http模块,`path`的路径模块,`os`操作系统信息模块,`url`专门对域名进行处理等`querystring`模块专门处理查询字符串,所有模块的使用都需要通过require()来获取

#### 3.4.1 path模块

[path模块api](http://nodejs.cn/api/path.html)

- **path.basename:**获取一个路径的文件名(默认包含扩展名)

- **path.dirname:**获取一个路径的目录部分

- **path.parse:**把一个路径转为对象

  **对象属性:**

  - root:根路径
  - dir:目录
  - base:包含后缀名的文件名
  - ext:后缀名
  - name:不包含后缀名的文件名

- **path.join:**当需要进行路径拼接的时候,推荐使用这个方法

- **path.isAbsolute:**判断一个路径是否是绝对路径

```js
const path = require('path')
const pathObj = path.parse(__filename)
console.log(pathObj)
/*
{ root: 'E:\\',
  dir: 'E:\\demo\\学习项目\\node',
  base: 'app.js',
  ext: '.js',
  name: 'app' }
*/
```



#### 3.4.2 os模块

[os模块api](http://nodejs.cn/api/os.html)

```js
const os = require('os')
const totalMemory = os.totalmem()
const freeMemory = os.freemem()
console.log(`Total Memory: ${totalMemory}`)
console.log(`Free Memory: ${freeMemory}`)
/*
Total Memory: 8468779008
Free Memory: 1898426368
*/
```



#### 3.4.3 fs模块

[fs模块api](http://nodejs.cn/api/fs.html)

**浏览器中的JS是没有操作文件的能力的,但是node中的JS具有文件操作的能力**

fs是file-system的简写,就是文件系统,在node中如果想进行文件操作,就要引入fs这个核心模块,这个核心模块中提供了所以文件相关的API操作

- **读取文件**

  ```js
  //例子:使用fs.readFile()读取文件
  var fs=require("fs");//使用require方法加载fs模块
  
  //读取文件,第一个参数是要读取的文件路径,第二个参数是options选项,包括度入文件的编码(这个参数可以不写),第三个参数是回调函数
  //读取文件时不管里面是什么文件,得到的都会是Buffer格式,除非在第二个参数写入了文件编码
  fs.readFile("a.txt","utf8",function(err,data){//如果不写第二个参数下面就要toString()
      /*
      	如果读取成功,data就是得到的数据,而err为null
      	如果读取失败,data为undefined,err为错误对象
      */
      //对是否读取成功进行判断
      if(err){
          console.log("读取失败");
      }else{
       	//注意:如果不进行toString()操作转换为我们可以认识的字符串,会输出16进制编码
      	console.log(data.toString());   
      }
  })
  
  //注意:fs.readFile()有局限性,只能读取体积较小的文件
  ```

- **写入文件**

  ```js
  //例子:通过fs.writeFile()写入文件,如果没有该文件则自动创建
  var fs=require("fs");
  /*
  	第一个参数:文件路径
  	第二个参数:文件内容,可以在文件里面写入任何的数据类型,所以数据都会被隐式转换为字符串
  	第三个参数:options选项,包括写入文件的方式(这个参数可以不写)
  	第四个参数:回调函数
  */
  //异步写入文件
  fs.writeFile("b.txt","hello world",function(err){
  	/*
  		如果文件写入成功,error是null
  		如果文件写入失败,error是错误对象
  	*/
      //判断是否写入成功
      if(err){
       	console.log(err);   
      }else{
       	console.log("写入成功");   
      }
  });
  
  //同步写入文件
  fs.writeFileSync("b.txt","hello world");
  
  //注:可通过先读再写入文件的方式实现文件的复制
  ```

- **流式读取写入文件**

  ```js
  var fs=require("fs");
  var rs=fs.createReadStream("a.txt");//流体式打开一个文件
  var ws=fs.createWriteStream("b.txt");//流体式写入一个文件
  
  rs.resum();//同过该方法可以让可读流中的数据流动,否则不会进行读取操作
  rs.on("data",(chunk)=>{//也可已通过绑定data事件让可读流运动
      ws.write(chunk);//chunk就是读取到的数据,通过可写流可以直接写入可读流读取的数据
  });//通过这样的方法复制文件是通过两根管子进行复制的
  /*
  rs.pipe(ws);//直接使用pipe函数可以只通过一个函数进行复制
  */
  rs.on("end",()=>{//全部读取完成后才会执行该方法
     console.log("读取完成"); 
  });
  ```

- **删除文件**

  ```js
  //使用fs.unlinkSync()删除文件,返回值为undefined
  var fs=require("fs");
  fs.unlinkSync("b.txt");
  ```

- **删除目录**

  ```JS
  var fs=require("fs");
  fs.rmdir("testDir",(err,data)=>{
      if(err){
  		console.log(err);
      }else{
          console.log(data);
      }
  })
  ```

- **新建目录**

  ```js
  var fs=require("fs");
  fs.mkdirSync("testDir");
  ```

- **查看目录**

  ```js
  var fs=require("fs");
  fs.readdir("testDir",(err,data)=>{
       if(err){
  		console.log(err);
      }else{
          console.log(data);
      }
  })
  ```

- **其他**

  - 限制指定文件字节长度
  - 判断指令路径下的文件或目录是否存在
  - 查看文件信息
  - 移动文件
  - 监听文件

  ```js
  var fs=require("fs");
  fs.truncateSync("b.txt",5);//会将文件中的字节进行限制,如果该文件里有超过5个字节,会出现乱码
  
  let result=fs.existsSync("b.txt");//判断文件是否存在,存在返回true
  
  let result2=fs.statSync("b.txt");
  console.log(result2);//查看文件信息,返回的是一个对象
  
  /*
  	rename(旧路径,新路径,回调函数)
  	1.把旧路径的文件移动到新路径
  	2.改名
  */
  fs.rename("../b.txt","b.txt",(err)=>{
      if(err){
  	console.log(err);
      }
  })
  
  fs.watchFile("b.txt",(cur,pre)=>{
  	console.log(cur,pre);//cur为现在文件的状态,pre为上次文件的状态
  })
  ```



#### 3.4.4 events模块

[event模块api](http://nodejs.cn/api/events.html)

大多数 Node.js 核心 API 构建于惯用的异步事件驱动架构，其中某些类型的对象（又称触发器，Emitter）会触发命名事件来调用函数（又称监听器，Listener）。

例如，[`net.Server`](http://nodejs.cn/s/gBYjux) 会在每次有新连接时触发事件，[`fs.ReadStream`](http://nodejs.cn/s/C3Eioq) 会在打开文件时触发事件，[stream](http://nodejs.cn/s/kUvpNm)会在数据可读时触发事件。

所有能触发事件的对象都是 `EventEmitter` 类的实例。 这些对象有一个 `eventEmitter.on()` 函数，用于将一个或多个函数绑定到命名事件上。 事件的命名通常是驼峰式的字符串，但也可以使用任何有效的 JavaScript 属性键。

当 `EventEmitter` 对象触发一个事件时，所有绑定在该事件上的函数都会被同步地调用。 被调用的监听器返回的任何值都将会被忽略并丢弃。

```js
// 注意，导入的对象其实是一个事件类，events模块的许多方法都是基于这个类实现
const EventEmitter = require('events')
const emitter = new EventEmitter()

// 注册一个监听器 on 与 addeventListener 是一样的
emitter.on('messageLogged', arg => {
  console.log('Listener called', arg)
})

emitter.addListener('messageLogged', arg => {
  console.log('Listener2 called', arg)
})

// 抛出事件，传参数更好的方法是通过传一个对象，而不是传多个形参
emitter.emit('messageLogged', { id: 1, url: 'http://' })
// 注意要先监听，不然没效果

/*
Listener called { id: 1, url: 'http://' }
Listener2 called { id: 1, url: 'http://' }
*/
```

**注意：**一般来说我们只会包装整个程序中有一个`EventEmitter`的实例，因为不同实例的监听与抛出事件是不会有效果的

```js
// logger.js
const EventEmitter = require('events')
const url = 'http://mylogger.io/log'

class Logger extends EventEmitter {
  log(message) {
    // send a http request
    console.log(message)

    // raise an event
    this.emit('messageLogged', { id: 1, url: 'http://' })
  }
}

module.exports = Logger
```

```js
// app.js

const Logger = require('./logger')
const logger = new Logger()

logger.on('messageLogged', arg => {
  console.log('Listener called', arg)
})

logger.log('message')
```



#### 3.4.5 http模块

[http模块api](http://nodejs.cn/api/http.html)

**使用node可以轻松得构建一个web服务器,在Node中专门提供了一个核心模块http,这个模块就是用来编写创建服务器的**

```js
const http = require('http')

// 返回值其实是一个EventEmitter，继承了前者所有的方法
const server = http.createServer((req, res) => {
  if (req.url === '/') {
    res.write('Hello World')
    res.end()
  } else if (req.url === '/api/courses') {
    res.write(JSON.stringify([1, 2, 3]))
    res.end()
  }
})

// 监听连接的句柄，如果有人进入了监听的端口就会有响应,当然，一般不这样写，一般是在http.createServer函数的回调函数中邪
// server.on('connection', socket => {
//   console.log('New connection...')
// })

server.listen(3000, () => {
  console.log('connect completely...')
})
```

```js
//加载http模块
var http=require("http");
//使用http.createServer()方法创建一个web服务器,该方法返回一个Server实例
var server=http.createServer();
/*
也可以直接var server=http.createServer((request,response)=>{
		
});
不用注册请求事件,默认为请求事件
*/

//客户端发送请求->服务端接收处理反馈请求
/*
注册request（与上面的connection事件是一个效果）请求事件,当客户端请求过来,就会自动触发服务器的request请求事件,然后执行回调函数
*/
server.on("request",function(request,response){
    /*
    	request请求事件的处理函数接收两个参数:
    	1.request请求对象,请求对象可以用来获取客户端的一些请求信息,如请求路径(url属性)等
    	2.response响应对象,响应对象可以给客户端发送信息
    	
    	一个请求对应一个响应,如果一个请求的过程中已经结束响应了,则不能重复发出响应,同时没有请求就没有		响应
    */
    console.log("接收到请求,请求的路径为"+request.url);
    /*
    	response对象有一个write方法可以接收到请求后给服务端发出响应数据
    	注意:响应内容只能是二进制数据或者字符串,如果要传入一个对象可以用JSON转换
    */
    	response.write("hello world");
    	response.write("你好世界");
    /*注意:
    	1.write方法可以使用多次,但最后一定要加上end方法表示响应完成,否则会一直响应
    	2.在服务端默认发送的数据其实是UTF-8编码的内容,但是浏览器不知道是UTF-8d的编码,会默认按照当前		  操作系统的默认编码去解析,中文操作系统的默认编码是gbk,所以需要告诉浏览器发送的是什么编码
    */	
    	response.setHeader("Content-Type","text/plain;charset=utf-8");
    	//在http协议中,Content-Type就是告知要发送的数据类型是什么,不同的响应内容对应的类型不一样
    	//plain为普通文本
    	response.end();
    /*
    	上面的写法几乎不用,可以直接用response.end("hello world你好事件");
    	并且这种写法每次请求的结果都一样,即使访问的路径不同
    */
    
});
//绑定端口号,启动服务器
server.listen(3000,function(){
   console.log("服务器已成功启用在3000端口,可以通过localhost:3000访问");           
});
```

```js
var http=require("http");
var server=http.createServer();

server.on("request",function(request,response){
    /*
    	根据不同的请求路径发送不同的响应结果
    	1.获取请求路径,request.url获取到的是端口号之后的那一部分路径,也就是说所有的url都是以\开头的
    	2.判断路径处理响应
    */
    var url=request.url;
    if(url==="/"){
        response.end("index page");
    }else if(url==="/login"){
        response.end("login page");
    }else{
        response.end("404 Not Found");
    }
    
});

server.listen(3000,function(){
   console.log("服务器已成功启用在3000端口,可以通过localhost:3000访问");           
});
```

```js
var http=require("http");
var server=http.createServer();

server.on("request",function(request,response){
    /*
    	根据不同的请求路径发送不同的响应结果
    	1.获取请求路径,request.url获取到的是端口号之后的那一部分路径,也就是说所有的url都是以\开头的
    	2.判断路径处理响应
    */
    var url=request.url;	
    if(url==="/plain"){
        response.setHeader("Content-Type","text/plain;charset=utf-8");
        response.end("你好世界");
    }else if(url==="/html"){
        //如果不写类型浏览器会默认解析html标签,但是又不会转换为UTF-8的编码
        response.setHeader("Content-Type","text/html;charset=utf-8");
        response.end("<p>你好世界</p>");
    }else{
        response.end("404 Not Found");
    }
    
});

server.listen(3000,function(){
   console.log("服务器已成功启用在3000端口,可以通过localhost:3000访问");           
});
```

```js
//用fs和http搭建简易服务器环境
var fs=require("fs");
var http=require("http");
var url=require("url");//引入url模块可以更好的解决域名的传输
var server=http.createServer();

server.on("request",function(request,response){
    var {pathname,query}=url.parse(request.url,true);//pares方法会将request请求发起的域名进行打包,而后面的参数true是讲打包后的字符串转换为对象,pathname就是要请求的地址,query是后面的参数
    if(pathname==="/html"){
/*
要注意的是如果请求的html页面有图片等需要再次加载的后台资源,在请求这里必须要设置一个pathname对应的请求
分支
*/  
  fs.readFile("index.html",function(err,data){
            if(err){
          		response.setHeader("Content-Type","text/plain;charset=utf-8");      
          		response.end("文件读取失败");  	
            }else{
          		response.setHeader("Content-Type","text/html;charset=utf-8");
                //其实可以不要上面的代码,因为在网页中也可用申明是UTF-8编码
                response.end(data);
                //response.end()支持两种数据类型,二进制和字符串,不需要用toString()转换
            }
        })
    }else if(pathname==="/plain"){
         fs.readFile("a.txt",function(err,data){
            if(err){
          		response.setHeader("Content-Type","text/plain;charset=utf-8");      
          		response.end("文件读取失败");  	
            }else{
          		response.setHeader("Content-Type","text/plain;charset=utf-8");
                response.end(data);
            }
        });
    }else if(pathname==="/image.jpg"){//这是如果请求的html文件中有图片需要加载时请求对应图片的分支
         fs.readFile("image.jpg",function(err,data){
            if(err){
          		response.setHeader("Content-Type","text/plain;charset=utf-8");      
          		response.end("文件读取失败");  	
            }else{
          		response.setHeader("Content-Type","image/jpeg");
                //图片不需要字符编码,一般我们说字符编码都是字符
                response.end(data);
            }
        })
    }else{
        response.setHeader("Content-Type","text/plain;charset=utf-8");
        response.end("404 Not Found");
    }
    
});
//可以对读取的文件进行修改后再传输出去

server.listen(3000,function(){
   console.log("服务器已成功启用在3000端口,可以通过localhost:3000访问");           
});
```

**具体的Content-Type可以去**[HTTP Content-type对照表](http://tool.oschina.net/commons)



### 3.5 用户自定义模块

在node中没有全局作用域,只有模块作用域,默认都是封闭的,外部不能访问到内部的数据,而内部也无法访问到外部,所有不会有污染的问题,但是只能通过require()方法来加载多个JS脚本文件,如果想要访问一个模块作用域中的内容,**需要同时使用require()方法与exports对象**

**require()的作用**

- 加载文件模块并执行里面的代码
- 拿到被加载文件模块导出的接口对象

**exports的作用**

在每个文件模块中都提供了一个接口对象exports,exports默认是空对象,用户所要做的就是将想要被外部访问的变量或方法挂载到这个对象上,然后通过require()方法就会返回这个exports对象,通过赋值给变量就能实现对模块内部成员的使用了

```js
//a.js
var a=require("./b.js");
console.log(a.foo);//123
console.log(a.add());//3
```

```js
//b.js
var foo=123;
exports.foo=foo;
exports.add=function(){
	return 1+2;
}
```

**exports与module.exports**

两者默认情况下实际是连接的同一个对象,但真正起主导作用的是module.exports

```js
//a.js
console.log(exports===module.exports);//true
exports={//此时exports为另一个对象
    a:1
}
module.export={
    b:2
}

//b.js
a=require("./a.js");
console.log(a.a);//undefined
console.log(a.b);//2
```

我们可以理解为模块导出的是module.exports这个属性上的值，而最开始exports和module.exports相同是因为有同一个引用，我们主动将exports变量上的引用改变了，所以导出的值是不一样的，我们不应该改变exports上面的引用



**前后端的区别**

- require:node和es6都支持的引入
- export / import:只有es6支持的导出引入(node 8.x版本以后已经支持)
- module.exports / exports:只有node支持的导出



### 3.6 第三方模块

**模块导入：**如果模块的加载是以`./`、`../`、`/`开始的，那么就是路径模块加载模式不以`./`、`../`、`/`开始的模块，按照的是node默认的模块解析机制，会从当前目录一次向上级目录查找`node_modules`，直到不能再往上查找，例如如果是windows系统就会查找到盘符



## 4.npm

**npm全称为node package manager(包管理工具)**

### 4.1 npm网站

[npm官网](www.npmjs.com),专门用来下载包的地方



### 4.2 npm命令行工具

**升级npm(自己升级自己)**

```shell
npm install --global npm
```



### 4.3 常用命令

- **npm init**,初始化项目,`npm init -y`可以跳过向导,快速生成初始化项目`

- **npm install**,一次性下载package.json文件中的dependencies选项中的所有依赖项

  **简写:**`npm i`

- **npm install 包名**,只下载包名

  **简写:**`npm i 包名` 

  **注：**如果想下载指定版本，需要在后面加上`@版本号`

- **npm intall --save 包名**,下载指定包名并保存依赖项(保存在package.json文件中的dependencies选项)

  **简写:**`npm i -S 包名`

- **npm uninstall 包名**,只删除包名

  **简写:**`npm un 包名`

  **注：**如果想删除指定版本，需要在后面加上`@版本号`

- **npm uninstall --save 包名**,删除包名的同时也会把依赖信息去除

  **简写:**`npm un -S 包名`

- **npm list**,以树形的形式列出所有的包依赖。**npm list --depth=0**,列出当前项目直接的包依赖

- **npm view 包名**,查看指定包的`package.json`文件，如果只想看该文件中的某个属性，后面再加个空格更上属性名。同时，**npm view 包名 versions**可以查看该包发布过的所有版本

- **npm outdated**,对比查看依赖包的版本信息

- **npm update**,更新依赖包的次要版本和补丁版本

  **注：**如果主要版本改变，只会更新到次要版本的最高版本。如果要更新主要版本，需要再全局按照一个包管理工具**`npm-check-updates`**

  ```shell
  npm i npm-check-updates -g
  ```

  然后在项目中运行`npm-check-updates`就能找到需要更新的包，再运行`ncu -u`就能更新大版本了。

  **注：**只是更新了`package.json`中的版本，并没有安装依赖，需要`npm i`安装依赖

- **npm login**,登录npm账号

- **npm publish**,发布一个包到npm（需要登录）

- **npm help**,查看命令使用帮助

- **npm 命令 --help**,查看指定命令使用帮助



### 4.4 包版本管理

npm使用语义版本号分为`X.Y.Z`三位，分别代表**主版本号**、**次版本号**和**补丁版本号**。当代码变更时，版本号按以下原则更新：

1. 如果只是修复bug，需要更新Z位。
2. 如果是新增了功能，但是向下兼容，需要更新Y位。
3. 如果有大变动，向下不兼容，需要更新X位。

**同时，npm还使用了`~`和`^`来帮忙推断安装依赖时应该选择哪个版本：**

- ~version（也就是`X.x`）
  - 如果次版本号指定，那么次版本号不变，而补丁版本号可以任意
  - 如果次版本号和补丁版本号未指定，那么次版本号和补丁版本号可以任意

- ^version（也就是X.Y.x）
  - 版本号中最左非零位不变，它右侧其他位可以任意
  - 如果缺少某个版本号，那么这个版本号可以任意

当然，如果要安装指定版本直接改成x.y.z的形式就行了。



## 5.进程process

[process的api文档](http://nodejs.cn/api/process.html#process_process)

`process` 对象是一个全局变量（默认挂载在global对象上的），它提供有关当前 Node.js 进程的信息并对其进行控制。 作为一个全局变量，它始终可供 Node.js 应用程序使用，无需使用 `require()`。 它也可以使用 `require()` 显式地访问：

```js
const process = require('process')
```

它提供当前Node.js的进程有关的信息，以及控制当前Node.js进程，该变量下面有很多属性，下面列出几个属性:

- **argv：**接收在命令行输入的参数的数组

- **env：**列举出当前系统的所有环境变量，是一个json对象，在windows中环境变量通过set来申明，mac是export

  **注：**一个好用的获取环境变量的库`config`，`npm i config -S`

### 5.1 处理未被捕获的错误

在node的运行中，如果在中途遇到错误会直接导致node程序崩溃，在很多情况下我们都能够去捕获到这些错误，但是如果并没有捕获，又不想程序崩溃，我们可以善用process监听`uncaughtException`事件与`unhandledRejection`（可以看出process也继承了事件触发器）

```js
// 同步错误
process.on('uncaughtException', (e) => {
    console.log(e)
})
// 没有被catch的promise的reject
process.on('uncaughtException', (e) => {
    console.log(e)
	// throw e // 也可以抛出错误变成同步的
})
```

**注：**使用`uncaughtException`只能够捕获到如`throw Error()`这样的同步代码错误，无法监听到被rejected的promise。

**注意：**在程序报异常的时候，我们最好的做法是立刻终止程序，而不是让它处于未完成的状态。
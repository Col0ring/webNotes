# Express

**express是一款Node.js框架**——[express中文文档](http://www.expressjs.com.cn/)

## 1.引入express

```shell
npm install express --save
```

```js
//app.js
const express=require("express");
//这句代码相当于原生的http.createServer(),创建一个服务器端口
const app=express();
//express可以自动将静态的文件设置好路由
app.use(express.static("public"));
/*
 上面的代码有两个作用:
 1.自动为public目录下的文件设置路由(public与app.js为同一层级)
 2.自动把public目录下的index.html设置为根路径对应的首页
*/
/*
	同时可以公开指定目录,通过这种方法可以直接访问文件目录下的所有资源(通过完整的文件名)
	第一个参数不一定是static,可以是随便的路由标识,但是后面可以直接使用static目录的资源
	app.use("/static/",express.static("./static/"));
	而上面的方法其实是个省略第一个参数的简便写法,可以通过省略/public的方式获取资源
*/
app.listen(3000);
```



**热部署服务器**

- node.js热部署,修改后立刻重启服务器`cnpm install -g supervisor`,全局安装后运行`supervisor app.js`(假设Node.js程序主入口是app.js)
- 热部署第二种方法为`cnpm install nodemon -g`,全局安装后运行`nodemon app.js`(假设Node.js程序主入口是app.js)
- 在使用项目前可以先初始化项目使用`npm init -y`



## 2.request请求

- **app.get(路由,回调函数)**,回调函数在符合指定路径并且是get请求时触发
- **app.post(路由,回调函数)**,回调函数在符合指定路径并且是post请求时触发
- **app.use(路由,回调函数)**,回调函数在符合指定路径并且不论请求方式是什么都会触发

```js
const express=require("express");
const app=express();

app.use('/login',(req,res)=>{//query和params等属性只有通过get方式进行请求的时候才会有
    console.log(req.query);//请求后面的查询数据保存在req.query中,req.query是一个对象
    console.log(req.params);//后面的如:number形式的数据保存在req.params里
    res.end("login");
})
```



**获取post请求数据**

express原生的post请求获取数据的方式很麻烦,所以推荐使用第三方的插件

**注:**现在好像不需要这个现在这个插件就能使用,只需要设置`app.use(express.json())`就能获取到

- **formidable**

    ```shell
    cnpm i -S formidable
    ```

    ```js
    const formidable=require("formidable");
    const path=require("path");
    var form = new formidable.IncomingForm();//创建实例
    form.uploadDir="./upload";//如果是要得到post请求中上传的文件,需要指定上传文件目录,注意必须先创建
    form.parse(req,(err,fileds,files)=>{
        //fileds为字符型的post数据
        //files为文件类的post数据
        let extname=path.extname(files.myfile.name);
        //因为通过formidable存入的文件默认是没有后缀名的,所以加上
        fs.rename(files.myfile.path,files.myfile.path+extname,()=>{
            res.send("OK");
        }) 
    })
    ```

- **body-parser**

    ```shell
    cnpm install body-parser -S
    ```

    ```js
    const bodyParser = require('body-parser');
    const express=require("express");
    const app=express();
    app.use(bodyParser.urlencoded({//因为post数据传入的是url编码,所以在写中间件的时候需要解析
        extended:false //是否激活扩展,默认是激活状态
    }));
    app.use(bodyParser.json());//转换为json
    //加入了body-parser配置后就会在req对象上多出body属性
    app.post("/login",(req,res)=>{
        console.log(req.body);//post请求就可以直接在req.body中取得
    })
    ```

- **blueimp-md5**

  ```shell
  cnpm install body-parser -S
  ```

  ```js
  const md5 = require("blueimp-md5");
  const bodyParser = require('body-parser');
  const express=require("express");
  const app=express();
  app.use(bodyParser.urlencoded({//因为post数据传入的是url编码,所以在写中间件的时候需要解析
      extended:false //是否激活扩展,默认是激活状态
  }));
  app.use(bodyParser.json());//转换为json
  //加入了body-parser配置后就会在req对象上多出body属性
  app.post("/login",(req,res)=>{
      console.log(req.body);//post请求就可以直接在req.body中取得
      console.log(md5(md5(req.body.password)));
      //md5二次加密,以后在使用的时候也是会加密后再比较,只能加密不能解密
  })
  ```

- **bcrypt**

    ```shell
    cnpm install bcrypt -S
    ```

    ```js
    //bcrypt也是一个加密的模块
    const bcrypt=require("bcrypt");
    const bodyParser = require('body-parser');
    const express=require("express");
    const app=express();
    app.use(bodyParser.urlencoded({//因为post数据传入的是url编码,所以在写中间件的时候需要解析
        extended:false //是否激活扩展,默认是激活状态
    }));
    app.use(bodyParser.json());//转换为json
    //加入了body-parser配置后就会在req对象上多出body属性
    app.post("/resgister",(req,res)=>{
        console.log(req.body);//post请求就可以直接在req.body中取得
        console.log(body.password = bcrypt.hashSync(req.body.password),10);
        //通过散列的方法进行加密,后面的数字散列强度,数字越大强度越高,但是效率会减弱
    })
    ```

    ```js
    app.post("/login",(req,res)=>{
        //加密后比对密码,如果一致返回true,否则为false
        const isPasswordValid=bcrypt.compareSync(req.body.password,user.password);
        //req.body.password是接收到的登录密码user.password是数据库保存的密码
        if(!isPasswordValid){
            return res.status(422).send({
                message:"密码无效"
            })
        }
        res.send({
            user
        })  
    })
    ```

    

## 3.response回应

- **res.send(数据),**作为结束响应的标志,该方法比元素的**res.end()**方法更加强大,不只可以穿buffer数据和字符串,也可以直接传入对象和数组等值

- **res.json(json对象),**express封装过的方法,可以直接返回给前端一个json格式的对象字符串

- **res.render(传入的模板页面,传入ejs页面的参数对象)**,专门用来渲染**views目录(这是必须的)**下的ejs文件的方法

  **在这里使用ejs模板引擎**

  **注:**也可使用**[art-template模板引擎](http://aui.github.io/art-template/zh-cn/docs/syntax.html#%E6%A8%A1%E6%9D%BF%E7%BB%A7%E6%89%BF)**

  ```shell
  cnpm install ejs -S
  ```

  ```js
  const express = require("express");
  const app = express();
  app.set("view engine", "ejs");//在这里设置使用ejs模板引擎
  /*或者
  app.engine("ejs",require("ejs"));
  */
  app.use('/login',(req,res)=>{
      let result=10;
      res.render("showIndex",{
      	a:result    
      });
      /*
      render()方法默认是不可用的,但是如果配置了模板引擎就可以使用
      res.render()可以默认将views目录下的ejs文件直接渲染到页面,只用传文件名,后面跟一个对象,对象中	 的属性就是传入ejs页面的值
      如果要修改默认的views目录,可以使用app.set("views",render函数渲染的默认路径)
      */
  })
  ```

  ```ejs
  <!--showIndex.ejs-->
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
    </head>
    <body>
      <ul>
        <%for(let i=0;i<a;i++){ %>    <!--a是从render函数传过来的值-->
        <li>我是第<%= i%>个li</li>
        <%}%>
        <!--
  		ejs是后台模板
  		通过<% js代码%>可以在中间写js代码,通过<%= 变量%>可以在中间赋值,实现传值进入页面
  	  -->  
      </ul>
    </body>
  </html>
  ```



## 4.cookie和session

### 4.1 cookie

**在路由的response中可以使用下面的cookie方法进行对cookie的设置**

- **res.cookie(name,value,[option]):**设置Cookie

  **opition包括:**domain / expires / httpOnly / maxAge / path / secure / signed

    ```js
    const express=require("express");
    const app=express();
    app.get("/login",(req,res)=>{
        res.cookie("key","666",{
            maxAge:1000 //设置此cookie过期时长
        });
        res.send();
    });
    ```

- **res.clearCookie():**清除Cookie

  ```js
  const express=require("express");
  const app=express();
  app.get("/login",(req,res)=>{
      res.clearCookie();
      res.send();
  });
  ```




**获取cookies信息**

**在express中获取cookies的信息需要使用对应的中间件cookie-parser**

```shell
cnpm install cookie-parser --save
```

```js
const cookieParser=require("cookie-parser");
const express=require("express");
const app=express();
app.use(cookieParser());
app.get("/login",(req,res)=>{
    console.log(req.cookies);//注意查询cookies在req中查看
    res.send();
});
```



### 4.2 session

**在express设置和获取session需要使用中间件express-session**

```shell
cnpm install express-session --save
```

```js
const session=require("express-session");
const express=require("express");
const app=express();
app.use(session({
    secret:"key",//配置加密字符串,与存入的session配合加密,安全性更高,防止客户端恶意伪造
    cookie:{maxAge:60000},
    resave:true,
    //每次请求都重新设置session cookie，假设cookie是10分钟过期,每次请求都会再设置10分钟
    saveUninitialized:true
	/*
	无论有没有session cookie,每次请求都设置个session cookie,默认给个标示为 connect.sid,如果为		false就是当正在存了数据才会发送
	*/
}));
app.get("/",(req,res)=>{
    console.log(req.session);	
    console.log(req.session.user);	
});

app.get("/login",(req,res)=>{
    req.session.user=req.session.user||[];//通过属性进行设置
    req.session.user.push(req.query.user);
    res.send();
})
/*
	读取:req.session.xxx
	设置:req.session.xxx=xxx
	删除:delete req.session.xxx
*/
```

**注意:**默认session数据是内存存储的,服务器一旦重启就会丢失,真正的生产环境会吧session进行持久化存储



### 4.3 token

**在express中同样可以设置token来获取本地的存储信息,需要使用中间件jsonwebtoken**

```shell
cnpm i jsonwebtoken -S
```

```js
const jwt=require("jsonwebtoken");
const SECRET="sadasdasda";//密钥
app.post("/login",(req,res)=>{
    //加密后比对密码,如果一致返回true,否则为false
    const isPasswordValid=bcrypt.compareSync(req.body.password,user.password);
    //req.body.password是接收到的登录密码user.password是数据库保存的密码
    if(!isPasswordValid){
        return res.status(422).send({
            message:"密码无效"
        })
    }
    const token=jwt.sign({//设置用户的_id为token
        id:String(user._id)//只需要传入用户ID,不要传密码
    },SECRET);//第二个参数是一个密钥,随便填什么都可以,但是应该是保密的,不应该在git项目中,应该在本地,需要的时候获取,因为要多次使用,所以直接定义成一个变量
    res.send({
        user,
        token
    })  
})
//读取服务端返回的信息
app.get("/login/profile",(req,res)=>{
    const raw=String(req.headers.authorization);
    //客户端发送请求的时候应该带上请求头,请求头中就是服务端返回给客户端的token
    const  {id}=jwt.verify(raw,SECRET);//解密,通过token和密钥解密
    const user=User.findById(id);//数据库查找用户
    res.send(user);
})
```

```js
//一般每一步都需要用户进行验证,所以需要设置一个中间件在每一次需要用户信息的时候就调用这个中间件进行判断
const auth=async(req,res,next)=>{
    const raw=String(req.headers.authorization);
    //客户端发送请求的时候应该带上请求头,请求头中就是服务端返回给客户端的token
    const  {id}=jwt.verify(raw,SECRET);//解密,通过token和密钥解密
    req.user=User.findById(id);//数据库查找用户,同时因为是使用中间件,所以需要挂载到req对象上
    next();
}

app.get("/login/profile",auth,(req,res)=>{
    res.send(req.user);
})
```



## 5.路由分离

- **常规写法**

  ```js
  //app.js
  /*
  	入门模块:创建服务、做服务相关配置、模板引擎、body-parser解析post请求、提供静态资源、监听端口
  */
  let express=require("express");
  let router=require("./router");
  let app=express();
  router(app);//通过这种方法将app传入router.js
  app.listen(3000,function(){
      console.log("running 3000");
  })
  ```

  ```js
  //router.js
  /*
  	路由模块:处理路由、根据不同请求方法和请求路径进行具体处理
  */
  module.exports=function(app){
      app.get("/",function(req,res){
          res.send("index");
      });
  }
  ```

- **express的router写法(express推荐)**

  ```js
  //app.js
  let express=require("express");
  let router=require("./router");
  let app=express();
  //把路由容器挂载到app,这样就可以直接访问路由了
  app.use(router);//注意配置模板引擎和body-parser必须在这之前
  /*
  	上面那种写法是讲所有路由都放在一个文件中,如果需要放入多个文件的话同时二级目录指定的话
  	app.use("/login",require(./route/login));另一个文件的内部写法同上,不过如果内部也只用写	二级的目录格式,不需要在前面加上/login,不过在访问的时候还是需要加上的,这种方法可以实现分路由的		管理
  */
  app.listen(3000,function(){
  	console.log("running 3000");
  })
  ```

  ```js
  //router.js
  let express=require("express");
  //创建一个路由容器
  let router=express.Router();//express推荐使用的挂载路由的方式
  //把路由都挂载到路由容器上
  router.get("/",function(req,res){
          res.send("index");
  });
  module.exports=router;
  ```




## 6.中间件

中间件的本质就是一个请求处理方法,我们把用户从请求到响应的整个过程分发到多个中间件中去处理,这样做的目的是提高代码的灵活性,动态可扩展的

### 6.1 应用程序级别中间件

**中间件的处理顺序是从上到下依次处理,而且相同的请求路径中的参数属性一致,所以才说第三方中间件必须要写在所有路由请求前,因为这样就能用中间件的各种查询了**

- 万能匹配的中间件(不关心任何请求路径和请求方法),全部都能匹配

  **注:**第三方中间件常常就是这种万能匹配能够匹配到所有的路径

  ```js
  app.use(function(req,res,next){
      console.log("Time",Data.now());
      next();
  })
  ```

- 匹配所有以`/xxx/`开头的

  ```js
  app.use("/a",function(req,res,next){
      console.log("Time",Data.now());
      next();
  })
  ```



### 6.2 路由级别中间件

- **get**

  ```js
  app.get("/",function(req,res){
      res.send("Hello world");
  })
  ```

- **post:**

  ```js
  app.post("/",function(req,res){
      res.send("Hello world");
  })
  ```

- **put:**

  ```js
  app.put("/",function(req,res){
      res.send("Hello world");
  })
  ```

- **delete:**

  ```js
  app.delete("/",function(req,res){
      res.send("Hello world");
  })
  ```

**注意:**路由的中间件也是有next的,如果不调用就不会执行后续中间件,但是因为路由一般都只进入一个中间件,所有一般没有写next参数,但是如果写两个相同的路由,就必须写next才会从前面一个路由进入后面一个路由



### 6.3 错误处理中间件

**可以在所有的路最后写入一个错误处理中间件,只要遇到错误就会直接跳到这个错误处理中间中.不必每一个路由就要进行专门的错误处理**

**注意:**只要向路由中的第三个形参传入值,就可以直接找到有四个参数的中间件

```js
//错误处理中间件必须要写四个参数,第一个参数是其他中间件传过来的错误,如果只写三个就只是形参不同而已
app.use(function(err,req,res,next){
    console.log(err.message);
    res.status(500).send("error!");
})
```

```js
app.get("/",function(req,res,next){
    fs.readFile(function(err,data){
        if(err){//一般都是直接传入错误对象
     		next(err);   
    	}  
    })
})

app.use(function(err,req,res,next){//上面传入的错误对象会直接复制给第一个形参err
    console.log(err.message);
    res.status(500).send("error!");
})
```


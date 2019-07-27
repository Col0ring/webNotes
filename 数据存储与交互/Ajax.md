# Ajax

**Ajax全称 Asynchronous JavaScript and XML(异步的JavaScript和XML),既是一个对象,也是一种方法模式**

AJAX是一种用于创建快速动态网页的技术,通过在后台与服务器进行少量数据交换,Ajax可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新,传统的不使用Ajax的网页如果需要更新内容,必需重载整个网页面

## 1.原生Ajax

### 1.1 用法

#### 1.1.1 创建Ajax对象

- **通过XMLHttpRequest()类创建一个Ajax对象(此方法不兼容IE6以下浏览器)**

  ```js
  var Ajax=new XMLHttpRequest();
  ```

- **在IE6以下浏览器通过ActiveXObject("Microsoft.XMLHTTP")创建一个Ajax对象**

  ```js
  var Ajax=new ActiveXObject("Microsoft.XMLHTTP")
  ```

  

#### 1.1.2 连接服务器

**通过open()方法,可以设置跟后台交互的一些行为,该方法有三个参数**

**参数**

- **请求方式**,一个包含具体请求方式的字符串

  **主要请求方式**

  - **POST**,建议在添加数据时使用

  - **GET**,建议在查询数据时使用
  - **PUT**,建议在修改数据时使用
  - **DELETE**,建议在删除数据时使用

- **url**,要请求数据的具体地址
- **bool**,进行同步或异步传输,true为异步,false为同步

```js
//请求方式可以用大写也可以用小写
Ajax.open("GET","a.txt",true);
```

**注意:在IE浏览器中,如果通过Ajax发生GET请求,那么IE浏览器认为同一个url只有一个结果，修改文件内容浏览器中显示的值也不会发生改变**

```js
//一般按照如下格式写
Ajax.open("GET","a.txt?t="+newDate().getTime(),true);
```



**设置需求**

- **如果想用GET的时候直接添加需求,可以在url后面加上?再写具体的需求,多个需求通过&符号连接**

  ```js
  Ajax.open("GET","https://www.baidu.com?user=zhangsan&pwd=123456",true);
  ```

- **如果是POST请求,需要设置请求头信息,如:**

  ```js
  //表单格式请求头信息的写法
  Ajax.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
  //在下一步的send()方法中传入需求
  Ajax.send(“user=zhangsan&pwd=123456");
  ```

  

#### 1.1.3 发送请求

**通过send()方法正式发送请求**

```js
Ajax.send();
```

**注:**如果请求方式是POST方式,添加的需求作为参数参入send()方法中



#### 1.1.4 返回请求结果

- **通过readyState(Ajax自身的状态码)属性来返回请求进行到了某一步,该属性存有请求的状态**

  **请求状态**

  - 0: 请求未初始化
  - 1: 服务器连接已建立
  - 2: 请求已接收
  - 3: 请求处理中
  - 4: 请求已完成，且响应已就绪

- **通过onreadystatechange事件(在状态码发生改变时就会执行此事件)或onload事件来对状态码发生改变时进行操作**

- **通过status(http的状态码)属性的值判断请求后的结果从而得到请求信息,一般该值为200到300直接和等于304时代表请求成功,值为404代表请求失败**
- **response属性保留着后台返回到前端的数据**

```js
Ajax.onreadystatechange=function(){
    if(Ajax.readyState===4){
        //确定http的状态码
        if(Ajax.status>=200&&Ajax.status<300||Ajax.status===304){
            console.log(Ajax.response);
        }
    }
}
```



### 1.2 超时请求

如果请求事件太长就是请求超时,如果事件过长则可能内部出现了错了,可以设置超过一定时间结束请求

```js
//可以通过定时器进行结束请求
setTimeout(function(){
    Ajax.abort();
},3000)
//低版本IE可以使用
Ajax.timeout=3000;
```



### 1.3 跨域请求

如果通过Ajax跨域传输一个请求,浏览器会自动将该请求阻止并返回报错信息。如果要进行跨域就要进行特殊方法进行实现

**实现跨域**

- JSON方式,通过script标签中的src属性传输请求

  **JSONP实现原理**

  - 由于浏览器的安全性现在,不允许Ajax访问协议不同,域名不同,端口不同的数据接口,浏览器会认为这种访问不安全

  - 可以通过动态创建script标签的形式,把script标签的src属性,指向数据接口的地址,因为script标签不存在跨域限制,这种输数据获取方式就被称为JSONP

    **注意:**由于JSONP的内部实现原理,JSONP只能支持Get请求

  - **具体实现:**

    - 先在客户端定义一个回调方法,预定义对数据的操作
    - 再把这个回调方法的名称通过url传参的形式提交到服务器的数据接口
    - 服务器数据接口组织好要发送给客户端的数据,再拿着客户端传递过来的回调方法名称,拼接处一个调用这个方法的字符串,发送给客户端去解析执行
    - 客户端拿到服务器返回的字符串之后,当作script脚本去解析执行,这样就能够拿到JSONP的数据了

- CORS方式,在后台中设置可以让对应的域进行访问

- 通过信任服务器代理请求进入(翻墙)



### 1.4 封装Ajax

```js
function request(){
    ajax({
        type:'POST',//请求类型
        url:'data.json',//请求路径
        asyn:true,//是否异步
        data:{//数据
            username:'千寻',
            password:'888'
        },
        dataType:'text',//请求数据类型
        success:function(response){//请求成功处理函数
            console.log( response );
            var response = JSON.parse(response);
            console.log( response[2].name );
        },
        error:function(status){//请求失败处理函数
            console.log(status);

        }

    })

}

//Ajax 封装
function ajax(obj){
    var type = obj.type||'GET',//请求类型
        url = obj.url,//url处理
        asny = obj.asny!==true,//异步处理
        data = '',//预设写入数据
        dataType = obj.dataType||'json',//请求类型
        success = obj.success,//回调函数
        error = obj.error;//错误处理函数
    for(key in obj.data){//数据处理
        data += key+'='+obj.data[key]+'&';
    }
    data = encodeURI(data);
    //console.log(data);
    var xhr=new XMLHttpRequest();
    //console.log(xhr);
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        try{
            xhr = new ActiveXObject('Msxml2.XMLHTTP.6.0');
        }catch(e){
            xhr = new ActiveXObject('Msxml2.XMLHTTP.3.0');
        }
    }
    //如果是GET请求方式,设置数据
    if(type.toUpperCase() == 'GET'){
        var date = new Date().getTime();//添加一个时间用来处理，GET方式缓存的问题
        //console.log(date);
        url = url+'?'+data+date;
        data =null;
    }else if(dataType.toUpperCase() == 'XML'){
        data =null;
    }
    xhr.open(type,url,asny);//请求架设
    xhr.setRequestHeader('content-Type','application/x-www-form-urlencoded');
    //设置请求头信息
    //console.log(data);
    xhr.send(data);//发送数据
    xhr.onreadystatechange= function(){//监听请求
        //readyState 为XMLHttpRequest对象的状态
        //status 为服务器返回的http状态码

        if(xhr.readyState== 4 && xhr.status==200){
            var response;
            if(dataType === 'json' || dataType === 'text'){
                if(dataType === 'json'){
                    //解析json数据
                    response = JSON.parse(xhr.responseText);
                }else{
                    //普通数据
                    response = xhr.responseText;
                }

            }else{
                response = xhr.responseXML;
            }
            success&&success(response);
        }else{
            error&&error(xhr.status);
        }
    }

}
```



## 2.Fetch

**传统的Ajax指的是XMLHttpResquest(XHR),现在已经已经可以用fetch来代替**

**注意:**

- Fetch是全局对象的方法,可以直接使用
- Fetch是基于Promise设计的,所以值能在支持ES6版本的浏览器使用

### 2.1 用法

#### 2.1.1 **Fetch的参数**

- **url,该参数定义要获取的资源,可以是一个url字符串,也可以是一个Request对象**

- **options,该参数可选,为一个对象,内部含有一些对请求的设置属性**

  **内部属性**

  - **method:**请求使用的方法。如:GET、POST等
  - **headers:**请求头信息。形式为Headers对象或ByteString
  - **body:**请求的body信息,可能是一个Blob、BufferSource、FormData、URLSearchParams或者USVString对象。注意GET或HEAD方法的请求不能包含body信息
  - **mode:**请求的模式。如cors、no-cors或者same-origin
  - **credentiala:**请求的credentials。如omit、same-origin或者include
  - **cache:**请求的cache模式:default,no-store,reload,no-cache或者only-if-cached



#### 2.1.2 response

**response为一个Promise对象成功时传回的对象,该属性含有对应返回数据的属性和方法**

- **属性**
  - **status (number),HTTP请求结果参数,在100-599范围**
  - **statusText (string),服务器返回的状态报告**
  - **ok (boolean),如果返回200表示请求成功则为true**
  - **headers (Headers),返回头部信息,该属性后面也有对应的方法**
    - has(name) (boolean),判断是否存在该信息头
    - get(name) (string),获取信息头的数据
    - getAll(name) (Array),获取所有头部数据
    - set(name,value),设置请求头信息
    - append(name,value),添加header的内容
    - delete(name),删除header的信息
    - forEach(function(value,name){...},[thisContext]),循环读取header的信息
  - **url (string),详细的地址**
- **方法**
  - **text(),以string的形式生成请求text**
  - **json(),生成JSON.parse(responseText)的结果**
  - **blob(),生成一个Blob**
  - **arrayBuffer(),生成一个ArrayBuffer**
  - **formData(),生成格式化数据,可用于其它的请求**
- **其他方法**
  - **clone(),创建一个Response对象的克隆**
  - **Response.error(),返回一个绑定了网络错误的新的Response对象**
  - **Response.redirect(),用另一个URL创建一个新的Resonse对象**



#### 2.1.3 使用案例

- **get**

  ```js
  fetch("a.html").then(function(response){
      return response.text();
  }).then(function(body){
      document.body.innerHTML=body;
  });
  ```

- **post**

  ```js
  fetch("a.html",{
      method:"POST",
      headers:{
          "Accept":"application/json",
          "Content-Type":"application/json";
      },
      body:JSON.stringify({
          user:"zhangsan",
          pwd:123456;
      })
  })
  ```

  

### 2.2 与原生Ajax的比较

- 原生Ajax

  ```js
  var Ajax=new XMLHttpRequest();
  Ajax.open("GET",url);
  Ajax.responseType="json";
  Ajax.send();
  Ajax.onreadystatechange=function(){
      console.log(Ajax.response);
  };
  Ajax.onerror=function(){
      console.log("error");
  };
  ```

- Fetch

  ```js
  fetch(url).then(function(response){
      return response.json();//相当于JSON.parse(response);
  }).then(function(data){
      console.log(data);
  }).catch(function(err){
      console.log("error",err);
  });
  
  //使用箭头函数
  fetch(url).then(
      response=>response.json();
  ).then(
      data=>console.log(data);
  ).catch(
      err=>console.log("error",err);
  );
  //使用async函数
  let Fetch=async function(){
      try{
          let response=await fetch(url);
          let data=await response.json();
          console.log(data);   
      }catch(err){
  		console.log("error",err);
      }
  }
  ```

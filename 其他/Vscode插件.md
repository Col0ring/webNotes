# Vscode插件

## 1.rest-client

**`rest-client`是一个Vscode用代码在线请求接口的插件,用户可以在Vscode中达到`postman`的类似效果**

- 首先需要创建一个`.http`文件,在这个`.http`文件实现请求接口的模拟

- **基本使用**

  - **创建变量**

      因为可能一个接口会多次进行请求,同时请求的首地址可能都是一样的,所以可以创建变量简化使用

      ```http
      #注意不能加引号,所有的内容默认都是字符串
      @url=http://www.baidu.com/
      ```

  - **GET请求**

      ```http
      #注意发起请求的时候一个新的发起应该使用###来进行分离,不然请求下一个无法发起
      
      GET {{url}}
      ###在上面是直接使用了变量,变量用{{}}模板字符串进行使用
      
      GET {{url}}site
      ###上面变量可以和任意的字符串组合成为一个新的接口,如上面这个就是http://www.baidu.com/site
      ```

  - **POST请求**

      POST请求较为麻烦,需要添加一个请求头才能发起请求,一般需要返回json数据,所以直接添加`Content-Type:application/json;charset=UTF-8`

      ```http
      #下面的Content-Type应该写在POST请求的下面,注意header与传入数据之间必须有空行
      
      POST {{url}}login
      Content-Type:application/json;charset=UTF-8
      
      {
          "username:":"admin",
          "password":"123456"
      }
      ###上面那种是定义传入后台参数的用法,传入进去的是JSON格式对象,POST请求会将下面定义的JSON字符串作为传入借口的参数传入到后台
      ```

      ```http
      #为了方便,也可以将请求头赋值给一个变量,以后直接调用,下面的表示服务端返回客户端的格式
      @json=Content-Type:application/json;charset=UTF-8
      @ajson=Accept:application/json
      
      POST {{url}}login
      {{json}}
      
      {
          "username:":"admin",
          "password":"123456"
      }
      ```

  - **PUT请求**

      **用法同POST请求,在Vscode中一样可以通过请求实现数据增删改查**

      ```http
      #PUT请求常用来做修改操作
      
      PUT {{url}}users
      {{json}}
      
      {
      	"username:":"admin",
          "password":"123",
          "_id":"1"
      }
      ```

  - **DELETE请求**

      ```http
      #DELETE请求常用来做删除数据的操作
      
      DELETE {{url}}/users
      {{json}}
      
      {
      	"username:":"admin",
          "password":"123",
          "_id":"1"
      }
      ```

      
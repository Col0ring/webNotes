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




## 2.koroFileHeader

koraFileHeader 是一个非常好用的在 vscode 中用于生成文件头部注释和函数注释的插件，可用于自己配置注释模板。

主要功能有：

- **文件头部添加注释**:
  - 在文件开头添加注释，记录文件信息/文件的传参/出参等
  - 支持用户高度自定义注释选项, 适配各种需求和注释。
  - 保存文件的时候，自动更新最后的编辑时间和编辑人
  - 快捷键：`window`：`ctrl+alt+i`,`mac`：`ctrl+cmd+i`, `linux`: `ctrl+meta+i`
- **在光标处添加函数注释**:
  - 在光标处自动生成一个注释模板
  - 支持用户高度自定义注释选项
  - 快捷键：`window`：`ctrl+alt+t`,`mac`：`ctrl+cmd+t`,`linux`: `ctrl+meta+t`
  - 快捷键不可用很可能是被占用了,[参考这里](https://github.com/OBKoro1/koro1FileHeader/issues/5)
- 支持一键添加佛祖保佑永无BUG、神兽护体等注释图案

github 地址：https://github.com/OBKoro1/koro1FileHeader

配置项：

```json
// 头部注释
"fileheader.customMade": {
    // 头部注释默认字段
    "Author": "your name",
    "Date": "Do not edit", // 设置后默认设置文件生成时间
    "LastEditTime": "Do not edit", // 设置后，保存文件更改默认更新最后编辑时间
    "LastEditors": "your name", // 设置后，保存文件更改默认更新最后编辑人
    "Description": "",
    "FilePath": "Do not edit", // 设置后，默认生成文件相对于项目的路径
    "custom_string_obkoro1": "可以输入预定的版权声明、个性签名、空行等"
},
// 函数注释
"fileheader.cursorMode": {
  // 默认字段
  "description":"",
  "param":"",
  "return":""
},
// 插件配置项
"fileheader.configObj": {
    "autoAdd": true, // 检测文件没有头部注释，自动添加文件头部注释
    "autoAddLine": 100, // 文件超过多少行数 不再自动添加头部注释
    "autoAlready": true, // 只添加插件支持的语言以及用户通过`language`选项自定义的注释
   // 自动添加头部注释黑名单
   "prohibitAutoAdd": [
      "json"
    ],
    "prohibitItemAutoAdd": [ "项目的全称禁止项目自动添加头部注释, 使用快捷键自行添加" ],
    "wideSame": false, // 头部注释等宽设置
   "wideNum": 13,  // 头部注释字段长度 默认为13
   // 头部注释第几行插入
    "headInsertLine": {
      "php": 2 // php文件 插入到第二行
    },
    "beforeAnnotation": {}, // 头部注释之前插入内容
    "afterAnnotation": {}, // 头部注释之后插入内容
    "specialOptions": {}, // 特殊字段自定义
    "switch": {
      "newlineAddAnnotation": true // 默认遇到换行符(\r\n \n \r)添加注释符号
    },
    "moveCursor": true, // 自动移动光标到Description所在行
    "dateFormat": "YYYY-MM-DD HH:mm:ss",
    "atSymbol": "@", // 更改所有文件的自定义注释中的@符号
    "atSymbolObj": {}, //  更改单独语言/文件的@
    "colon": ": ", // 更改所有文件的注释冒号
    "colonObj": {}, //  更改单独语言/文件的冒号
    "filePathColon": "路径分隔符替换", // 默认值： mac: / window是: \
     "showErrorMessage": false, // 是否显示插件错误通知 用于debugger
    "CheckFileChange": false, // 单个文件保存时进行diff检查
    "createHeader": true, // 新建文件自动添加头部注释
    "useWorker": false, // 是否使用工作区设置
    "designAddHead": false, // 添加注释图案时添加头部注释
     // 自定义语言注释符号，覆盖插件的注释格式
    "language": { 
        "java": {
            "head": "/$$",
            "middle": " $ @",
            "end": " $/"
        },
       // 一次匹配多种文件后缀文件 不用重复设置
       "h/hpp/cpp": {
          "head": "/*** ", // 统一增加几个*号
          "middle": " * @",
          "end": " */"
        },
        // 针对有特殊要求的文件如：test.blade.php
        "blade.php":{
          "head": "<!--",
          "middle": " * @",
          "end": "-->",
        }
    },
 // 默认注释  没有匹配到注释符号的时候使用。
 "annotationStr": { 
      "head": "/*",
      "middle": " * @",
      "end": " */",
      "use": false
    },
}
```


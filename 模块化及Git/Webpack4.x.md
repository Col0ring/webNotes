# Webpack

## 1.安装Webpack

**webpack可以在项目中进行本地的安装，也可以进行全局的安装，一般我们都是使用本地安装的webpack**

```shell
#先初始化项目
npm init -y
#本地安装，webpack4.x中需要同时安装webpack和webpack-cli
npm i webpack webpack-cli -D
```

**注：**webpack可以进行零配置，也就是说安装好之后就可以直接进行打包命令，使用`npx webpack`就可以直接让webpack进行打包了，默认会找到根目录下的`src`目录下的`index.js`进行打包（默认只支持js模块），打包后会生成一个`dist`目录,打包后的js文件会整合到一个`main.js`文件中



## 2.配置Webpack

webpack默认的零配置功能很弱，所以大多数时候都是需要我们手动进行配置

### 2.1 运行打包命令

**在webpack中默认的配置文件是`webpack.config.js`或者是`webpackfile.js`，一般情况下会选择前者**。当然，**我们也可以手动控制webpack执行自定义命名的配置文件**，使用`npx webpack --config 自定义的配置文件名`就能手动控制webpack找到对应的配置文件

**一般情况下，我们打包都是通过`npm run bulid`这样的方法进行配置的，这种就需要在`package.json`中自己配置自定义脚本了，在下面的`scripts`中配置脚本**

```json
{
  "name": "webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack --config webpack.config.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.39.2",
    "webpack-cli": "^3.3.6"
  }
}
```

**注：**

- **在自定义的脚本中运行webpack的指令前面后面写参数就不需要加上`npx`了**

- 也可以直接只在build后面写一个webpack，在打包的时候传入参数就行了（不过一般我们不会这么做），不过不能普通地进行拼接，还需要在`npm run build`后面加上两个`--`才能正常传参，**如`npm run build -- --config webpack.config.js`**，不然npm并不会认为后面传的东西是一个参数



### 2.2 基础配置

**webpack是node写出来的，所以我们需要使用node的语法来进行配置**，下面是webpack的基本配置，仅能够对js文件进行打包

```js
// webpack.config.js
//使用node内置的path模块来出来绝对路径问题
let path = require('path')
module.exports = {
  mode: 'development',
  /* 
    mode是打包后的模式，默认有两种，production和development,production模式会压缩代码，是生产环境后打包的，而development不会压缩代码，便于开发者观察打包后的结果
   */
  entry: './src/index.js', //打包程序的入口
  output: {
    filename: 'main.js', //打包后的文件名
    /*
    filename: 'main.[hash].js'// 生成hash文件名，写成这种方式可以防止覆盖和缓存，每次生成的文件都不一样
    filename: 'main.[hash:8].js'// 生成8位的hash文件名
    */
    path: path.resolve(__dirname, 'dist') //打包后的路径，注意，该路径必须是绝对路径
    /*
      path.resolve方法会将生成的文件路径拼接成一个绝对路径，该路径默认是以整个系统盘为路径的，可以在前面加一个__dirname参数改变成以当前根目录为绝对路径
      */
  }
}
```

webpack中三种三种hash值区别：[webpack三种哈希区别]( <https://www.cnblogs.com/giggle/p/9583940.html>)

#### 2.2.1 简单打包后的文件

**这个如果不想了解原理可略过**

```js
 (function(modules) { // webpackBootstrap
 	// The module cache 先定义一个缓存
 	var installedModules = {};

 	// The require function 配置实现了require函数
 	function __webpack_require__(moduleId) { // 参数"./src/index.js"

 		// Check if module is in cache 模块是否在缓存中
 		if(installedModules[moduleId]) {
 			return installedModules[moduleId].exports;
 		}
 		// Create a new module (and put it into the cache)
 		var module = installedModules[moduleId] = {
 			i: moduleId,
 			l: false,
 			exports: {}
 		};

 		// Execute the module function 执行传入this指向，模块，模块的空对象exports: {}，require方法
 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

 		// Flag the module as loaded
 		module.l = true;

 		// Return the exports of the module
 		return module.exports;
 	}


 	// expose the modules object (__webpack_modules__)
 	__webpack_require__.m = modules;

 	// expose the module cache
 	__webpack_require__.c = installedModules;

 	// define getter function for harmony exports
 	__webpack_require__.d = function(exports, name, getter) {
 		if(!__webpack_require__.o(exports, name)) {
 			Object.defineProperty(exports, name, { enumerable: true, get: getter });
 		}
 	};

 	// define __esModule on exports
 	__webpack_require__.r = function(exports) {
 		if(typeof Symbol !== 'undefined' && Symbol.toStringTag) {
 			Object.defineProperty(exports, Symbol.toStringTag, { value: 'Module' });
 		}
 		Object.defineProperty(exports, '__esModule', { value: true });
 	};

 	// create a fake namespace object
 	// mode & 1: value is a module id, require it
 	// mode & 2: merge all properties of value into the ns
 	// mode & 4: return value when already ns object
 	// mode & 8|1: behave like require
 	__webpack_require__.t = function(value, mode) {
 		if(mode & 1) value = __webpack_require__(value);
 		if(mode & 8) return value;
 		if((mode & 4) && typeof value === 'object' && value && value.__esModule) return value;
 		var ns = Object.create(null);
 		__webpack_require__.r(ns);
 		Object.defineProperty(ns, 'default', { enumerable: true, value: value });
 		if(mode & 2 && typeof value != 'string') for(var key in value) __webpack_require__.d(ns, key, function(key) { return value[key]; }.bind(null, key));
 		return ns;
 	};

 	// getDefaultExport function for compatibility with non-harmony modules
 	__webpack_require__.n = function(module) {
 		var getter = module && module.__esModule ?
 			function getDefault() { return module['default']; } :
 			function getModuleExports() { return module; };
 		__webpack_require__.d(getter, 'a', getter);
 		return getter;
 	};

 	// Object.prototype.hasOwnProperty.call
 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };

 	// __webpack_public_path__
 	__webpack_require__.p = "";


 	// Load entry module and return exports
 	return __webpack_require__(__webpack_require__.s = "./src/index.js"); // 传入入口模块
 })
//  自执行函数传入一个对象即modules
 ({
 "./src/a.js": // key
 (function(module, exports) { // value
  eval("module.exports = 'a_moudle'\n\n//# sourceURL=webpack:///./src/a.js?");
  }),
 "./src/index.js":
 (function(module, exports, __webpack_require__) {
  eval("let str = __webpack_require__(/*! ./a.js */ \"./src/a.js\")\r\n\r\nconsole.log(str)\n\n//# sourceURL=webpack:///./src/index.js?");
 })
});
```



### 2.3 本地服务

在之前的打包过程中我们只能通过打包好文件然后双击文件来在浏览器中运行打包后的代码，而我们再开发的时候更希望能通过本地服务器的方式进行访问，这时候我们需要借助第三方插件来生成这个服务，**在webpack中内置了一个开发服务`webpack-dev-server`**

**注意：**该服务进行的不是实际的打包，所以不会在dist目录中生成文件，而是进行的内存打包，生成的文件之间运行在服务器上，我们是看不到这些文件的，**在实际开发中我们都是用的这种内存的打包**

```shell
npm i webpack-dev-server -D
```

**安装完成后运行`npx webpack-dev-server`就能开启该服务（一般也是在package.json文件中配置自己的脚本，如`npm run dev`），默认的端口为`localhost:8080`**，当然也可以在`webpack.config.js`配置文件中进行配置，**服务配置写在`devServer`中**

```json
{
  "name": "webpack",
  "version": "1.0.0",
  "description": "",
  "main": "webpack.config.js",
  "scripts": {
    "dev": "webpack-dev-server",
    "build": "webpack --config webpack.config.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.39.2",
    "webpack-cli": "^3.3.7",
    "webpack-dev-server": "^3.8.0"
  }
}
```

```js
// webpack.config.js
module.exports = {
    devServer: {
        // 开发服务器的配置
        port: 3000, // 改变开启的端口号
        progress: true, // 开启内存打包中的进度条，这样我们能清楚地看到打包的进程
        open: true, // 自动打开浏览器
        contentBase: './build',
        /*
        内存打包指向的目录，开启服务器指向的入口打包文件的地址(默认是index.html文件，到目前需要自己创建该html文件和引入对应打包后的js文件，之后会有自动创建)
        */
        compress: true //压缩
    },
}
```

**注意：**上面的`contentBase`对应的路径中一定要有这个html文件，不然就会找不到，**但是我们在实际开发中其实是没有这个文件的，也不会打包好后再去开启服务，**我们希望的是在启动服务时自动在**内存**中创建这个文件，这样我们就能在服务端上直接访问了，这个时候我们就需要一个html插件解决这个问题



### 2.4 支持模板html

使用第三方的webpackl插件`html-webpack-plugin`可以帮我们自动建立一个模板html文件打包到内存中，**然后将主要要打包的文件加入到我们设置的模板html（设置的模板html文件中的代码会原封不动的打包输出，所有的配置文件只是针对格式的，内部并不会发生变化，也就是说连注释都会输出过去）里面，并且把结果放在`devServer`指定的打包指定目录下**

```shell
npm i html-webpack-plugin -D
```

**安装完成后，在`plugins`内部添加一个对象来实现使用**

```js
let HTMLWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  plugins: [
    // plugins是一个数组，放着所有的webpack插件
    new HTMLWebpackPlugin({
      template: './src/index.html', //html模板，会去找到这个模板
      filename: 'index.html', //打包后的文件名，默认不写也是index.html
      minify: {// 对打包的html模板进行压缩
        removeAttributeQuotes: true, // 删除属性的双引号，除了一些特殊的删除不了以外都能删除
        collapseWhitespace: true // 折叠空行将所有代码变成一行
      },
      hash: true // 给打包后在模板中引入的文件名生成hash戳，防止缓存
    })
  ]
}
```

**其余具体的插件参数选项见：[html-webpack-plugin#options](https://github.com/jantimon/html-webpack-plugin#options)**

```html
<!--打包前的模板-->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <!-- 模板 -->
  </body>
</html>
```

```html
<!--加了上述参数后打包后的文件-->
<!DOCTYPE html><html lang=en><head><meta charset=UTF-8><meta name=viewport content="width=device-width,initial-scale=1"><meta http-equiv=X-UA-Compatible content="ie=edge"><title>Document</title></head><body><!-- 模板 --><script type=text/javascript src=main.js?9a08db68a07b35e1f58a></script></body></html>
```



### 2.5 样式处理
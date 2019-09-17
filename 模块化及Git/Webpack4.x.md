# Webpack

> webpackl的功能模块详见[《webpack中文网》](https://webpack.docschina.org/)

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
  mode: 'development',// 这个模式只对压缩js文件有效
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

**webpack中三种三种hash值区别：[webpack三种哈希区别]( <https://www.cnblogs.com/giggle/p/9583940.html>)**

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
    // plugins是一个数组，放着所有的webpack插件，注意是从下到上，如果有关联项需要考虑位置
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

在进行引入样式处理时也需要进行额外的配置，需要在`module`模块中写入对应的css规则，给对应匹配上的规则添加loader，**然后直接在入口的js文件中使用引用模块import或require引用文件即可（不引用是不会打包的）**

**注意：**该css的loader会将解析后的css内容插入到页面head标签的最后，如果自己写了对于的css样式可能会覆盖掉，如果想要自己在模板html中写的样式生效，可以在`style-loader`的options选项中改变样式插入到模板html的head标签的位置（暂时只能通过这种样式写法，后面会有抽离成文件的插件）

**loader的用法**

- 如果只有一个loader，可以写成单个字符串
- 如果有多个loader，需要写成字符串数组
- loader还可以写成对象的形式，`loader`属性对应要写的loader，而还有一个options属性则是可以传入对应的参数
- **loader的顺序默认是从右向左，从下到上执行**（执行顺序非常重要，如必须要先执行`css-loader`才能执行`style-loader`）

#### 2.5.1 处理css

**处理普通的css文件只需要下载css-loader与style-loader就可以实现**

```shell
npm i css-loader style-loader -D
```

```js
module.exports = {
  module: {
    // 模块
    // 配置规则
    rules: [
      /*
      css-loader解析css中的语法，如@import这种
      style-loader用于把css插入到模板html的head标签中
      至于为什么要两个，是因为loader的能力尽量要求单一
      */
      {
        // 匹配css结尾的文件
        test: /\.css$/,
        use: [
          {
            loader: 'style-loader',
            options: {
              //改变样式插入的位置为head标签的顶部
              insertAt: 'top'
            }
          },
          'css-loader' //主要解析@import这种路径
        ]
      }
    ]
  }
}
```



#### 2.5.2 处理less

使用css预处理器除了要下载css-loader与style-loader，还需要额外下载对应预处理器需要的loader，如`sass-loader`、`less-loader`与`stylus-loader`，用法都相同，这里仅以less为例

```shell
#下载less-loader肯定是要使用less的，所以一般是一起下载了
npm i less less-loader -D
```

```js
module.exports = {
  module: {
    // 模块
    // 配置规则
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: 'style-loader',
            options: {
              insertAt: 'top'
            }
          },
          'css-loader'
        ]
      },
      //可以解析less文件
      {
        test: /\.less$/,
        use: [
          {
            loader: 'style-loader',
            options: {
              //改变样式插入的位置为head标签的顶部
              insertAt: 'top'
            }
          },
          'css-loader', //主要解析@import这种路径
          'less-loader' //主要把less转换为css
        ]
      }
    ]
  }
}
```

**其余的less-loader的配置详见：[less-loader](https://webpack.js.org/loaders/less-loader/#src/components/Sidebar/Sidbar.jsx)**



#### 2.5.3 抽离样式

如果想要将添加到html模板中的css样式单独作为一个文件抽离出来，自动通过link标签进行引入，还需要引入专门抽离css样式的插件`mini-css-extract-plugin`

```shell
npm i mini-css-extract-plugin -D
```

```js
// 引入插件
let MiniCssExtractPlugin = require('mini-css-extract-plugin')
module.exports = {
  plugins: [
    /*
    注意,如果想要分为多个不同的css文件，那么就require引入多次插件，取不同的名字，然后都在这使用，通常这种方式是要区分各个预处理器转换后的文件
    */
    new MiniCssExtractPlugin({
      filename: 'main.css' //要抽离后在打包后目录中的文件名
    })
  ],
  module: {

      {
        test: /\.css$/,
        use: [
          // 将style-loader替换为插件的loader
          // MiniCssExtractPlugin.loader, 可以直接这样写，这个loader直接是个字符串
          {
            loader: MiniCssExtractPlugin.loader
          },
          'css-loader' //主要解析@import这种路径
        ]
      }
    ]
  }
}
```

**其余mini-css-extract-plugin的配置信息详见：[mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)**



#### 2.5.4 压缩css与js

**要压缩抽离出来`mini-css-extract-plugin`的css文件就必须要打乱webpack默认的压缩js文件的规则（webpackl默认调用了一个压缩js的插件terser-webpack-plugin进行压缩），这时候压缩js文件需要直接引用插件来进行优化**

**注意：**要看到压缩效果需要改为生产环境观察，否则如果是开发环境将直接跳过优化项

```shell
#terser-webpack-plugin不需要安装，webpack本身就自带了
npm i optimize-css-assets-webpack-plugin -D
```

```js
//这三个插件进行组合可以打包压缩css和js
let MiniCssExtractPlugin = require('mini-css-extract-plugin')
let TerserJSPlugin = require('terser-webpack-plugin')
let OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin')
module.exports = {
  //优化代码属性
  optimization: {
    // 压缩代码的选项,传入压缩js与压缩css的插件
    minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})]
  },
  plugins: [
    /*
    注意,如果想要分为多个不同的css文件，那么就require引入多次插件，取不同的名字，然后都在这使用，通常这种方式是要区分各个预处理器转换后的文件
    */
    new MiniCssExtractPlugin({
      filename: 'main.css' //要抽离后的文件名
    })
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader
          },
          'css-loader'
        ]
      }
    ]
  }
}
```





#### 2.5.5 添加浏览器前缀

要为打包后的css文件自动兼容其余浏览器（也就是要在前面添加前缀），**需要引入postcss-loader与css优化插件autoprefixer**

```shell
#postcss-loader是专门优化css的loader,而autoprefixer只是其中的一个插件
npm i postcss-loader autoprefixer -D
```

```js
let MiniCssExtractPlugin = require('mini-css-extract-plugin')
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader
          },
          'css-loader',
          'postcss-loader'
          //postcss-loader自动为浏览器添加前缀，不过需要先处理这个loader再处理css-loader
        ]
      },
      //可以解析less文件
      {
        test: /\.less$/,
        use: [
          {
            loader: 'style-loader',
            options: {
              //改变样式插入的位置为head标签的顶部
              insertAt: 'top'
            }
          },
          'css-loader',
          'postcss-loader',//要处理less文件也是一样
          'less-loader'
        ]
      }
    ]
  }
}
```

**上面只是分配了postcss-loader，下面还需要处理postcss-loader对应的插件**，postcss-loader需要我们有一个`postcss.config.js`的文件来处理该loader

```js
// postcss.config.js`
module.exports = {
  plugins: [//导出postcss-loader需要的插件，该插件属性是一个数组，装有需要解析的插件
    require('autoprefixer')({ overrideBrowserslist: ['last 10 versions'] })
    // 引入并直接使用autoprefixer，方法的对象参数是对应要支持的浏览器版本，具体看文档
  ]
}
```

**其余postcss-loader的配置信息详见：[postcss-loader](https://github.com/postcss/postcss-loader)**



### 2.6 高级js转换ES5

**将高级的js转换为ES5都需要用到babel**

```shell
#需要使用babel-loader对高级语法进行转换，@babel/core 则是babel核心模块，调用transform方法进行转换
npm i babel-loader @babel/core -D
```

#### 2.6.1 ES6转换为ES5

将ES6转换为ES5还需要配置babel的转换模块`@babel/preset-env `，该模块将一些标准的js语法转换为低级的语法

```shell
#所以要ES6转换为ES5需要这三个
npm i babel-loader @babel/core @babel/preset-env -D
```

```js
let path = require('path')
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: 'babel-loader',
          options: {
            //用babel-loader需要把ES6转换为ES5,为loader添加预设选项，该选项就能解决狭义的ES6语法
            presets: ['@babel/preset-env']
          }
        },
        // 这里的js转换只包括src目录下的
        include: path.resolve(__dirname, 'src'),
        // 排除node_modules目录
        exclude: /node_modules/
      }
    ]
  }
}
```



#### 2.6.2 ES7转换为ES5

如果要兼容一些ES6以上的语法，需要按照对应语法解析的插件，如ES7的在类中constructor外直接写实例属性、使用装饰器修饰类

```js
@log
class A{
    a = 1
}
let a = new A()
console.log(a.a)
function log(target){
	console.log(target)
}
```

这种语法需要`@babel/plugin-proposal-class-properties`这个类的转换插件和`@@babel/plugin-proposal-decorators`这个装饰器插件

```shell
#解析ES7类的语法与装饰器语法
npm i @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators -D
```

```js
let path = require('path')
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: 'babel-loader',
          options: {
            //用babek-loader需要把ES6转换为ES5
            presets: ['@babel/preset-env'],
            // 预设是一个大插件的集合，在预设下我们还可以自己配置自己需要的小插件
            plugins: [
              //'@babel/plugin-proposal-class-properties', 如果只解析类可以直接写字符串这样写
              ['@babel/plugin-proposal-decorators', { legacy: true }],
              ['@babel/plugin-proposal-class-properties', { loose: true }]
              /*
              	上面如果要解析修饰类的装饰器必须使用这种顺序，也就是必须先解析class语法，否则会报错
              */
            ]
          }
        }
      }
    ]
  }
}
```



#### 2.6.3 js语法

**babel默认只会转换内置的高级的语法，但是不能转换该语法内置的api，就算转换了内置的api也不会内置转换的方法，**也就是说一些高级的js语法是由低级语法的api实现的，但是babel只是将该api转换了出来，但是低级的api本身不在代码中，**这时候还需要用到一个插件包`@babel/plugin-transform-runtime`，**该插件是一个开发时用的插件，但是，**如果要上线还会为生产后的代码中输出一些脚本，这时，还需要另外一个`@babel/runtime `在上线的时候为我们添加该插件（注意这个插件是要在生产环境使用的，因为我们是要转换到生产环境使用的，需要`--save`）**

```shell
npm i @babel/plugin-transform-runtime -D
```

```shell
npm i @babel/runtime -S
```

```js
let path = require('path')
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: [
              ['@babel/plugin-proposal-decorators', { legacy: true }],
              ['@babel/plugin-proposal-class-properties', { loose: true }],
              '@babel/plugin-transform-runtime'
            ]
          }
        }
      }
    ]
  }
}
```

但是，上面的方法不能够解决高级的实例方法，如`arr.includes()`，如果想解决这个问题，还需要一个`@babel/polyfill`模块，还是因为是要在生产环境中使用，并且该模块会在代码中使用，所有我们使用`--save`

```shell
npm i @babel/polyfill -S
```

```js
// index.js,程序主入口
require('@babel/polyfill')//通过在最上方引用就能转换后面所有的高级实例语法
```



#### 2.6.4 语法校验

语法校验在我们编程中其实很常见，使用ESLint就能帮我们校验代码，ESLint与css和js一样都需要配置想要的loader

```shell
npm i eslint eslint-loader -D
```

**eslint需要一个自己制定的校验代码的规则，需要创建一个`.eslintrc.json`的配置文件，下面是一个eslint项目的配置文件**

```json
{
    "parserOptions": {
        "ecmaVersion": 5,
        "sourceType": "script",
        "ecmaFeatures": {}
    }, 
    "rules": {
        "constructor-super": 2,
        "for-direction": 2,
        "getter-return": 2,
        "no-async-promise-executor": 2,
        "no-case-declarations": 2,
        "no-class-assign": 2,
        "no-compare-neg-zero": 2,
        "no-cond-assign": 2,
        "no-const-assign": 2,
        "no-constant-condition": 2,
        "no-control-regex": 2,
        "no-debugger": 2,
        "no-delete-var": 2,
        "no-dupe-args": 2,
        "no-dupe-class-members": 2,
        "no-dupe-keys": 2,
        "no-duplicate-case": 2,
        "no-empty": 2,
        "no-empty-character-class": 2,
        "no-empty-pattern": 2,
        "no-ex-assign": 2,
        "no-extra-boolean-cast": 2,
        "no-extra-semi": 2,
        "no-fallthrough": 2,
        "no-func-assign": 2,
        "no-global-assign": 2,
        "no-inner-declarations": 2,
        "no-invalid-regexp": 2,
        "no-irregular-whitespace": 2,
        "no-misleading-character-class": 2,
        "no-mixed-spaces-and-tabs": 2,
        "no-new-symbol": 2,
        "no-obj-calls": 2,
        "no-octal": 2,
        "no-prototype-builtins": 2,
        "no-redeclare": 2,
        "no-regex-spaces": 2,
        "no-self-assign": 2,
        "no-shadow-restricted-names": 2,
        "no-sparse-arrays": 2,
        "no-this-before-super": 2,
        "no-undef": 2,
        "no-unexpected-multiline": 2,
        "no-unreachable": 2,
        "no-unsafe-finally": 2,
        "no-unsafe-negation": 2,
        "no-unused-labels": 2,
        "no-unused-vars": 2,
        "no-useless-catch": 2,
        "no-useless-escape": 2,
        "no-with": 2,
        "require-atomic-updates": 2,
        "require-yield": 2,
        "use-isnan": 2,
        "valid-typeof": 2
    },
    "env": {}
}
```

```js
module.exports = {
  module: {
     rules: [
      {
        test: /.js$/,
        use: {
          loader: 'eslint-loader', 
          //我们应该先校验js再转换为ES5代码，所以正常来说这个应该写在下面，也就是要先执行
          options: {
            enforce: 'pre' //previous，强制让这个loader在最先执行，还可以写post让这个loader强制最后执行
          }
        },
        exclude: /node_modules/ //不检验node_modules中的代码
      },
      //相同的loader可以写多个，符合从上到下，从左到右的默认规则
      {
        test: /\.js$/,
        use: {
          loader: 'babel-loader', //普通的loader
          options: {
            presets: ['@babel/preset-env'],
            plugins: [
              ['@babel/plugin-proposal-decorators', { legacy: true }],
              ['@babel/plugin-proposal-class-properties', { loose: true }],
              '@babel/plugin-transform-runtime'
            ]
          }
        },
        include: path.resolve(__dirname, 'src'),
        exclude: /node_modules/
      }
    ]
  }
}
```



### 2.7 全局变量引入

**在webpack中引入全局变量主要有四种形式：**

- 直接引入全局变量后使用window来接收全局变量
- 使用`expose-loader`将全局变量暴露给window对象和模块中，也就是说**使用这个在所有模块都可以使用`window.暴露对象`或直接使用暴露对象**
- 使用`webpack.providePlugin`插件为每个模块注入全局对象（此时并没有挂载到window上，只是为每个模块注入了全局变量）
- 在模板html中通过script标签引入

**注：**这里的全局变量都以jquery的$符合为例

```shell
npm i jquery -S
```

#### 2.7.1 直接接收

```js
// index.js
import $ from 'jquery'
window.$ = $
conosle.log(window.$)
```



#### 2.7.2 使用expose-loader

expose-loader可以直接写在项目代码中，也可以写在webpack的配置项中，因为这种特性，这类loader又被叫做内联loader

**注：**配置了这个在所有的模块都能使用

```shell
npm i expose-loader -D
```

- **内联使用**

  ```js
  // index.js,直接在项目入口引入全局变量
  import 'expose-loader?$!jquery' // 不需要使用import from 语法，因为不需要获取到暴露的对象
  /*
  是通过expose-loader的类似查询字符串的写法，全局暴露jquery为$符号，可以使用window.$或直接使用$来获取jquery对象
  */
  console.log($)
  console.log(window.$)
  ```

- **在webpack配置文件中添加**

  ```js
  module.exports = {
    module: {
       rules: [
  		{
          //配置loader,这种loader的验证规则是只要在模块中引用的jquery就会对应上这个loader
          test: require.resolve('jquery'),
          use: 'expose-loader?$' //然后使用expose-loader将引入的jquery对象变为$在全局使用
        	},
      ]
    }
  }
  ```

  ```js
  // index.js
  import 'jquery'
  console.log($)
  console.log(window.$)
  ```



#### 2.7.3 在每个模块中注入

如果我们要使用很多全局变量，而又不想自己配，又不想暴露给window对象造成污染，可以通过webpack插件自动为每一个模块注入该全局变量

```js
//webpack也是一个模块
let webpack = require('webpack')
module.exports = {
  plugins: [
    new webpack.ProvidePlugin({
      $: 'jquery' // 在每个模块中将会jquery的对象都注入为$符,这个jquery是在node_modules中获取的
    })
  ]
}
```

```js
// index.js
// 现在在文件中不需要进行引入就能使用了，不过不会暴露给window对象
console.log($)
console.log(window.$) // undefined
```



#### 2.7.4 在script标签引用

**通过script标签引用的对象会自动挂载到window对象上，也可以直接在模块内部使用**

但是，在模板html中通过`script`标签引入`jquery`, 但是在`js`中，如果又想再次引入是用`import`引用一次jquery,会重新打包`jquery`,而我们所想的应该是如果使用了script标签引用了的全局变量应该不被打包，所以我们可以使用webpack提供的`externals`功能，**进行变量挂载，从而使用index.html中引入的cdn库，避免了打包时将复杂的第三方库打包**

```js
// 我们不希望通过这样使用
const $ = window.$
// 我们只希望这样引用
const $ = require('jquery')
```

```js
module.exports = {
	externals: {// 下面这种externals无论在哪使用jquery都会匹配，并且使用$才能够生效
     'jquery': '$' //将jquery库中的对象赋给$，如果没用script标签引入都会报错
  }
}
```

```js
// index.js
import $ from 'jquery' //引入不打包，所以这段代码没有实际意义，只是给编译器看的
console.log($)// 如果没有用script标签引入就会报错，上面的代码没有意义
```



### 2.8 图片处理

一般来说有三种方式创建图片：

- 使用H5的API在JS中创建图片
- 在css中使用background-image导入背景图片
- 在img标签中引用图片

在webpack中使用图片时，因为是要形成打包后的文件，所以路径就会有问题，一般通过require直接引入图片，这时会创建一个存在内存中的新的图片地址，通过这种方式才能在打包后使用该图片，不过引入图片就要配置与引入静态文件相关的loader

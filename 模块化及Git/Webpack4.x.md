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



#### 2.2.2 实时打包

如果想要webpack监听工作区中文件的改变而实时的打包出新的代码，可以使用webpack自带的watch选项

```js
// webpack.config.js
module.exports = {
    watch: true,
    // 监听的配置项
    watchOptions: {
        poll: 1000 // 每秒询问100次
        aggregateTimeout: 500, //防抖
        ignored: /node_modules/ //忽略文件
    }
}
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
        collapseWhitespace: true, // 折叠空行将所有代码变成一行
        removeComments: true // 移除注释
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
            // 创建style标签，将js的样式资源插入进行，添加到head中生效
            loader: 'style-loader',
            options: {
              //改变样式插入的位置为head标签的顶部
              insertAt: 'top'
            }
          },
           // 将css变成commonjs模块加入到js中，里面的内容是字符串
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

默认情况下样式文件是放在js中的，如果想要将添加到html模板中的css样式单独作为一个文件抽离出来，自动通过link标签进行引入，还需要引入专门抽离css样式的插件`mini-css-extract-plugin`

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
      filename: 'css/main.css' //要抽离后在打包后目录中的文件名
    })
  ],
  module: {
      {
        test: /\.css$/,
        use: [
          // 将style-loader替换为插件的loader，因为style-loader是将js中的css变成style标签插入html中
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

- **通过改变压缩项**

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

- **通过插件引入**

  ```js
  let MiniCssExtractPlugin = require('mini-css-extract-plugin')
  let OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin')
  module.exports = {
    plugins: [
      /*
      注意,如果想要分为多个不同的css文件，那么就require引入多次插件，取不同的名字，然后都在这使用，通常这种方式是要区分各个预处理器转换后的文件
      */
      new MiniCssExtractPlugin({
        filename: 'main.css' //要抽离后的文件名
      }),
      // 压缩css
      new OptimizeCSSAssetsPlugin()
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

一般来说处理css相关的文件以及转换，我们都会使用`postcss`一系列的插件

- **使用postcss-loader与postcss-preset-env**

  ```shell
  npm i postcss-loader postcss-preset-env -D
  ```

  ```js
  let MiniCssExtractPlugin = require('mini-css-extract-plugin')
  // postcss默认会找browserslist中的production的配置项，如果想要使用开发环境的配置项，需要设置NODE_ENV
  process.env.NODE_ENV = 'development'
  
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
                      {
                          // 帮postcss找到package.json中的browserslist里面的配置来加载执行兼容样式
                          loader: 'postcss-loader',
                          options: {
                              ident: 'postcss',
                              plugins: () => {
                                  //postcss插件
                                  require('postcss-preset-env')()
                              }
                          }
                      }
                  ]
              }
          ]
      }
  }
  ```

  ```json
  {
      "browserslist": {
          "development": [
              "last 1 chrome version",
              "last 1 firefox version",
              "last 1 safari version",
          ],
          "production": [
              ">0.2%",
              "not dead",
              "not op_mino all"
          ]
      }
  }
  ```

  

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

#### 2.6.1 基本转换

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
            presets: ['@babel/preset-env'],
            cacheDirectory: true // 开启babel缓存，第二次构建时会读取之前的缓存
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

**注意：**只是用`@babel/preset-env`只能转换基本的ES6语法，很多高级api是不能转换的，比如Promise



#### 2.6.2 全部转换

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

但是，上面的方法不能够解决高级的实例方法，如`arr.includes()`。

如果想解决这个问题，只需要一个`@babel/polyfill`模块（可以不使用`@babel/plugin-transform-runtime`了），还是因为是要在生产环境中使用，并且该模块会在代码中使用，所有我们使用`--save`

**使用`@babel/polyfill`将ES6的语法全部兼容**

```shell
# 该插件不是在webpack中配置，而是在入口js文件前直接引入即可
npm i @babel/polyfill -S
```

```js
// index.js
import '@babel/polyfill'
```

**使用问题：**该方法会将所有的高级js语法全部转换，会造成项目体积过大的问题，如果只想解决部分兼容性问题并不推荐这样做



#### 2.6.3 按需转换

因为使用`@babel/polyfill`兼容所有ES6语法会有项目体积过大的问题，我们可以使用按需加载的方法来处理，使用`core-js`插件

```shell
npm i core-js -D
```

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            '@babel/preset-env',
                            {
                                // 按需加载
                                useBuiltIns: 'usage',
                                // 指定core-js版本
                                corejs: {
                                    version: 3
                                },
                                // 指定兼容哪个版本浏览器
                                targets: {
                                    chrome: '60',
                                    firefox: '60',
                                    ie: '9',
                                    safari: '10',
                                    edge: '17',
                                }
                            }
                        ]
                    }
                }
            }
        ]
    }
}
```





#### 2.6.4 ES7转换为ES5

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



#### 2.6.5 语法校验

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
            fix: true,//自动修复
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



##### 2.6.5.1 使用airbnb风格

airbnb是一个非常有名的js风格语法，我们可以使用eslint来让我们书写的代码符合这种规范

```shell
npm i eslint-loader eslint eslint-config-airhnb-base eslint-plugin-import-eslint -D
```

```shell
module.exports = {
	rules: [
		{
			test: /\.js$/,
			exclude: /node_modules/,
			loader: 'eslint-loader',
			options: {
				fix: true
			}
		}
	]
}
```

然后需要在`package.json`中增加：

```json
{
    "eslintConfig": {
        "extend": "airbnb-base",
        "env": {
            "browser": true // 支持浏览器的全局变量
        }
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
     'jquery': '$' //将jquery库中的对象赋给全局的$变量，如果没用script标签引入都会报错
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

在webpack中使用图片时，因为是要形成打包后的文件，所以路径就会有问题，一般通过require直接引入图片，这时会创建一个存在内存中的新的图片地址，通过这种方式才能在打包后使用该图片，不过引入图片就要配置与引入静态文件相关的loader，我们这里使用url-loader和file-loader（**只对于在JS中引入图片和在js中引入了css，css又引入了背景图片的行为**）。实际使用url-loader，不过url-loader依赖于file-loader

#### 2.8.1 在不同环境下使用

```shell
npm i url-loader file-loader -D
```

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.(png|jpg|gif)$/,
                // 当图片小于多少，用base64,否则用url-loader产生真实的图片
                /*
                	优点：减少请求数量（减轻服务器压力）
                	缺点：图片体积会更大（文件请求速度更慢）
                */
                use: {
                    loader: 'url-loader',
                    options: {
                        // 用作限制
                        limit: 8 * 1024,  // 8kb之内用base64
                    }
                }
            }
        ]
    }
}
```

flie-lodaer默认会在内部生成一张图片到build的目录下，并且把生成图片的名字返回出来

- 第一种情况: 图片地址要`import`引入，直接写图片的地址，会默认为字符串

  ```js
  import logo from './logo.png'
  
  console.log(logo)
  let image = new Image()
  
  image.src = logo
  
  document.body.appendChild(image)
  ```

- 第二种情况: `css-loader`会将`css`里面的图片转为`require`的格式

    ```css
    div {
        background: url("./logo.png"); /* require("./logo.png") */
    }
    ```

- 第三种情况: 解析`html`中的`image`，需要使用loader

    - 使用`html-withimg-loader`
    
        ```shell
        npm i html-withimg-loader -D
        ```

        ```js
        module.exports = {
            module: {
                rules: [
                    {
                        test: /\.html$/,
                        // 处理html的img图片（负责引入img,从而能被url-loader进行处理）
                        use: 'html-withimg-loader'
                    }
                ]
            }
        }
        ```
        
    - 使用`html-loader`
    
        ```shell
        npm i html-loader -D
        ```
    
        ```js
        module.exports = {
            module: {
                rules: [
                    {
                        test: /\.(png|jpg|gif)$/,
                        use: {
                            loader: 'url-loader',
                            options: {
                                limit: 8 * 1024,
                                // 使用html-loader时需要将url的esModule关闭，因为url-loader默认是使用es6模块解析，而html-loader使用的commonjs，解析时会出现[Object Module]
                                esModule:false
                            }
                        }
                    },
                    {
                        test: /\.html$/,
                        // 处理html的img图片（负责引入img,从而能被url-loader进行处理）
                        use: 'html-loader'
                    }
                ]
            }
        }
        ```
    
        
    



#### 2.8.2 打包文件分类

如果我们先想要把文件资源分类处理，添加配置项，webpack也会自动的加打包后的路径转换

 ```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.(png|jpg|gif)$/,
                // 当图片小于多少，用base64,否则用file-loader产生真实的图片
                use: {
                    loader: 'url-loader',
                    options: {
                        // 用作限制
                        limit: 1,  // 200k 200 * 1024
                         outputPath: 'img/'   // 打包后输出地址 在dist/img
                    }
                }
            }
        ]
    }
}
 ```

同时，在我们前面的CSS插件中，也可以直接写入要分类的目录

```js
module.exports = {
    plugins: [
        new MiniCssExtractPlugin({
            filename: 'css/main.css'
        }),
    ]
}
```



#### 2.8.3 加入CDN前缀

如果我们希望输出的时候，给这些`css\img`加上前缀，传到服务器也能访问，可以为`output`加上`publicPath`属性

```js
module.exports = {
    output: {
        filename: 'bundle.[hash:8].js',   // hash: 8只显示8位
        path: path.resolve(__dirname, 'dist'),
        publicPath: 'http://www.mayufo.cn'  // 给静态资源统一加
    },
}
```

如果我们只希望处理某一类文件，比如图片：

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.(png|jpg|gif)$/,
                // 当图片小于多少，用base64,否则用file-loader产生真实的图片
                use: {
                    loader: 'url-loader',
                    options: {
                        limit: 1,  // 200k 200 * 1024
                        outputPath: '/img/',   // 打包后输出地址
                        publicPath: 'http://www.baidu.com'
                    }
                }
            }
        ]
    }
}
```



### 2.9 打包其他资源

除了直接指定需要打包的资源外，我们还可以通过直接使用`exclude`选项来打包其他资源

```js
module.exports = {
    module: {
        rules: [
            {
                exclude: /\.(css|js|html0)$/,// 假设只有这三种资源为主要资源
                loader: 'file-lodaer',
                options: {
				name:'[hash:10].[ext]'
                }
            }
        ]
    }
}
```



## 3.webpack性能优化

### 3.1 HMR

hmr（hot module replacement）意为热模板替换，能够在一个模块发生变化时值重新打包这个模块，而不是打包所有模块，极大的提高构建速度，一般用于开发环境使用。

- 样式文件：因为style-loader内部实现了hmr功能，所以默认可以用使用hmr功能（所以开发环境不需要将样式文件打包）

- **js文件：**默认不支持hmr功能

  ```js
  // index.js
  if(module.hot){
      // 一旦module.hot为true,说明开启了hmr功能
      module.hot.accept('./log.js',()=>{
          // 方法会监听 log.js文件的变化，一旦变化就触发回调函数，其他文件就不会被重新打包，否者就会打包所有的文件
          // dosomething
      })
  }
  ```

  

  **注意：**hmr功能js的处理，只能处理非入口js文件的其他文件。

- **html文件：**默认不支持hmr功能，同时html文件是默认是不能热更新的。

  **注：**因为项目中一般都只有一个html文件，所以我们是不需要为html文件做hmr的，改变html文件项目就一定要重新打包。

  **解决热更新：**修改entry入口，将html文件引入，这样每次修改html文件的时候都会重新打包项目。

  ```js
  // 多入口
  let path = require('path')
  let HtmlWebpackPlugin = require('html-webpack-plugin')
  
  module.exports = {
      entry: ['./src/index.js','/public/index.htnl']
  }
  ```



### 3.2 source-map

**source-map是一种提供源代码到构建后代码映射技术（如果代码出错了，通过映射可以追踪代码错误）**

```js
module.exports = {
    mode: 'development',
	devtool:'source-map'    
}
```

可选值有`[inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map`

大体有以下大类，还有一些是通过下面的大类组合而来的：

- **source-map：**外部，能够提示出错误代码的准确信息和源代码的错误位置
- **inline-source-map：**内联，只生成一个内联source-map，能够提示出错误代码的准确信息和源代码的错误位置
- **hidden-source-map：**外部source-map，能够提示错误代码的错误原因，但是没有错误位置，不能追踪到源代码错误，只能提示到构建后代码的错误位置（只隐藏源代码，不隐藏构建后的代码）
- **eval-source-map：**内联，但每一个文件都会生成对应的source-map在eval中，能够提示出错误代码的准确信息和源代码的错误位置
- **nosource-source-map：**外部，能找到错误代码的准确信息，但是没有任何源代码信息（隐藏所有代码）
- **cheap-source-map：**外部，能够提示出错误代码的准确信息和源代码的错误位置，但是只能精确到某一行，不能找到某一行中的哪一个错误
- **cheap-module-source-map：**外部，能够提示出错误代码的准确信息和源代码的错误位置



**内联与外部的区别**

- 外部生成了对应的文件，而内联没有
- 内联的构建速度更快



**两种环境的选择**

- **开发环境：**需要速度快，调试更友好。

  - **速度快：**由于`eval>inline>cheap>...`，所以可以选择`eval-cheap-source-map`和`eval-source-map`
  - **调试更友好：**可以选择`source-map`、`cheap-module-source-map`和`cheap-source-map`

  **综上：**一般可以选择`eval-source-map`（Vue和React脚手架默认就是这种）或者`eval-cheap-module-source-map`

- **生产环境：**需要考虑是否隐藏源代码，是否需要调试友好。同时要注意的是生产环境不要用内联，这样会使得文件体积增大

  - **调试友好：**可以使用`source-map`和`cheap-module-source-map`
  - **需要隐藏源代码：**可以使用`nosource-source-map`和`hidden-source-map`



### 3.3 oneof

在之前我们配置loader的时候，其实每一个文件都会对每一个loader进行对比适配，不管是否能匹配上，这样做会很消耗性能，而oneof就可以帮助我们提高性能，它作用类似于switch，在匹配上一个之后就不会继续往下面匹配了。

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const optimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')

process.env.NODE_ENV = process.env.NODE_ENV
    ? process.env.NODE_ENV
: 'development'

const isDev = process.env.NODE_ENV === 'development' ? true :false

// 复用loader
const commonCSSLoader = [
    MiniCssExtractPlugin.loader,
    'css-loader',
    {
        loader: 'postcss-loader',
        options: {
            ident: 'postcss',
            plugins: () => {
                require('postcss-preset-env')
            }
        }
    }
]

module.exports = {
    module: {
        rules: [
            // loader
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'eslint-loader',
                // 优先执行
                enforce: 'pre',
                /*
          package.json里面写上"eslintConfig":{
            "extends":'airbnb-base"
          }
        */
                options: {
                    fix: true
                }
            },
            {
                // 但是需要注意不能同时有两个配置匹配同一类型的文件，所有拿出一个js配置到外面
                oneOf: [
                    {
                        test: /\.less$/,
                        use: [
                            ...(isDev ? ['style-loader', 'css-loader'] : commonCSSLoader),
                            'less-loader'
                        ]
                    },
                    {
                        test: /\.css$/,
                        use: [...(isDev ? ['style-loader', 'css-loader'] : commonCSSLoader)]
                    },
                    {
                        test: /\.js$/,
                        exclude: /node_modules/,
                        loader: 'babel-loader',
                        options: {
                            presets: [
                                '@babel/preset-env',
                                {
                                    // 按需加载
                                    useBuiltIns: 'usage',
                                    // 指定core-js版本
                                    corejs: {
                                        version: 3
                                    },
                                    // 指定兼容哪个版本浏览器
                                    targets: {
                                        chrome: '60',
                                        firefox: '60',
                                        ie: '9',
                                        safari: '10',
                                        edge: '17'
                                    }
                                }
                            ]
                        }
                    },
                    {
                        test: /\.(jpg|png|gif)$/,
                        loader: 'url-loader',
                        options: {
                            limit: 8 * 1024,
                            name: '[hash:10].[ext]',
                            esModule: false,
                            outputPath: 'imgs'
                        }
                    },
                    {
                        // html资源
                        test: /\.html$/,
                        loader: 'html-loader'
                    },
                    {
                        exclude: /\.(html|css|less|js|jpg|png|gif)$/,
                        loader: 'file-loader',
                        options: {
                            name: '[hash:10].[ext]',
                            outputPath: 'media'
                        }
                    }
                ]
            }
        ]
    }
}
```



### 3.4 缓存

为了使得项目性能更加优化，很多地方都会使用缓存，但是有些时候缓存不能起到预定的效果，需要根据实际来使用缓存

- **babel缓存：**

  之前在处理ES6的时候写到过，它内置可以开启`cacheDirectory:true`，让第二次打包构建速度更快

- **文件资源缓存：**

  该缓存是默认存在于webpack打包中的，但是这种缓存在很多时候会让我们想要修改的数据无效，需要我们自己为打包后的文件设置哈希值来清除缓存。文件资源缓存的作用是让代码上线运行缓存更好使用

  + **hash：**每次webpack构建时会生成一个唯一的hash值。

    **问题：**因为js和css同时使用一个hash值，如果重新打包，会导致所有缓存失效，但是用户却只想要修改一个文件

  + **chunkhash：**根据chunk生成的hash值，如果打包来源于同一个chunk，那么hash值就一样

    **问题：**js和css的hash值还是一样的，因为css是在js中引入，所以属于同一个chunk

  + **contenthash：**根据文件的内容生成hash值，不同文件hash值一定不相同



### 3.5 tree shaking

`tree shaking`的作用是去除无用的代码，减少代码的体积。

但是使用`tree shaking`需要有两个前提：

- 必须使用ES6模块化
- 需要开启production环境

**注意：**有些时候我们开启`tree shaking`时可能会把我们项目中的`css、@babel/polyfill`文件消除掉

在`package.json`中：

```json
{
    "sideEffects":false // 当开启为false时会让所有代码都没有副作用，tree shaking就会将css文件一起删除掉
}
```

```json
{
    "sideEffects":["*.css", "*.less"] // 我们最好手动配置清除
}
```



### 3.6 代码分割

#### 3.6.1 打包多页应用

打包多页应用的方式很简单，使用多个入口和多个模板就能完成多个页面的打包

```js
// 多入口
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    mode: 'development',
    entry: {
        home: './src/index.js',
        other: './src/other.js'
    },
    output: {
        // 使用[name]会让各在入口文件输出各自的出口文件
        filename: "[name].js",
        path: path.resolve(__dirname, 'dist2')
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './index.html',
            filename: 'home.html',
            // 模板要引入哪些入口文件
            chunks: ['home']
        }),
        new HtmlWebpackPlugin({
            template: './index.html',
            filename: 'other.html',
            chunks: ['other', 'home']   // other.html 里面有 other.js & home.js
        }),
    ]
}
```



#### 3.6.2 配置node_modules的chunk

```js
// 多入口
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    mode: 'production',
    entry: {
        home: './src/index.js',
        other: './src/other.js'
    },
    output: {
        // 使用[name]会让各在入口文件输出各自的出口文件
        filename: "[name].js",
        path: path.resolve(__dirname, 'dist2')
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './index.html',
           	minify: {
                collapseWhitespace: true,
                removeComments: true
            }
        })
    ],
    /*
    	1.如果是单文件，可以自动将node_modules中代码单独打包一个chunk最终输出
    	2.自动分析多入口chunk中有没有公共文件，如果有会单独打包成一个chunk用来共用
    */
    optimizaion: {
        splitChunks: {
            chunks: 'all'
        }
    }
}
```



#### 3.6.3 js代码打包

通过js代码的注释效果能够将webpack的打包机制将相同chunk名字的代码打包在一起

```js
/*
	通过js代码，让某个文件单独打包为一个chunk：
	使用import动态导入的语法，能让一个文件单独被打包，但是默认是随机的id，可以在前面加上注释生成想要的名字
*/
import(/* webpackChunkName: 'test' */).then(res => {
    console.log(res)
}).catch(() => {
    console.log('文件加载失败')
})
```



### 3.7 懒加载与预加载

- **懒加载：**js中使用懒加载其实非常简单，我们只需要使用`import()`这样的语法通过`.then()`的方式获取回调就能够实现这样的效果。同时该方法也不会重复加载模块，第二次使用会引用缓存中的。

    ```js
    document.getElementById('btn').onclick = function() {
        import(/* webpackChunkName: 'test' */).then(res => {
            console.log(res)
        })
    }
    ```
    
- **预加载：**预加载（prefetch）会在使用之前，提前加载js文件，正常的加载可以认为是并行加载（同一时间加载多个文件），而预加载是等其他资源加载完成之后，浏览器空闲时偷偷进行加载资源。

    ```js
    document.getElementById('btn').onclick = function() {
        // 使用webpackPrefetch开启预加载
        import(/* webpackChunkName: 'test', webpackPrefetch: true */).then(res => {
            console.log(res)
        })
    }
    ```



### 3.8 PWA（离线可访问）

PWA（渐进式网络开发应用程序）可以为我们提供即使离线状态下浏览器也可以访问网站的能力。

要在webpack中启用PWA，我们需要下载`workbox-webpack-plugin`作为插件使用

```shell
npm i workbox-webpack-plugin -D
```

```js
const workboxWebpackPlugin = require('workbox-webpack-plugin ')
module.exports = {
    plugins: [
        /*
        	该插件最终会帮我们生成一个serviceworker配置文件
        	下面两个配置的作用：
        	1.帮助serviceworker快速启动
        	2.删除旧的serviceworker
        */
        new workboxWebpackPlugin({
            clientClaim: true,
            skipWaiting: true
        })
    ]
}
```

```js
// index.js
/*
	注册serviceworker
	处理兼容性问题
*/
if('serviceWorker' in navigator){
    window.addEventListener('load', () => {
		navigator.serviceWorker.register('/service-worker.js')
        .then(()=>{
            console.log('注册成功')
        })
        .catch(()=>{
            console.log('注册失败')
        })
    })
}
```



### 3.9 多进程打包

webpack中启用多进程打包需要使用`thread-loader`

**注意：**开启多进程打包不一定会让打包速度更加快，因为进程启动和通信之间也有时间开销，只有当工作消耗时间比较长，比如有很多js代码的时候，才需要多进程打包

```shell
npm i thread-lodaer -D
```

```js
module.exports = {
    module: {
        rules: [
            {
                // 但是需要注意不能同时有两个配置匹配同一类型的文件，所有拿出一个js配置到外面
                oneOf: [
                    {
                        test: /\.js$/,
                        exclude: /node_modules/,
                        use: [
                            // 一般多进程打包都是和babel一起使用
                            {
                                loader: 'thread-loader',
                                options: {
                                    workers: 2 // 两个进程
                                }
                            },
                            {
                                loader: 'babel-loader',
                                options: {
                                    presets: [
                                        '@babel/preset-env',
                                        {
                                            // 按需加载
                                            useBuiltIns: 'usage',
                                            // 指定core-js版本
                                            corejs: {
                                                version: 3
                                            },
                                            // 指定兼容哪个版本浏览器
                                            targets: {
                                                chrome: '60',
                                                firefox: '60',
                                                ie: '9',
                                                safari: '10',
                                                edge: '17'
                                            }
                                        }
                                    ]，
                                    cacheDirectory: true // 开启babel缓存，第二次构建时会读取之前的缓存
                                }
                            }
                        ]
                    }
                ]
            }
        ]
    }
}
```



### 3.10 externals

在前面也说到过，当我们引入全局的库的时候我们可能会使用externals，使用externals可以让webpack强制不打包内部指定的包，要使用的时候直接从script标签获取，起到减少打包体积的作用。



### 3.11 dll

**使用dll技术，对某些库（第三方库：jquery、react、vue...）等进行单独打包。**dll已经由webpack进行集成过了

```js
// webpack.dll.js
/*
    该文件名为webpack.dll.js，所以在运行打包时是 webpack --config webpack.dll.js 来打包外部库
*/
const { resolve } = require('path')
const webpack = require('webpack')

module.exports = {
  entry: {
    // 最终打包生成的[name] --> jquery
    // ['jquery'] --> 要打包生成的库叫jquery
    jquery: ['jquery']
  },
  output: {
    filename: '[name].js',
    path: resolve(__dirname, 'dll'),
    library: '[name]_[hash]' // 打包的库里面向外面暴露出去的内容叫什么名字，相当于就是全局变量名
  },
  plugins: [
    new webpack.DllPlugin({
      name: '[name]_[hash]', // 映射库的暴露的内容名称
      path: resolve(__dirname, 'dll') // 输出的文件路径
    })
  ],
  mode: 'production'
}
```

```js
// webpack.config.js
const { resolve } = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const AddAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin')
const webpack = require('webpack')

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'built.js',
    path: resolve(__dirname, 'build')
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    }),
    // 告诉webpack哪些库不参与打包，同时使用名称也会改变
    new webpack.DllReferencePlugin({
      // 通过映射来寻找
      manifest: resolve(__dirname, 'dll/manifest.json')
    }),
    // 将某个文件打包出去，比你html资源中引入
    new AddAssetHtmlWebpackPlugin({
      filename: resolve(__dirname, 'dll/jquery.js')
    })
  ],
  mode: 'development'
}
```



## 4.配置详解

### 4.1 entry

entry是webpack打包的入口起点 ，该入口可以使用多种形式打包：

- **string：**单入口，打包形成一个chunk，输出一个bundle文件，此时chunk的名称默认是main

  ```js
  module.exports = {
      entry: './src/index.js'
  }
  ```

- **[]:string：**多入口，所有入口文件最终会形成一个chunk，输出一个bundle文件，一般可以用来处理hmr中的html热更新的问题

  ```js
  module.exports = {
      entry: ['./src/index.js', './src/home.js']
  }
  ```

- **object：**多入口，有几个入口文件就形成几个chunk，会输出多个bundle文件，此时chunk的名称为key

  ```js
  module.exports = {
      // 下面的代码最终会行2个bundle文件
      entry: {
          // 多入口，所有入口文件最终会形成一个chunk，输出一个bundle文件，文件名为index
          index: ['./src/index.js', './src/home.js'],
          // 形成一个chunk，输出一个bundle
          add: ' ./src/add.js'
      }
  }
  ```

  一般这种情况可以让我们单独打包第三方库：

  ```js
  module.exports = {
      entry: {
          jquery: ['jquery'],
          react: ['react', 'react-dom', 'react-router-dom']
      }
  }
  ```




### 4.2 output

output是webpack打包的出口

```js
const { resolve } = require('path')
module.exports = {
    entry: './src/index.js'
    output: {
        // 文件名称（指定名称+目录）
        filename: 'js/[name].js',
        // 输出文件目录（将来所有资源输出的公共目录）
        path: resolve(__dirname, 'build'),
        // 所有资源引入公共路径，如将前面我们写的js/[name].js，在引入的时候会成为/js/[name].js
        publicPath: '/',
        chunkFilename: 'js/[name]_chunk.js', // 非入口chunk的名称
        library: '[name]', // 向外暴露库的变量名
        libraryTarget: 'window' // 变量名添加到那个对象下（不填就为一个全局变量的形式）
        /*
        如：
        	browser：window
        	node：global
        	commonjs模块内部引入：commonjs
        */
    }
}
```



### 4.3 module

```js
const { resolve } = require('path')
module.exports = {
    module: {
		rules: [
         	// loader配置   
            {
                test: /\.css$/,
                // 多个loader使用use
                use: ['style-loader','css-loader']
            },
            {
                test: /\.js$/,
                // 排除node_modules下的js文件
                exclude: /node_modules/,
                // 只检查src下的js文件
                include: resolve(__dirname, 'src'),
                // 优先执行
                enforce: 'pre',
                // 延后执行
                // enforce: 'post'
                loader: 'eslint-loader', // 单个loader使用loader，可以配置options
                options:{}
            }
        ]
    }
}
```



### 4.4 resolve

resolve用于配置webpack解析模块规则

```js
const { resolve } = require('path')
module.exports = {
    // 解析模块的规则
    resolve: {
        // 配置解析模块路径别名。优点：简写路径。缺点：没有路径智能提示。（可以使用ts或编译器配置）
        alias: {
            $css: resolve(__dirname, 'src/css')
        }.
        // 配置省略文件路径的后缀名，按照数组顺序依次寻找
        extensions: ['.js', '.jsx', '.json', '.css']
    	// 告诉webpack解析模块是去哪个目录寻找
    	modules: [resolve(__dirname, 'node_modules'), 'node_modules']
    }
}
```



### 4.5 devServer

devServer专门用来配置webpack开发环境的服务配置

```js
const { resolve } = require('path')
module.exports = {
    mode: 'development',
    devServer: {
        // 运行代码的目录
        contentBase: resolve(__dirname, 'build'),
        // 监视contentBase目录下的所有文件，一旦文件发生变化就会reload
        watchContentBase: true,
        watchOptions: {
          // 忽略文件
            ignored: /node_modules/
        },
        // 启动gzip压缩
        compress: true,
        // 端口号
        port: 3000,
        // 域名
        host: 'localhost',
        // 自动打开浏览器
        open: true,
        // 开启hmr功能
        hot: true,
        // 不需要显示启动服务器日志信息
        clientLogLevel: 'none',
        // 除了一些基本启动信息以为，不打印其他内容
        quiet: true,
        // 如果出错了，不需要全屏提示
        overlay: false,
        // 服务器代理
        proxy: {
            '/api': {
                // 代理地址
                target: 'http://localhost:3000',
                // 路径重写
                pathRewrite: {
                    '^/api': ''
                }
            }
        }
    }
}
```



### 4.6 optimization

optimization专门用来处理webpack打包时性能优化的部分

```js
let TerserWebpackPlugin = require('terser-webpack-plugin')
module.exports = {
    mode: 'production',
    optimization: {
        // 处理公共代码打包
        splitChunks: {
            chunks: 'all'
            // 下面的配置都是默认值
            /*
          minSize: 30 * 1024, // 分割的chunk最小为30kb
          maxSize: 0, // 最大没有限制
          minChunks: 1, // 要提取的chunk最少被引用一次
          maxAsyncRequests: 5, // 按需加载时并行加载文件的最大数量
          maxInitialRequests: 3, // 入口js最大并行请求数量
          name: true, // 可以使用命名规则，开启后下面的命名相关的声明规则就可以生效
          automaticNameDelimiter: '~', // 名称连接符，打包后文件名以~连接
          cacheGroups: { // 分割chunk的组
              // 下面的键是分割后组的名称
              // node_modules文件会被打包到vendors组的chunk中，生成vendors~xxx.js
              // 同时所有分组均满足上面所写的所有规则，如果在下面重复写了会覆盖掉上面的
              vendors: {
                  // 只分割node_modules中的
                  test: /[\\/]node_modules[\\/]/,
                  // 优先级，冲突时会根据优先级大小打包
                  priority: -10
              },
              default: {
                  // 要提取的chunk最少被引用2次
                  minChunks: 2,
                  priority: -20,
                  // 如果当前被打包的模块和已被提出的模块是同一个，就不会重复打包
                  reuseExistingChunk: true
              }
          }
              */
        },
        // 将当前模块的记录其他模块的hash单独打包为一个文件runtime，用于解决修改a文件时导致b文件contenthash变化所产生的影响
        runtimeChunk: {
            name: entrypoint => `runtime-${entrypoint}`
        },
        minimizer: [
            // 配置生产环境的压缩方案：js和css
            /*
            	在webpack 4.2.6之后使用的是terser-webpack-plugin进行代码压缩
            */
            new TerserWebpackPlugin({
                // 开启缓存
                cache: true,
                // 开启多进程打包
                parallel: true,
                // 启动source-map
                sourceMap: true
            })
        ]
    }
}
```



## 4.webpack配置模板

### 4.1 基本模板

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const optimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')

process.env.NODE_ENV = process.env.NODE_ENV
  ? process.env.NODE_ENV
  : 'development'

const isDev = process.env.NODE_ENV === 'development' ? true : false

// 复用loader
const commonCSSLoader = [
  MiniCssExtractPlugin.loader,
  'css-loader',
  {
    loader: 'postcss-loader',
    options: {
      ident: 'postcss',
      plugins: () => {
        require('postcss-preset-env')
      }
    }
  }
]

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'js/built.js',
    path: path.resolve(__dirname, 'build')
  },
  module: {
    rules: [
      // loader
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'eslint-loader',
        // 优先执行
        enforce: 'pre',
        /*
          package.json里面写上"eslintConfig":{
            "extends":'airbnb-base"
          }
        */
        options: {
          fix: true
        }
      },
      {
        // 但是需要注意不能同时有两个配置匹配同一类型的文件，所有拿出一个js配置到外面
        oneOf: [
          {
            test: /\.less$/,
            use: [
              ...(isDev ? ['style-loader', 'css-loader'] : commonCSSLoader),
              'less-loader'
            ]
          },
          {
            test: /\.css$/,
            use: [...(isDev ? ['style-loader', 'css-loader'] : commonCSSLoader)]
          },
          {
            test: /\.js$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
            options: {
              presets: [
                '@babel/preset-env',
                {
                  // 按需加载
                  useBuiltIns: 'usage',
                  // 指定core-js版本
                  corejs: {
                    version: 3
                  },
                  // 指定兼容哪个版本浏览器
                  targets: {
                    chrome: '60',
                    firefox: '60',
                    ie: '9',
                    safari: '10',
                    edge: '17'
                  }
                }
              ]，
             cacheDirectory: true // 开启babel缓存，第二次构建时会读取之前的缓存
            }
          },
          {
            test: /\.(jpg|png|gif)$/,
            loader: 'url-loader',
            options: {
              limit: 8 * 1024,
              name: '[hash:10].[ext]',
              esModule: false,
              outputPath: 'imgs'
            }
          },
          {
            // html资源
            test: /\.html$/,
            loader: 'html-loader'
          },
          {
            exclude: /\.(html|css|less|js|jpg|png|gif)$/,
            loader: 'file-loader',
            options: {
              name: '[hash:10].[ext]',
              outputPath: 'media'
            }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
      minify: {
        removeAttributeQuotes: true, // 删除属性的双引号，除了一些特殊的删除不了以外都能删除
        collapseWhitespace: true, // 折叠空行将所有代码变成一行
        removeComments: true // 移除注释
      }
    }),
    // 抽离css
    new MiniCssExtractPlugin([{ filename: 'css/built.css' }]),
    // 压榨css
    new optimizeCssAssetsWebpackPlugin()
  ],
  devServer: {
    contentBase: path.resolve(__dirname, 'build'),
    compress: true,
    port: 3000,
    open: true
  }
}
```


## Babel使用规则

**[Babel](https://babeljs.io/)是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。**

**这意味着，你可以现在就用 ES6 编写程序，而不用担心现有环境是否支持。下面是一个例子。**

```js
// 转码前
input.map(item => item + 1);

// 转码后
input.map(function (item) {
  return item + 1;
});
//上面的原始代码用了箭头函数，这个特性还没有得到广泛支持，Babel将其转为普通函数，就能在现有的JavaScript环境执行了。
```

### 1.安装`babel`

Babel提供`babel-cli`工具，用于命令行转码。

它的安装命令如下：

```js
//需要先安装node环境
npm install --global babel-cli
```



### 2.配置文件`.babelrc`

Babel的配置文件是`.babelrc`，存放在项目的根目录下。使用Babel的第一步，就是配置这个文件。

该文件用来设置转码规则和插件，基本格式如下。

```js
{
  "presets": [],
  "plugins": []
}
```

`presets`字段设定转码规则，官方提供以下的规则集，你可以根据需要安装。

```js
# ES2015转码规则
npm install --save-dev babel-preset-es2015

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
npm install --save-dev babel-preset-stage-0
npm install --save-dev babel-preset-stage-1
npm install --save-dev babel-preset-stage-2
npm install --save-dev babel-preset-stage-3
```

然后，将这些规则加入`.babelrc`。

```js
//例如：按需求添加
{
    "presets": [
      	"es2015",
      	"stage-2"
    ],
	"plugins": []
}
```

### 3.用法

```js
# 转码结果输出到标准输出
$ babel example.js

# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
$ babel example.js --out-file compiled.js
# 或者
$ babel example.js -o compiled.js

# 整个目录转码
# --out-dir 或 -d 参数指定输出目录
$ babel src --out-dir lib
# 或者
$ babel src -d lib

# -s 参数生成source map文件
$ babel src -d lib -s
```

------

上面代码是在全局环境下，进行Babel转码。这意味着，如果项目要运行，全局环境必须有Babel，也就是说项目产生了对环境的依赖。另一方面，这样做也无法支持不同项目使用不同版本的Babel。

一个解决办法是将`babel-cli`安装在项目之中。

```js
# 安装
npm install --save-dev babel-cli
```

然后，改写`package.json`。

```js
{
  // ...
  "devDependencies": {
    "babel-cli": "^6.0.0"
  },
  "scripts": {
    "build": "babel src -d lib"
  },
}
```

转码的时候，就执行下面的命令。

```js
npm run build
```


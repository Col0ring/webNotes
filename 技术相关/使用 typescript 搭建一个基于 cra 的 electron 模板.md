# 使用 typescript 搭建一个基于 cra 的 electron 模板

## 前言
最近要写一个 electron 项目，决定采用的技术栈是 react + typescript 。本来想着用市面上现有的模板进行搭建，比如`electron-react-boilerplate`。不过后来感觉这些模板有些庞大，就自己用`create-react-app`简单搭建了个模板，写篇文章记录一下搭建过程。

## 准备工作
### 1.使用 cra 创建项目
```sh
npx create-react-app electron-template --typescript
```
### 2.添加 electron
```sh
yarn add electron -D
```
**注：** 因为下载源的原因，electron 经常会出现下载失败的情况，建议更换下载源或手动下载。

**`yarn`替换淘宝的下载源：**
```sh
yarn config set electron_mirror https://npm.taobao.org/mirrors/electron/
```

### 3.配置入口及目录结构
在根路径新建一个`main`目录作为存放主进程相关代码，创建`index.ts`文件作为入口

```typescript
import { app, BrowserWindow } from 'electron'
import path from 'path'

let win: BrowserWindow | null = null
function createWindow() {
  // 创建浏览器窗口。
  win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      // 加上这个就可以在渲染进程使用 winodw.require 引入 electron 模块
      nodeIntegration: true
    }
  })

  const urlLocation = 'http://localhost:3000'

  win.loadURL(urlLocation)

  // 当 window 被关闭，触发该事件
  win.on('closed', () => {
    win = null
  })
}

app.on('ready', createWindow)

// 当全部窗口关闭时退出程序
app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  if (win === null) {
    createWindow()
  }
})
```

## 开发模式配置
一般来说主进程用 JS 进行书写，然后直接在`package.json`中配置`main`就可以直接运行，不过对于一个 TypeScriot 爱好者来说，开发不使用 TypeScript 浑身难受，所以这里我使用了`webpack`对主进程代码打包然后再使用`electron`启动。

### 1.配置 webpack
> webpack 的配置我也使用的 TypeScript 进行书写。

在这里使用 cra 内置安装过的`babel-loader + @babel/preset-typescript`作为编译 TypeScript的方案就行了。在根路径创建一个`.babelrc`文件：

```json
{
  "presets": ["@babel/preset-typescript"]
}
```

然后在根路径下创建`config`目录，然后创建一个`webpackConfig.ts`文件作为`webpack`的基本配置文件：

```typescript
// config/webpackConfig.ts
import { Configuration } from 'webpack'
import path from 'path'

class WebpackConfig implements Configuration {
  // 修改 target 为 electron-main，这样才能正确打包主进程
  target: Configuration['target'] = 'electron-main'
  entry: Configuration['entry'] = [path.resolve(__dirname, '../main/index.ts')]
  output: Configuration['output'] = {
    filename: 'main.js',
    path: path.resolve(__dirname, '../build')
  }
  resolve: Configuration['resolve'] = {
    alias: {
      '@': path.resolve(__dirname, '../src'),
      '@@': path.resolve(__dirname, '../')
    },
    extensions: ['.ts', '.tsx', '.js', '.jsx', '.json']
  }
  module: Configuration['module'] = {
    rules: [
      {
        test: /\.tsx?$/,
        use: [
          'babel-loader'
        ]
      }
    ]
  }
  constructor(public mode: Configuration['mode'] = 'production') {}
}

export default WebpackConfig
```

再创建一个启动文件（因为是用的 TypeScript，所以采用 api 式的启动方式）：

```typescript
// config/start.ts
import webpack from 'webpack'

import WebpackConfig from './WebpackConfig'

const env =
  process.env.NODE_ENV === 'development' ? 'development' : 'production'

// 创建编译时配置
const config = new WebpackConfig(env)

function errorHandler(err: Error & { details?: string }, stats: webpack.Stats) {
  const info = stats.toJson()
  if (err || stats.hasErrors()) {
    if (err) {
      console.error(err.stack || err)
      if (err.details) {
        console.error(err.details)
      }
    }
    if (stats.hasWarnings()) {
      console.warn(info.warnings)
    }
    if (stats.hasErrors()) {
      console.error(info.errors)
    }
    return false
  } else {
    if (stats.hasWarnings()) {
      console.warn(info.warnings)
    }
    return true
  }
}

if (env === 'development') {
  // 通过watch来实时编译
  const watching = webpack(config).watch(
    {
      aggregateTimeout: 300,
      ignored: /node_modules/
    },
    (err, stats) => {
      const flag = errorHandler(err, stats)
      if (flag) {
        console.log('webpack start successfully')
      } else {
        watching.close(() => {})
        throw Error('webpack start failed')
      }
    }
  )
} else {
  webpack(config).run((err: Error & { details?: string }, stats) => {
    const flag = errorHandler(err, stats)
    if (flag) {
      console.log('webpack build successfully')
    } else {
      throw Error('webpack build failed')
    }
  })
}
```



### 2.配置启动脚本

已经配置好了`webpack`，因为是使用的 TypeScript，所以使用`ts-node`运行启动文件，但是由于`ts-loader`运行时会默认使用项目根目录的`tsconfig.json`文件，而 cra 本身的`tsconfig.json`并不是打包成 commonjs，所以我们复制一份到 config 目录中，修改一下`module`的配置：

```json
// config/tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "commonjs",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "noEmit": true,
    "isolatedModules": true
  }
}
```

开发模式的脚本如下：

```json
{
  "start:render": "cross-env BROWSER=none react-scripts start",
  "start:watch-main": "cross-env NODE_ENV=development ts-node --project ./config/tsconfig.json ./config/start.ts",
  "start:main": "wait-on http://localhost:3000 && nodemon --watch ./build --exec electron .",
  "prestart": "rimraf ./build",
  "start": "npm-run-all --parallel start:*",
}
```

**配置说明：**

- 使用`cross-env`兼容 windows 和 mac 环境下的环境变量。
- 将 node 设置为开发环境使`webpack`实时监听主进程相关文件变化重新打包。
- 使用`nodemon`监听`build`目录变化，每次目录变化就重新运行主程序，同时使用`wait-on`等渲染进程执行完后再运行主进程。
- 使用`npm-run-all --parallel`让所有脚本同步触发，只用开一个端口就可启动程序。

然后运行`yarn start`即可启动程序。



## 应用打包配置

市面上主要的打包插件有`electron-builder`和`electron-packager`，这里我使用的`electron-builder`。

```sh
yarn add electron-builder -D
```

### 1.修改配置文件

因为打包后与运行时的路径有差别，所以这里要修改一下之前的代码：

引入`electron-is-dev`判断程序运行环境：

```sh
yarn add electron-is-dev -D
```

```typescript
// main/index.ts
// 该文件的修改主要是关于引入了 electron-is-dev 后的操作
import { app, BrowserWindow } from 'electron'
import path from 'path'
import isDev from 'electron-is-dev'

let win: BrowserWindow | null = null
function createWindow() {
	// ...
  // 根据运行环境选择
  const urlLocation = isDev
    ? 'http://localhost:3000'
    : `file://${path.join(__dirname, './index.html')}` // 这里的路径是相对于打包后的文件路径

  win.loadURL(urlLocation)

  // ...
}

// ...
```

然后增加一项`webpack`配置：

```typescript
// config/webpackConfig.ts
import { Configuration } from 'webpack'
import path from 'path'

class WebpackConfig implements Configuration {
 	// ...
   node: Configuration['node'] = {
    // 默认值是 'mock'，会将其转化为'/'，我们这里并不是服务端，应该设置为 false ，表示输出文件的目录名，在打包代码里面也要一直将其当作打包后的文件路径使用
    __dirname: false,
    __filename: false
  }
  // ...
}

export default WebpackConfig
```

再修改一下`package.json`中的配置项：

```json
{
  "homepage": "./"，
  "build": {
    "appId": "testId",
    "productName": "testName",
    "files": [
      "build/**/*"
    ],
    "extraMetadata": {
      "main": "./build/main.js"
    },
    "directories": {
      "buildResources": "assets"
    }
  },
}
```

**配置说明：**

- 修改`homepage`时因为 cra 打包时默认会以该项的路径为基础进行打包，因为我们打包后不是在服务器使用，所以应该改为相对路径。

- 至于`build`选项则是`electron builder`的打包配置，具体细节要根据实际项目进行配置，该项目要能正常打包成桌面应用其实只需要上面的:

  ```json
  {
    "files": [
      "build/**/*"
    ],
    "extraMetadata": {
      "main": "./build/main.js"
    }
  }
  ```

  就可以成功进行应用打包了。其余的配置项请自行参考 [electron-builder 官网](https://www.electron.build/)。



### 2.配置打包脚本

生产模式脚本如下：

```json
{
  "build:render": "react-scripts build",
  "build:main": "cross-env NODE_ENV=productment ts-node --project ./config/tsconfig.json ./config/start.ts",
  "build": "npm-run-all build:*",
  "prepack": "npm run build",
  "dist": "electron-builder",
  "pack": "electron-builder --dir",
  "package-all": "npm run build && electron-builder build -mwl",
  "package-mac": "npm run  build && electron-builder build --mac",
  "package-linux": "npm run  build && electron-builder build --linux",
  "package-win": "npm run  build && electron-builder build --win --x64"
}
```

然后运行`yarn run pack`就可在当前目录生成符合用户对应操作系统的应用了。



### 3.注意事项

由于`electron-builder`会将`dependencies`的依赖都打包进去，所以为了减小打包体积，尽量将依赖都放到`devDependencies`中



## 总结

于此，就搭建了一个简易的集成了 react 与 typescript 的开发 electron 开发环境。

该模版的全部代码已发布到 [github](https://github.com/Col0ring/ts-react-electron-template)。
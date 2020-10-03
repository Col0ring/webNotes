# 如何开发一个脚手架工具

## 前言
因为经常创建代码模板以便于二次开发，有时会将所写的模板上传到 git 仓库以便于下次直接使用。但是由于模板数量的不断增加，有时可能会无法快速找到所需要的模板仓库。最主要的是，每次都要去手动查找，太麻烦了！！并且现在主流的框架都搭配了一个相应的 cli 用于创建初始项目，于是打算模仿它们做一个属于自己的脚手架工具。



## 准备工作

因为是使用`node.js`环境进行开发，所以会依赖一些第三方库：
- **交互**
  + [commander.js](https://github.com/tj/commander.js)：完整的 node.js 命令行解决方案，灵感来自 Ruby 的 commander。
  + [Inquirer.js](https://github.com/SBoudrias/Inquirer.js)：通用交互式命令行用户界面的集合。
  + [clear](https://github.com/bahamas10/node-clear#readme)：清除终端，相当于直接在终端使用清除命令。
  + [chalk](https://github.com/chalk/chalk)：为终端的字符串添加正确的颜色（自定义终端字体颜色）。
  + [log-symbols](https://github.com/sindresorhus/log-symbols)：为打印日志添加的彩色符号（比如 √ 和 ×）。
  + [figlet.js](https://github.com/patorjk/figlet.js)：一个用 JavaScript 编写的 FIG 驱动程序，能够生成基于`ASCII`的艺术字。
- **创建模板**
  + [download-git-repo](https://gitlab.com/flippidippi/download-git-repo)：基于`node.js`下载并提取 git 仓库（GitHub、GitLab、Bitbucket）。
  + [handlebars.js](https://github.com/wycats/handlebars.js)：模板引擎，用于将与用于交互所得到的信息填入到模板文件中，比如对`package.json`的内容进行动态创建。
  + [ora](https://github.com/sindresorhus/ora)：优雅的终端 spinner，可用于处理下载过程中的下载动画。
  + [clui](https://github.com/nathanpeck/clui)（可选）：一个更加完善的终端图形库，内部也有包括 spinner 等组件。

**注：**因为该文章只是做一个演示，所以暂时只会用到上面的部分库，对其他库有需求的同学可以下来自行了解。

```sh
npm install commander inquirer clear chalk log-symbols figlet download-git-repo handlebars ora -S
# or
yarn add commander inquirer clear chalk log-symbols figlet download-git-repo handlebars ora
```



## 编码

基于上面的第三方库，现在要创建一个叫作`cotpl-cli`的脚手架工具，该工具拥有一个选择创建代码模板的功能。具体使用如下：

```sh
cotpl create <project>
```

当执行以上命令时，如果已存在指定项目会提示用户已存在并结束进程，如果不存在则会让用户选择模板。

### 基本交互

要实现命令行的基本交互只需要搭配`commander.js`和`Inquirer.js`使用就足够了。
```js
// ./src/index.js
// version
const fs = require('fs')
const path = require('path')
const program = require('commander')
const inquirer = require('inquirer')

// 设置版本号，这里直接取 package.json 中的
program.version(
  require(path.resolve(__dirname, '../package.json')).version,
  '-V, --version'
)

// 创建一个命令
program
		// 如何使用
   .command('create <name>')
		// 命令的描述
   .description('create a new project powered by cotpl-cli')
	 // 运行结果
   .action(async (name) => {
   // name 为上面创建项目时传的项目名
   if (fs.existsSync(name)) {
      console.error(`Target directory ${process.cwd()}/${name} already exists.`)
    }else {
      // 引入交互
      const { type } = await inquirer.prompt([ // 传入数组，每个成员是一个提问，从上到下进行，用户回答的值将赋值给成员的 name 属性对应的值
        {
          type: 'list',
          name: 'type',
          message: 'choose a templete type you want to create:',
          choices: ['Vue', 'React', 'Node.js', 'Electron']
        }
      ])
      console.log(type)
    }
   });

// 解析用户传入的命令和参数
program.parse(process.argv);
```

```json
{
  "scripts": {
    "start": "node ./src/index.js"
  },
}
```

然后在项目中运行`npm run start create demo`

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a69bd4209bae4dcb8da7faa187231b34~tplv-k3u1fbpfcp-zoom-1.image)

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8377bb232bf941fbbe4f714485c956ae~tplv-k3u1fbpfcp-zoom-1.image)



### 交互优化

- `log-symbols`、`chalk`对用户提供更加个性化的回复。

- `clear`在用户输入命令后清空控制台内容。
- `figlet`为用户展示个性化的`ASCII`艺术字。

```js
// ./src/index.js
// ...
const chalk = require('chalk')
const clear = require('clear')
const figlet = require('figlet')
const symbols = require('log-symbols')


// create
program
  .command('create <name>')
  .description('create a new project powered by cotpl-cli')
  .action(async (name) => {
    if (fs.existsSync(name)) {
      //...
      console.error(
        // 错误符号
        symbols.error,
        // chalk 的用法之一，可自行查看
        chalk`{rgb(255,255,255) Target directory {rgb(130,223,226) ${process.cwd()}/${name}} already exists.}`
      )
    } else {
			//...
    }
  })

// clear terminal
clear()

// logo
console.log(
  chalk.blueBright(figlet.textSync('Cotpl', { horizontalLayout: 'full' }))
)
// parse the argv from terminal
program.parse(process.argv)
```

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fea1d845664d48d9a2ddb22a8886e125~tplv-k3u1fbpfcp-zoom-1.image)



### 下载模板

使用`download-git-repo`对`git`托管仓库的模板项目进行下载。

```js
// ./src/download.js
const download = require('download-git-repo')
// 下载的 loading 图标
const ora = require('ora')
/**
 *
 * @param {string} path
 * @param {string} name
 */
function downloadRepositorie(path, name) {
  return new Promise((resolve, reject) => {
    const spinner = ora('downloading template...')
    spinner.start()
    download(path, name, { clone: true }, (err) => {
      if (err) {
        spinner.fail()
        reject(err)
      }
      spinner.succeed()
      resolve()
    })
  })
}

module.exports = downloadRepositorie
```

```js
// ./src/repositories.js
// 模板仓库映射
module.exports = {
  Vue: {},
  React: {
    'umi-admin-template':
      'https://github.com/Col0ring/umi-admin-template.git#master'
  },
  'Node.js': {},
  Electron: {
    'create-react-app + typescript + electron':
      'https://github.com/Col0ring/ts-react-electron-template.git#master',
    'create-react-app + typescript + electron + antd + less + css-modules':
      'https://github.com/Col0ring/ts-react-electron-template.git#antd'
  }
}
```

```js
// ./src/index.js
const fs = require('fs')
const path = require('path')
const program = require('commander')
const chalk = require('chalk')
const clear = require('clear')
const figlet = require('figlet')
const inquirer = require('inquirer')
const symbols = require('log-symbols')
const download = require('./download')
const repositories = require('./repositories')

// version
program.version(
  require(path.resolve(__dirname, '../package.json')).version,
  '-V, --version'
)

// create
program
  .command('create <name>')
  .description('create a new project powered by cotpl-cli')
  .action(async (name) => {
    if (fs.existsSync(name)) {
      console.error(
        symbols.error,
        chalk`{rgb(255,255,255) Target directory {rgb(130,223,226) ${process.cwd()}/${name}} already exists.}`
      )
    } else {
      const { type } = await inquirer.prompt([
        {
          type: 'list',
          name: 'type',
          message: 'choose a templete type you want to create:',
          choices: ['Vue', 'React', 'Node.js', 'Electron']
        }
      ])
      // 获取类型
      const typeRepositories = repositories[type]
      const typeKeys = Object.keys(typeRepositories)
      // 判断是否有对应仓库
      if (typeRepositories && typeKeys.length > 0) {
        const { templateName } = await inquirer.prompt([
          {
            name: 'templateName',
            message: `choose the template you need:`,
            type: 'list',
            choices: typeKeys
          }
        ])
        // 这里的路径见 download-git-repo，前面加 direct: 代表将完整的 url 给 git 存储库，同时需要指定分支名 direct:url#my-branch
        const downloadPath = `direct:${typeRepositories[templateName]}`
        // 下载模板
        download(downloadPath, name)
          .then(() => {
          // 成功
            console.log(
              symbols.success,
              chalk`{rgb(87,190,56) download successfully!}`
            )
          })
          .catch((err) => {
          // 失败
            console.error(symbols.error, chalk`{rgb(255,255,255) ${err}}`)
            console.error(
              symbols.error,
              chalk`{rgb(255,255,255) download template fail,please check your network connection and try again.}`
            )
            process.exit()
          })
      } else {
        console.log(
          symbols.info,
          chalk`{rgb(255,255,255) There are no templates for ${type}}`
        )
      }
    }
  })

// clear terminal
clear()

// logo
console.log(
  chalk.blueBright(figlet.textSync('Cotpl', { horizontalLayout: 'full' }))
)
// parse the argv from terminal
program.parse(process.argv)
```

上面的代码就已经完成了选择模板已经从 git 仓库下载模板到指定文件夹的功能。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f15256dd6492499d89fce54cab19701d~tplv-k3u1fbpfcp-zoom-1.image)



### 渲染元信息

通过上面的步骤我们已经完成了下载模板的功能，如果要修改项目信息可以直接进入到项目中进行修改，但是如果想让用户在创建项目的时候就输入一些信息并自动填入模板文件中，还需要借助模板引擎的帮助。

```js
// ./src/render.js
const fs = require('fs')
const handlebars = require('handlebars')
/**
 *
 * @param {object} meta metadata
 * @param {string} meta.description
 * @param {string} meta.name
 */
function render(meta) {
  const fileName = `${meta.name}/package.json`
  const content = fs.readFileSync(fileName).toString()
  // compile the package.json template
  const result = handlebars.compile(content)(meta)
  fs.writeFileSync(fileName, result)
}

module.exports = render
```

`package.json`中要对要写入要编译的模板字段：

```json
{
  "name":"{{name}}"
  "description":"{{description}}"
}
```

上面代码需要我们传入`name`和`description`，`name`我们使用用户创建的项目名，`description`还需要用户手动进行输入

```js
// .src/index.js
//...
const render = require('./render')

// create
program
  .command('create <name>')
  .description('create a new project powered by cotpl-cli')
  .action(async (name) => {
  			//...
  			// 选择模板后马上填写项目描述
        const { templateName, description } = await inquirer.prompt([
          {
            name: 'templateName',
            message: `choose the template you need:`,
            type: 'list',
            choices: typeKeys
          },
          {
            name: 'description',
            message: 'description:'
          }
        ])
        // ...
        download(downloadPath, name)
          .then(() => {
           // 下载完模板后修改 package.json
            render({ description, name })
            // ...
          })
      // ...
    }
  })

// ...
```

进行如上配置后就能在项目的`package.json`中看到修改后的信息了。



## 如何使用

在前面我们完成了 cli 的所有编码工作，但是如果要像使用命令一样去使用该 cli，还需要修改一下项目配置。

### 1.在 pagckage.json 中添加 bin 字段

```json
{
  "bin": {
    "cotpl": "./src/index.js"
  } 
}
```

**如果为`package.json`添加了 bin 字段，对应的可执行文件就会被链接到当前项目的`./node_modules/.bin`中，在本地项目中就可以使用`npx cotpl`运行命令，如果是全局模块则可以直接使用该命令。**



### 2.为运行文件添加脚本解释程序

在被指定的 bin 的运行文件中添加以下代码，这行代码的意思就是告诉系统动态的去环境变量中查找 node 来执行该文件。

```js
// ./src/index.js
#!/usr/bin/env node
```



### 3.将命令添加到环境中

- 如果是在本地开发使用，可以直接在项目根目录运行`npm link`，这样就能直接在本机使用该 cli 工具了。
- 如果要在不同电脑或与其他人共享使用，按照发布第三方库的流程将库发布到`npm`上然后使用`npm install 库名 -g`来全局使用。



## 总结

本文从零到一搭建了一个能够下载代码模板的脚手架，并尽可能地优化了界面显示。如果你对如何创建一个命令行工具感兴趣或者想要快速生成一个代码模板，可以尝试手动编写一个属于自己的脚手架工具。

相关代码已上传至[github](https://github.com/Col0ring/cotpl-cli)



## 参考

- [使用 Node.js 开发简单的脚手架工具](https://segmentfault.com/a/1190000015222967)
- [Build a JavaScript Command Line Interface (CLI) with Node.js](https://www.sitepoint.com/javascript-command-line-interface-cli-node-js/)
- [npm-package.json](https://docs.npmjs.com/files/package.json.html)
# Vue项目创建打包与UI资源

## 1.Vue项目创建

### 1.1 vue-cli脚手架

**vue-cli是一个基于vue的构建工具,用于搭建vue项目的环境,有着兼容,方便,快速的优点,能够完全遵循前后端分离的原则,用vue开发单网页项目(SPA)的能力尤其的好**

**注:**可以不用脚手架(vue-cli)就可以基于 webpack 打包工具 ,webpack最终会把整个项目打包成一个js文件但需要自己进行配置各个版本兼容问题,正因为这样,前端有一个专门的配置工程师



### 1.2 下载Node.js

**去Node.js的官网下载最新版的node,需要用到其包管理工具npm** ([Node.js官网](https://nodejs.org/en/))



### 1.3 配置淘宝镜像

**因为npm是国外的,在国内用会特别慢,所以需要先用淘宝的cnpm代替npm**

在命令行窗口输入   ` npm install -g cnpm --registry=https://registry.npm.taobao.org`   配置淘宝镜像



### 1.4  安装vue-cli

在命令行窗口输入 `cnpm i -g vue-cli` 全局安装vue-cli脚手架

**注:**安装完成后可以使用 `vue -V` 查看是否安装成功



### 1.5 安装项目文件

先到项目文件夹,打开命令行窗口输入` vue init webpack 项目文件夹名`

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20190326233204681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkwMzY5Ng==,size_16,color_FFFFFF,t_70)



### 1.6 运行项目文件

在项目文件中使用`npm run dev`运行项目文件

出现![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20190326233323424.png)字样后在 http://localhost:8080 查看生产的Vue项目,出现下面的页面证明Vue项目创建成功

![1553613036200](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\1553613036200.png)



### 1.7 打包项目

在项目文件夹中运行 `npm run build` 将项目打包 ,打包后的文件将会保存在该文件的ldst文件夹中

**注意**:要点击打开dist中的`index.html`,需要修改两个地方

- 找到 ` config->index.js`

  ![img](https://img-blog.csdn.net/20180410235639689?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NzA4OTkx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- 找到`build->utils.js`

  ![img](https://img-blog.csdn.net/20180411000014630?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NzA4OTkx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)





## 2.UI资源

- **ElementUI**
  **介绍：**Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库
  **链接：**https://element.eleme.cn/#/zh-CN

- **Mint UI**
  **介绍：**基于 Vue.js 的移动端组件库
  **链接：**https://mint-ui.github.io/#!/zh-cn

- **Mui**
  **介绍：**最接近原生APP体验的高性能前端框架
  **链接：**http://dev.dcloud.net.cn/mui/ui/

- **Aui 框架**
  **介绍：**使用了大量弹性响应式布局，采用容器+布局结构+控件的嵌套形式,借鉴了市场上其他优秀UI框架
  **链接：**http://www.auicss.com/

- **we-vue**
  **介绍：**使用 Vue2.x + weui1.x 开发的组件
  **链接：**https://github.com/tianyong90/we-vue

- **Vue-Layout**
  **介绍：**vue可视化布局
  **链接：**https://jaweii.github.io/Vue-Layout/dist/#/

- **muse-ui**

  **链接：**https://muse-ui.org/#/zh-CN/tabs
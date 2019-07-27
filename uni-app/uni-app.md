# **uni**-app

## 1.建立uni-app项目

官方教程:https://uniapp.dcloud.io/quickstart



## 2.目录结构以及生命周期

[uni-app的目录结构与生命周期](https://uniapp.dcloud.io/frame?id=%e7%9b%ae%e5%bd%95%e7%bb%93%e6%9e%84)



## 3.路由

`uni-app`路由全部交给框架统一管理，开发者需要在`pages.json`里配置每个路由页面的路径及页面样式（与微信小程序类似），不支持 `Vue Router`

### 3.1 路由跳转

`uni-app` 有两种路由跳转方式：使用[navigator](https://uniapp.dcloud.io/component/navigator)组件跳转、调用[API](https://uniapp.dcloud.io/api/router)跳转



### 3.2 页面栈

| 路由方式 | 页面栈表现           | 触发时机                           |
| ---------------- | --------------------------------- | ------------------------------------------------------------ |
| 初始化 | 新页面入栈                 | uni-app 打开的第一个页面                                     |
| 打开新页面       | 新页面入栈                        | 调用 API   [uni.navigateTo](https://uniapp.dcloud.io/api/router?id=navigateto)  、使用组件  [<navigator open-type="navigateTo"/>](https://uniapp.dcloud.io/component/navigator?id=navigator) |
| 页面重定向       | 当前页面出栈，新页面入栈          | 调用 API   [uni.redirectTo](https://uniapp.dcloud.io/api/router?id=redirectto)  、使用组件  [<navigator open-type="redirectTo"/>](https://uniapp.dcloud.io/component/navigator?id=navigator) |
| 页面返回         | 页面不断出栈，直到目标返回页      | 调用 API  [uni.navigateBack](https://uniapp.dcloud.io/api/router?id=navigateback)   、使用组件 [<navigator open-type="navigateBack"/>](https://uniapp.dcloud.io/component/navigator?id=navigator) 、用户按左上角返回按钮、安卓用户点击物理back按键 |
| Tab 切换         | 页面全部出栈，只留下新的 Tab 页面 | 调用 API  [uni.switchTab](https://uniapp.dcloud.io/api/router?id=switchtab)  、使用组件  [<navigator open-type="switchTab"/>](https://uniapp.dcloud.io/component/navigator?id=navigator)  、用户切换 Tab |
| 重加载           | 页面全部出栈，只留下新的页面      | 调用 API  [uni.reLaunch](https://uniapp.dcloud.io/api/router?id=relaunch)  、使用组件  [<navigator open-type="reLaunch"/>](https://uniapp.dcloud.io/component/navigator?id=navigator) |



## 4.页面样式与布局

[uni-app的页面样式与布局](<https://uniapp.dcloud.io/frame?id=%e9%a1%b5%e9%9d%a2%e6%a0%b7%e5%bc%8f%e4%b8%8e%e5%b8%83%e5%b1%80>)

### 4.1  尺寸单位

**推荐使用upx进行响应式布局,同微信小程序里面的rpx**



### 4.2 背景图片与字体图标

#### 4.2.1 背景图片

`uni-app` 支持使用在 css 里设置背景图片，使用方式与普通 `web` 项目相同，需要注意以下几点：

- 支持 base64 格式图片。

- 支持网络路径图片

- 使用本地路径背景图片需注意：

  1. 图片小于 40kb，`uni-app` 会自动将其转化为 base64 格式

  2. 图片大于等于 40kb， 需开发者自己将其转换为base64格式使用，或将其挪到服务器上，从网络地址引用

  3. 本地背景图片的引用路径仅支持以 ~@ 开头的绝对路径（不支持相对路径）

     ```css
      .test2 {
          background-image: url('~@/static/logo.png');
      }
     ```

### 

#### 4.2.2 字体图标

`uni-app` 支持使用字体图标，使用方式与普通 `web` 项目相同，需要注意以下几点：

- 支持 base64 格式字体图标

- 支持网络路径字体图标

- 网络路径必须加协议头 `https`

- 从 [http://www.iconfont.cn](http://www.iconfont.cn/) 上拷贝的代码，默认是没加协议头的

- `uni-app`本地路径图标字体支持情况：

  1. 字体文件小于 40kb，`uni-app` 会自动将其转化为 base64 格式

  2. 字体文件大于等于 40kb， 需开发者自己转换，否则使用将不生效

  3. 字体文件的引用路径仅支持以 ~@ 开头的绝对路径（不支持相对路径）

     ```css
      @font-face {
          font-family: test1-icon;
          src: url('~@/static/iconfont.ttf');
      }
     ```



### 4.3 注意点

**创建完项目后的`App.vue`中的script标签中的事件就是小程序应用的生命周期，而style标签中的样式就是全局的样式**



## 5.使用`<template/> `和 `<block/> `

`uni-app` 支持在 template 模板中嵌套 `<template/>` 和 `<block/>`，用来进行列表渲染和 条件渲染

`<template/>` 和 `<block/>` 并不是一个组件，它们仅仅是一个包装元素，不会在页面中做任何渲染，只接受控制属性

**代码示例**

```vue
<template>
	<div>
        <template>
            <view>
                <template v-if="test">
                    <view>test 为 true 时显示</view>
                </template>
                <template v-else>
                    <view>test 为 false 时显示</view>
                </template>
            </view>
        </template>
        <template>
            <view>
                <block v-for="(item,index) in list" :key="index">
                    <view>{{item}} - {{index}}</view>
                </block>
            </view>
        </template>     
    </div>
</template>
```



## 6.配置文件

[uni-app配置文件](https://uniapp.dcloud.io/collocation/pages)

**配置文件包括pages,json、manifest.json、uni.scss**

- `pages.json` 文件用来对 uni-app 进行全局配置，决定页面文件的路径、窗口样式、原生的导航栏、底部的原生tabbar 等。它类似微信小程序中`app.json`的**页面管理**部分，但是不同的是每一个页面的pages.json也会在这一个文件中通过嵌套的方式来使用
- `manifest.json` 文件是应用的配置文件，用于指定应用的名称、图标、权限等
- `uni-app` 内置了常用的样式变量，采用 scss 预处理方式，文件名为 `uni.scss`，在代码中无需 import 这个文件即可使用这些样式变量



## 7.组件

[uni-app中的组件](https://uniapp.dcloud.io/component/README)

**使用的组件大多同微信小程序**



## 8.API

[uni-app中的API](https://uniapp.dcloud.io/component/README)

**也同微信小程序，只是将前缀wx改为了uni**



## 9.事件处理

[uni-app中的事件](https://uniapp.dcloud.io/use?id=%e4%ba%8b%e4%bb%b6%e5%a4%84%e7%90%86%e5%99%a8)

```js
// 事件映射表，左侧为 WEB 事件，右侧为 ``uni-app`` 对应事件
{
    click: 'tap',
    touchstart: 'touchstart',
    touchmove: 'touchmove',
    touchcancel: 'touchcancel',//手指触摸动作被打断，如在打游戏的时候接电话，手指的动作就会被打断
    touchend: 'touchend',
    tap: 'tap',
    longtap: 'longtap',//长时间触碰屏幕，触发该事件tap事件就不会触发
    longpress: 'longpress',//长时间触碰屏幕，同longtap，但是推荐使用这个
    input: 'input',
    change: 'change',//表单中的文字发生改变
    submit: 'submit',//表单提交,点击了提交按钮后
    blur: 'blur',//input失去焦点
    focus: 'focus',//input得到焦点
    reset: 'reset',//表单重置，点击了重置按钮后
    confirm: 'confirm',//点击手机键盘中的确认按钮，或者web端的回车键
    columnchange: 'columnchange',//列表的列发生变化，在多列选择器中所拥有的事件
    linechange: 'linechange',//一般用在textarea文本域中，表示文本域中的行发生了变化，如点击回车
    error: 'error',
    //下面三个都是可滚动视图组件中的事件
    scrolltoupper: 'scrolltoupper',//向上滚动
    scrolltolower: 'scrolltolower',//向下滚动
    scroll: 'scroll'//只要滚动就会触发
}
```

**注意：**

- 为兼容各端，事件需使用 `v-on` 或 `@` 的方式绑定，请勿使用小程序端的`bind` 和 `catch` 进行事件绑定。

- 事件修饰符

  - `.stop`：各平台均支持， 使用时会阻止事件冒泡，在非 H5 端同时也会阻止事件的默认行为
  - `.prevent` 仅在 H5 平台支持
  - `.self`：仅在 H5 平台支持
  - `.once`：仅在 H5 平台支持
  - `.capture`：仅在 H5 平台支持
  - `.passive`：仅在 H5 平台支持

- 若需要禁止蒙版下的页面滚动，可使用`@touchmove.stop.prevent="moveHandle"`，moveHandle 可以用来处理 touchmove 的事件，也可以是一个空函数

  ```html
  <view class="mask" @touchmove.stop.prevent="moveHandle"></view>
  ```

- 按键修饰符：`uni-app`运行在手机端，没有键盘事件，所以不支持按键修饰符

- H5 的select 标签用 picker 组件进行代替

- 表单元素 radio 用 radio-group 组件进行代替



## 10.条件编译

[uni-app的条件编译](https://uniapp.dcloud.io/platform?id=%e6%9d%a1%e4%bb%b6%e7%bc%96%e8%af%91)
# mpVue

## 1.框架原理

- `mpvue` 保留了 vue.runtime 核心方法,无缝继承了 `Vue.js` 的基础能力
- `mpvue-template-compiler` 提供了将 vue 的模板语法转换到小程序的`wxml`语法的能力
- 修改了 vue 的建构配置,使之构建出符合小程序项目结构的代码格式:`json/wxml/wxss/js `文件



## 2.安装一个mpVue项目

```shell
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 mpvue-quickstart 模板的新项目
$ vue init mpvue/mpvue-quickstart my-project

# 安装依赖
$ cd my-project
$ npm install
# 启动构建
$ npm run dev
```

**然后启动微信开发者工具,引入项目即可**

运行`npm start`将原生Vue转换为微信小程序,将dist目录的文件引入到微信开发者工具项目中

```shell
$ npm start
```



## 3.生命周期

除了Vue本身的生命周期外,mpVue还兼容小程序生命周期,这部分生命周期钩子的来源于微信小程序的page,但是,除了特殊情况外,不建议使用小程序的生命周期钩子

![img](http://mpvue.com/assets/lifecycle.jpg)

### 3.1 Vue生命周期

- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- activated
- deactivated
- beforeDestroy
- destroyed



### 3.2 小程序生命周期

#### 3.2.1 app部分

- onLaunch，初始化
- onShow，当小程序启动，或从后台进入前台显示
- onHide，当小程序从前台进入后台

#### 3.2.2 page部分

- onLoad，监听页面加载
- onShow，监听页面显示
- onReady，监听页面初次渲染完成
- onHide，监听页面隐藏
- onUnload，监听页面卸载
- onPullDownRefresh，监听用户下拉动作
- onReachBottom，页面上拉触底事件的处理函数
- onShareAppMessage，用户点击右上角分享
- onPageScroll，页面滚动
- onTabItemTap, 当前是 tab 页时，点击 tab 时触发 （mpvue 0.0.16 支持）

**注意:**

- 不要在选项属性或回调上使用箭头函数，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数是和父级上下文绑定在一起的，`this` 不会是如你做预期的 Vue 实例，且 `this.a` 或 `this.myMethod` 也会是未定义的

  -微信小程序的页面的 `query` 参数是通过 `onLoad` 获取的，mpvue 对此进行了优化，直接通过 `this.$root.$mp.query` 获取相应的参数数据，其调用需要在 `onLoad` 生命周期触发之后使用，比如 `onShow` 等



## 4.无法支持Vue部分

- 不支持BOM和DOM,不支持`v-html表达式`

- 模块字符串里不支持赋值的JS表达式,以为微信小程序本身就不支持

- 不支持过滤器

- 不支持在template

- 不支持对象形式的绑定class和style

  - **class支持语法**

    ```html
    <p :class="{ active: isActive }">111</p>
    <p class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }">222</p>
    <p class="static" :class="[activeClass, errorClass]">333</p>
    <p class="static" v-bind:class="[isActive ? activeClass : '', errorClass]">444</p>
    <p class="static" v-bind:class="[{ active: isActive }, errorClass]">555</p>
    ```

  - **style支持语法**

    ```html
    <p v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">666</p>
    <p v-bind:style="[{ color: activeColor, fontSize: fontSize + 'px' }]">777</p>
    ```

  - **使用`computed`属性**

    ```vue
    <template>
        <!-- 支持 -->
        <div class="container" :class="computedClassStr"></div>
        <div class="container" :class="{active: isActive}"></div>
    
        <!-- 不支持 -->
        <div class="container" :class="computedClassObject"></div>
    </template>
    <script>
        export default {
            data () {
                return {
                    isActive: true
                }
            },
            computed: {
                computedClassStr () {
                    return this.isActive ? 'active' : ''
                },
                computedClassObject () {
                    return { active: this.isActive }
                }
            }
        }
    </script>
    ```

- 暂不支持在组件上使用class和style的绑定,除了小程序的组件

- Vue组件中的不支持

  - 暂不支持在组件引用时，在组件上定义 click 等原生事件、v-show（可用 v-if 代替）和 class style 等样式属性(例：`<card class="class-name"> </card>` 样式是不会生效的)，因为编译到 wxml，小程序不会生成节点，建议写在内部顶级元素上。
  - Slot（scoped 暂时还没做支持）
  - 动态组件
  - 异步组件
  - inline-template
  - X-Templates
  - keep-alive
  - transition
  - class
  - style

- mpVue中的页面跳转不推荐使用Vue-router,而是尽量使用原生小程序中提供的页面跳转的API



## 5.踩坑

- 如果要改变页面`page`的样式,不能加上`scoped`,因为page实质上是在Vue组件外面,不能控制该页面的样式

- 如果增加了新的页面,需要重新`npm start`项目,否则新建的页面不会进入编译后的项目中

- 在mpVue无法像原生小程序一样直接调用`getApp()`得到app实例,所以如果要控制全局变量需要使用Vuex

- 在mpVue中使用Vuex需要通过原型链的映射进行挂载,其余用法同Vue中

  ```js
  import Vue from "vue";
  import Store from "./store/store";
  import App from "./app.vue";
  
  //设置Vue的提示功能关闭
  Vue.config.productionTip = false;
  
  //声明当前组件的类型
  App.mpType = "app"; //组件类型为应用
  
  //将Store对象放在Vue的原型上,为的是每个对象都可以使用
  Vue.prototype.$store = Store;
  
  //生成应用的实例
  const app = new Vue(App);
  //挂载整个应用
  app.$mount();
  ```

- mpVue中的页面跳转不推荐使用Vue-router,而是尽量使用原生小程序中提供的页面跳转的API

- mpVue更支持使用组件化开发,所以尽量不使用模板`template`,并且不支持在`template`内使用methods中的函数

- axios在小程序中不能使用,可以使用小程序自带的API或者是其他库(如[Flyio](https://github.com/wendux/fly))

- 在 `input` 和 `textarea` 中 `change` 事件会被转为 `blur` 事件

- 列表中没有的原生事件也可以使用例如 bindregionchange 事件直接在 dom 上将bind改为@ `@regionchange`,同时这个事件也非常特殊，它的 event type 有 begin 和 end 两个，导致我们无法在`handleProxy` 中区分到底是什么事件，所以你在监听此类事件的时候同时监听事件名和事件类型既 `<map @regionchange="functionName" @end="functionName" @begin="functionName"><map>`

- 小程序能力所致，bind 和 catch 事件同时绑定时候，只会触发 bind ,catch 不会被触发，要避免踩坑

- 事件修饰符

  - `.stop` 的使用会阻止冒泡，但是同时绑定了一个非冒泡事件，会导致该元素上的 catchEventName 失效！
  - `.prevent` 可以直接干掉，因为小程序里没有什么默认事件，比如submit并不会跳转页面
  - `.capture` 支持 `1.0.9`
  - `.self` 没有可以判断的标识
  - `.once` 也不能做，因为小程序没有 removeEventListener, 虽然可以直接在 handleProxy 中处理，但非常的不优雅，违背了原意，暂不考虑
  - 其他修饰符不可用,因为没键盘

- 建议在开发中使用小程序提供的表单组件

- 在所有 页面 的组件内可以通过 `this.$root.$mp.query` 进行获取**page onLoad 时候传递的 options**

- 在所有的组件内可以通过 `this.$root.$mp.appOptions` 进行获取**app onLaunch/onShow 时候传递的 options**

- 要捕捉app的onError可以直接当做一个生命周期使用

- **小程序mpvue中input和textarea使用v-model绑定时，当删除或改变数据时，光标自动跑到最后**

  **解决方案:**使用lazy修饰符,即v-model.lazy进行数据绑定,如果后台没有拿到数据,就是因为onchange事件改为了onblur事件,需**要手动设置函数转换**,或者**使用定时器延时进行数据传输调用接口**
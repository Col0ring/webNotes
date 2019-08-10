# Vue.js

> 用法案例参考《Vue官方教程》

## 1.Vue的特性

**Vue是一套用于构建用户界面的渐进式框架,与其它大型框架不同的是,Vue 被设计为可以自底向上逐层应用,Vue拥有以下特性:**

- 组件化思想
- 模板的使用和数据渲染非常灵活，层次结构鲜明
- 简单的语法并能够简单快速构建一个项目
- 轻量级，体积小渲染速度更快
- Vue采用的脚手架工具为vue-cli
- 初期是尤雨溪维护，现在有加入的团队组织个人提供技术一同维护迭代更新
- Vue中指令和组件分得更清晰。指令只封装 DOM 操作,而组件代表一个自给自足的独立单元,都拥有有自己的视图样式和数据逻辑



## 2.绑定Vue对象

**Vue也是一个构造函数,通过new Vue()可以创建一个Vue对象,通过Vue对象进行对DOM元素以及内部的子孙元素的操作**

```html
<div id="div">
    {{msg}}<!--显示123-->
</div>
<script>
new Vue({
    el:"#div",
    /*
    在Vue中通过el属性来绑定一个DOM元素,从而让该DOM元素以及内部都绑定Vue的相关操作,一般是通过ID进行查	找,因为这样才能够精确绑定
    */
    data:{//data属性中包含着在el中使用的使用的变量或属性
        msg:123
    }
    //也可以使用函数形式的
    data(){
    return{
        msg:123
    };
    //methods属性包含着需要使用的方法
    methods:{
        show(){
            console.log(this.msg);
        }
    }
}
})
</script>
```

**注:在实例内部使用定义的属性或方法时不能直接使用,必须通过this来指定需要用的属性 **



### 2.1 vm.$mount

如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。可以使用 `vm.$mount()` 手动地挂载一个未挂载的实例

**注意:**这个方法返回实例自身,因而可以链式调用其它实例方法

```js
var MyComponent = Vue.extend({
  template: '<div>Hello!</div>'
})

// 创建并挂载到 #app (会替换 #app)
new MyComponent().$mount('#app')

// 同上
new MyComponent({ el: '#app' })

// 或者，在文档之外渲染并且随后挂载
var component = new MyComponent().$mount()
document.getElementById('app').appendChild(component.$el)
```



## 3.基本指令

### 3.1 v-text

**v-text,**该指令的用法同原生JS中的innerText,更新绑定元素内部的文本内容

```html
<div id="div">
<span v-text="msg"></span><!--不会有闪烁问题-->
<!-- 和下面的一样 -->
<span>{{msg}}</span>
</div>
```



### 3.2 v-html

**v-html,**该指令用法同原生JS的innerHTML,可以解析内部的HTML标签



### 3.3 v-cloak

**v-cloak**,该指令CSS一起使用,用于隐藏还没有开始编译的{{}}(插值表达式)直到编译完成,解决其闪烁问题

```CSS
[v-cloak] {
  display: none;/*如果没有起作用可能是由于优先级不够导致的,可以加上!important提高优先级*/
}
```

```html
<div v-cloak>
  {{ message }}
</div>
```



### 3.4 v-show

**v-show,**该指令用于显示和隐藏DOM元素,根据表达式之真假值,切换元素的display属性值

**注意:**

- 当条件变化时该指令触发过渡效果

- v-show指令不支持在< template>< /template>标签上写,也不支持v-else,一般来说,v-if 有更高的切换开销,而 v-show 有更高的初始渲染开销。因此,如果需要非常频繁地切换,则使用 v-show 较好,如果在运行时条件很少改变,则使用 v-if 较好



### 3.5 v-if,v-else与v-else-if

- **v-if,**该指令用于是否渲染DOM元素,传入一个布尔值,通过该布尔值的条件改变实现对于DOM元素的删除和重建,该指令不同于v-show,而是会完全销毁这个DOM元素

  **注意:**当条件变化时该指令触发过渡效果

- **v-else,**该指令用于**不需要任何表达式**,但是在该指令的前一个兄弟元素必须有v-if或者v-else-if指令,该指令的用处是为v-if 或者 v-else-if添加else的选项

  ```html
  <div v-if="Math.random() > 0.5">
    Now you see me
  </div>
  <div v-else>
    Now you don't
  </div
  ```

- **v-else-if,**该指令用于用于v-if的else if选项,前一个兄弟元素必须要v-if指令,并且该指令可以链式调用,也就是说可以调用多次v-else-if指令

  ```html
  <div v-if="type === 'A'">
    A
  </div>
  <div v-else-if="type === 'B'">
    B
  </div>
  <div v-else-if="type === 'C'">
    C
  </div>
  <div v-else>
    Not A/B/C
  </div>
  ```



### 3.6 v-for

**v-for,**该指令可以基于源数据多次渲染元素或模板块,用于循环遍历某个数组或对象,如果只用一个变量代表得到是数组或对象的value值

**注意:**在内部必须使用固定的i**tem in/of items**形式的特殊语法(下面的in都可以用of代替,并且更符合实际)

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }} <!--Foo Bar-->
  </li>
</ul>
<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
</script>
```

- **遍历数组时其实还支持第二个变量,代表的是遍历数组的索引值**

  ```html
  <ul id="example-2">
    <li v-for="(item, index) in items">
      {{ parentMessage }} - {{ index }} - {{ item.message }}
      <!--
  		Parent-0-Foo
  		Parent-1-Bar
  	-->
    </li>
  </ul>
  <script>
  var example2 = new Vue({
    el: '#example-2',
    data: {
      parentMessage: 'Parent',
      items: [
        { message: 'Foo' },
        { message: 'Bar' }
      ]
    }
  })
  </script>
  ```

- **遍历对象时还可以接受第二三两个变量,分别代表了键名和索引值**

  ```html
  <ul id="v-for-object" class="demo">
   <div v-for="(value, key, index) in object">
    {{ index }}. {{ key }}: {{ value }}
    <!--
      0. firstName: John
  	1. lastName: Doe
      2. age: 30
     -->
  </div>
  </ul>
  <script>
      new Vue({
          el: '#v-for-object',
          data: {
              object: {
                  firstName: 'John',
                  lastName: 'Doe',
                  age: 30
              }
          }
  })
  </script>
  ```

- **该指令还可以遍历一个整数,遍历的量会分别从1开始得到赋值直到到这个整数的值**

  ```html
  <p v-for="count in 10">{{count}}</p>
  //1 2 3 4 5 6 7 8 9 10
  ```

**注意:**当v-for和v-if处于同一节点时,v-for 的优先级比 v-if更高，这意味着 v-if将分别重复运行于每个 v-for循环中,当想为仅有的一些项渲染节点时,这种优先级的机制会十分有用

```html
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
<!--
官方并不建议v-for和v-if指令一起使用,如果要使用这种方式,就进行条件渲染
-->

<!--
如果想有条件才继续v-for指令,则可以将v-if置于外层元素(或<template>)上
-->
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
<!--
条件渲染
v-if如果想只写一个而同时切换多个元素,可以在最外成包裹一层<template></template>标签,在这个标签上面
使用v-if,在渲染的时候并不会将<template>标签渲染出来
-->
```



### 3.7 v-bind

**v-bind**,该指令用于动态绑定一个或多个特性,该方法绑定的特性与普通的属性效果一致,只是其内部会被解析为JS表达式,而不是普通特性一样会是一个字符串

**修辞符:**

- **.prop** - 被用于绑定 DOM 属性(property)
- **.camel** -  将短横线命名法特性名转换为驼峰命名法
- **.sync** - 会扩展成一个更新父组件绑定值的 v-on 侦听器

**注意:**

- 该指令有其简写写法,通过:来代替v-bind:
- 该指令可以动态对绑定的参数进行改变,参数用[]括起来

```html
<!-- 绑定一个属性 -->
<img v-bind:src="imageSrc">

<!-- 动态特性名 (2.6.0+) -->
<button v-bind:[key]="value"></button>

<!-- 缩写 -->
<img :src="imageSrc">

<!-- 动态特性名缩写 (2.6.0+) -->
<button :[key]="value"></button>

<!-- 内联字符串拼接 -->
<img :src="'/path/to/images/' + fileName">

<!-- class 绑定 -->
<div :class="{ red: isRed }"></div><!--对象通过布尔值确定是否传入class中-->
<div :class="[classA, classB]"></div><!--数组会直接将类名传入到class中-->
<div :class="[classA, { classB: isB, classC: isC }]"><!--对象和数组可以混用-->

<!-- style 绑定 -->
<div :style="{ fontSize: size + 'px' }"></div><!--通过对象写入每个属性-->
<div :style="[styleObjectA, styleObjectB]"></div><!--数组的成员中实际是一个个对象-->

<!-- 绑定一个有属性的对象 -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- 通过 prop 修饰符绑定 DOM 属性 -->
<div v-bind:text-content.prop="text"></div>

<!-- prop 绑定。“prop”必须在 my-component 中声明。-->
<my-component :prop="someThing"></my-component>

<!-- 通过 $props 将父组件的 props 一起传给子组件 -->
<child-component v-bind="$props"></child-component>

<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```



### 3.8 v-on

**v-on**,该指令用于绑定监听事件,表达式可以是一个方法的名字或一个内联语句(也就是说在传入事件的时候可以选择加()可以选择不加(),推荐在传入参数的时候添加,在不传参的时候写事件名)

**参数:**该指令的参数为原生JS中的事件名,只不过没有on

**修饰符**

- **.stop** - 调用 event.stopPropagation(),会阻止本元素上的事件进行冒泡传播
- **.prevent** - 调用 event.preventDefault(),不能和.passive一起使用
- **.capture** - 添加事件侦听器时使用 capture 模式,父元素会执行在子元素上进行的同名事件
- **.self** - 只当事件是从侦听器绑定的元素本身触发时才触发回调,只会真正自己触发的事件才会进行,和.stop是有区别的,这个只会阻止自己的冒泡,但不会阻止该元素的父元素事件的冒泡进行
- **.{keyCode | keyAlias}** - 只当事件是从特定键触发时才触发回调,可以是表示键盘字符的数字或者表示特效事件的按键修饰符
- **.native** - 监听组件根元素的原生事件,一个Vue实例内部通过只能v-on只能绑定自己内部的方法,不能绑定原生DOM事件的方法,通过该修饰符就可以使用原生JS的事件方法了
- **.once** - 只触发一次回调
- .**left** -  只当点击鼠标左键时触发
- **.right** -  只当点击鼠标右键时触发
- **.middle** - 只当点击鼠标中键时触发
- **.passive** - 不用查找是否阻止默认事件的请求直接进行操作。如在滚动页面时的onscroll事件,每次触发事件时浏览器都会查看是否有阻止默认滚动事件的操作,但是如果本来就没有进行这个操作,在滚动的时候就会出现卡的的情况,因为在触发滚动条滚动时总会先查找请求,这个修饰符就是告诉浏览器不用进行查找直接进行滚动操作。因为作用的冲突,所以不能和.prevent一起使用

**注意:**

- 该指令有简写形式,通过@来代替v-on:
- 该指令可以动态对绑定的参数进行改变,参数用[]括起来

```html
<!-- 方法处理器 -->
<button v-on:click="doThis"></button>

<!-- 动态事件 (2.6.0+) -->
<button v-on:[event]="doThis"></button>

<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 缩写 -->
<button @click="doThis"></button>

<!-- 动态事件缩写 (2.6.0+) -->
<button @[event]="doThis"></button>

<!-- 停止冒泡 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认行为 -->
<button @click.prevent="doThis"></button>

<!-- 阻止默认行为，没有表达式 -->
<form @submit.prevent></form>

<!--  串联修饰符 -->
<button @click.stop.prevent="doThis"></button>

<!-- 键修饰符，键别名 -->
<input @keyup.enter="onEnter">

<!-- 键修饰符，键代码 -->
<input @keyup.13="onEnter">

<!-- 点击回调只会触发一次 -->
<button v-on:click.once="doThis"></button>

<!-- 对象语法 (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>

<!-- 组件中的自定义事件 -->
<my-component @my-event="handleThis"></my-component>

<!-- 内联语句 -->
<my-component @my-event="handleThis(123, $event)"></my-component>

<!-- 组件中的原生事件 -->
<my-component @click.native="onClick"></my-component>
```



### 3.9 v-model

**v-model**,该指令用于表单内的标签进行双向的数据绑定

**注意:**v-model只能绑定给input,textarea,select等表单元素和自定义的组件中,除此之外不能在其他标签上绑定

**修辞符:**

- **.lazy** - 取代 input监听 change 事件,默认在用户写入值时是使用的input事件,也就是当值输入完成后才会触发事件,而change是一些有输入法的语言在值还没有输入时就时刻监听改变
- **.number**- 输入字符串转为有效的数字
- **.trim** - 输入首尾空格过滤

**用法:**

- **如果是对input文本框和textarea绑定的**,绑定的值会根据输入的内容,就是将内部的变量的值改为value中的值

- **如果是对复选框绑定的**

  - **如果绑定的变量不是数组**,会根据复选框是否被选中而改变为false或true,即使原来不是布尔值也会被强制转换为布尔值,这是因为双向的数据绑定,如果是有多个复选框,那么则会一起被选中或不选中
  - **如果变量是数组**则会将value属性中的值(没有写value属性会传入null)传入该数组作为其中的一个成员,Vue会根据数组内部的值来判断是否选中该复选框(内部其实就是这样运作的),如果value值一样会有多个复选框被选中,再次点击就会将该值删去然后取消复选框的选中

  ```html
  <div id='example'>
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
    <br>
    <span>Checked names: {{ checkedNames }}</span>
  </div>
  
  <!--也可以在被选中和没被选择直接设置不同的值-->
  <input
    type="checkbox"
    v-model="toggle"
    true-value="yes"
    false-value="no"
  >
  <!--
   当选中时
  vm.toggle === 'yes'
   当没有选中时
  vm.toggle === 'no'
  注意:点击了才会改变值,如果没有点击过而变量本身有值的话就不会是fasle-value的值
  -->
  ```

- **如果是对单选按钮进行绑定的**,变量值会随着选中单选框的value值而变化,如果变量的值刚开始就是一个单选框的value值,那么就会自动选中这个单选框
  **注意:**

  - 单选框和复选框即使不写相同的name而只绑定了相同的modle也会认做是一类单选框的,但是最好还是将name写上
  - 单选框和复选框都可以通过v-bind:value绑定的value值来设置值自身的value值

- **如果是对选择框select进行绑定的**,绑定的变量的值会随着选中的option选项内部的内容而发生变化,如果option没有写value属性,该变量会变成< option>< /option>内部的值,如果有value属性,变量会变成value值而不是option的内容

  ```html
  <div id="example">
    <select v-model="selected">
      <option disabled value="">请选择</option>
      <option>A</option>
      <option>B</option>
      <option>C</option>
    </select>
    <span>Selected: {{ selected }}</span>
  </div>
  <script>
  new Vue({
    el: '#example',
    data: {
      selected: ''
    }
  })
  </script>
  ```

  **注意:**在s多选时要绑定一个数组,将上方的selected:[]，这样在选入的时候就能将每个选择的选项的值加到变量中去,还可以是一个对象,但是要通过v-bind:value绑定

  ```html
  <select v-model="selected">
      <!-- 内联对象字面量 -->
    <option v-bind:value="{ number: 123 }">123</option>
  </select>
  ```




### 3.10 v-pre

**该指令不接受任何表达式,使用了该指令的元素Vue在编译时跳过这个元素和它的子元素的编译过程,可以用来显示原始的模板{{}}标签,跳过大量没有指令的节点会加快编译**



### 3.11 v-once

**该指令不需要表达式,只渲染元素和组件一次。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能**



## 4.绑定class和style

### 4.1 绑定class

- 通过数组的书写传入**:class**形式绑定的类名

  ```html
  <h1 :class="['red','big']">Vue</h1>
  ```

- 通过对象书写的形式绑定类名,对象中的属性名就是要传入的class名,而属性名对应的属性值为一个布尔值,可以是变量,如果是真就作为一个类名传入class中,如果是假的就不计入class中

  ```html
  <h1 :class="{red:true,big:true}">Vue</h1>
  ```

  **注意:**传入一个对象的时候完全可以在Vue的实例创建的时候传入变量,直接在class中写入变量名

- 传入三目表达式进行切换是否用该类名的判断

  ```html
  <h1 :class="['red',isActive?'big':'small']">Vue</h1>
  ```

- 在数组中使用对象,可以使用该方法代替繁琐的三目表达式

  ```html
  <h1 :class="['red',{big:isActive}]">Vue</h1>
  ```

  

### 4.2 绑定style

**绑定的style样式可以通过驼峰命名法会短横线分割法进行命名(短横线命名需要在外层用单引号包住)**

- 直接在元素上同过**:style**的形式,通过对象的形式书写

  ```html
  <h1 :style="{color:'red','font-size':'40px'}">Vue</h1>
  ```

- 将样式定义在data中,再引用到**:style**中

  ```html
  <h1 :style="foo">Vue</h1>
  ```

  ```js
  data:{
      foo:{color:'red','font-size':'40px'}
  }
  ```

- 在**:style**中通过数组引用多个data中的样式对象,其实每个对象就相当于一个class,只是需要在data中设置

  ```html
  <h1 :style="[foo,foo2]">Vue</h1>
  ```

  ```js
  data:{
      foo:{color:'red','font-size':'40px'},
      foo2:{fontWight:200}//驼峰命名法也可以
  }
  ```



## 5.过滤器

**Vue可以通过自定义过滤器,将需要的文本格式化输出**

**注意:**

- 过滤器只能写在插值表达式或者v-bind表达式中,并且过滤器需要添加到要添加的JS表达式的后方并用管道符|分割

  ```html
  <!-- 在双花括号中 -->
  {{ message | capitalize }}
  
  <!-- 在 `v-bind` 中 -->
  <div v-bind:id="rawId | formatId"></div>
  
  <!-- 管道符后方的都是声明的过滤器函数 -->
  ```

- 每个过滤器函数会将接收到的表达式作为函数内部的第一个参数,如果在使用时传入参数,那么传入的参数会依次成为第二个第三个等参数

  ```html
  {{ message | filterA('arg1', arg2) }}
  ```

- 多个过滤器可以进行串联操作,将传入的表达式依次进行过滤

  ```html
  {{ message | filterA | filterB }}
  ```

### 5.1 全局过滤器

**全局的过滤器可以在任意的Vue实例中使用**

```js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})
```



### 5.2 私有过滤器

**私有的过滤器只能在自身的本地实例中使用**

```js
new Vue({
    data:{},
    methods{},
    filters: {
        capitalize: function (value) {
            if (!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
        }
    }  
})
```



## 6.按键修饰符

**在绑定了键盘或鼠标的点击事件后,可以通过按下的按键触发要进行的事件,这时需要在后方添加表示按键的修辞符**

### 6.1按键码

- **可以通过keyCode的值来绑定需要触发的按键**

  ```html
  <input v-on:keyup.13="submit">
  ```

- **部分特殊的按键码可以使用别名**
  - **.enter**
  - **.tab**
  - **.delete**(捕获“删除”和“退格”键)
  - **.esc**
  - **.space**
  - **.up**
  - **.down**
  - **.left**
  - **.right**

- **也可以自定义按键修饰符的别名**

  ```js
  Vue.config.keyCodes.f1 = 112;//使用Vue对象上的全局属性config.keyCodes来定义别名
  // 在绑定时就可以使用v-on:keyup.f1直接对f1按键进行操作了
  ```

  

### 6.2 系统修饰符

**系统修辞符监听仅在同时按下了绑定键盘或鼠标按钮时才会触发事件**

- **键盘**
  - **.ctrl**
  - **.alt**
  - **.shift**
  - **.meta**

  **注意:**在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

  ```html
  <!-- Alt + C -->
  <input @keyup.alt.67="clear">
  
  <!-- Ctrl + Click -->
  <div @click.ctrl="doSomething">Do something</div>
  ```

- **鼠标**

  - **.left**
  - **.right**
  - **.middle**

**注:**修饰键与常规按键不同，在和 `keyup` 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 `ctrl` 的情况下释放其它按键，才能触发 `keyup.ctrl`。而单单释放 `ctrl` 也不会触发事件。如果只想要单独触发,则需要使用keycode编码或**.exact**修饰符

- **.exact**

  .exact 修饰符允许用户控制由精确的系统修饰符组合触发的事件

  ```html
  <!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
  <button @click.ctrl="onClick">A</button>
  
  <!-- 有且只有 Ctrl 被按下的时候才触发 -->
  <button @click.ctrl.exact="onCtrlClick">A</button>
  
  <!-- 没有任何系统修饰符被按下的时候才触发 -->
  <button @click.exact="onClick">A</button>
  ```

  

## 7.自定义指令

**自定义指令可以通过全局或局部的方式创建,在使用时需要在前面加上v-的标识符表示时一个指令**

### 7.1 全局指令

**通过全局的Vue的Vue.directive()可以创建一个能用在整个Vue实例内的指令**

```js
// 注册一个全局自定义指令 v-focus
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时聚焦
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```



### 7.2 局部指令

**通过局部创建的directives属性可以创建多个内部的指令,该指令只能用在自身的Vue实例中**

```js
new Vue({
    data:{},
    methods{},	
        directives: {
        focus: {
        	// 指令的定义
        	inserted: function (el) {
   			 el.focus()
			}
		}
	}
}
```



### 7.3 钩子函数

**在定义的一个指令对象中可以绑定多个可选的钩子函数,这些钩子函数会在特定的时刻自动调用**

#### 7.3.1 函数

- **bind：**只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
- **inserted：**被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)
- **update：**所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新

- **componentUpdated：**指令所在组件的 VNode 及其子 VNode全部更新后调用
- **unbind：**只调用一次，指令与元素解绑时调用

**函数简写**

当只想要对bind与update设置相同行为而不设置其他的钩子函数时,可以直接写成一个函数,不用写一个包含了钩子函数的对象

```js
//全局
Vue.directive('focus', function(el){
    el.focus();
})
//局部
new Vue({
    data:{},
    methods{},	
        directives: {
        function (el) {
   			 el.focus()
		}			
	}
}
```



#### 7.3.2 钩子函数的参数

- **el**：指令所绑定的元素,可以用来直接操作DOM

- **binding**：一个对象，还包含以下属性

  - `name`：指令名，不包括 `v-` 前缀

  - `value`：指令的绑定值，会将参数的表达式进行解析,如果内部绑定的参数为1+1,那么绑定后的值就为2

    **注意:**如果想要传递多个值,可以传入一个对象,通过对象进行获取多个值

    ```html
    <div v-demo="{ color: 'white', text: 'hello!' }"></div>
    <script>
        Vue.directive('demo', function (el, binding) {
          console.log(binding.value.color) // => "white"
          console.log(binding.value.text)  // => "hello!"
        })
    </script>
    ```

  - `oldValue`：指令绑定的前一个值，仅在 **update** 和 **componentUpdated** 钩子函数中中可以使用,无论值是否改变都可用

  - `expression`：字符串形式的指令表达式,不会对表达式进行解析,如果参数为1+1那么值还为 "1 + 1"

  - `arg`：传给指令的参数,可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。

  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`

- **vnode**：Vue 编译生成的虚拟节点

- **oldVnode**：上一个虚拟节点，仅在 **update** 和 **componentUpdated** 钩子函数中可以使用

```html
<div id="hook-arguments-example" v-demo:foo.a.b="message"></div>
```

```js
Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})

new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'hello!'
  }
})
/*
name: "demo"
value: "hello!"
expression: "message"
argument: "foo"
modifiers: {"a":true,"b":true}
vnode keys: tag, data, children, text, elm, ns, context, fnContext, fnOptions, fnScopeId, key, componentOptions, componentInstance, parent, raw, isStatic, isRootInsert, isComment, isCloned, isOnce, asyncFactory, asyncMeta, isAsyncPlaceholder
*/
```



## 8.生命周期函数

![Vue å®ä¾çå½å¨æ](https://cn.vuejs.org/images/lifecycle.png)

**生命周期函数写在一个Vue实例内部,这些函数直接作为一个内部的方法创建,根据不同的状况会自动的调用**

### 8.1 Vue实例创建阶段

- var vm=new Vue(),创建出一个Vue实例对象

- **beforeCreat**，当Vue实例被完全创建出来之前,就会执行该函数，这是表示刚初始化了一个空的Vue对象,这时候这个对象身上只有一些

  **注意:**在beforeCreat生命周期函数执行的时候,data和methods等中的数据还没有初始化,所以在这里面调用这些data或methods等中的属性和方法会报错

- **created**,在该函数中,data和methods等数据已经被初始化好了,如果要调用methods中的方法或者data中的数据,最早只能在created中进行操作

  **注:**如果要发Ajax请求尽量在这个阶段发送

- 在这两个生命周期函数之间进行Vue的编译模板,把Vue代码中的那些指令进行执行,最终在内存中生成一个编译好的最终模板字符串,然后把这个字符串渲染为内存中的DOM,但是此时只是在内存中渲染DOM,还没有将其挂载到页面中去

- **beforeMount**,在该函数中模板已经在内存中了,但是还没有把模板渲染到页面中,也就是说{{}}中的内容还没有被解析,页面中的元素还没有真正被替换,只是写了一些模板字符串

- 将内存中的DOM挂载到页面中去

- **mounted,**表示内存中的DOM已经挂载到页面中了,用户已经可以看到渲染好的页面了

  **注意:**

  - mounted是实例创建期间执行的最后一个生命周期函数,当执行完mounted就表示实例已经完全被创建好了
  - 如果要通过某些插件操作页面上的DOM节点,最早要在mounted中进行



### 8.2 Vue实例运行阶段

**下面两个生命周期必须要数据发生改变时才会进行,会根据data数据的改变触发0次或多次**

- **beforeUpdate,**这个函数表明Vue实例在运行时数据已经被更新,而页面还没有被更新的时间节点,当执行该函数时,页面中显示的数据还是没有更新前的数据,而data中的数据是最新的,页面还没有和数据实现同步
- 在这两个函数之间,会根据data中的最新数据,重新渲染出一份最新的内存DOM树,当最新的内存DOM树被更新之后,会把最新的内存DOM树重新渲染到真实的页面中去,完成数据从data到页面视图的更新
- **updated,**当这个函数执行时证明data中的数据已经和页面中的数据保存同步更新了



### 8.3 Vue实例销毁阶段

- **beforeDestroy,** 当执行该函数时,Vue实例就已经从运行阶段进入到了销毁阶段,实例上的所以属性如data,methods等都还是处于可用状态,还没有真正的执行销毁
- **destroyed,**当执行到该函数时,Vue实例已经被完全销毁,此时所有实例中的属性都不可用了



## 9.过渡与动画

**Vue中可以通过`transition`组件来实现过渡效果Vue 在插入,更新或者移除 DOM 时,提供多种不同方式的应用过渡效果**

- 在 CSS 过渡和动画中自动应用 class
- 可以配合使用第三方 CSS 动画库，如 Animate.css
- 在过渡钩子函数中使用 JavaScript 直接操作 DOM
- 可以配合使用第三方 JavaScript 动画库，如 Velocity.js

### 9.1 过渡的类名

#### 9.1.1 自身实现的类名

**这些类名是transition组件中自定好的,在使用的时候只需要在CSS中写入特定的类名和样式就可以了**

- **v-enter**：定义进入过渡的开始状态。在元素被插入之前生效，此时元素还没有进入过渡，在元素被插入之后的下一帧移除
- **v-enter-active**：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数
- **v-enter-to**: 定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 **v-enter** 被移除)，在过渡/动画完成之后移除
- **v-leave**: 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除
- **v-leave-active**：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数
- **v-leave-to**:定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 **v-leave** 被删除)，在过渡/动画完成之后移除

**注:**

- **一般都是使用v-enter,v-leave-to与v-enter-active和v-leave-active的组合**

- **对于这些在过渡中切换的类名来说,如果使用一个没有名字的 `<transition>`,则 `v-` 是这些类名的默认前缀,如果使用了 `<transition name="my-transition">`,那么 `v-enter` 会替换为 `my-transition-enter**`,通过这个可以绑定:name实现动态过渡的效果

```html
<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>

<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>

<script>
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
</script>
```

- **动画效果同过渡效果,只是在动画中v-enter这个类名在插入DOM中后不会立刻被删除,**而是在**animationend**事件触发时才会被删除

  **注:**动画可以实现多个阶段的样式

  ```html
  <style>
  .bounce-enter-active {
    animation: bounce-in .5s;
  }
  .bounce-leave-active {
    animation: bounce-in .5s reverse;
  }
  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(1);
    }
  }
  </style>
  
  <div id="example">
    <button @click="show = !show">Toggle show</button>
    <transition name="bounce">
      <p v-if="show">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.</p>
    </transition>
  </div>
  
  <script>
  new Vue({
    el: '#example-2',
    data: {
      show: true
    }
  })
  </script>
  ```

  

#### 9.1.2  自定义过渡的类名

**可以通过引入第三方库实现过渡效果,这时需要使用自定义过渡的类名来加第三方库的过渡效果使用**

可以通过以下特性来自定义过渡类名,这些类名分别也对应着上方的执行时期,它们的优先级高于普通的类名

- **enter-class**
- **enter-active-class**
- **enter-to-class**
- **leave-class**
- **leave-active-class**
- **leave-to-class**

**在这些自定义的属性中可以写入要引入的第三方库的CSS样式就能实现效果**

```html
<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">

<div id="example">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">hello</p>
  </transition>
</div>

<script>
new Vue({
  el: '#example',
  data: {
    show: true
  }
})
</script>
```



### 9.2 初始渲染过渡

**为`transition`组件添加一个简单的属性appear就能够实现在刚开始渲染的时候就使用一次`enter-active-class`来实现开始渲染列表的过渡效果**

**初始渲染时也可以使用自定义的CSS类名进行初始渲染**

+ **appear-class**
+ **appear-active-class**
+ **appear-to-class**

**注意:**使用自定义类名进行渲染时也需要添加appear属性

```html
<transition
  appear
  appear-class="custom-appear-class"
  appear-to-class="custom-appear-to-class"
  appear-active-class="custom-appear-active-class"
>
  <!-- ... -->
</transition>

<!--当然也可以使用JS的钩子函数-->
<transition
  appear
  v-on:before-appear="customBeforeAppearHook"
  v-on:appear="customAppearHook"
  v-on:after-appear="customAfterAppearHook"
  v-on:appear-cancelled="customAppearCancelledHook"
>
  <!-- ... -->
</transition>
```



### 9.3 显性定义过渡时间

**默认情况下，Vue 会等待其在过渡效果的根元素的第一个 `transitionend` 或 `animationend` 事件。**然而也可以不这样设定——比如，我们可以拥有一个精心编排的一系列过渡效果，其中一些嵌套的内部元素相比于过渡效果的根元素有延迟的或更长的过渡效果。在这种情况下,**可以用 `<transition>` 组件上的绑定的 `duration` 属性定制一个显性的过渡持续时间 (以毫秒为单位)**

```html
<transition :duration="1000">...</transition>
```

**可以传入一个对象分别对移入和移除的时间进行设置**

```html
<transition :duration="{ enter: 500, leave: 800 }">...</transition>
```



### 9.4 JS钩子

**除了通过CSS实现过渡效果,还可以用JS中的钩子函数实现过渡效果**,这些钩子可以结合CSS使用,也可以全部单独使用

```html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>
```

```js
// ...
methods: {
  // --------
  // 进入中
  // --------

  beforeEnter: function (el) {
    // ...
  },
  // 当与 CSS 结合使用时
  // 回调函数 done 是可选的
  enter: function (el, done) {
    // ...
    el.offsetWidth;
     //玄学操作,不加这个设置全JS样式时,动画不会有过渡效果出现,可以认为是这个操作会强制动画刷新
    done()
     //done()所代表的函数就是下一个结束afterEnter的函数,只有调用了这个回调函数才会执行下面的方法
  },
  afterEnter: function (el) {
    // ...
  },
  enterCancelled: function (el) {
    // ...
  },

  // --------
  // 离开时
  // --------

  beforeLeave: function (el) {
    // ...
  },
  // 当与 CSS 结合使用时
  // 回调函数 done 是可选的
  leave: function (el, done) {
    // ...
    done()
  },
  afterLeave: function (el) {
    // ...
  },
  // leaveCancelled 只用于 v-show 中
  leaveCancelled: function (el) {
    // ...
  }
}
```

**注意:**

- 推荐对于仅使用 JavaScript 过渡的元素添加 `v-bind:css="false"`，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响
- 当只用 JavaScript 过渡的时候，**在 enter 和 leave 中必须使用 done 进行回调**。否则，它们将被同步调用，过渡会立即完成。



### 9.5 多个元素过渡

对于原生标签可以使用 `v-if`和`v-else` 来实现多标签过渡效果

**注意:**当有相同标签名的元素切换时，需要通过 `key` 特性设置唯一的值来标记以让 Vue 区分它们，否则 Vue 为了效率只会替换相同标签内部的内容

```html
<transition>
  <button v-if="isEditing" key="save">
    Save
  </button>
  <button v-else key="edit">
    Edit
  </button>
</transition>
<!--也可以也可以通过给同一个元素的 key 特性设置不同的状态来代替 v-if 和 v-else-->
<transition>
  <button v-bind:key="isEditing">
    {{ isEditing ? 'Save' : 'Edit' }}
  </button>
</transition>
```



**过渡模式**

**通过mode属性来定义多个元素过渡时的过渡模式**

- **in-out**：新元素先进行过渡，完成之后当前元素过渡离开
- **out-in**：当前元素先进行过渡，完成之后新元素过渡进入

```html
<transition  mode="out-in"></transition>
```



### 9.6 列表过渡

**渲染一整个列表时,如用v-for来渲染,就需要使用< transition-group> 组件**、

**注意:**

- 不同于 < transition>组件，< transition-group>组件会以一个真实元素呈现,默认为一个 `<span>`标签,可以通过 tag 特性更换为其他元素

  **如:**内部是li列表,那么可以用`<transition-grounp tag="ul">`来让该组件被渲染时表现为ul

- 过渡模式不可用，因为不再相互切换特有的元素

- 内部元素必须要有提供唯一的 key 属性值

#### 9.6.1 添加与删除列表

**通过对一个`transiton-grounp`用过渡的类名和实现当个成员在过渡时的效果**

```html
<style>
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to
/* .list-leave-active for below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
</style>

<div id="list-demo" class="demo">
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" v-bind:key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>

<script>
new Vue({
  el: '#list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
  }
})
</script>
```



#### 9.6.2 排序过渡列表

**上方的添加与删除列表只会对进行操作的成员自身产生过渡效果,对组件中的其他成员没有过渡效果,如果要想通过过渡效果进行添加,需要使用v-move的类名写入CSS样式**,该效果会在组件的成员改变其定位的时候起作用,所以要想起过渡效果还需要在过渡时将元素的定位改变为绝对定位.也可以通过 `name` 属性来自定义前缀,也能通过 `move-class` 属性CSS效果

```html
<style>
.flip-list-move {
  transition: transform 1s;
}
</style>

<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>

<div id="flip-list-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <transition-group name="flip-list" tag="ul">
    <li v-for="item in items" v-bind:key="item">
      {{ item }}
    </li>
  </transition-group>
</div>

<script>
new Vue({
  el: '#flip-list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9]
  },
  methods: {
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
</script>
```



#### 9.6.3 交错过渡列表

**通过JS钩子来实现列表的过渡效果,使data属性与JS进行数据通信,就可以实现列表的交错过渡效果**

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>

<div id="staggered-list-demo">
  <input v-model="query">
  <transition-group
    name="staggered-fade"
    tag="ul"
    v-bind:css="false"
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:leave="leave"
  >
    <li
      v-for="(item, index) in computedList"
      v-bind:key="item.msg"
      v-bind:data-index="index"
    >{{ item.msg }}</li>
  </transition-group>
</div>

<script>
new Vue({
  el: '#staggered-list-demo',
  data: {
    query: '',
    list: [
      { msg: 'Bruce Lee' },
      { msg: 'Jackie Chan' },
      { msg: 'Chuck Norris' },
      { msg: 'Jet Li' },
      { msg: 'Kung Fury' }
    ]
  },
  computed: {
    computedList: function () {
      var vm = this
      return this.list.filter(function (item) {
        return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
      })
    }
  },
  methods: {
    beforeEnter: function (el) {
      el.style.opacity = 0
      el.style.height = 0
    },
    enter: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 1, height: '1.6em' },
          { complete: done }
        )
      }, delay)
    },
    leave: function (el, done) {
      var delay = el.dataset.index * 150
      setTimeout(function () {
        Velocity(
          el,
          { opacity: 0, height: 0 },
          { complete: done }
        )
      }, delay)
    }
  }
})
</script>
```



## 10.组件

### 10.1 什么是组件

**组件的出现是为了拆分Vue实例的代码量,能够让我们以不同的组件来划分不同的功能模板,将来需要什么样的功能,只需要调用对应的组件就可以了**



**组件化与模块化的区别**

- 模块化是从代码逻辑的角度进行划分的;方便代码分层开发,保证每个功能模块的职能单一
- 组件化是从UI界面的角度进行划分的,前端的组件化是为了方便UI组件的重用



### 10.2 创建组件

**组件的注意事项:**

- **每个组件必须只有一个根元素,**可以将模板的内容包裹在一个父元素内来解决这个问题

- 组件中可以有自己的data数据,使用方式同Vue实例中一样,**但是组件的data只能是一个方法,而且必须要返回一个包含了数据的对象,**因为需要每个组件之间相互独立,如果不是一个函数,那么每个组件就会影响其他的组件

  **如果要想同时影响其他数据,可以创建一个外部的镀锡,每次data返回这个对象**

#### 10.2.1 组件名

**在创建一个组件时,我们始终需要给该组件一个名字用作标识**,组件名的有多种写法

- **短横线命名法**,使用短横线命名法来命名组件在使用的时候也是直接将引用的组件名作为标签名来使用

- **驼峰命名法,**使用驼峰命名法命名的组件在引用这个组件时可以通过两种命名的方式来引用

  **注意:**当用作标签时只能使用短横线命名法,所以最好都使用短横线命名法来命名



#### 10.2.2 全局组件

**创建全局组件有三种方式,都是基于Vue.component()方法来创建的**

- 通过`Vue.extend()`的方式,创建一个含有template属性的子类,引用子类到`Vue.component()`创建的组件中去

  ```js
  var tem1 = Vue.extend({
  	template:'<h3>这是使用 Vue.extend 创建的组件</h3>'
  });
  Vue.component('myTem1',tem1);
  
  <!--可以通过组合的形式引用-->
  Vue.component('myTem1',Vue.extend({
  	template :'<h3>这是使用 Vue.extend 创建的组件</h3>'
  }));
  ```

- 直接使用`Vue.component()`方法传入一个对象

  ```js
  Vue.component('my-component-name', {
    template:'<h3>这是直接使用Vue.component创建的组件</h3>'
  });
  ```

- 在一个 `<script>` 标签中，并为其带上 `text/x-template`的类型，然后通过ID引用模板(这种方式有代码提示)

  ```html
  <script type="text/x-template" id="hello-world-template">
    <p>Hello hello hello</p>
  </script>
  
  <script>
  Vue.component('hello-world', {
    template: '#hello-world-template'
  })
  </script>
  ```



#### 10.2.3 私有组件

**通过在一个Vue实例中向components对象中添加属性可以在局部注册只能在该Vue实例内部使用的私有组件**

**对于 components`对象中的每个属性来说，其属性名就是自定义元素的名字，其属性值就是这个组件的选项对象**

```js
new Vue({
  el: '#app',
  components: {
      'component-a': {
          template:""
      },
      'component-b': {
          template:""
      },
  }
})
/*
	也可以通过一个普通的变量来定义组件
*/
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }

new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

**注意:**局部注册的组件在其子组件中不可以使用,如果要使用需要嵌套创建组件或通过webpack等导出

```js
//嵌套组件
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```



### 10.3 组件切换

#### 10.3.1 通过v-if切换组件

**通过一个标识可以进行两个组件之间的切换**

```html
<div id="app">
    <a href="#" @click.prevent="flag=true">登录</a>
    <a href="#" @click.prevent="flag=false">注册</a>
    <login v-if="flag"></login>
    <register v-else></register>
</div>

<script>
    Vue.component("login",{
        template:"<h2>登录<h2>"
    })
     Vue.component("register",{
        template:"<h2>注册<h2>"
    })
    new Vue({
        el:"#app",
        data:{
            flag:true
        }
    })
</script>
```



#### 10.3.2 通过component切换组件

**通过Vue中自带的component标签的:is特性来实现多个组件之间的切换**

```html
<div id="app">
    <a href="#" @click.prevent="clickName='login'">登录</a>
    <a href="#" @click.prevent="clickName='register'">注册</a>
    <component :is="clickName"></component>
</div>

<script>
    Vue.component("login",{
        template:"<h2>登录<h2>"
    })
     Vue.component("register",{
        template:"<h2>注册<h2>"
    })
    new Vue({
        el:"#app",
        data:{
            clickName:"login"
        }
    })
</script>
```



#### 10.3.3 过渡切换

**组件的切换比其他的东西容易很多,只需要使用过渡模式就能完成切换**

```html
<style>
    .component-fade-enter-active, .component-fade-leave-active {
        transition: opacity .3s ease;
    }
    .component-fade-enter, .component-fade-leave-to{
        opacity: 0;
    }
</style>

<div id="app">
    <a href="#" @click.prevent="clickName='login'">登录</a>
    <a href="#" @click.prevent="clickName='register'">注册</a>
    <transition mode="out-in">
    <component :is="clickName" name="component-fade"></component>
    </transition>
</div>

<script>
    Vue.component("login",{
        template:"<h2>登录<h2>"
    })
     Vue.component("register",{
        template:"<h2>注册<h2>"
    })
    new Vue({
        el:"#app",
        data:{
            clickName:"login"
        }
    })
</script>
```



### 10.4 Prop

**子组件是无法直接使用父组件的data中的数据的,需要通过props属性才能够使用父组件传入过来的值**

#### 10.4.1 向子组件传入数据

**在父组件的引用中填入属性名与对应的属性值,然后在子组件的props值填入与引用子组件的属性名一致的属性名,就可以在子组件中引用父组件的数据了，就如访问data中的值一样**

```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
<script>
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
</script>
```

**注意:**

- **-一个组件默认可以拥有任意数量的 prop,**任何值都可以传递给任何 prop,但是为了方便可以传入一个对象,调用对象的属性来使用传入的数据
- 组件中的所有props中的数据都是通过父组件传递给子组件**的,不要试图去修改props中传入的数据,**一般都是只读的,如果修改Vue会抛出报错信息
- **可以通过v-bind动态传递给prop一个值**
- 子组件的data数据并不是通过父组件传递过来的,而是子组件自身私有的,比如子组件通过Ajax请求回来的数据都可以放在data身上,**子组件的data上的数据都是可读可写的**
- **可以通过props接收父组件传过来的方法,甚至this对象(也就是父组件自身的this),通过该this甚至可以用父组件的data数据等**



#### 10.4.2 Prop的大小写

**HTML中的特性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当使用 DOM 中的模板时，驼峰命名法的prop名需要使用其等价的短横线分隔命名**

```html
<blog-post post-title="hello!"></blog-post>

<script>
Vue.component('blog-post', {
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
</script>
```



#### 10.4.3 Prop类型

**如果希望传入的prop都有指令的值类型,可以用对象的方式来,可以以对象形式列出 prop，这些属性的名称和值分别是 prop 各自的名称和类型。除此之外,还可以用于配置其他高级选项**

```js
//数组形式
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
//对象形式
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```

```js
// 简单语法
Vue.component('props-demo-simple', {
  props: ['size', 'myMessage']
})

// 对象语法，提供校验
Vue.component('props-demo-advanced', {
  props: {
    // 检测类型
    height: Number,
    // 检测类型 + 其他验证
    age: {
      type: Number,
      default: 0,//设置默认值
      required: true,//是否必须
      validator: function (value) {//对值进行验证
        return value >= 0
      }
    }
  }
})
```



#### 10.4.4 动态传递Prop

**可以通过v-bind的方式动态传递一个变量值给一个prop**

- **传入一个数字**

  ```html
  <!-- 即便 `42` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
  <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
  <blog-post v-bind:likes="42"></blog-post>
  
  <!-- 用一个变量进行动态赋值。-->
  <blog-post v-bind:likes="post.likes"></blog-post>
  ```

- **传入一个布尔值**

  ```html
  <!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
  <blog-post is-published></blog-post>
  
  <!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
  <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
  <blog-post v-bind:is-published="false"></blog-post>
  
  <!-- 用一个变量进行动态赋值。-->
  <blog-post v-bind:is-published="post.isPublished"></blog-post>
  ```

- **传入一个数组**

  ```html
  <!-- 即便数组是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
  <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
  <blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>
  
  <!-- 用一个变量进行动态赋值。-->
  <blog-post v-bind:comment-ids="post.commentIds"></blog-post>
  ```

- **传入一个对象**

  ```html
  <!-- 即便对象是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
  <!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
  <blog-post
    v-bind:author="{
      name: 'Veronica',
      company: 'Veridian Dynamics'
    }"
  ></blog-post>
  
  <!-- 用一个变量进行动态赋值。-->
  <blog-post v-bind:author="post.author"></blog-post>
  ```

- **传入一个对象的所有属性**

  如果想要将一个对象的所有属性都作为 prop 传入，你可以使用不带参数的 `v-bind`(取代 `v-bind:prop-name`)

  ```js
  post: {
    id: 1,
    title: 'My Journey with Vue'
  }
  ```

  ```html
  <blog-post v-bind="post"></blog-post>
  <!--与下面的方法等价-->
  <blog-post v-bind:id="post.id"  v-bind:title="post.title"></blog-post>
  ```



### 10.5 $emit

**子组件也无法使用组件的事件,要想传递给父组件事件,需要通过内置的$emit方法并传入事件的名字(也可以用props来传输事件),来向父级组件触发一个事件**

#### 10.5.1 向子组件传递事件

**在子组件的引用中绑定事件的自定义名字,然后绑定父组件的事件,在子组件中通过抛出事件$emit来使用父组件的事件**

```html
<!--引用子组件-->
<blog-post v-on:enlarge-text="doSomething"></blog-post>

<!--子组件-->
<script>
Vue.component("enlarge-text",{
	template:"<button v-on:click="$emit('enlarge-text')">Enlarge text</button>"
})
</script>
```



#### 10.5.1 向父组件抛出值

**子父组件的函数中写入参数,那么当子组件使用$emit的时候可以传入后面多个参数来将值传递给父组件的参数,从而实现子组件向父组件传值**

```html
<div id="app">
    <blog-post
     v-on:enlarge-text="onEnlargeText"
   	 ></blog-post>
</div>

<script>
    Vue.component("blog-post",{
        template:"<button v-on:click="$emit('enlarge-text',0.1)">Enlarge text</button>"
    })
    new Vue({
        el="#app",
        data:{
        	postFontSize:1
    	}
        methods: {
            onEnlargeText: function (enlargeAmount) {
                this.postFontSize += enlargeAmount
        }
       }  
    })
</script>
```

**注意:**虽然子组件能触发父组件的方法或值,但是这些方法中的this指向依然是父组件,子组件只是使用的这些方法,同时传递了一些参数



### 10.6 $on与$once

- **$on属性用于监听在当前实例上的自定义事件,事件可以由`vm.$emit`触发。回调函数会接收所有传入事件触发函数的额外参数**

  **注:**用该方法实现非父子组件传值

- **$once属性与$on一致,只不过只会触发一次,触发一次后移除该事件**

```js
vm.$on('test', function (msg) {
  console.log(msg)
})
vm.$once('test', function (msg) {
  console.log(msg)
})

vm.$emit('test', 'hi')
// => "hi"
```

**注意:**在组件化中通过创建一个中间的Vue实例可以进行中转完成父子组件的事件传值



### 10.7 $ref

**可以使用ref属性为一个组件或是子元素赋予一个引用的ID,通过这个引用的ID,父组件可以直接通过`this.$refs`访问这个组件或子元素,这种方式是父组件主动获取子组件的值或方法**

```html
  <div id="app">
    <div>
      <input type="button" value="获取元素内容" @click="getElement" />
      <!-- 使用 ref 获取元素 -->
      <h1 ref="myh1">这是一个大大的H1</h1>

      <hr>
      <!-- 使用 ref 获取子组件 -->
      <my-com ref="mycom"></my-com>
    </div>
  </div>

  <script>
    Vue.component('my-com', {
      template: '<h5>这是一个子组件</h5>',
      data() {
        return {
          name: '子组件'
        }
      }
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        getElement() {
          // 通过 this.$refs 来获取元素
          console.log(this.$refs.myh1.innerText);
          // 通过 this.$refs 来获取组件
          console.log(this.$refs.mycom.name);
        }
      }
    });
  </script>
```



### 10.8 $parent

**在子组件中可以通过this.$parent来主动获取父组件对象,通过该对象可以主动获取父组件的数据和方法**

**注:**子组件必须是在父组件内部用components形式定义时才能使用



## 11.Vue-router

### 11.1 什么是路由

- **后端路由：**对于普通的网站，所有的超链接都是URL地址，所有的url地址都对应服务器上对应的资源

- **前端路由：**对于单页面应用程序来说，主要通过URL中的hash(#号)来实现不同页面之间的切换，同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以，单页面程序中的页面跳转主要用hash实现,在单页面应用中这就叫做前端路由



### 11.2 使用Vue-router

**1.导入 vue-router 组件类库**

```html
<!-- 1. 导入 vue-router 组件类库 -->
  <script src="./lib/vue-router-2.7.0.js"></script>
```

**2.使用 router-link 组件来导航**

```html
<!-- 
2. 使用 router-link 组件来导航
router-link组件默认渲染为一个a标签,可以使用tag属性来转换标签,但是无论转换为什么标签都可以进行跳转 
-->
<router-link to="/login">登录</router-link>
<router-link to="/register">注册</router-link>
<!--链接的跳转可以使用a标签,但是官方不推荐使用,因为a链接使用的时候需要在前面加上#,会很麻烦-->
```

**3.使用 router-view 组件来显示匹配到的组件**

```html
<!-- 
3. 使用 router-view 组件来显示匹配到的组件 
这是Vue-router提供的组件元素,专门用来当做占位符,当路由规则到了路径,就会把对应的组件展示到其中
可以使用多个router-view,这是会根据path属性的目录渲染多个组件
-->
<!--
	最外层也可以使用transition组件将 router-view 组件包裹实行过渡效果
-->
<router-view></router-view>
```

**4.创建使用`Vue.extend`创建组件**

```js
    // 4.1 使用 Vue.extend 来创建登录组件
    var login = Vue.extend({
      template: '<h1>登录组件</h1>'
    });

    // 4.2 使用 Vue.extend 来创建注册组件
    var register = Vue.extend({
      template: '<h1>注册组件</h1>'
    });
```

**5.创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则**

```js
// 5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则
    var router = new VueRouter({
      /*
      route这个配置对象中的route表示路由器匹配规则的意思
      每个路由规则都是一个规则对象,身上有两个必须的属性:
      第一个属性为path,表示监听哪个路由链接地址
      第二个属性为component,表示如果路由前面匹配到了path路径,则展示component属性对于的组件
      */  
      routes: [//路由器匹配规则
        
        {path:'/',redirect:'/login'},
        //通过redirect属性设置路由重定向,当访问根目录的时候自动跳转到/login目录
        { path: '/login', component: login },
        { path: '/register', component: register }
      ]
    });
```

**6.使用 router 属性来使用路由规则**

```js
// 6. 创建 Vue 实例，得到 ViewMode
    var vm = new Vue({
      el: '#app',
      router: router // 使用 router 属性来使用路由规则
    });
```



### 11.3 修改路由样式

**当点击了`router-link`组件时,会默认为该路由设置一个`router-link-active`的CSS类名,在通过给该类名设置样式可以达到修改路由样式的能力,如果要修改这个默认的类名选用自己需要的类名,可以通过在VueRouter的构造函数中添加`linkActiveClass`来改变一个新值**

```js
var router = new VueRouter({
    routes: [//路由器匹配规则    
        {path:'/',redirect:'/login'},
        //通过redirect属性设置路由重定向,当访问根目录的时候自动跳转到/login目录
        { path: '/login', component: login },
        { path: '/register', component: register }
    ],
	linkActiveClass:"myClass"
});
```



### 11.4 传入参数

**一个Vue组件内置的$route属性可以代表着正在进行跳转的路由**

#### 11.4.1 query

**当在路由器中链接中使用查询字符串(url后面的?后的一系列值),不用修改路由规则中的path属性的路径,在路由视图的组件中可用通过this.$route获取该路由实例,通过该实例的query属性获取传入的参数的属性和值**

```html
<router-link to="/login?id=123">登录</router-link>
<router-link to="/register?id=456">注册</router-link>
```

```js
var login = Vue.extend({
    template: '<h1>登录组件</h1>',
    created(){
        console.log(this.$route.query.id);
    }
});

var register = Vue.extend({
    template: '<h1>注册组件</h1>'
    created(){
    console.log(this.$route.query.id);
	}
});
```



#### 11.4.2 params

**使用params来获取参数需要在路由规则中定义参数,然后在路由器链接中需要在后面跟上对应数量的/+值传给参数实现传参,通过该实例的params属性获取传入的参数的属性和值**

```html
<router-link to="/login/123/zhangsan">登录</router-link>
<router-link to="/register/456/lisi">注册</router-link>
```

```js
var login = Vue.extend({
    template: '<h1>登录组件</h1>',
    created(){
        console.log(this.$route.params.id);
    }
});

var register = Vue.extend({
    template: '<h1>注册组件</h1>'
    created(){
    console.log(this.$route.params.id);
	}
});
```

```js
// 5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则
    var router = new VueRouter({
      /*
      route这个配置对象中的route表示路由器匹配规则的意思
      每个路由规则都是一个规则对象,身上有两个必须的属性:
      第一个属性为path,表示监听哪个路由链接地址
      第二个属性为component,表示如果路由前面匹配到了path路径,则展示component属性对于的组件
      */  
      routes: [//路由器匹配规则
        
        {path:'/',redirect:'/login'},
        //通过redirect属性设置路由重定向,当访问根目录的时候自动跳转到/login目录
        { path: '/login/:id/:name', component: login },
        { path: '/register/:id/:name', component: register }
      ]
    });
```



### 11.5 路由嵌套

**如果想要在一层路由组件下面开启第二层组件,不能够直接通过二级的路由链接得到二级的路由组件,因为这样一级的链接组件就会消失,需要通过`routes`路由器匹配规则中相应规则的children属性,在该属性构成的数组中写入子路由来实现路由嵌套的功能**

```html
  <div id="app">
    <router-link to="/account">Account</router-link>

    <router-view></router-view>
  </div>

  <script>
    // 父路由中的组件
    const account = Vue.extend({
      template: `<div>
        这是account组件
        <router-link to="/account/login">login</router-link> | 
        <router-link to="/account/register">register</router-link>
        <router-view></router-view>
      </div>`
    });

    // 子路由中的 login 组件
    const login = Vue.extend({
      template: '<div>登录组件</div>'
    });

    // 子路由中的 register 组件
    const register = Vue.extend({
      template: '<div>注册组件</div>'
    });

    // 路由实例
    var router = new VueRouter({
      routes: [
        { path: '/', redirect: '/account/login' }, // 使用 redirect 实现路由重定向
        {
          path: '/account',
          component: account,
          children: [ // 通过 children 数组属性，来实现路由的嵌套
            { path: 'login', component: login }, // 注意，子路由的开头位置，不要加 / 路径符
            { path: 'register', component: register }
          ]
        }
      ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      components: {
        account
      },
      router: router
    });
  </script>
```



### 11.6 命名路由

**通过在`<router-link></router-link>`的组件中to指向一个带有name属性的对象,而在路由规则中也添加一个对应的name属性实现路由跳转,该方法能让我们更加轻松的进行路由规则的匹配**

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
<script>
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
</script>
```



### 11.7 命名视图

**在Vue的路由实例中,一个路由路径可以对应多个路由组件,所以可以设置多个路由的视图,在一个路径中就可以,通过命名的视图在一个路径中使用多个路由组件,从而实现经典的布局效果**

```html
<!--CSS-->  
<style>
    .header {
      border: 1px solid red;
    }

    .content{
      display: flex;
    }
    .sidebar {
      flex: 2;
      border: 1px solid green;
      height: 500px;
    }
    .mainbox{
      flex: 8;
      border: 1px solid blue;
      height: 500px;
    }
  </style>
<!--HTML-->  
<div id="app">
    <router-view></router-view>
    <div class="content">
      <router-view name="a"></router-view>
<!--通过name属性可以只参与对应组件的渲染,没有name属性代表是default,会渲染属性名为defalut的组件-->
      <router-view name="b"></router-view>
    </div>
  </div>
<!--JS-->  
<script>
    var header = Vue.component('header', {
      template: '<div class="header">header</div>'
    });

    var sidebar = Vue.component('sidebar', {
      template: '<div class="sidebar">sidebar</div>'
    });

    var mainbox = Vue.component('mainbox', {
      template: '<div class="mainbox">mainbox</div>'
    });

    // 创建路由对象
    var router = new VueRouter({
      routes: [
        {
          path: '/', components: {
            default: header,//定义一个name为efalut的组件,即使视图不写name属性也能渲染该组件
            a: sidebar,
            b: mainbox
          }
        }
      ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router
    });
  </script>
```



### 11.8 route与router

- this.$route是路由的参数对象,所有的路由参数如pararms和query都属于该属性的参数

- this.$router是一个路由导航对象,用它可以方便的使用JS代码,实现路由器的前进,后退,跳转到新的url地址

  ```js
  // 字符串
  this.$router.push('home')
  
  // 对象
  this.$router.push({ path: 'home' })
  
  // 命名的路由
  this.$router.push({ name: 'user', params: { userId: '123' }})
  
  // 带查询参数，变成 /register?plan=private
  this.$router.push({ path: 'register', query: { plan: 'private' }})
  ```

  **注意:**

  - 如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况

  - 该属性的API`this.$router.push`、 `this.$router.replace` 和 `this.$router.go` 跟 `window.history.pushState`、 `window.history.replaceState` 和 `window.history.go`类似



## 12.watch与computed

### 12.1 watch

**watch属性为Vue实例或组件中的一个自定义的侦听器,通过该属性可以在实例或组件的数据发生变化时进行相应的操作,对于路由这些虚拟DOM的监听是非常好用的**

- **监听data数据变化**

  ```html
  <div id="app">
      <input type="text" v-model="firstName"> +
      <input type="text" v-model="lastName"> =
      <span>{{fullName}}</span>
    </div>
  
    <script>
      // 创建 Vue 实例，得到 ViewModel
      var vm = new Vue({
        el: '#app',
        data: {
          firstName: 'jack',
          lastName: 'chen',
          fullName: 'jack - chen'
        },
        methods: {},
        watch: {
          'firstName': function (newVal, oldVal) { // 第一个参数是新数据，第二个参数是旧数据
            this.fullName = newVal + ' - ' + this.lastName;
          },
          'lastName': function (newVal, oldVal) {
            this.fullName = this.firstName + ' - ' + newVal;
          }
        }
      });
    </script>
  ```

- **监听路由的变化**

  ```html
  <div id="app">
      <router-link to="/login">登录</router-link>
      <router-link to="/register">注册</router-link>
  
      <router-view></router-view>
    </div>
  
    <script>
      var login = Vue.extend({
        template: '<h1>登录组件</h1>'
      });
  
      var register = Vue.extend({
        template: '<h1>注册组件</h1>'
      });
  
      var router = new VueRouter({
        routes: [
          { path: "/login", component: login },
          { path: "/register", component: register }
        ]
      });
  
      // 创建 Vue 实例，得到 ViewModel
      var vm = new Vue({
        el: '#app',
        data: {},
        methods: {},
        router: router,
        watch: {
          '$route': function (newVal, oldVal) {
            if (newVal.path === '/login') {
              console.log('这是登录组件');
            }
          }
        }
      });
    </script>
  ```

  

### 12.2 computed

**计算属性computed是用于定义一些可以动态改变的属性的,模板内的表达式非常便利,但是设计它们的初衷是用于简单运算的。**在模板中放入太多的逻辑会让模板过重且难以维护,所以尽量不要在模板{{}}中使用计算表达式,这时就可以用Vue实例中的computed属性,**computed里面的属性的值为一个函数,可以在该函数中间一系列的计算,然后将需要的结果返回**

```html
<div id="app">
    <input type="text" v-model="firstName"> +
    <input type="text" v-model="lastName"> =
    <span>{{fullName}}</span>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstName: 'jack',
        lastName: 'chen'
      },
      methods: {},
      computed: { 
    /*
    计算属性； 特点：当计算属性中所以来的任何一个 data 属性改变之后，都会重新触发 本计算属性 的重新计	 算，从而更新 fullName 的值
    */
        fullName() {
          return this.firstName + ' - ' + this.lastName;
        }
      }
    });
  </script>
```

**计算属性默认只有getter,不过在需要时也可以提供一个setter,这时最外层的属性会是一个对象,内部分别有get()方法和set()方法**

```html
<div id="app">
    <input type="text" v-model="firstName">
    <input type="text" v-model="lastName">
    <!-- 点击按钮重新为 计算属性 fullName 赋值 -->
    <input type="button" value="修改fullName" @click="changeName">

    <span>{{fullName}}</span>
</div>

<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
        el: '#app',
        data: {
            firstName: 'jack',
            lastName: 'chen'
        },
        methods: {
            changeName() {
                this.fullName = 'TOM - chen2';
            }
        },
        computed: {
            fullName: {
                get: function () {
                    return this.firstName + ' - ' + this.lastName;
                },
                set: function (newVal) {
                    var parts = newVal.split(' - ');
                    this.firstName = parts[0];
                    this.lastName = parts[1];
                }
            }
        }
    });
</script>
```

**注意:**

- 计算属性本质上就是一个方法,不过只需要将其当做一个属性来用,**在使用这些属性的时候是直接使用它们的名称**,而不是将其当做方法来用,所以**在引用的时候不要在后面加上()**
- **只要计算属性的函数内部所用到的data中的数据发生了变化就会立刻调用该函数,计算属性的值会重新被计算**
- **与方法不同的是computed是有缓存的,意味着只要内部的响应式依赖变量没有发生改变访问的该函数的结果依然不会发生改变**,而如果是方法,每当再次访问方法时都会重新执行一次函数,所以不会有缓存,如果不相同要有缓存,computed就用方法methods来代替



### 12.3 wath,computed与methods的区别

- **`computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；**
- **`methods`方法表示一个具体的操作，主要书写业务逻辑**
- **`watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作,可以看作是`computed`和`methods`的结合体**



## 13.对象数组更新

### 13.1 Vue.set与vm.$set

**向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新属性，因为 Vue 无法探测普通的新增属性**

- **数组添加**
  Vue中的data中的数组可以通过Vue返回后的实例进行改变,所拥有的方法同原生JS,变异方法会直接改变数组,重新渲染页面,但**如果不是变异方法也可以通过重新赋值来使用,Vue不会直接重新渲染,**而是由其内部高效的机制
  **注意:**如果直接通过赋值改变数组中成员的值或者length的长度,并不能够渲染页面,这是由JS内部的机制决定的

  **解决方案:**

  - `Vue.set(vm.items, indexOfItem, newValue)`**(第一个参数为素组名,第二个为数组索引,第三个为成员值)**
  - `vm.items.splice(indexOfItem, 1, newValue)`
  - `vm.$set(vm.items, indexOfItem, newValue)`**(其实vm.$set()就是Vue.set()方法的别名)**
  - **如果要解决改变length属性的问题,**使用`vm.items.splice(newLength)`

- **对象添加**
  如果要更新对象的属性,在已有属性的情况下改变原来的值是可以进行动态更新的,但是**如果是添加一个新的属性或为一个对象添加新的属性不能做到响应式的更新,**这也是由于JS的内部机制决定的

  **解决方案:**

  - `Vue.set(vm.userProfile, 'age', 27`)**(第一个参数为对象名,第二个为键名,第三个为键值)**

  - `vm.$set(vm.userProfile, 'age', 27)`

  - **如果要同时给多个属性使用下面这种方式**

    ```js
    vm.userProfile = Object.assign({}, vm.userProfile, {
        age: 27,
        favoriteColor: 'Vue Green'
    })
    //不好的方式:
    Object.assign(vm.userProfile, {
        age: 27,
        favoriteColor: 'Vue Green'
    })
    ```




### 13.2 Vue.$forceUpdate

如果使用set还不能刷新,进行深度检索还不行,这时候就需要手动刷新视图,使用`this.$forceUpdate()`可以手动刷新



## 14.混入

**混入 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项**

### 14.1 全局混入

**注意:** 一旦使用全局混入对象，将会影响到**所有**之后创建的 Vue 实例。使用恰当时，可以为自定义对象注入处理逻辑

```js
// 为自定义的选项 'myOption' 注入一个处理器。
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
```



### 14.2 局部混入

```js
// 定义一个混入对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// 定义一个使用混入对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "hello from mixin!"
```

**当组件和混入对象含有同名选项时，这些选项将以恰当的方式混合,比如数据对象在内部会进行浅合并 (一层属性深度)，在和组件的数据发生冲突时以组件数据优先**

```js
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```

**值为对象的选项，例如 `methods`, `components` 和 `directives`，将被混合为同一个对象。两个对象键名冲突时，取组件对象的键值对**

```js
var mixin = {
  methods: {
    foo: function () {
      console.log('foo')
    },
    conflicting: function () {
      console.log('from mixin')
    }
  }
}

var vm = new Vue({
  mixins: [mixin],
  methods: {
    bar: function () {
      console.log('bar')
    },
    conflicting: function () {
      console.log('from self')
    }
  }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```

**注意:**混入对象的钩子将在组件自身钩子之前调用



## 15.插槽

### 15.1 用法

**在使用代理组件时可以在中间写入其他的HTML代码,在只需要在子组件写入对应的`<slot></slot>`就能够进行组件的合成**

```html
<navigation-link url="/profile">
    Your Profile
</navigation-link>

<script>
    Vue.component("navigation-link",{
        template:`
<a v-bind:href="url" class="nav-link">	
<slot></slot>
</a>`
    })
</script>
```

**注意:**

- 插槽的内容可以代替任何代码,甚至是其它组件
- 如果 `<navigation-link>` 没有包含一个 `<slot>` 元素，则任何传入它的内容都会被抛弃



### 15.2 具名插槽

**当需要传入多个插槽时,可以通过设置slot上的name属性来定义额外的插槽**

```js
Vue.component("base-layout",{
    template:`
<div class="container">
<header>
<slot name="header"></slot>
</header>
<main>
<slot></slot>
</main>
<footer>
<slot name="footer"></slot>
</footer>
</div>`
})
```

```html
<!--使用时在一个父组件的<template>元素上使用 slot 特性：-->
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>

<!--另一种写法-->
<base-layout>
  <h1 slot="header">Here might be a page title</h1>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <p slot="footer">Here's some contact info</p>
</base-layout>
```

**还可以保留一个未命名的插槽作为默认插槽,所以没有被匹配的内容都会在这个默认的插槽中显示出来**

```html
<!--因为保留了默认插槽,所以上面两种写法的结果都为-->
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

**注意:**

- **可以在组件模板里的 `<slot>` 标签内部指定默认的内容来对插槽显示的值进行默认的显示,如果父组件为这个插槽提供了内容,则默认的内容会被替换掉**
- **父子组件具有各自的作用域,都不能使用对方作用域的值,父组件模板的所有东西都会在父级作用域内编译,子组件模板的所有东西都会在子级作用域内编译**
- **可以通过父组件在插槽中写入template标签然后通过#具名插槽名获取到子组件中的数据,而且就能在父组件中使用子组件的数据**





## 16.Vuex

**Vuex是一个全局的共享存储区域,相当于是一个数据仓库**

Vuex是为了保存组件之间的共享数据而诞生的,如果组件之间要有共享数据,可以直接挂载到Vuex中,而不必通过父子组件直接的传值了,而私有的数据则不需要挂载到Vuex中,**只有需要共享的数据才放在Vuex中,私有的数据只需要放在组件的data中即可**

![vuex](https://vuex.vuejs.org/vuex.png)

![1561737099850](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\1561737099850.png)

### 16.1 基本用法

```js
//先下载Vuex
//main.js
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);
//使用new Vuex.store()实例得到一个数据存储对象
var store=new Vuex.Store({
    state:{
/*
可以把state想像成组件中的data,专门用来访问数据,如果想在组件中访问store中的数据,只能通过(this.)$store.state.属性名来获取
*/
        count:0
    },
    mutations:{
        /*
        如果要操作state中的数据,只能通过mutations中的方法,不推荐直接操作state中的数据,因为每个组件		都有能够操作数据的方法,如果导致了数据的紊乱就不能够快速进行定位
        */
       increment (state) {
      		state.count++
    	}  
        /*
        	如果组件想调用mutations中的方法,只能使用(this.)$store.commit("方法名")的方式,类似于
        	this.$emit("方法名")
        */
    },
    getters:{
        //该属性中的方法用于对外修改包装提供的数据和计算出新的数据
        optCount:function(state){
            return "当前的值为"+state.count;
        }
        /*
        getters的调用类似过滤器和计算属性,能够修改导出的数据(不修改元数据),同时只要元数据发生改变			就会立刻重新计算新的值,所以如果想要对数据进行一层包装推荐用它
        */
        /*
        在组件中引用getters中的方法使用this.$store.getters.方法名来使用
        */
    }
});
import App from './App.vue';

const vm=new Vue({
    el:"#app",
    component:APP,
    template:"<App/>",
    store:store;//将store挂载到vm实例上,只要挂载了就能全局使用store中的数据了
})
```



### 16.2 详细用法

```js
//store.js
/*
	Vuex的核心管理对象模板:store
*/
improt Vue from "vue";
import Vuex from "vuex";
//使用Vuex
Vue.use(Vuex);

//状态对象
const state={
	count:0
}
//包含多个更新state函数的对象
const mutations={//mutations里面的函数必须是同步函数
	//增加的mutation
    INCREMENT(state){
       state.count++;
    }
    //减少的mutation
    DECRMENT(state){
        state.count--;
    }
}
//包含对应事件回调函数的对象
const actions={//actions中是提交的mutations,可以带有异步函数
	//增加的action
    increment({commit}){//action中的函数接受一个与 store 实例具有相同方法和属性的 context 对象
        //第二个参数是传入的参数
        //提交mutation
        commit("INCREMENT");//如果有传参条件就传参
    },
    //减少的action
    decrment({commit}){
        //提交减少的mutation
        commit("DECRMENT");
    },
	//带条件的action
    incrementIfOdd({commit,state}){
        //action中的函数接受一个与 store 实例具有相同方法和属性的 context 对象
        if(state.count%2===1){
            //提交增加的mutation
            commit("INCREMENT")
        }
    },
    //异步的action
    incrementAsync({commit}){
        //在action中执行异步代码
        setTimeour(()=>{
            commit("INCREMENT");
        },1000)
    }
}
//包含多个getter计算属性函数的对象
const getters={
    evenOrOdd(state){//不需要调用,只需要读取属性值,函数会自调用
        return state.count%2===0?"奇数":"偶数";
    }
}

export default new Vuex.Store({
    state,//状态对象
    mutations,//包含多个更新state函数的对象
    actions,//包含多个对应事件回调函数的对象
    getters,//包含多个getter计算属性函数的对象
})
```

```js
//main.js
import Vue from "vue";
import App from "./App";
import router from "./router";
import store from "./store.js"
new Vue({
    el: "#app",
    router,
    store,//挂载后所有的组件对象都多一了一个属性:$store
    components: { App },
    template: "<App/>"
});
```

```vue
<!--App.vue-->
<!-- index页面 -->
<template>
  <div>
      {{this.$store.state.count}}
  </div>
</template>

<script>
export default {
  data() {
    return {
      userInfo: {},
      isShow: false //用户没有授权
    };
  },
  methods: {
      increment(){
    this.$store.dispatch("increment");//触发store中对应的action调用
	},
    decrment(){
        this.$store.dispath("decrment");
    },
    incrementIfOdd(){
        this.$store.dispath("incrementIfOdd");
    },
    incrementAsync(){
        this.$store.dispath("incrementAsync");
    }  
  }
};
</script>
<style >
</style>
```



#### 16.2.1 store对象

**进行了上述的映射后,所有的组件多出的$store属性就是store对象**

**store对象的两个属性:**

- **state:**注册的state对象
- **getters:**注册的getters对象

**store对象的方法:**

- **dispatch(actionName,data):**分发调用action



#### 16.2.2 state

**state是vuex管理的状态对象,它应该是唯一的**

```js
const state={
    xxx:intValue
}
```



#### 16.2.3 mutations

- 包含了多个直接更新state的方法(回调函数)的对象
- **触发方式:**
  - 直接在组件中通过`this.$store.commit("mutations名称",可选的参数)`
  - 通过mapMutations结构直接使用mutations的名称
  - 通过action中的`commit("mutations名称",可选的参数)`(commit是解构出来的)
- mutations中只能有同步代码,不能有异步代码

```js
const mutations={
    yyy(state,data){
        //更新state中的某个属性
    }
}
```

**注意:**mutations函数中的函数参数列表最多只支持两个参数,也就是state与要传入的一个参数

```js
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```

**因为只能传入一个参数,所以通常情况下都是传入一个对象**

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}

//在某个组件的方法中
this.$store.commit('increment', {
  amount: 10
})
```



#### 16.2.4 actions

- 包含了多个事件回调函数的对象
- 通过执行commit()来触发mutations的调用,间接更新state
- **触发方式:**
  - 组件中的`$store.dispatch("action名称",可选的参数)`
  - 通过mapActions结构直接使用actions-的名称
- actions中可以包含异步的代码(定时器,ajax等)

```js
const actions={
    zzz({commit,state},data1){
        commit("yyy",data1);
    }
}
```



#### 16.2.5 getters

- 包含多个计算属性(get)的对象
- **读取方式:**在组件中使用$store.getters.xxx

```js
const getters={
    mmm(state){
        return ...
    }
}
```



#### 16.2.6 modules

- 包含多个module
- 一个module是一个store的配置对象
- 与一个组件(包含有共享数据)对应



### 16.3 辅助函数

**为了不让书写起来太过于麻烦,Vuex引入了一些辅助函数辅助我们调用Vuex中的属性**

- **mapState:**写在coputed中
- **mapGetters:**写在coputed中
- **mapActions:**写在methods中,可以用来代替dispatch
- **mapMutations:**写在methods中

**下面是使用辅助函数改写:**

```vue
<!--App.vue-->

<template>
<div>
{{count}} <!--这里就是解构直接用了count-->
</div>
</template>

<script>
import {mapState,mapGetters,mapActions} from "vuex";
export default {
  data() {
    return {
      userInfo: {},
      isShow: false //用户没有授权
    };
  },
    computed(){
      ...mapState(['count']),//通过直接传入在Vuex对应的属性再通过解构的方法就可以直接使用
      ...mapGetters(["evenOrOdd"])
        //注意用数组作为参数的话名字必须要一样,一般也检验用数组,但是也可以用带有自定义命名的对象
      //...mapGetters({EVE:"evenOrOdd"})就可以自定义命名
      //对象形式的参数的成员还可以是一个函数
    },
  methods: {
    ...mapActions(["increment","decrement","incrementIfOdd","incrementAsync"])
      //如果在这里想使用自己定义的名字的话可以多次使用....mapActions传入不同的方法
};
</script>
<style >
</style>
```



### 16.4 模块化开发

**将所有的文件全部分开,一个文件放置一个属性,然后在Vue中挂载**

- state.js
- actions.js
- getters.js
- mutations.js
- mutation-type.js,专门用来存放mutations的函数名,更易于管理,注意因为导出的是个变量,要使用需要中括号[]
- store.js

```js
//stote.js
import Vue from "vue";
import Vuex from "vuex";
import state from "./state";
import actions from "./actions";
import getters from "./getters";
import mutations from "./mutations";

//声明使用Vuex
Vue.use(Vuex);

export default new Vuex.Store({
  state,
  actions,
  getters,
  mutations
});
```


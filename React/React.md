# React

## 1.什么是React?

React 是一个采用声明式，高效而且灵活的用来构建用户界面的框架，可以非常轻松地创建用户交互界面，为应用的每一个状态设计简洁的视图，在数据改变时 React 也可以高效地更新渲染界面



## 2.React语法

React使用`jsx`语法，所以不能直接写在`script`标签中，需要使用`babel`来进行转换，同时在学习语法时不需要使用脚手架工具进行搭建，直接在开发环境中使用React与`babe`l转换代码就能实现React的基本操作

### 2.1 安装配置文件

**使用React需要先后引入babel、react和react-dom三者才能正常运行**

```shell
cnpm babel-standalone react react-dom -D
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
    </head>
    <body>
        <script src="./node_modules/babel-standalone/babel.js"></script>
        <script src="./node_modules/react/umd/react.development.js"></script>
        <script src="./node_modules/react-dom/umd/react-dom.development.js"></script>
        <!-- 注意这里一定要是babel类型 -->
        <script type="text/babel">
      /* 在这里面就能写jsx代码了 */
        </script>
    </body>
</html>
```



### 2.2 核心方法

**jsx语法中的标签想要在网页上渲染出来必须要依耐ReactDOM提供的render方法**，该方法需要传入**要渲染的jsx代码以及要渲染到的根节点**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <!-- 要绑定到的元素 -->
    <div id="app"></div>
    <div id="app2"></div>
    <script src="./node_modules/babel-standalone/babel.js"></script>
    <script src="./node_modules/react/umd/react.development.js"></script>
    <script src="./node_modules/react-dom/umd/react-dom.development.js"></script>
    <script type="text/babel">
      //注意不是字符串
      const element = <div>123</div>
      ReactDOM.render(element, document.getElementById('app'))
      
      //如果要隔行写必须要加上括号代表是一个整体,不然模式会值解析第一行
      const element2 = (
        <div>
          <p>HelloWord</p>
        </div>
      )
      ReactDOM.render(element2, document.getElementById('app2'))
    </script>
  </body>
</html>
```

**注意：**传入的jsx语法的值可以是字符串，但是那样就是原封不动的写在网页上，而不是将内部的虚拟DOM渲染到网页根元素内



### 2.3 插值符号

**在jsx中需要要写入js代码只能在插值符号`{}`中，插值符号外写的是html标签**

**注意：**html的注释和js的注释也是不能写在jsx中的,都是通过插值符号进行注释



**插值符号`{}`内能写的内容**

- 表达式

- 数组

- 字符串

- 即时函数和调用外部的函数

  **注意：**要有返回值才能在网页中渲染出来，不然只是进行了一堆逻辑操作

- 布尔值

- 三元表达式（注意不能使用if判断，只能是表达式）

- 数组（注意不能是json对象），**写入数组会将数组中的每一个成员依次解析出来放在jsx的html中，数组常常通过map方法返回值渲染到网页中**

- js的注释（注意是js的注释）

**注：**上面的所有内容在一个插值表达式中都只能存在一个，如果存在多个插值表达式就无法解析出来，会报错



### 2.3 jsx传入类名与样式

#### 2.3.1 传入类名

**在jsx中写入元素类名的时候不能使用class声明类名**，因为jsx和js一样class都是作为关键词来使用的，需要**使用className代替class来为jsx中的html元素写入类名**

```jsx
const element1 = (
	<div className="red">
    	HelloWorld
    </div>
)
```

#### 2.3.2 传入行内样式

在jsx中写style可以使用插值符号动态设置，与Vue的用法一样，React可以直接传入一个包含有样式的对象来对元素的样式进行行内修饰

```jsx
cosnt style = {
    color:red;
}
const style2 = {
    bg:{
        background:blue;
    }
}

const element1 = (
	<div style={style}>
    	<h1 style={style2.bg}>HelloWorld</h1>
    </div>
)
```



### 2.4 事件处理

在jsx中为元素传入事件大体与普通html一样，只不过**事件名要使用驼峰命名法**，而不是全部小写，同时**写在插值表达式中的事件必须是声明式的,不能是函数调用（这点与Vue不一样,所以不能直接在事件中传入参数）**，因为插值表达式会自动解析调用的函数，而就不是通过事件进行触发了

```jsx
function handleKeyUp(){
	console.log(123)    
}
function handleClick(a,b,e){
    console.log(a+b)
    //如果传入了n个参数,第n+1个就是evebt对象
    console.log(e)//最后一个值为event对象
}

const element1 = (
	<div>
    	<input type="text" onKeyDown={alert(123)}/>
        {
          //不能是上面那种写法调用事件,因为插值表达式会直接解析函数的调用 ，使得一进去页面就会自动触发
        }
        <br/>
        <input type="text" onKeyUp={()=>alert(1)}/>
        {
		  //使用上面的声明式写法才能正常触发alert事件
        }
        <br/>
        <input type="text" onKeyUp={handleKeyUp}/>
        {
		  //不调用而是将函数本身给事件，这样事件也能触发
        }
        <br/>
        <button
            type="text"
            onClick={() => {
              handleClick(10, 11)
            }}
          >
            函数传参1
         </button>
        {
           //可以通过这种二次调用函数传参
        }
        <button type="text" onClick={handleClick.bind(null,10,11)}>函数传参2</button>
        {
		  //不能直接调用传参,而是通过bind传参
        }    
    </div>
)
```



### 2.5  声明式无状态组件

一个jsx的代码块也可以插入到另外一个jsx代码块中， 这种插入主要有两种写法，**一种是直接通过插值表达式进行渲染，一种是写成向Vue中的组件的形式进行渲染**

```jsx
//下面两个就是声明式的组件，并且没有state状态
cosnt head = <header>头部样式</header>
function Section(){
  return <section>主体样式</section>
}
let element = (
	<div>
    	<h2>{head}</h2>
    	<Setion />
        {
            //单标签和双标签都可以,推荐的是单标签
        }
    	<Setion></Setion>
    </div>
) 
```


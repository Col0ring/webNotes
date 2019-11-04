# React

## 1.什么是React?

React 是一个采用声明式，高效而且灵活的用来构建用户界面的框架，可以非常轻松地创建用户交互界面，为应用的每一个状态设计简洁的视图，在数据改变时 React 也可以高效地更新渲染界面

### 1.1 React理念

**react的理念是数据驱动,不操作DOM元素而是直接改变数据来使得页面发生变化,所以尽量不要操作DOM元素，只能改变数据**。而且react可以和其他的框架进行混合使用,react之后控制被选中的标签,所有操作都是在被选中的标签里面进行的,所以只要不去修改被选中的那个标签就可以实现混合使用



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

- jsx代码，通过是&&或三元表达式进行条件渲染

- 数组（注意不能是json对象），**写入数组会将数组中的每一个成员依次解析出来放在jsx的html中，数组常常通过map方法返回值渲染到网页中**

- js的注释（注意是js的注释）

**注：**上面的所有内容在一个插值表达式中都只能存在一个，如果存在多个插值表达式就无法解析出来，会报错



### 2.3 jsx传入类名与样式

#### 2.3.1 传入类名

**在jsx中写入元素类名的时候不能使用class声明类名**，因为jsx和js一样class都是作为关键词来使用的，需要**使用className代替class来为jsx中的html元素写入类名。（同理在写入for属性需要写成 htmlFor，因为for也是html的保留字）**

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
            onClick={(e) => {
              handleClick(10, 11)
            }}
          >
            函数传参1
         </button>
        {
           //可以通过这种二次调用函数传参，这也是推荐的事件传参方式,甚至可以将event事件传入
        }
        <button type="text" onClick={handleClick.bind(null,10,11)}>函数传参2</button>
        {
		  //不能直接调用传参,而是通过bind传参
        }    
    </div>
)
```

**注：**React中的事件是合成事件，并不是dom的原生事件，在dom中可以通过放回false来阻止事件的默认行为，但在react中必须显示的调用事件对象e.preventDefault来阻止事件的默认行为，除了这有点外和原生dom事件并无差别

```jsx
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('链接被点击');
  }
 
  return (
    <a href="#" onClick={handleClick}>
      点我
    </a>
  );
}
```



### 2.5 Fragment

Fragment是一个特殊的标签组件，该组件与Vue中的template类似，在渲染的时候都会将其取出掉，该子类常在最外层包裹render内部的元素（当想要外部额外包裹一层多余函数的时候）

```jsx
import React,{Fragement} from 'react'
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('链接被点击');
  }
 
  return (
    <React.Fragement>
    <a href="#" onClick={handleClick}>
      点我
    </a>
     <a href="#" onClick={handleClick}>
      点我
     </a>
    {
       /*
       Fragement常常会被结构出来使用
       	<Fragement>
        <a href="#" onClick={handleClick}>
          点我
        </a>
         <a href="#" onClick={handleClick}>
          点我
         </a>
         </Fragement>
       */   
    }
      </React.Fragement>
  );
}
```





## 3.组件

### 3.1  无状态组件

一个jsx的代码块也可以插入到另外一个jsx代码块中， 这种插入主要有两种写法，**一种是直接通过插值表达式进行渲染，一种是写成函数的形式向Vue中的组件的形式进行渲染（也可以写成ES6中的class形式）**

**注意：**原生 HTML 元素名以小写字母开头，而自定义的 React 类名以大写字母开头，比如 HelloMessage 不能写成 helloMessage。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错

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

#### 3.1.1 无状态组件传参

**通过写成函数的形式能将父组件的参数传入到子组件中**

```jsx
//无状态组件的props参数就是父组件传入进来的值,不想有状态组件可以随时使用this.props获取参数
function Section(props){
  return <section>主体样式{props.title} {props.data[0]}</section>
}
let element = (
	<div>
        {
            //注意下面直接传对象或者数组是不行的,因为来不及进行解析,会报错,下面data中就会报错
        }
    	<Setion title="title" data={['内容']}/>
        {
            //单标签和双标签都可以,推荐的是单标签
        }
    	<Setion></Setion>
    </div>
) 
```

```jsx
//如果感觉要输入props很麻烦可以直接使用解构的方法
function Section({data}){
  return <section>主体样式{props.title} {data[0]}</section>
}
let element = (
	<div>
        {
            //注意下面直接传对象或者数组是不行的,因为来不及进行解析,会报错,下面data中就会报错
        }
    	<Setion title="title" data={['内容']}/>
        {
            //单标签和双标签都可以,推荐的是单标签
        }
    	<Setion></Setion>
    </div>
) 
```



### 3.2 有状态组件

#### 3.2.1 state

React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。**React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）**

**通过ES6的类的写法继承React中的React.Component类就能创建一个有状态（state）的组件**

```jsx
//添加一个类构造函数来初始化状态 this.state，类组件应始终使用 props 调用基础构造函数
class Clock extends React.Component {
  constructor(props) {
    super(props);//注意使用super()调用父组件的构造函数
    this.state = {date: new Date()};
  }
 /*如上也可写成
    constructor(...args) {
    super(...args);
    this.state = {date: new Date()};
  }
  */
    render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
 
ReactDOM.render(
  <Clock />,
  document.getElementById('example')
);
```

**state中的数据初始化之后需要通过setState()方法才能重新在页面上渲染出来**

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
 //下面两个都是生命周期钩子
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
 
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
 
  tick() {
    this.setState({
      date: new Date()
    });
  }
 
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
 
ReactDOM.render(
  <Clock />,
  document.getElementById('example')
);
```



#### 3.2.2 props

**state 和 props 主要的区别在于 props是不可变的（只读的），而 state 可以根据与用户交互来改变，并且state只能在当前组件的内部才能访问**。这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据

```jsx
function HelloMessage(props) {
    return <h1>Hello {props.name}!</h1>;
}
 
const element = <HelloMessage name="Runoob"/>;
 
ReactDOM.render(
    element,
    document.getElementById('example')
);
//实例中 name 属性通过 props.name 来获取，在前面的无状态组件传参已说明
```

##### 3.2.2.1 默认的props

**可以通过组件类的 defaultProps 属性为 props 设置默认值**

```jsx
class HelloMessage extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

//默认的props
HelloMessage.defaultProps = {
  name: 'Runoob'
};
 
const element = <HelloMessage/>;
 
ReactDOM.render(
  element,
  document.getElementById('example')
);
```



##### 3.2.2.2 props验证

**Props 验证使用 propTypes，它可以保证我们的应用组件被正确使用，React.PropTypes 提供很多验证器 (validator) 来验证传入数据是否有效。当向 props 传入无效数据时，JavaScript 控制台会抛出警告**

**注：**使用时需要先引入prop-types库，但是如果使用脚手架开发的React项目，下载React时已经安装好了，不需要额外安装

```jsx
import PropTypes from 'prop-types';
//创建一个 Mytitle 组件，属性 title 是必须的且是字符串，非字符串类型会自动转换为字符串
var title = "Col0ring";
// var title = 123;
class MyTitle extends React.Component {
  /*
  这种写法与下面的写法相同，个人更倾向于这种写法
  static propTypes = {
 	 title: PropTypes.string
	};
  */
  render() {
    return (
      <h1>Hello, {this.props.title}</h1>
    );
  }
}
 
MyTitle.propTypes = {
  title: PropTypes.string
};
ReactDOM.render(
    <MyTitle title={title} />,
    document.getElementById('example')
);
```

**验证器说明**

```jsx
MyComponent.propTypes = {
    // 可以声明 prop 为指定的 JS 基本数据类型，默认情况，这些数据是可选的
   optionalArray: React.PropTypes.array,
    optionalBool: React.PropTypes.bool,
    optionalFunc: React.PropTypes.func,
    optionalNumber: React.PropTypes.number,
    optionalObject: React.PropTypes.object,
    optionalString: React.PropTypes.string,
 
    // 可以被渲染的对象 numbers, strings, elements 或 array
    optionalNode: React.PropTypes.node,
 
    //  React 元素
    optionalElement: React.PropTypes.element,
 
    // 用 JS 的 instanceof 操作符声明 prop 为类的实例。
    optionalMessage: React.PropTypes.instanceOf(Message),
 
    // 用 enum 来限制 prop 只接受指定的值。
    optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),
 
    // 可以是多个对象类型中的一个
    optionalUnion: React.PropTypes.oneOfType([
      React.PropTypes.string,
      React.PropTypes.number,
      React.PropTypes.instanceOf(Message)
    ]),
 
    // 指定类型组成的数组
    optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),
 
    // 指定类型的属性构成的对象
    optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),
 
    // 特定 shape 参数的对象
    optionalObjectWithShape: React.PropTypes.shape({
      color: React.PropTypes.string,
      fontSize: React.PropTypes.number
    }),
 
    // 任意类型加上 `isRequired` 来使 prop 不可空。
    requiredFunc: React.PropTypes.func.isRequired,
 
    // 不可空的任意类型
    requiredAny: React.PropTypes.any.isRequired,
 
    // 自定义验证器。如果验证失败需要返回一个 Error 对象。不要直接使用 `console.warn` 或抛异常，因为这样 `oneOfType` 会失效。
    customProp: function(props, propName, componentName) {
      if (!/matchme/.test(props[propName])) {
        return new Error('Validation failed!');
      }
    }
  }
}
```



#### 3.2.3 组合使用state与props

**在父组件中设置 state， 并通过在子组件上使用 props 将其传递到子组件上**

```jsx
class WebSite extends React.Component {
  constructor() {
      super();
 
      this.state = {
        name: "Col0ring",
        site: "www.baidu.com"
      }
    }
  render() {
    return (
      <div>
        <Name name={this.state.name} />
        <Link site={this.state.site} />
      </div>
    );
  }
}
 
class Name extends React.Component {
  render() {
    return (
      <h1>{this.props.name}</h1>
    );
  }
}
 
class Link extends React.Component {
  render() {
    return (
      <a href={this.props.site}>
        {this.props.site}
      </a>
    );
  }
}
 
ReactDOM.render(
  <WebSite />,
  document.getElementById('example')
);
```



#### 3.2.4 事件机制

**有状态组件中的事件机制同先前jsx语法中一样，当使用 ES6 class 语法来定义一个组件的时候，事件处理器会成为类的一个方法**

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};
 
    // 这边绑定是必要的，这样的this才能在回调函数中使用,下面会着重说明this的指向问题
    this.handleClick = this.handleClick.bind(this);
  }
 
  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }
 
  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
 
ReactDOM.render(
  <Toggle />,
  document.getElementById('example')
);
```

##### 3.2.4.1 this指向

**更需要注意的是要了解React事件中的this指向问题，由于React是声明式的组件语法，所以不向Vue那样的组件定义式在组件定义的时候就能直接绑定好this，React在创建实例后才会初始化this,这样就会导致用普通创建函数的方法会丢失上下文，使用箭头函数贼不会，而Vue必须要使用普通的写法，不能使用箭头函数**

因为在函数赋值的时候会丢失函数内部的指向，这时再调用会找不到指向，而箭头函数是创建的时候就写好了this的指向，但是要注意这时的this指向的是创建时的this区域，所以如果是对象调用不是绑定对象自身，而是上一个层级，严格模式下最外层为undefined

```js
const obj={
    a(){
        console.log(this)
    },
    b:()=>{
        console.log(this)
    }
}
obj.a() //obj
obj.b() //undefined
```

**那为什么在类的内部可以绑定？**

这里要说的就是，ES5创建类的方法也会和对象一样会丢失this，但ES6的`class`写法不会，如在`react`中，因为这是实验性质的ES7写法，它内部其实也不是将函数加在原型上，所以这并不是解析机制有错，而是`class`创建的类内部将所有箭头函数定义的函数直接加载创建好的实例上的

```js
class A{
    x = ()=>{
        console.log(this) A{x:()=>{}} // 打印的时候只有一个x属性在实例上，y属性在原型上
        return this
    }
    y(){
        console.log(this)
        return this
    }
}
const a = new A()
console.log(a.x() === a.y()) // true，可见指向同一个this
```

这样的问题就是每次在创建实例的时候都会重新创建一个函数来加在不同实例上，造成性能上的缺失，但这是react中不可避免的

**对于静态方法类方法：**我们可以发现如果静态类使用static关键字不会在类上出现（控制台上打印类是原封返回定义的样子），但是我们却可以调用它，同时this的指向也是正确的，可以看出静态类属性也是经过了封装后的



**React中this指向解决方法**

- **在调用函数时使用bind来绑定到事件中**

  ```jsx
  class LoggingButton extends React.Component {
    constructor(props) {
      super(props);
    }
    handleClick(){
      console.log('this is:', this);
    }
   
    render() {
      return (
        <button onClick={this.handleClick.bind(this)}>
          Click me
        </button>
      );
    }
  }
  ```

- **在构造函数constructor中改变this指向**

  ```jsx
  class LoggingButton extends React.Component {
    constructor(props) {
      super(props);
      this.handleClick.bind(this);
    }
    handleClick(){
      console.log('this is:', this);
    }
   
    render() {
      return (
        <button onClick={this.handleClick}>
          Click me
        </button>
      );
    }
  }
  ```

- **使用ES7的写法通过赋值使用箭头函数改变this指向**

  ```jsx
  class LoggingButton extends React.Component {
    constructor(props) {
      super(props);
    }
    //ES7写法
    handleClick = () => {
      console.log('this is:', this);
    }
   
    render() {
      return (
        <button onClick={this.handleClick}>
          Click me
        </button>
      );
    }
  }
  ```

- **直接在事件赋值中使用箭头函数**

  **注：**这种方法其实不太好，因为会多创建一些函数浪费性能
  
  ```jsx
  class LoggingButton extends React.Component {
    constructor(props) {
      super(props);
    }
      
    handleClick() {
      console.log('this is:', this);
    }
   
    render() {
      //  这个语法确保了 this 绑定在handleClick 中
      return (
        <button onClick={(e) => this.handleClick(e)}>
          Click me
        </button>
      );
    }
  }
  ```



#### 3.2.5 PureComponent与Component

**Compoent存在的问题**

- 父组件重新render()，当前组件也会重新执行render()，即使没有任何变化
- 当前组件setState()，重新执行render()，即使state没有任何变化

**解决办法**

- 重写shouldComponentUpdate()，判断如果数据有变化返回true，否则返回false
- 使用PureComponent代替Component



**PureComponent原理**

- 重写实现shouldComponentUpdate()
- 对组件的新/旧state和props中的数据数据进行**浅比较，**如果都没有变化，返回false，否则返回true
- 一旦shouldComponentUpdate()返回false不再执行用于更新的render()



### 3.3 组件传值

#### 3.3.1 父组件向子组件传值

父组件向子组件传值还是通过组件传参的形式,**父组件传的值会在子组件的props属性中**,包括对象和函数,子组件可以直接通过this.props调用父组件的属性或方法,而且父组件里传入的state值到子组件,当父组件的state变化时子组件也会变化，这种方式也是叫作通过父组件改变子组件的值

```jsx
class List extends React.Component {
    render() {
        return (
            <div>
                <p>{this.props.msg}</p>
            </div>
        )
    }
}
class MyComponent extends React.Component {
    render() {
        return (
            <div>
                <List msg="传给子组件的值" />
            </div>
        )
    }
}

ReactDOM.render(<MyComponent />, document.getElementById('example'))
```

##### 3.3.1.1 children

children是父组件传给子组件的props中一个特殊的对象，当我们想传递某些标签之类的内容时，这时候就可以使用chirldren属性（当然也可以按照正常的组件传值的方式通过手动输入属性进行传递）。**说到底该属性与Vue的slot类似**，都是**通过将组件写成双标签的形式在中间写入slot内容**

```jsx
class List extends React.Component {
    render() {
        console.log(this.props)
        //这时this.props就会多一个chilredn对象,该对象是完全可以直接使用的react元素
        return (
            <div>
                {this.porps.children//会直接渲染出来}                
            </div>
        )
    }
}
class MyComponent extends React.Component {
    render() {
        return (
            <div>
                <List>
                	<h2>传给子组件的值</h2>
                </List>
            </div>
        )
    }
}

ReactDOM.render(<MyComponent />, document.getElementById('example'))
```

```jsx
class List extends React.Component {
    render() {
        console.log(this.props)
        //这时this.props就会多一个chilredn对象,该对象是完全可以直接使用的react元素
        return (
            <div>
                {this.porps.children//会直接渲染出来}                
            </div>
        )
    }
}
class MyComponent extends React.Component {
    state={
		value:123
    }
    render() {
        return (
            <div>
                <List>
                	<h2>传给子组件的值{this.state.value//传入动态的值是插槽最多的用法}</h2>
                </List>
            </div>
        )
    }
}

ReactDOM.render(<MyComponent />, document.getElementById('example'))
```



##### 3.3.1.2 使用解构

**有时在一个组件中会经常用到父组件传入的属性方法，这个时候如果一直写this.props会感觉很麻烦，如果推荐使用解构的方法进行解构出来，这样会使得我们的代码更加的简单**

**注：**在传参的时候我们也可以通过解构传入，不过这时就只能使用传参时的对象属性了

```jsx
class List extends React.Component {
    render() {
        const {msg} = this.props
        return (
            <div>
                <p>{msg}</p>
            </div>
        )
    }
}
class MyComponent extends React.Component {
    state={
        msg:"传给子组件的值"
    }
    render() {
        return (
            <div>
                <List {...this.state//直接通过解构传参} />
            </div>
        )
    }
}

ReactDOM.render(<MyComponent />, document.getElementById('example'))
```



#### 3.3.2 子组件向父组件传值

子组件通过父组件传入过来的方法可以通过传入方法参数的方法将子组件的值传导父组件中,子组件不能改变父组件的值,只能够由父组件自身调用方法改变自身的值。而传值的过程其实就是父组件自身调用传给子组件的函数实现的

```jsx
class List extends React.Component {
    render() {
        return (
            <div>
                <button
                    onClick={() => {
                        this.props.handleClick(this.props.msg)
                    }}
                    >
                    点击按钮
                </button>
                <p>List组件</p>
            </div>
        )
    }
}
class MyComponent extends React.Component {
    handleClick = msg => {
        console.log(msg)
        //父组件点击时事件对象,子组件点击是传给父组件的值
        console.log(this) //两个this都是父组件
    }
    render() {
        return (
            <div>
                <input type="button" value="点击" onClick={this.handleClick} />
                <List msg="传给子组件的值" handleClick={this.handleClick} />
            </div>
        )
    }
}

ReactDOM.render(<MyComponent />, document.getElementById('example'))
```



### 3.4 constructor与super详解

#### 3.4.1 this与实例

在一般的类中,this就是指向类的实例的，用Chilid类为例，this就相当于`new Child()`，但是**在React中对应类做了封装，this是在原本类的基础上进行了包装，包含了一些React的内部方法，所以这是this就不是简单的new Child()了**



#### 3.4.2 constructor存在性

ES5 的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面。ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到`this`上面（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`

**如果子类没有定义`constructor`方法，这个方法会被默认添加，也就是说，不管有没有显式定义，任何一个子类都有`constructor`方法**

```js
class ColorPoint extends Point {
}

// 等同于
class ColorPoint extends Point {
  constructor(...args) {
    super(...args);
  }
}
// 可见没有写constructor，在执行过程中会自动补上
```

**由ES6的继承规则得知：**不管子类写不写constructor，在new实例的过程都会给补上constructor。所以，constructor钩子函数并不是不可缺少的，子组件可以在一些情况略去

##### 3.4.2.1 this.constructor

- **有constructor钩子函数的 this.constructor** 

   ![img](https://images2018.cnblogs.com/blog/905482/201806/905482-20180624005200299-97647880.png)

- **无constructor钩子函数的 this.constructor** 

  ![img](https://images2018.cnblogs.com/blog/905482/201806/905482-20180624005249326-760117579.png)

**故：**有constructor钩子函数时候，Child类会多一个constructor方法



##### 3.4.2.2 this.state

在ES6中想要有state必须要在constructor中进行赋值声明，没有声明的话state就会是null（不过现在用ES7的语法可以直接在Class内部写属性，不写constructor声明state一样可行，不过还是提醒一下）



#### 3.4.3 super中的props

首先明确，**可以不写constructor，一旦写了constructor，就必须在此函数中写super()，在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字，否则会报错。这是因为子类实例的构建，基于父类实例，只有`super`方法才能调用父类实例**

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y);
    this.color = color; // 正确
  }
}
```



**所以在React中是否需要在super中传入props参数呢？**

- **有props：**

  ![img](https://images2018.cnblogs.com/blog/905482/201806/905482-20180624012646344-1650230433.png)

  ![img](https://images2018.cnblogs.com/blog/905482/201806/905482-20180624012721420-1438956371.png)

- **无props：**

  ![img](https://images2018.cnblogs.com/blog/905482/201806/905482-20180624012912272-763738012.png)

  ![img](https://images2018.cnblogs.com/blog/905482/201806/905482-20180624012955198-135846386.png)

 

**故：**当想在constructor中使用this.props的时候，super需要加入(props)，不过此时用props也行，用this.props也行，两者实质是一个。如果在custructor生命周期不使用 this.props或者props时候，可以不传入props



**那么在其他生命周期呢？**

如果constructor中不通过super来接收props，在其他生命周期同样可以使用this.props，**react在除了constructor之外的生命周期已经传入了this.props了，完全不受super(props)的影响**

**故：**super中的props是否接收，只能影响constructor生命周期能否使用this.props，其他的生命周期已默认存在this.props



## 4.ref与findDOMNode

### 4.1 ref

React 支持一种非常特殊的属性**ref**，可以用来绑定到 render() 输出的任何组件上。这个特殊的属性允许引用 render() 返回的相应的支撑实例（ backing instance ）。这样就可以确保在任何时间总是拿到正确的实例。**在普通元素节点上就和原生JS获取节点对象一样，而对组件引用代表的就是对应的组件实例**

- **当组件插入到 DOM 后，ref 属性添加一个组件的引用于到 this.refs**

    ```jsx
    function element(){
        render(){
            return(
                <input ref="myInput" />
            )
        },
        getInput(){
             var input = this.refs.myInput;
             var inputValue = input.value;
             var inputRect = input.getBoundingClientRect();
        }
    }
    ```

    ```jsx
    class List extends React.Component{
        render(){
            return(
                <div>
                <p>List组件</p>
                </div>
            )
        }
    }
    class MyComponent extends React.Component {
      handleClick() {
        console.log(this.refs.List);
      }
      render() {
        //  当组件插入到 DOM 后，ref 属性添加一个组件的引用于到 this.refs
        return (
          <div>
            <input
              type="button"
              value="点击"
              onClick={this.handleClick.bind(this)}
            />
            <List ref="List"></List>
          </div>
        );
      }
    }
    
    ReactDOM.render(
      <MyComponent />,
      document.getElementById('example')
    );
    ```

- **如果不想通过this.refs引用可以使用函数式的方式传入到ref中**

    ```jsx
    class List extends React.Component{
        render(){
            return(
                <div>
                <p>List组件</p>
                </div>
            )
        }
    }
    class MyComponent extends React.Component {
      handleClick() {
        console.log(this._List);//通过这种方式也能获取
        console.log(this._div);
      }
      render() {
        //  当组件插入到 DOM 后，ref 属性添加一个组件的引用于到 this.refs
        return (
          <div>
            <input
              type="button"
              value="点击"
              onClick={this.handleClick.bind(this)}
            />
            <div ref="div=>this._div=div"></div>
            <List ref="List=>this._List=List"></List> 
            {
            //这种方式没有将字符串给refs作为属性,而是直接在父组件的实例上定义了一个私有的属性       
            }
          </div>
        );
      }
    }
    
    ReactDOM.render(
      <MyComponent />,
      document.getElementById('example')
    );
    ```

- **还可以创建一个ref：**可以通过React.createRef()创建Refs并通过ref属性联系到React组件。Refs通常当组件被创建时被分配给实例变量，这样它们就能在组件中被引用

    ```jsx
    class MyComponent extends React.Component {
      constructor(props) {
        super(props);
        this.myRef = React.createRef();
      }
      render() {
        return <div ref={this.myRef} />;
      }
    }
    ```

**注意：**不要在render或render之前对refs进行调用



### 4.2 findDOMNode

findDOMNode类似元素JS的选择器获取到DOM节点，能直接对该方法返回的元素节点进行操作改变样式或值，更重要的是**该方法可以将ref获取的组件实例解析成render渲染的DOM原生节点的样式**，简单来说就是得到实际的DOM的

```jsx
class List extends React.Component{
    render(){
        return(
            <div>
            <p>List组件</p>
            </div>
        )
    }
}
class MyComponent extends React.Component {
  handleClick() {
    console.log(ReactDOM.findDOMNode(this._List));//一个render返回函数中的jsx
    console.log(this._List);//组件对象
    console.log(ReactDOM.findDOMNode(this._div));//还是节点对象本身
    console.log(this._div);
    const oDiv=document.quertSelector('div');
    //可以通过这种方式获取
    ReactDOM.findDOMNode(oDiv).style.backgroundColor="red";
  }
  render() {
    //  当组件插入到 DOM 后，ref 属性添加一个组件的引用于到 this.refs
    return (
      <div>
        <input
          type="button"
          value="点击"
          onClick={this.handleClick.bind(this)}
        />
        <div ref="div=>this._div=div"></div>
        <List ref="List=>this._List=List"></List> 
        {
        //这种方式没有将字符串给refs作为属性,而是直接在父组件的实例上定义了一个私有的属性       
        }
      </div>
    );
  }
}

ReactDOM.render(
  <MyComponent />,
  document.getElementById('example')
);
```

**注：**如果组件已经挂载到DOM中，该方法返回对应的本地浏览器 DOM 元素。当**render**返回**null** 或 **false**时，**this.findDOMNode()**也会返回**null**。从DOM 中读取值的时候，该方法很有用，如：获取表单字段的值和做一些 DOM 操作



## 5.条件渲染

在 React 中，可以创建不同的组件来封装各种你需要的行为。还可以根据应用的状态变化只渲染其中的一部分。

React 中的条件渲染和 JavaScript 中的一致，**使用 JavaScript 操作符 if 或条件运算符来创建表示当前状态的元素，然后让 React 根据它们来更新 UI**

```jsx
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}
 
ReactDOM.render(
  // 尝试修改 isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('example')
);
```

### 5.1 使用变量

**一般都是通过变量的改变来条件判断是否要显示该UI**

```jsx
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }
 
  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }
 
  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }
 
  render() {
    const isLoggedIn = this.state.isLoggedIn;
 
    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }
 
    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}
 
ReactDOM.render(
  <LoginControl />,
  document.getElementById('example')
);
```



### 5.2 使用运算符

- **&&运算符：**可以通过用花括号包裹代码在 JSX 中嵌入任何表达式 ，也包括 JavaScript 的逻辑与 &&，它可以方便地条件渲染一个元素

  ```jsx
  function Mailbox(props) {
    const unreadMessages = props.unreadMessages;
    return (
      <div>
        <h1>Hello!</h1>
        {unreadMessages.length > 0 &&
          <h2>
            您有 {unreadMessages.length} 条未读信息。
          </h2>
        }
      </div>
    );
  }
   
  const messages = ['React', 'Re: React', 'Re:Re: React'];
  ReactDOM.render(
    <Mailbox unreadMessages={messages} />,
    document.getElementById('example')
  );
  ```

- **三目运算符：**使用`condition ? true : false`的方式进行条件渲染

  ```jsx
  render() {
    const isLoggedIn = this.state.isLoggedIn;
    return (
      <div>
        {isLoggedIn ? (
          <LogoutButton onClick={this.handleLogoutClick} />
        ) : (
          <LoginButton onClick={this.handleLoginClick} />
        )}
      </div>
    );
  }
  ```



### 5.3 阻止条件渲染

在极少数情况下，可能希望隐藏组件，即使它被其他组件渲染。**让 render 方法返回 null 而不是它的渲染结果即可实现**

**注意：**组件的 render 方法返回 null 并不会影响该组件生命周期方法的回调。例如，`componentWillUpdate` 和 `componentDidUpdate` 依然可以被调用,**因为已经经历过组件了，只是没有渲染出UI界面**

```jsx
function WarningBanner(props) {
  if (!props.warn) {//返回null不会在render函数中显示出来
    return null;
  }
 
  return (
    <div className="warning">
      警告!
    </div>
  );
}
 
class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true}
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }
 
  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }
 
  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? '隐藏' : '显示'}
        </button>
      </div>
    );
  }
}
 
ReactDOM.render(
  <Page />,
  document.getElementById('example')
);
```



## 6.列表渲染

**对于数组来讲，通常我们会使用JS中的map方法来进行列表渲染**

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((numbers) =>
  <li>{numbers}</li>
);
 
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('example')
);
```

**注意：**在进行列表渲染的时候我们通常都会绑定key值,这样既能提高渲染效率，也能让我们操作列表中元素的时候不会出错，否则React会出现警告`a key should be provided for list items`

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}
 
const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('example')
);
```

### 6.1 keys

**Keys 可以在 DOM 中的某些元素被增加或删除的时候帮助 React 识别哪些元素发生了变化。**因此应当给数组中的每一个元素赋予一个确定的标识

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

**一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。**通常，我们使用来自数据的 id 作为元素的 key

```jsx
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

**当元素没有确定的 id 时，可以使用他的序列号索引 index 作为 key**

```jsx
const todoItems = todos.map((todo, index) =>
  // 只有在没有确定的 id 时使用
  <li key={index}>
    {todo.text}
  </li>
);
```

**注意：**

-  如果列表可以重新排序，不建议使用索引来进行排序，因为这会导致渲染变得很慢

- 组件内部无法访问key值,因为该值是提供给react组件内部使用的

#### 6.1.1 用keys提取组件

**元素的 key 只有在它和它的兄弟节点对比时才有意义。**比方说，如果提取出一个 `ListItem` 组件，应该把 key 保存在数组中的这个` <ListItem />元素上，而不是放在 ListItem 组件中的 `<li>` 元素上

```jsx
function ListItem(props) {
  // 对啦！这里不需要指定key:
  return <li>{props.value}</li>;
}
 
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 又对啦！key应该在数组的上下文中被指定
    <ListItem key={number.toString()}
              value={number} />
 
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
 
const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('example')
);
```

**注意：**key 会作为给 React 的提示，但不会传递给组件。如果组件中需要使用和 key 相同的值，请将其作为属性传递

```jsx
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
//Post 组件可以读出 props.id，但是不能读出 props.key
```



## 7.组件API

**7个主要的组件API：**

- **设置状态：**setState

- **替换状态：**replaceState

- **设置属性：**setProps

- **替换属性：**replaceProps

- **强制更新：**forceUpdate

- **获取DOM节点：**findDOMNode（在之前已提到）

- **判断组件挂载状态：**isMounted（已废除）

  ```jsx
  //作为替代,在生命周期中使用这种方法判断
  componentDidMount() {
      this.mounted = true;
  }
  
  componentWillUnmount() {
      this.mounted = false;
  }
  ```

  

### 7.1 setState

**setState是React通过继承在一个组件中的方法，该方法可以改变该组件中state值，从而实现更新页面**

**合并nextState和当前state，并重新渲染组件。setState是React事件处理函数中和请求回调函数中触发UI更新的主要方法**

**用法：**`setState(object nextState[, function callback])`

**参数：**

- **nextState**，将要设置的新状态，该状态会和当前的**state**合并
- **callback**，可选参数，回调函数。该函数会在**setState**设置成功，且组件重新渲染后调用，传入的参数为改变后的state的值

**注意：**

- **不能在组件内部通过this.state修改状态，因为该状态会在调用setState()后被替换**
- setState()总是会触发一次组件重绘，除非在shouldComponentUpdate()中实现了一些条件渲染逻辑

```jsx
class Counter extends React.Component{
  constructor(props) {
      super(props);
      this.state = {clickCount: 0};
      this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    this.setState(function(state) {
      return {clickCount: state.clickCount + 1};
    });
  }
  render () {
    return (<h2 onClick={this.handleClick}>点我！点击次数为: {this.state.clickCount}</h2>);
  }
}
ReactDOM.render(
  <Counter />,
  document.getElementById('example')
);
```

- **setState与后面的几种API都有两种传参方式**，除了传入一个对象之外还可以传入一个函数作为第一个参数，并在该回调函数中返回新的state对象

  ```jsx
  this.setState((prevState, props) => {
    return {
      age: prevState.age + props.age,
    };
  });
  ```

- **State 的更新可能是异步的，**setState()并不会立即改变this.state，而是创建一个即将处理的state。setState()并不一定是同步的，为了提升性能React会批量执行state和DOM渲染

  **执行setState()的位置**
  
  + 在react控制的回调函数中：生命周期钩子、react时间监听回调
  + 在非react控制的异步回调函数中：定时器回调、原生事件监听回调、Promise回调等
  
  **异步与同步**

  + 在react相关回调函数中为异步

  + 在其他异步回调中为同步，（注意异步函数中的宏队列与微队列，宏队列依次执行，微队列只用记住Promise会在每次Promise所在的宏队列的函数执行完才会执行（**但是微队列中再次调用微任务会一直添加到队列中，直到解决完毕才会跳出，所有如果添加微队列的速度大于执行速度就会照成宏队列堵塞**），每次执行新的宏队列前都会确保微队列是空的）
  
    **注意：**在异步更新时使用this.setState()**对对象形式的值进行设置时会先将要改变的值根据现在的this.state中的值进行设置后再改变的**，也就是说如果是使用对象形式的异步操作，要改变的值其实找就已经写好了，只是延迟更新，**而函数形式是要等到真正要更新界面的时候才会去那道this.state内部的值从而更新界面**
    
    ![1567610943133](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\1567610943133.png)
  
  因为 this.props 和 this.state 可能是异步更新的，所以不应该用它们当前的值去计算下一个 state 的值

  ```jsx
  // Wrong
  this.setState({
    counter: this.state.counter + this.props.increment,
  });
  ```
  
  要修复它，请**使用第二种形式的 setState() 来接受一个函数而不是一个对象。** 该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数
  
  ```jsx
  // Correct
  //第一个参数为原来的状态值
  this.setState((prevState, props) => ({
    counter: prevState.counter + props.increment
  }));
  ```
  
  **总结：**
  
  - 如果新状态不依赖于原状态，可以使用对象方式
  - 如果新这状态依赖于原状态，需要使用函数方式



### 7.2 replaceState

**replaceState()**方法与**setState()**类似，但是方法只会保留**nextState**中状态，原**state**不在**nextState**中的状态都会被删除，也就是说如果没有写在更新项中的值会被直接删除掉

**用法：**`replaceState(object nextState[, function callback])`

**参数：**

- **nextState**，将要设置的新状态，该状态会替换当前的**state**
- **callback**，可选参数，回调函数。该函数会在**replaceState**设置成功，且组件重新渲染后调用



### 7.3 setProps

该方法可以直接改变父组件设置到子组件的值（单方面改变子组件中映射的父组件的值，不会改变父组件的值本身），设置组件属性，并重新渲染组件。

**props**相当于组件的数据流，它总是会从父组件向下传递至所有的子组件中。当和一个外部的JavaScript应用集成时，我们可能会需要向组件传递数据或通知**React.render()**组件需要重新渲染，可以使用**setProps()**。

更新组件，可以在节点上再次调用**React.render()**，也可以通过**setProps()**方法改变组件属性，触发组件重新渲染

**用法：**`setProps(object nextProps[, function callback])`

**参数：**

- **nextProps**，将要设置的新属性，该状态会和当前的**props**合并
- **callback**，可选参数，回调函数。该函数会在**setProps**设置成功，且组件重新渲染后调用



### 7.3 replaceProps

**replaceProps()**方法与**setProps**类似，但它会删除原有 prop

**用法：**`replaceProps(object nextProps[, function callback])`

**参数：**

- **nextProps**，将要设置的新属性，该属性会替换当前的**props**
- **callback**，可选参数，回调函数。该函数会在**replaceProps**设置成功，且组件重新渲染后调用



### 7.4 forceUpdate

**forceUpdate()方法会使组件调用自身的render()方法重新渲染组件，组件的子组件也会调用自己的render()。**但是，组件重新渲染时，依然会读取this.props和this.state，如果状态没有改变，那么React只会更新DOM

forceUpdate()方法适用于this.props和this.state之外的组件重绘（如：修改了this.state后），通过该方法通知React需要调用render()，所以该方法相当于用户手动自己调用页面渲染

**注意：**一般来说，应该尽量避免使用forceUpdate()，而仅从this.props和this.state中读取状态并由React触发render()调用

**用法：**`forceUpdate([function callback])`

**参数：**

- **callback**，可选参数，回调函数。该函数会在组件**render()**方法调用后调用



## 8.组件生命周期

**组件的生命周期可分成三个状态：（以下是16.3前常用的，但是几个will（componentWillUnmount除外）会在17.0弃用）**

- **Mounting：**已插入真实 DOM
  - **constructor：**构造函数，在创建组件的时候调用一次，适合给state对象赋值初始化项目
  - **componentWillMount**：在渲染前调用,在客户端也在服务端
  - **render：**渲染UI
  - **componentDidMount** : 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)
- **Updating：**正在被重新渲染
  - **componentWillReceiveProps** 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用
  - **shouldComponentUpdate** 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用
    可以在你确认不需要更新组件时使用
  - **componentWillUpdate**在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用
  - **componentDidUpdate** 在组件完成更新后立即调用。在初始化时不会被调用，在这个阶段通过比较DOM树的更新可以根据情况发送Ajax请求
- **Unmounting：**已移出真实 DOM
  - **componentWillUnmount：**在组件中有元素（包含组件和内部的子元素，如删除一个购物车的商品）从 DOM 中移除之前立刻被调用，大多数在这个阶段都是用来保存数据的

```jsx
class Button extends React.Component {
  constructor(props) {
      super(props);
      this.state = {data: 0};
      this.setNewNumber = this.setNewNumber.bind(this);
  }
  
  setNewNumber() {
    this.setState({data: this.state.data + 1})
  }
  render() {
      return (
         <div>
            <button onClick = {this.setNewNumber}>INCREMENT</button>
            <Content myNumber = {this.state.data}></Content>
         </div>
      );
    }
}
 
 
class Content extends React.Component {
  componentWillMount() {
      console.log('Component WILL MOUNT!')
  }
  componentDidMount() {
       console.log('Component DID MOUNT!')
  }
  componentWillReceiveProps(newProps) {
        console.log('Component WILL RECEIVE PROPS!')
  }
  shouldComponentUpdate(newProps, newState) {
        return true;
  }
  componentWillUpdate(nextProps, nextState) {
        console.log('Component WILL UPDATE!');
  }
  componentDidUpdate(prevProps, prevState) {//在该生命周期中实现Vue中的updated和watch的效果
        console.log('Component DID UPDATE!')
  }
  componentWillUnmount() {
         console.log('Component WILL UNMOUNT!')
  }
 
    render() {
      return (
        <div>
          <h3>{this.props.myNumber}</h3>
        </div>
      );
    }
}
ReactDOM.render(
   <div>
      <Button />
   </div>,
  document.getElementById('example')
);
```

![img](https://upload-images.jianshu.io/upload_images/5287253-bd799f87556b5ecc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

### 8.1 getDerivedStateFromProps与getSnapshotBeforeUpdate

- **static getDerivedStateFromProps(nextProps, prevState)** 在组件创建时和更新时的render方法之前调用，它应该返回一个对象来更新状态，或者返回null来不更新任何内容

  **注意：**这个函数是一个静态函数，也就是说不能在里面调用this的方法

  ```jsx
  static getDerivedStateFromProps(nextProps, prevState) {
    //根据nextProps和prevState计算出预期的状态改变，返回结果会被送给setState
  }
  ```

  **该方法也是用来代替上面三个will的方法,上面三个wll能实现的功能都可以在这里实现**

- **getSnapshotBeforeUpdate()** 被调用于render之后，可以读取但无法使用DOM的时候。它使组件可以在可能更改之前从DOM捕获一些信息（例如滚动位置）。此生命周期返回的任何值都将作为参数传递给componentDidUpdate()

  ```jsx
  class ScrollingList extends React.Component {
    constructor(props) {
      super(props);
      // 创建ref
      this.listRef = React.createRef();
    }
  
    getSnapshotBeforeUpdate(prevProps, prevState) {
      //我们是否要添加新的 items 到列表?
      // 捕捉滚动位置，以便我们可以稍后调整滚动.
      if (prevProps.list.length < this.props.list.length) {
        const list = this.listRef.current;
        return list.scrollHeight - list.scrollTop;
      }
      return null;
    }
  
    componentDidUpdate(prevProps, prevState, snapshot) {
      //如果我们有snapshot值, 我们已经添加了 新的items.
      // 调整滚动以至于这些新的items 不会将旧items推出视图。
      // (这边的snapshot是 getSnapshotBeforeUpdate方法的返回值)
      if (snapshot !== null) {
        const list = this.listRef.current;
        list.scrollTop = list.scrollHeight - snapshot;
      }
    }
  
    render() {
      return (
        <div ref={this.listRef}>{/* ...contents... */}</div>
      );
    }
  }
  ```

![img](https://pic1.zhimg.com/80/v2-930c5299db442e73dbb1d2f9c92310d4_hd.jpg)



## 9.表单控件

**React对于表单控件做处理，不像原生的表单一样填写，而是需要通过onChange事件与事件对象属性value等相互结合实现双向数据绑定（React是单向数据流，但是是通过这种方法和Vue一样实现双向数据绑定）**

在 HTML 当中，像 `<input>`, `<textarea>`, 和 `<select>` 这类表单元素会维持自身状态，并根据用户输入进行更新。但在React中，可变的状态通常保存在组件的状态属性中，**并且只能用 setState() 方法进行更新**

**注意：**如果在React的表单控件中还是用原生的方法使用表单控件会没有作用，而且如果设置了默认值的value而不使用onChange事件React会丢出警告，并且表单默认值也不是改在value属性上,因为这个属性应该是一个动态的属性，**React中应该使用另外一个H5表单控件属性defaultValue来显示表单的默认值**

**在React中的两种表单控件**

- **非受控型（非约束型）表单控件：**用户输入的值我们根本不知道，也无法进行操作，这种表单控件没有将自己的状态与React联系起来
- **受控型（约束型）表单控件：**让React管理表单的数据，能够监视表单的一举一动

```jsx
class HelloMessage extends React.Component {
  constructor(props) {
      super(props);
      this.state = {value: 'Hello Runoob!'};
      this.handleChange = this.handleChange.bind(this);
  }
 
  handleChange(event) {
    this.setState({value: event.target.value});
  }
  render() {
    var value = this.state.value;
    return <div>
            <input type="text" value={value} onChange={this.handleChange} /> 
            <h4>{value}</h4>
           </div>;
  }
}
ReactDOM.render(
  <HelloMessage />,
  document.getElementById('example')
);
```

**注意：**受控组件中表单组件的值不能是undefined或null，否则react将认为这是一个非受控组件，会报错

### 9.1 在子组件使用表单

需要在父组件通过创建事件句柄 (**handleChange**) ，并作为 prop (**updateStateProp**) 传递到子组件上

```jsx
class Content extends React.Component {
  render() {
    return  <div>
        	{
            	//htmlFor用在表单lable这里
        	}
        	<label htmlFor="example">Example</label>
            <input type="text" name="example" value={this.props.myDataProp} onChange={this.props.updateStateProp} /> 
            <h4>{this.props.myDataProp}</h4>
            </div>;
  }
}
class HelloMessage extends React.Component {
  constructor(props) {
      super(props);
      this.state = {value: 'Hello Runoob!'};
      this.handleChange = this.handleChange.bind(this);
  }
 
  handleChange(event) {
    this.setState({value: event.target.value});
  }
  render() {
    var value = this.state.value;
    return <div>
            <Content myDataProp = {value} 
              updateStateProp = {this.handleChange}></Content>
           </div>;
  }
}
ReactDOM.render(
  <HelloMessage />,
  document.getElementById('example')
);
```



### 9.2 表单元素类型

在React中，所有的表单元素的值都需要通过onChange事件来改变，除了`checkbox`多选框与`radio`单选框是通过`checked`之外，所有的表单元素都是通过value属性来判断绑定的值的

- **select下拉菜单**

  **在 React 中，不使用 selected 属性，而在根 select 标签上用 value 属性来表示选中项**

  ```jsx
  class FlavorForm extends React.Component {
    constructor(props) {
      super(props);
      this.state = {value: 'coconut'};
   
      this.handleChange = this.handleChange.bind(this);
      this.handleSubmit = this.handleSubmit.bind(this);
    }
   
    handleChange(event) {
      this.setState({value: event.target.value});
    }
   
    handleSubmit(event) {
      alert('Your favorite flavor is: ' + this.state.value);
      event.preventDefault();
    }
   
    render() {
      return (
        <form onSubmit={this.handleSubmit}>
          <label>
            选择您最喜欢的网站
              {
                  // 也建议使用非受控组件直接通过ref获取select的value值
              }
            <select value={this.state.value} onChange={this.handleChange}>
              <option value="gg">Google</option>
              <option value="rn">Runoob</option>
              <option value="tb">Taobao</option>
              <option value="fb">Facebook</option>
            </select>
          </label>
          <input type="submit" value="提交" />
        </form>
      );
    }
  }
   
  ReactDOM.render(
    <FlavorForm />,
    document.getElementById('example')
  );
  ```

- **radio**

  ```jsx
  class FlavorForm extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
          value:0
      };
   
      this.handleChange = this.handleChange.bind(this);
      this.handleSubmit = this.handleSubmit.bind(this);
    }
   
    handleChange(e) {
      this.setState({
          value:e.target.value
      });
    }
   
    handleSubmit(event) {
      console.log(this.state.value)
      event.preventDefault();
    }
   
    render() {
      return (
        <form onSubmit={this.handleSubmit}>
          <label>
            选择您最喜欢的网站
            <input 
                value={0} 
                checked={this.state.value === 0} 
                onChange={this.handleChange}
                />Google
               <input 
                value={1} 
                checked={this.state.value === 2} 
                onChange={this.handleChange}
                />Runoob
          </label>
          <input type="submit" value="提交" />
        </form>
      );
    }
  }
   
  ReactDOM.render(
    <FlavorForm />,
    document.getElementById('example')
  );
  ```

- **checkbox**

  ```jsx
  class FlavorForm extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
          arr:[{
              value:'Google',
              checked:false
          },
          {
              value:'Runoob',
              checked:false
          },
          {
              value:'Taobao',
              checked:false
          },
          {
              value:'Facebook',
          	checked:false         
          }]
      };
   
      this.handleChange = this.handleChange.bind(this);
      this.handleSubmit = this.handleSubmit.bind(this);
    }
   
    handleChange(index) {
      let arr = this.state.arr;
      arr[index].checked = !arr[index].checked
      this.setState({
          arr
      });
    }
   
    handleSubmit(event) {
      console.log(this.state.arr)
      event.preventDefault();
    }
   
    render() {
      return (
        <form onSubmit={this.handleSubmit}>
          <label>
            选择您最喜欢的网站
              {
                  this.state.arr.length>0&&this.state.arr.map((item,index)=>{
  						return (
                          <div key={index}>
                              <input 
                                  type="checkbox" 
                                  checked={item.checked} 
                                  onChange={this.handleChange(index)}/>
                          </div>
                          
                          )
                  })
              }
          </label>
          <input type="submit" value="提交" />
        </form>
      );
    }
  }
   
  ReactDOM.render(
    <FlavorForm />,
    document.getElementById('example')
  );
  ```



### 9.3 多个表单

当处理多个 input 元素时，可以通过给每个元素添加一个 name 属性，来让处理函数根据 **event.target.name** 的值来选择做什么，**这是进行多个表单处理的最便捷的方法**

```jsx
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };
 
    this.handleInputChange = this.handleInputChange.bind(this);
  }
 
  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;
 
    this.setState({
      [name]: value
    });
  }
 
  render() {
    return (
      <form>
        <label>
          是否离开:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          访客数:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```




## 10.脚手架

- 使用`npm i create-react-app -g`全局安装脚手架工具
- cd到指定目录通过`create-react-app 项目名`创建一个react项目（注意项目名必须是小写字母或下划线）
- 使用`npm start`启动项目，使用`npm run build`打包项目
- 默认启动的时候会占有3000端口，如果端口被占用会默认一次加一

**目录结构**

- **public目录：**静态资源目录,如静态html,css,js
- **src目录：**资源目录，包含组件（component）和图片、html、css、js

### 10.1 图片引入问题

当使用脚手架时，在使用以前直接写字符串的方式引入本地图片的路径会自动在前面加上public目录来查找（包括写在外部CSS的背景图片）,所以应该写在public目录中,而如果不是在public目录中的话需要使用`require('图片相对路径')`的方式引入模块来作为img标签的src属性值，这样才能正常渲染出来

**注意：**在用require的时候相对路径引用同级时一定要在前面加上`./`，如果不加会在`node_modules`中查找

```jsx
import React from 'react'
import logo from './logo.svg'//外部引入图片作为对象
import './App.css'

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <img src="/static/1.jpg" alt="" />
        {
        	//该目录应该是从public目录开始的    
        }
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  )
}

export default App
```



## 11.路由

**路由可以让操作者更加简便的控制页面要显示的内容，它可以让操作者向应用中快速地添加视图和数据流，同时保持页面与 URL 间的同步**

**在react中有两种路由组件库：**

- **react-router：**提供了一些router的核心api，静态的，如Router, Route, Switch等，但是它没有提供dom操作进行跳转的api

- **react-router-dom：**提供了BrowserRouter, Route, Link等api，动态的，该组件库是react-router的加强版，我们可以通过DOM的事件控制路由，不过react-router-dom是依耐于react-router的，所以下载这个库也会下载react-router

### 11.1 BrowerserRouter路由和HashRouter路由

**在React中这两个组件时路由的基本,进行路由跳转的时候需要跳转的组件都需要放在这两个组件内部**，类似路由器一样去管理网络及给每个接入进来的用户分发ip一样，这两者是一个大容器包裹着路由组件，页面内要进行跳转的UI只会在这个容器中发生变化

#### 11.1.1 HashRouter

**HashRouter**是通过hash值来对路由进行控制。如果使用HashRouter，你的路由就会默认有#，如果向服务器发送请求，#后面的部分不会作为请求地址，仅仅只会讲#前面的地址作为服务器的请求地址。`<HashRouter>` 使用 URL 的 `hash` 部分（即 `window.location.hash`）来保持 UI 和 URL 的同步

```jsx
import { HashRouter } from 'react-router-dom';

<HashRouter>
  <App />
</HashRouter>
```

**注意：**使用 `hash` 记录导航历史不支持 `location.key` 和 `location.state`。在以前的版本中，视图 shim 这种行为，但是仍有一些问题无法解决。任何依赖此行为的代码或插件都将无法正常使用。由于该技术仅用于支持旧式（低版本）浏览器，因此对于一些新式浏览器，鼓励使用 `<BrowserHistory>` 代替

**参数属性：**

+ **basename: string：**所有位置的基准 URL。`basename` 的正确格式是前面有一个前导斜杠，但不能有尾部斜杠

  ```jsx
  <HashRouter basename="/calendar">
    <Link to="/today" />
  </HashRouter>
  ```

  上例中的 `<Link>` 最终将被呈现为：

  ```jsx
  <a href="#/calendar/today" />
  ```

+ **getUserConfirmation: func：**用于确认导航的函数，默认使用 `window.confirm`。例如，当从 `/a` 导航至 `/b` 时，会使用默认的 `confirm` 函数弹出一个提示，用户点击确定后才进行导航，否则不做任何处理

  ```jsx
  // 这是默认的确认函数
  const getConfirmation = (message, callback) => {
    const allowTransition = window.confirm(message);
    callback(allowTransition);
  }
  
  <HashRouter getUserConfirmation={getConfirmation} />
  ```

+ **hashType: string：**`window.location.hash` 使用的 `hash` 类型

  有如下几种：**默认为 `slash`**

  - `slash` - 后面跟一个斜杠，例如 #/ 和 #/sunshine/lollipops
  - `noslash` - 后面没有斜杠，例如 # 和 #sunshine/lollipops
  - `hashbang` - Google 风格的 [ajax crawlable](https://developers.google.com/webmasters/ajax-crawling/docs/learn-more)，例如 #!/ 和 #!/sunshine/lollipops



#### 11.1.2 BrowerserRouter

**BrowerserRoute**r的原理是使用HTML5 history API (pushState, replaceState, popState)来使内容随着url动态改变的，该路由的地址也是真实的地址，如果发送请求会以该地址为请求地址发送

```jsx
import { BrowserRouter } from 'react-router-dom';

<BrowserRouter
  basename={string}
  forceRefresh={bool}
  getUserConfirmation={func}
  keyLength={number}
>
  <App />
</BrowserRouter>
```

**参数属性：**

+ **basename: string：**所有位置的基准 URL。如果你的应用程序部署在服务器的子目录，则需要将其设置为子目录。`basename` 的正确格式是前面有一个前导斜杠，但不能有尾部斜杠

  ```jsx
  <BrowserRouter basename="/calendar">
    <Link to="/today" />
  </BrowserRouter>
  ```

  上例中的 `<Link>` 最终将被呈现为：

  ```jsx
  <a href="/calendar/today" />
  ```

+ **forceRefresh: bool：**如果为 `true` ，在导航的过程中整个页面将会刷新。一般情况下，只有在不支持 HTML5 history API 的浏览器中使用此功能

  ```jsx
  const supportsHistory = 'pushState' in window.history;
  
  <BrowserRouter forceRefresh={!supportsHistory} />
  ```

+ **getUserConfirmation: func：**用于确认导航的函数，默认使用 `window.confirm`

  ```jsx
  // 这是默认的确认函数
  const getConfirmation = (message, callback) => {
    const allowTransition = window.confirm(message);
    callback(allowTransition);
  }
  
  <BrowserRouter getUserConfirmation={getConfirmation} />
  ```

  **注：**需要配合 `<Prompt>` 一起使用

+ **keyLength: number：**`location.key` 的长度，默认为 `6`

  ```jsx
  <BrowserRouter keyLength={12} />
  ```

##### 11.1.1 BrowerserRouter模式配置

- **BrowerserRouter模式下配置nginx**

  ```js
  location / {
    try_files $uri $uri/ /index.html;
  }
  ```

- **BrowerserRouter模式下配置Apache**

  ```html
  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.html$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.html [L]
  </IfModule>
  ```

- **BrowerserRouter模式下配置Node.js**	

  ```js
  const http = require('http')
  const fs = require('fs')
  const httpPort = 80
  
  http.createServer((req, res) => {
    fs.readFile('index.htm', 'utf-8', (err, content) => {
      if (err) {
        console.log('We cannot open "index.htm" file.')
      }
  
      res.writeHead(200, {
        'Content-Type': 'text/html; charset=utf-8'
      })
  
      res.end(content)
    })
  }).listen(httpPort, () => {
    console.log('Server listening on: http://localhost:%s', httpPort)
  })
  ```



#### 11.1.3 注意事项

- 在开发阶段还没有部署到服务器上无论hash还是history模式的路由都可以正常使用，但是**部署到服务器上的时候只有hash路由能够正常使用**
- 所以服务器需要做好处理 URL 的准备。处理应用启动最初的 `/` 这样的请求应该没问题，但当用户来回跳转并在 `/accounts/123` 刷新时，服务器就会收到来自 `/accounts/123` 的请求，**这时需要处理这个 URL 并在响应中包含 JavaScript 应用代码，如果在服务器上找不到这个请求的文件就会报404错误**
- 最好再Router内部只有一个直接的子元素
- 一般情况下是直接包裹最外层的App组件，这样内部的无论做什么都可以直接使用路由其他的组件进行操作了



### 11.2 Route

`<Route>` 可能是 React Router 中最重要的组件，**最基本的职责是在其 `path` 属性与某个 location 匹配时呈现一些 UI**

#### 11.2.1 Route的渲染方法

使用 `<Route>` 渲染一些内容有以下三种方式：

- **Route component**
- **Route render**
- **Route children**

在不同的情况下使用不同的方式。在指定的 `<Route>` 中，**应该只使用其中的一种**。在大多数情况下你会使用 `component`

**注：**三种渲染方式都将为被渲染的组件提供相同的三个路由属性：`match`、`location`和`history`



#### 11.2.2 参数属性

下面参数属性的前三个是三种渲染方法

- **component：**指定只有当位置匹配时才会渲染的 React 组件，该组件会接收route props（也就是路由自带的三个路由属性）作为属性

  ```jsx
  const User = ({ match }) => {
    return <h1>Hello {match.params.username}!</h1>
  }
  
  <Route path="/user/:username" component={User} />
  ```

  **注意：**当使用 `component`（而不是 `render` 或 `children`）时，Router 将根据指定的组件，使用 `React.createElement` 创建一个新的 React 元素。这意味着，如果你向 `component` 提供一个内联函数，那么每次渲染都会创建一个新组件。这将导致现有组件的卸载和新组件的安装，而不是仅仅更新现有组件。所以，当使用内联函数进行内联渲染时，请使用 `render` 或 `children`

- **render: func：**使用 `render` 可以方便地进行内联渲染和包装，而无需进行上文解释的不必要的组件重装。

  可以传入一个函数，以在位置匹配时调用，而不是使用 `component` 创建一个新的 React 元素。`render` 渲染方式接收所有与 `component` 方式相同的route props

  ```jsx
  // 方便的内联渲染
  <Route path="/home" render={() => <div>Home</div>} />
  
  // 包装
  const FadingRoute = ({ component: Component, ...rest }) => (
    <Route {...rest} render={props => (
      <FadeIn>
        {
             // 需要通过解构获取路由属性
  	   }
        <Component {...props} />
      </FadeIn>
    )} />
  )
  
  <FadingRoute path="/cool" component={Something} />
  ```

  **总的来说，render的作用更加强大和全面**

- **children: func：**有时候不论 `path` 是否匹配位置，都想渲染一些内容。在这种情况下，可以使用 `children` 属性。除了不论是否匹配它都会被调用以外，它的工作原理与 `render` 完全一样。

  `children` 渲染方式接收所有与 `component` 和 `render` 方式相同的route props，**除非路由与 URL 不匹配，不匹配时 `match` 为 `null**`。这允许你可以根据路由是否匹配动态地调整用户界面。如下所示，如果路由匹配，我们将添加一个激活类：

  ```jsx
  const ListItemLink = ({ to, ...rest }) => (
    <Route path={to} children={({ match }) => (
      <li className={match ? 'active' : ''}>
        <Link to={to} {...rest} />
      </li>
    )} />
  )
  
  <ul>
    <ListItemLink to="/somewhere" />
    <ListItemLink to="/somewhere-else" />
  </ul>
  ```

  **对动画效果来说也有大用**

  ```jsx
  <Route children={({ match, ...rest }) => (
    {/* Animate 将始终渲染，因此你可以利用生命周期来为其子元素添加进出动画 */}
    <Animate>
      {match && <Something {...rest} />}
    </Animate>
  )} />
  ```

- **path:string：**用作标识路由的路径，在后面可以跟params与query参数.可以是 [path-to-regexp](https://www.npmjs.com/package/path-to-regexp) 能够理解的任何有效的 URL 路径

  ```jsx
  <Route path="/users/:id" component={User} />
  ```

  **注：**没有定义 `path` 的 `<Route>` 总是会被匹配

- **exact:bool：**严格匹配路由的路径，必须要在与对应的path完全一致的路径时才能匹配成功，如果不写这个在有相同的路径匹配成功时会一起显示组件

  ```jsx
  <Route exact path="/one" component={OneComponent} />
  ```

  ![å¾çæè¿°](https://segmentfault.com/img/bV8fyF?w=237&h=69)

- **strict: bool：**如果为 `true`，则具有尾部斜杠的 `path` 仅与具有尾部斜杠的 `location.pathname` 匹配。当 `location.pathname` 中有附加的 URL 片段时，`strict` 就没有效果了

  ```jsx
  <Route strict path="/one/" component={OneComponent} />
  ```

  ![å¾çæè¿°](https://segmentfault.com/img/bV8fz8?w=206&h=97)

  **注意：**可以使用 `strict` 来强制规定 `location.pathname` 不能具有尾部斜杠，但是为了做到这一点，`strict` 和 `exact` 必须都是 `true`

  ![å¾çæè¿°](https://segmentfault.com/img/bV8fAP?w=202&h=95)

- **location: object：**一般情况下，`<Route>` 尝试将其 `path` 与当前历史位置（通常是当前的浏览器 URL）进行匹配。但是，也可以传递具有不同路径名的位置进行匹配。当需要将 `<Route>` 与当前历史位置以外的 `location` 进行匹配时，此功能非常有用

  **注：**如果一个 `<Route>` 被包含在一个 `<Switch>` 中，并且需要匹配的位置（或当前历史位置）传递给了 `<Switch>`，那么传递给 `<Route>` 的 `location` 将被 `<Switch>` 所使用的 `location` 覆盖

- **sensitive: bool：**如果为 `true`，进行匹配时将区分大小写

  ```jsx
  <Route sensitive path="/one" component={OneComponent} />
  ```

  ![å¾çæè¿°](https://segmentfault.com/img/bV8f5g?w=255&h=98)

```jsx
import React from 'react'
import { BrowerserRouter as  Router,Route } from 'react-router-dom'
import App from './App'
import About from './About'
import Inbox from './Inbox'

React.render((
  <Router>
      <div>
      <Route path="/" exact component={App}>
      <Route path="/about" component={About} />
      <Route path={"/inbox"} component={Inbox} />
      </div>
  </Router>
), document.body)
```

**注：**Route必须包含在Router组件类才能使用，不然会报错



### 11.3 Link和NavLink

**两者都是可以控制路由跳转的，实质上时a标签，不过封装了一些内部的API**，Link标签本身有一个**to**属性，**可以接受string或者一个object，来控制url，**而NavLink则是有更多的API使用，除了控制url跳转，还可以为当前选中的路由设置类名、样式以及回调函数等

#### 11.3.1 Link

```jsx
import { Link } from 'react-router-dom';

<Link to="/about">About</Link>
```

**参数属性：**

- **to: string：**一个字符串形式的链接地址，通过 `pathname`、`search` 和 `hash` 属性创建

  ```jsx
  <Link to='/courses?sort=name' />
  ```

- **to: object：**一个对象形式的链接地址，可以具有以下任何属性：

  - `pathname` - 要链接到的路径
  - `search` - 查询参数
  - `hash` - URL 中的 hash，例如 #the-hash
  - `state` - 存储到 location 中的额外状态数据

  ```jsx
  <Link to={{
    pathname: '/courses',
    search: '?sort=name',
    hash: '#the-hash',
    state: {
      fromDashboard: true
    }
  }} />
  ```

- **replace: bool：**当设置为 `true` 时，点击链接后将替换历史堆栈中的当前条目，而不是添加新条目。默认为 `false`

  ```jsx
  <Link to="/courses" replace />
  ```

- **innerRef: func：**允许访问组件的底层引用

  ```jsx
  const refCallback = node => {
    // node 指向最终挂载的 DOM 元素，在卸载时为 null
  }
  
  <Link to="/" innerRef={refCallback} />
  ```

- **others：**还可以传递一些其它属性，例如 `title`、`id` 或 `className` 等

  ```jsx
  <Link to="/" className="nav" title="a title">About</Link>
  ```



#### 11.3.2 NavLink

一个特殊版本的 `<Link>`，它会在与当前 URL 匹配时为其呈现元素添加样式属性

```jsx
import { NavLink } from 'react-router-dom';

<NavLink to="/about">About</NavLink>
```

**参数属性：**

- **activeClassName: string：**当元素处于激活状态时应用的类，默认为 `active`。它将与 `className` 属性一起使用

  ```jsx
  <NavLink to="/faq" activeClassName="selected">FAQs</NavLink>
  ```

- **activeStyle: object：**当元素处于激活状态时应用的样式

  ```jsx
  const activeStyle = {
    fontWeight: 'bold',
    color: 'red'
  };
  
  <NavLink to="/faq" activeStyle={activeStyle}>FAQs</NavLink>
  ```

- **exact: bool：**如果为 `true`，则只有在位置完全匹配时才应用激活类/样式

  ```jsx
  <NavLink exact to="/profile">Profile</NavLink>
  ```

- **strict: bool：**如果为 `true`，则在确定位置是否与当前 URL 匹配时，将考虑位置的路径名后面的斜杠

  ```jsx
  <NavLink strict to="/events/">Events</NavLink>
  ```

- **isActive: func：**添加额外逻辑以确定链接是否处于激活状态的函数。如果你要做的不仅仅是验证链接的路径名与当前 URL 的路径名相匹配，那么应该使用它

  ```jsx
  // 只有当事件 id 为奇数时才考虑激活
  const oddEvent = (match, location) => {
    if (!match) {
      return false;
    }
    const eventID = parseInt(match.params.eventID);
    return !isNaN(eventID) && eventID % 2 === 1;
  }
  
  <NavLink to="/events/123" isActive={oddEvent}>Event 123</NavLink>
  ```

  - **location: object：**`isActive` 默认比较当前历史位置（通常是当前的浏览器 URL）。也可以传递一个不同的位置进行比较

```jsx
import React from 'react'
import { BrowerserRouter as  Router,Route,NavLink } from 'react-router-dom'
import App from './App'
import About from './About'
import Inbox from './Inbox'

React.render((
  <Router>
      <div>
      
       {
       	//exact精准匹配,不然在其他两个页面一样会显示App页面     
       }
      <ul>
          {
          	//NavLink组件也需要使用exact来完全匹配
            //activeClassName是匹配到对应路由是NavLink的活跃样式
		  }
          <li><NavLink activeClassName={'hover'} exact to="/">About</NavLink></li>
          <li><NavLink activeClassName={'hover'} to="/about">About</NavLink></li>
          <li><NavLink activeClassName={'hover'} to="/inbox">Inbox</NavLink></li>
        </ul>
      <Route path="/" exact component={App}>
      <Route path="/about" component={About} />
      <Route path="/inbox" component={Inbox} />
     </div>
    </Route>
  </Router>
), document.body)
```



### 11.4 Switch

**用于渲染与路径匹配的第一个子 `<Route>` 或 `<Redirect>`，它里面不能放其他html元素，该组件的作用是用来只显示一个路由，就和JS中的switch用法一样，从上至下进行匹配，匹配到之后就不会再进行匹配了，该组件与Route组件的exact属性一样都是用做处理精准匹配问题的**

```jsx
import { Switch, Route } from 'react-router';

<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/about" component={About} />
  <Route path="/:user" component={User} />
  <Route component={NoMatch} />
</Switch>
```

当我们在 `/about` 路径时，`<Switch>` 将开始寻找匹配的 `<Route>`。我们知道，`<Route path="/about" />` 将会被正确匹配，这时 `<Switch>` 会停止查找匹配项并立即呈现 `<About>`。同样，如果我们在 `/michael` 路径时，那么 `<User>`会呈现

**同时，这对于动画转换也很有用，因为匹配的 `<Route>` 与前一个渲染位置相同**

```jsx
<Fade>
  <Switch>
    {/* 这里只会渲染一个子元素 */}
    <Route />
    <Route />
  </Switch>
</Fade>

<Fade>
  <Route />
  <Route />
  {/* 这里总是会渲染两个子元素，也有可能是空渲染，这使得转换更加麻烦 */}
</Fade>
```

**参数属性：**

- **location: object：**用于匹配子元素而不是当前历史位置（通常是当前的浏览器 URL）的location对象

- **children: node：**所有 `<Switch>` 的子元素都应该是 `<Route>` 或 `<Redirect>`。只有第一个匹配当前路径的子元素将被呈现。**`<Route>` 组件使用 `path` 属性进行匹配，而 `<Redirect>` 组件使用它们的 `from` 属性进行匹配。没有 `path` 属性的 `<Route>` 或者没有 `from` 属性的 `<Redirect>` 将始终与当前路径匹配。**

  **当在 `<Switch>` 中包含 `<Redirect>` 时，你可以使用任何 `<Route>` 拥有的路径匹配属性**：`path`、`exact` 和 `strict`。`from` 只是 `path` 的别名。并且，**如果给 `<Switch>` 提供一个 `location` 属性，它将覆盖匹配的子元素上的 `location` 属性**

  ```jsx
  //404的处理方法1
  <Switch>
    <Route exact path="/" component={Home} />
    <Route path="/users" component={Users} />
    <Redirect from="/accounts" to="/users" />
    <Route component={NoMatch} />
  </Switch>
  ```

  ```jsx
  //404的处理方法2
  <Switch>
    <Route exact path="/" component={Home} />
    <Route path="/users" component={Users} />
    <Redirect from="/accounts" to="/users" />
    <Route path="/404" component={NoMatch} />
    <Redirect to="/404" />
  </Switch>
  ```

```jsx
import React from 'react'
import { BrowerserRouter as  Router,Route } from 'react-router-dom'
import App from './App'
import About from './About'
import Inbox from './Inbox'
import No404 from './No404'

React.render((
  <Router>
      <Switch>
        {
        	//这个时候如果不完全匹配只会匹配到第一个APP组件      
        }
      <Route path="/"  component={App}>
      <Route path="/about" component={About} />
      <Route path="/inbox" component={Inbox} />
       {
       	//都没有匹配到最后一个全局匹配常用来做404页面       
       }
      <Route path="*" component={No404} />
      </Switch>
  </Router>
), document.body)
```



### 11.5 Redirect

**该组件可以理解为一个理解执行的方法，与编程式导航类似，只要在页面上渲染了该组件，页面就会立刻重定向到指定的页面（默认是替换网页），所以该组件一般是通过调节渲染的方式渲染到页面上的**，如：进入到404页面后5秒后跳转到首页、将其余路由重定向到404页面（需配合Switch使用，放在Switch最后一个）

```jsx
import { Route, Redirect } from 'react-router-dom';

<Route exact path="/" render={() => (
  loggedIn ? (
    <Redirect to="/dashboard" />
  ) : (
    <PublicHomePage />
  )
)}
```

**参数属性：**

- **to: string：**要重定向到的 URL，可以是 [path-to-regexp](https://www.npmjs.com/package/path-to-regexp) 能够理解的任何有效的 URL 路径。所有要使用的 URL 参数必须由 `from` 提供

  ```jsx
  <Redirect to="/somewhere/else" />
  ```

- **to: object：**要重定向到的位置，其中 `pathname` 可以是 [path-to-regexp](https://www.npmjs.com/package/path-to-regexp) 能够理解的任何有效的 URL 路径

  ```jsx
  <Redirect to={{
    pathname: '/login',
    search: '?utm=your+face',
    state: {
      referrer: currentLocation
    }
  }} />
  ```

  **注：**上例中的 `state` 对象可以在重定向到的组件中通过 `this.props.location.state` 进行访问。而 `referrer` 键（不是特殊名称）将通过路径名 `/login` 指向组件`this.props.location.state.referrer` 进行访问

- **push: bool：**如果为 `true`，重定向会将新的位置推入历史记录，而不是替换当前条目

  ```jsx
  <Redirect push to="/somewhere/else" />
  ```

- **from: string：**要从中进行重定向的路径名，可以是 [path-to-regexp](https://www.npmjs.com/package/path-to-regexp) 能够理解的任何有效的 URL 路径。所有匹配的 URL 参数都会提供给 `to`，必须包含在 `to` 中用到的所有参数，`to` 未使用的其它参数将被忽略

  **注意：**只能在 `<Switch>` 组件内使用 `<Redirect from>`，以匹配一个位置

  ```jsx
  <Switch>
    <Redirect from='/old-path' to='/new-path' />
    <Route path='/new-path' component={Place} />
  </Switch>
  ```

  ```jsx
  // 根据匹配参数进行重定向
  <Switch>
    <Redirect from='/users/:id' to='/users/profile/:id' />
    <Route path='/users/profile/:id' component={Profile} />
  </Switch>
  ```

  **注意：**经过实践，发现以上“根据匹配参数进行重定向”的示例存在bug，没有效果。`to` 中的 `:id` 并不会继承 `from` 中的 `:id` 匹配的值，而是直接作为字符串显示到浏览器地址栏

- **exact: bool：**完全匹配

- **strict: bool：**严格匹配

```jsx
//404页面
import { Route, Redirect } from 'react-router-dom';
class No404 extends React.Component {
   state={
            count:0
      }
    constructor(props) {
        super(props);
      }
    componentDidMount(){
        //5秒后跳转
        setTimeout({
          this.setState({
            count:1
          })
        },5000)
    }
    render() {
        return (
            <div>{this.state.count?<Redirect to="/"></Redirect>:null}</div>
        );
    }
}
export default No404
```

```jsx
//跳转到404
import React from 'react'
import { BrowerserRouter as  Router,Route,Redirect } from 'react-router-dom'
import App from './App'
import About from './About'
import Inbox from './Inbox'
import No404 from './No404'

React.render((
  <Router>
      <Switch>
        {
        	//这个时候如果不完全匹配只会匹配到第一个APP组件      
        }
      <Route path="/"  component={App}>
      <Route path="/about" component={About} />
      <Route path="/inbox" component={Inbox} />
      <Route path="/404" component={No404} />
      <Redirect to='/404'/>
      </Switch>
  </Router>
), document.body)
```



### 11.6 Prompt

用于在位置跳转之前给予用户一些确认信息。当你的应用程序进入一个应该阻止用户导航的状态时（比如表单只填写了一半），弹出一个提示

```jsx
import { Prompt } from 'react-router-dom';

<Prompt
  when={formIsHalfFilledOut}
  message="你确定要离开当前页面吗？"
/>
```

**属性参数：**

- **message: string：**当用户试图离开某个位置时弹出的提示信息

  ```jsx
  <Prompt message="你确定要离开当前页面吗？" />
  ```

- **message: func：**将在用户试图导航到下一个位置时调用。需要返回一个字符串以向用户显示提示，或者返回 `true` 以允许直接跳转

  ```jsx
  <Prompt message={location => {
    const isApp = location.pathname.startsWith('/app');
  
    return isApp ? `你确定要跳转到${location.pathname}吗？` : true;
  }} />
  ```

  **注：**上例中的 `location` 对象指的是下一个位置（即用户想要跳转到的位置）。你可以基于它包含的一些信息，判断是否阻止导航，或者允许直接跳转

- **when: bool：**在应用程序中，你可以始终渲染 `<Prompt>` 组件，并通过设置 `when={true}` 或 `when={false}` 以阻止或允许相应的导航，而不是根据某些条件来决定是否渲染 `<Prompt>` 组件

  ```jsx
  <Prompt when={true} message="你确定要离开当前页面吗？" />
  ```

  **注：**`when` 只有两种情况，当它的值为 `true` 时，会弹出提示信息。如果为 `false` 则不会弹出



### 11.7 路由参数

**路由参数主要分为两种：params类型与query类型**

- **params：**在React的路由中,可以直接将params解析到对应组件的this.props的match属性中作位一个对象存在

  ```jsx
  import React from 'react'
  import { BrowerserRouter as  Router,Route,NavLink } from 'react-router-dom'
  import App from './App'
  import About from './About'
  import Inbox from './Inbox'
  
  React.render((
    <Router>
        <div>
        <ul>
            <li><NavLink activeClassName={'hover'} exact to="/1">About</NavLink></li>
            <li><NavLink activeClassName={'hover'} to="/about">About</NavLink></li>
            <li><NavLink activeClassName={'hover'} to="/inbox">Inbox</NavLink></li>
          </ul>
        {
         //加入?表示时可选的参数，这样在路由匹配的时候才不会直接跳过去       
  	   }
        <Route path="/:id?" exact component={App}>
        <Route path="/about" component={About} />
        <Route path="/inbox" component={Inbox} />
      </Route>
      </div>
    </Router>
  ), document.body)
  ```

  ```jsx
  //App.jsx
  import React,{Component} from 'react'
  
  class App extends Component{
      render(){
         	//获取上面得到的id
          const id = this.props.match.params.id
          console.log(id)
          return(
          	<div>我的id为{id}</div>
          )
      }
  }
  
  export default App
  ```

- **query：**该值通过`?key=value`的形式在url中进行传输,在this.props中只有字符查询字符串形式的参数，并没有作为对象将键值对解析出来，所以一般我们会**引用node中的url模块对query进行解析（当然也可以使用其它的，如query-string）**

  ```jsx
  import React from 'react'
  import { BrowerserRouter as  Router,Route,NavLink } from 'react-router-dom'
  import App from './App'
  import About from './About'
  import Inbox from './Inbox'
  
  React.render((
        <div>
        <ul>
            <li><NavLink activeClassName={'hover'} exact to="/1">About</NavLink></li>
            <li><NavLink activeClassName={'hover'} to="/about">About</NavLink></li>
            <li><NavLink activeClassName={'hover'} to="/inbox">Inbox</NavLink></li>
          </ul>
        <Route path="/:id" exact component={App}>
        <Route path="/about" component={About} />
        <Route path="/inbox" component={Inbox} />
      </Route>
      </div>
    </Router>
  ), document.body)
  ```

  ```shell
  #下载url模块
  npm i url -S
  ```

  ```jsx
  //App.jsx
  import React,{Component} from 'react'
  import Url from 'url'
  
  class App extends Component{
      render(){
         	//查询字符串的id在props对象的loaction中的search字符串中
          const search = Url.parse(this.props.location.search,true)
          //不写第二个参数或第二个参数为false之后进行一级的解析JSON,不会递归解析为JSON对象
          console.log(search)// 此时search被转化为一个对象
          const id = search.query.id
          return(
          	<div>我的id为{id}</div>
          )
      }
  }
  
  export default App
  ```

- **其余参数**

  路由组件中的其他父组件的传参不能传到对应的子组件中，因为传值实际是传给Route组件，而不是传给想要渲染的那个组件，之所以在子组件中能接收到match等属性是因为Route组件做过传值处理，**想要传入其他参数必须使用Route的reander方法，在render方法内部写上要传的属性**

  **注意：**如果使用render方法渲染就会发现原来的组件没有了路由的一些属性，因为没有通过components传路由无法进行处理，所以需要接受`props`并用`...props`进行展开赋值给需要渲染的组件

  ```jsx
  import React from 'react'
  import { BrowerserRouter as  Router,Route,NavLink } from 'react-router-dom'
  import App from './App'
  import About from './About'
  import Inbox from './Inbox'
  
  React.render((
    <Router>
        <div>
        <ul>
            <li><NavLink activeClassName={'hover'} exact to="/1">About</NavLink></li>
            <li><NavLink activeClassName={'hover'} to="/about">About</NavLink></li>
            <li><NavLink activeClassName={'hover'} to="/inbox">Inbox</NavLink></li>
          </ul>
        <Route path="/:id" exact rednder={(props)=><APP {...props} msg="传给App的值"/>}>
        <Route path="/about" component={About} />
        <Route path="/inbox" component={Inbox} />
      </Route>
      </div>
    </Router>
  ), document.body)
  ```

  



### 11.8 嵌套路由

嵌套路由是写在父级路由内部的路由，要达到该路由一定会触发父级的路由，所以父级路由一定不能使用`exact`属性，不然无法触发该路由

**注意：**该嵌套不是路径上的嵌套，而是实际意义上组件代码的嵌套，就和Vue的`router-view`一样

```jsx
//app.jsx
import React from 'react'
import './App.css'
import { BrowserRouter as Router, Route } from 'react-router-dom'
import App2 from './components/app'
import Home from './components/home'

function App() {
  return (
    <div className="App">
      <Router>
        <Route exact path="/" component={Home} />
        <Route path="/app" component={App2} />
      </Router>
    </div>
  )
}

export default App
```

```jsx
// app2.jsx
import React, { Component } from 'react'
import { BrowserRouter as Router, Route } from 'react-router-dom'
import News from './news'

class App extends Component {
  state = {
    time: 0
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({
        time: 1
      })
    }, 5000)
  }
  render() {
    return (
      <div>
        APP组件
        <Router>
          <div>
            <Route path="/app/news" component={News} />
          </div>
        </Router>
      </div>
    )
  }
}

export default App
```

```jsx
// news.jsx
import React, { Component } from 'react'

class News extends Component {
  render() {
    return <div>新闻组件</div>
  }
}

export default News
```



### 11.9 路由跳转后的卸载状态

在组件卸载后，我们如果在前一个页面使用了定时器等，定时器在组件卸载后一样会继续执行，如果里面有更新状态的代码就会报错，这样就会造成很多问题，所以推荐在这些组件中组件卸载的时候不要更新更新状态，这时我们只需要**将对应更新的函数赋值为另外的空函数**就可以了

```jsx
class ScrollingList extends React.Component {
   state={
            count:0
      }
    constructor(props) {
        super(props);
      }
    componentDidMount(){、
    	//如果两秒内跳转页面会报错
        setTimeout({
          this.setState({
            count:1
          })
        },2000)
    }
    componentWillUnmount(){
        //重新改变this.setState,让组件被销毁的时候不报错
        this.setState=()=>{
            return
        }
    }
    render() {
        return (
            <div>{this.state.count}</div>
        );
    }
}
```



### 11.10 路由属性

#### 11.10.1 history

**react的history经常使用的有三种：**

- `browser history` - 针对 DOM 环境，用于支持 HTML5 history API 的浏览器
- `hash history` - 针对 DOM 环境，用于传统的旧式（低版本） 浏览器
- `memory history` - `history` 在内存上的实现，用于测试以及 React Native 等非 DOM 环境

**history 对象通常具有以下属性和方法：**

- `length` - number 历史堆栈中的条目数
- `action` - string 当前的导航操作（`push`、`replace` 或 `pop`）
- `location` - object 当前访问的位置信息，可能具有以下属性：
  - `pathname` - string URL 路径
  - `search` - string URL 中的查询字符串
  - `hash` - string URL 中的 hash 片段
  - `state` - object 存储至 `location` 中的额外状态数据，仅在 `browser history` 和 `memory history` 中有效。
- `push(path, [state])` - function 将一个新条目推入到历史堆栈中
- `replace(path, [state])` - function 替换历史堆栈中的当前条目
- `go(n)` - function 将历史堆栈中的指针移动 n 个条目
- `goBack()` - function 返回到上一个页面，相当于 go(-1)
- `goForward()` - function 进入到下一个页面，相当于 go(1)
- `block(prompt)` - function 阻止导航（请参阅 [history](https://github.com/ReactTraining/history#blocking-transitions) 文档）

**注意：**`history` 对象是可变的。因此建议从 `<Route>` 渲染组件时接收的属性中直接访问 `location`，而不是通过 `history.location` 进行访问。这样可以保证 React 在生命周期中的钩子函数正常执行

```jsx
class Comp extends React.Component {
  componentWillReceiveProps(nextProps) {
    // locationChanged 会是 true
    const locationChanged = nextProps.location !== this.props.location;

    // 错误，locationChanged 永远是 false，因为 history 是可变的。
    const locationChanged = nextProps.history.location !== this.props.history.location;
  }
}

<Route component={Comp} />
```



#### 11.10.2 location

**`location` 代表应用程序的位置。如当前的位置，将要去的位置，或是之前所在的位置**

```js
{
  key: 'ac3df4', // 使用 hash history 时，没有这个属性
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
```

**Router 将在以下几个地方提供一个 `location` 对象：**

- 在 `Route component` 中，以 `this.props.location` 方式获取
- 在 `Route render` 中，以 `({ location }) => ()` 方式获取
- 在 `Route children` 中，以 `({ location }) => ()` 方式获取
- 在 `withRouter` 中，以 `this.props.location` 方式获取

**`location` 对象永远不会发生改变，**因此可以在生命周期钩子函数中使用 `location` 对象来查看当前访问地址是否发生改变。这种技巧在获取远程数据以及使用动画时非常有用。

```js
componentWillReceiveProps(nextProps) {
  if (nextProps.location !== this.props.location) {
    // 已经跳转了！
  }
}
```

**可以在以下不同情境中使用 `location`：**

- Web [`<Link to={location}>`](https://reacttraining.com/react-router/web/api/Link)
- React Native [`<Link to={location}>`](https://reacttraining.com/react-router/native/api/Link)
- [`<Redirect to={location}>`](https://reacttraining.com/react-router/web/api/Redirect)
- [history.push(location)](https://reacttraining.com/react-router/web/api/history)
- [history.replace(location)](https://reacttraining.com/react-router/web/api/history)

通常情况下只是使用一个字符串，但是如果你需要添加一些额外的 `state`，以在应用程序跳转到特定位置时可以使用，那么你就可以使用 `location` 对象。如果你想根据导航历史而不是路径来组织 UI（如模态对话框），这也很有用

```jsx
// 通常情况下我们这么做
<Link to="/somewhere" />

// 但是我们可以改为使用 location 对象
const location = {
  pathname: '/somewhere',jsx
  state: { fromDashboard: true }
};

<Link to={location} />
<Redirect to={location} />
history.push(location);
history.replace(location);
```

**最终，`location` 将传递给以下组件：**

- Route
- Switch

**这将阻止它们在 Router 状态下使用实际位置。这对动画和等待导航非常有用，或者任何时候你想诱导一个组件在不同于真实位置的地方渲染**



#### 11.10.3 match

**一个 `match` 对象包含有关 `<Route path>` 如何匹配 URL 的信息**

- `params` - object 根据 `path` 中指定的动态片段，从 URL 中解析出的键值对
- `isExact` - boolean 如果整个 URL 匹配（不包含尾随字符），则为 `true`
- `path` - string 用于匹配的路径模式。可用于构建嵌套的 `<Route>`
- `url` - string URL 的匹配部分。可用于构建嵌套的 `<Link>`

**可以在以下几个地方访问 `match` 对象：**

- 在 `Route component` 中，以 `this.props.match` 方式获取
- 在 `Route render` 中，以 `({ match }) => ()` 方式获取
- 在 `Route children` 中，以 `({ match }) => ()` 方式获取
- 在 `withRouter` 中，以 `this.props.match` 方式获取
- `matchPath` 的返回值

**注意：**

- 如果 `<Route>` 没有定义 `path`，并因此始终匹配，则会得到最接近的父匹配。`withRouter` 也是一样

- 在 `<Route path="/somewhere" children={({ match }) => ()} />` 中，即使 `path` 与当前位置不匹配，`children` 指定的内联函数也依然会被调用。这种情况下，`match` 为 `null`。能够在不匹配时依然呈现 `<Route>` 的内容可能很有用，但是这样会带来一些挑战。解析 URL 的默认方式是将 `match.url` 字符串连接到 relative-path。

  ```jsx
  `${match.url}/relative-path`
  ```

  如果你在 `match` 为 `null` 时尝试执行此操作，最终会出现 `TypeError` 错误。这意味着在使用 `children` 属性时尝试在 `<Route>` 内部连接 relative-path 是不安全的

- 当您在产生空匹配对象的 `<Route>` 内部使用没有定义 `path` 的 `<Route>` 时，会出现类似但更微妙的情况

  ```jsx
  // location.pathname = '/matches'
  <Route path='/does-not-match' children={({ match }) => (
    // match === null
    <Route render={({ match: pathlessMatch }) => (
      // pathlessMatch === ???
    )} />
  )} />
  ```

  没有 `path` 的 `<Route>` 从它的父节点继承 `match` 对象。如果它的父匹配为 `null`，那么它的匹配也将为 `null`。这意味着：

  - 任何子路由/子链接必须是绝对的
  - 一个没有定义 `path` 的 `<Route>`，它的父匹配可以为 `null`，但它本身需要使用 `children` 来呈现内容



#### 11.10.4 matchPath

在正常的渲染周期之外，可以使用和 `<Route>` 所使用的相同的匹配代码，例如在服务器上呈现之前收集数据依赖关系

```jsx
import { matchPath } from 'react-router';

const match = matchPath('/users/123', {
  path: '/users/:id',
  exact: true,
  strict: false
});
```

**参数：**

- **pathname：**第一个参数是要匹配的路径名。如果您在服务器上通过 Node.js 使用，它将是 `req.path`。

- **props：**第二个参数是匹配的属性，它们与 `<Route>` 接受的匹配属性相同：

```jsx
{
  path, // 例如 /users/:id
  strict, // 可选，默认为 false
  exact // 可选，默认为false
}
```



#### 11.10.5 withRouter

可以通过 `withRouter` 高阶组件访问 `history` 对象的属性和最近（UI 结构上靠的最近）的 `<Route>` 的 `match` 对象。当组件渲染时，`withRouter` 会将更新后的 `match`、`location` 和 `history` 传递给它

```jsx
import React from 'react';
import PropTypes from 'prop-types';
import { withRouter } from 'react-router-dom';

// 显示当前位置的 pathname 的简单组件
class ShowTheLocation extends React.Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
    location: PropTypes.object.isRequired,
    history: PropTypes.object.isRequired
  }

  render() {
    const { match, location, history } = this.props;

    return (
      <div>You are now at {location.pathname}</div>
    );
  }
}

// 创建一个连接到 Router 的新组件（借用 redux 术语）
const ShowTheLocationWithRouter = withRouter(ShowTheLocation)
```

**注意：withRouter 不会订阅位置更改**，如 React Redux 的 connect 对状态更改所做的更改。而是在位置更改从 `<Router> `组件传播出去之后重新呈现。**这意味着除非其父组件重新呈现，否则使用 withRouter 不会在路由转换时重新呈现。**如果使用 withRouter 来防止更新被 shouldComponentUpdate 阻塞，那么使用router 包装实现 shouldComponentUpdate 的组件是非常重要的。例如，使用 Redux 时：

```jsx
// This gets around shouldComponentUpdate
withRouter(connect(...)(MyComponent))
// or
compose(
  withRouter,
  connect(...)
)(MyComponent)

// This does not
connect(...)(withRouter(MyComponent))
// nor
compose(
  connect(...),
  withRouter
)(MyComponent)
```

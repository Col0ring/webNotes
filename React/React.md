

React

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
//无状态组件的props参数就是父组件传入进来的值
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

**state 和 props 主要的区别在于 props是不可变的，而 state 可以根据与用户交互来改变**。这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据

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

```jsx
//创建一个 Mytitle 组件，属性 title 是必须的且是字符串，非字符串类型会自动转换为字符串
var title = "菜鸟教程";
// var title = 123;
class MyTitle extends React.Component {
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
        name: "菜鸟教程",
        site: "https://www.runoob.com"
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

**解决方法**

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

Keys 可以在 DOM 中的某些元素被增加或删除的时候帮助 React 识别哪些元素发生了变化。因此应当给数组中的每一个元素赋予一个确定的标识

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

**注意：**如果列表可以重新排序，不建议使用索引来进行排序，因为这会导致渲染变得很慢

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
- **callback**，可选参数，回调函数。该函数会在**setState**设置成功，且组件重新渲染后调用

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

- **State 的更新可能是异步的，**setState()并不会立即改变this.state，而是创建一个即将处理的state。setState()并不一定是同步的，为了提升性能React会批量执行state和DOM渲染，因为 this.props 和 this.state 可能是异步更新的，所以不应该用它们当前的值去计算下一个 state 的值

  ```jsx
  // Wrong
  this.setState({
    counter: this.state.counter + this.props.increment,
  });
  ```

  要修复它，请使用第二种形式的 setState() 来接受一个函数而不是一个对象。 该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数

  ```jsx
  // Correct
  //第一个参数为原来的状态值
  this.setState((prevState, props) => ({
    counter: prevState.counter + props.increment
  }));
  ```

  

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

**组件的生命周期可分成三个状态：（以下是16.3前常用的，但是几个will会在17.0弃用）**

- **Mounting：**已插入真实 DOM
  - **constructor：**构造函数，在创建组件的时候调用一次
  - **componentWillMount**：在渲染前调用,在客户端也在服务端
  - **render：**渲染UI
  - **componentDidMount** : 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)
- **Updating：**正在被重新渲染
  - **componentWillReceiveProps** 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用
  - **shouldComponentUpdate** 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用
    可以在你确认不需要更新组件时使用
  - **componentWillUpdate**在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用
  - **componentDidUpdate** 在组件完成更新后立即调用。在初始化时不会被调用
- **Unmounting：**已移出真实 DOM
  - **componentWillUnmount：**在组件从 DOM 中移除之前立刻被调用，大多数在这个阶段都是用来保存数据的

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
  componentDidUpdate(prevProps, prevState) {
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

### 9.1 在子组件使用表单

需要在父组件通过创建事件句柄 (**handleChange**) ，并作为 prop (**updateStateProp**) 传递到子组件上

```jsx
class Content extends React.Component {
  render() {
    return  <div>
            <input type="text" value={this.props.myDataProp} onChange={this.props.updateStateProp} /> 
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

当处理多个 input 元素时，可以通过给每个元素添加一个 name 属性，来让处理函数根据 **event.target.name** 的值来选择做什么

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

### 11.1 react-router

- 路由概念

  - 路由作用

  - react-router

    - 提供了一些router的核心api，静态的
      - Router, Route, Switch等，但是它没有提供dom操作进行跳转的api

  - React-router-dom

    - 提供了BrowserRouter, Route, Link等api，动态的
      - 我们可以通过DOM的事件控制路由

  - BrowerserRouter路由和HashRouter路由

    - 是路由的基本
    - 就像路由器一样去管理网络及给每个接入进来的用户分发ip
    - 是一个大容器 包裹着路由
    - HashRouter它是通过hash值来对路由进行控制。如果你使用HashRouter，你的路由就会默认有这个#。
    - BrowerserRouter它的原理是使用HTML5 history API (pushState, replaceState, popState)来使你的内容随着url动态改变的，如果放在二级目录下给BrowerserRouter增加个属性

  - Switch

    - 会用来包裹Route，它里面不能放其他html元素，用来只显示一个路由

  - Route

    - 控制路径对应显示的组件
    - 标签属性有exact、path以及component
      - exact 是严格匹配，控制匹配到/路径时不会再继续向下匹配
      - path 是标识路由的路径
        - /path/:id路由参数
      - component 则表示路径对应显示的组件

  - Link和NavLink

    - 两者都是可以控制路由跳转的

    - NavLink的api更多

    - Link标签有to属性

      - to可以接受string或者一个object，来控制url

    - NavLink它可以为当前选中的路由设置类名、样式以及回调函数等

      ```js
      <NavLink  exact activeClassName='select'> to='/'</NavLink>
      ```

  - Redirect

    - 重定向
      - 属性 from='*' to='/'
    - 404

  - params与query

    - this.props.match 来获取路由(/news/list123)参数
    - 可以通过node-url 来获取路由(/news/list?id=123&a=456&b=789)参数
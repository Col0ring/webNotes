# React高级特性

## 1.非受控组件

非受控组件与受控组件的区别在于它的值不能通过单向数据流的方式设置和获取，只能够通过获取 DOM 元素，从而获取到需要的值。

**一般我们通过以下方法对非受控组件进行操作：**

- ref
- defaultValue 和 defaultChecked
- 手动操作 DOM

```jsx
function Demo() {
    const input = React.createRef()
    const handleClick = () => {
        const el = input.current // 通过ref获取 DOM
        console.log(el.value)
    }
    return( 
        <>
        <input  ref = {input} defaultValue = '0'/>
        <button onClick = (handleClick)>click</button>
        </>
    )
}
```

**非受控组件的使用场景：**

- 必须手动操作 DOM 元素，setState 无法实现。
- 文件上传，使用`<input type = "file"/>`时。
- 某些富文本编辑器，需要传入 DOM元素。



**受控组件和非受控组件的使用注意：**

- 优先使用受控组件，这也符合 React 的设计原则。
- 必须操作 DOM 时，再使用非受控组件。



## 2.Portals

React 的组件默认是会按照既定的层级进行嵌套渲染的，而如果有些时候我们不想要安装这样的层级进行渲染，而是想要渲染到父组件以外，这时候我们就需要使用`Portals。`

**`Portals`的使用场景：**

- 父组件使用的`overflow:hiddren`，子组件的展示会受到影响。
- 父组件的`z-index`层级太低，子组件无法展示。
- 子组件使用的是`fixed`定位，需要放在 body 的第一层级上。

```jsx
function Demo({children}) {
    // Protal可以让一个元素作为指定原生的子元素存在
    return ReactDOM.createPortal(<div className = 'modal'>{children}</div>, document.body)
    /*
    	比如我们要将一个fixed定位的元素放在body上，这样有这更好的浏览器兼容性
    */
}
```



## 3.Context

关于现阶段的 Context 相关内容可以查看`React Hooks`笔记。



## 4.异步加载组件

React 对异步加载有着相应的 API 进行支持：

- import()
- React.lazy
- React.Suspense

```jsx
const ContextDemo = React.lazy(() => import('./ContextDemo'))
function Demo() {
	return (
        <React.Suspense fallback = {<div>loading...</div>}>
        	<ContextDemo/>
        </React.Suspense>
    )
}
```

由于`import()`动态引入组件返回一个 Promise，使用`React.lazy`对返回的 Promise 进行解析，然后获取到组件，同时返回的是成功的 Promise，而`React.Suspense`的特性是内部组件是正在执行的 Promise 的时候会返回`fallback`内的内容，而 Promise 执行完成后就会展示内部的子组件，所以能很轻松的做到异步加载组件。



## 5.性能优化

React性能优化的主要有：

- shouldComponentUpdate (SCU)

  ```jsx
  // 比如作为子组件时，父组件只有传入到相应组件的值更新了，改子组件才会更新
  shouldComponentUpdate(nextProps, nextState) {
      if(nextState.count !== this.state.count){
          return true // 可以渲染
      }
      return false // 不可以渲染
  }
  ```

  **SCU 优化必须要配合“不可变值”一起使用**，也可以不用 SCU，当遇到性能优化问题的时候再考虑。

- PureComponent 和 React.memo

- 不可变值 immutable.js



## 6.组件公共逻辑抽离

- mixin（已被废弃）。
- 高阶组件（HOC)。
- Render Props（传入给子组件一个返回JSX的函数，该函数可以接收子组件的 state 中的值从而在子组件中渲染出来），与 HOC 相比代码更加简洁，但学习成本更高。
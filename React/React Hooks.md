#  React Hooks

## 引言

**设计Hooks主要是解决ClassComponent的几个问题：**

- 很难复用逻辑（只能用 HOC，或者 render props），会导致组件树层级很深
- 会产生巨大的组件（指很多代码必须写在类里面）
- 类组件很难理解，比如方法需要 bind，this 指向不明确

**同时，也为了让 FunctionalComponent 也拥有 ClassComponent 的一些特性。**

**使用注意：**

- 不能将 hooks 放在循环、条件语句或者嵌套方法内。react 是根据 hooks 出现顺序来记录对应状态的。
- 只在 function 组件和自定义 hooks 中使用 hooks。
- 命名规范：
  - useState 返回数组的第二项以 set 开头（仅作为约定）。
  - 自定义 hooks 以 use 开头（可被 lint 校验）。

**React 中提供的 hooks：**

- useState： setState
- useReducer： setState，同时 useState 也是该方法的封装
- useRef: ref
- useImperativeHandle: 给 ref 分配特定的属性
- useContext: context，需配合 createContext 使用
- useCallback: 可以对 setState 的优化
- useMemo: useCallback 的变形，对函数进行优化
- useEffect: 类似 componentDidMount/Update, componentWillUnmount，当效果为 componentDidMount/Update 时，总是在整个更新周期的最后（页面渲染完成后）才执行
- useLayoutEffect: 用法与 useEffect 相同，区别在于该方法的回调会在数据更新完成后，页面渲染之前进行，该方法会阻碍页面的渲染
- useDebugValue：用于在 React 开发者工具中显示自定义 hook 的标签



## 1.State Hooks

### 1.1 useState

```jsx
const [state, setState] = useState(initialState)
```

- useState 有一个参数，该参数可传如**任意类型的值**或者**返回任意类型值的函数**。
- useState 返回值为一个数组，数组的**第一个参数为我们需要使用的 state，第二个参数为一个`setter`函数，可传任意类型的变量，或者一个接收 state 旧值的函数，其返回值作为 state 新值。**

```jsx
function Counter({ initialCount }) {
  const [count, setCount] = useState(initialCount)
  // Lazy initialization
  const [state, setState] = useState(() => {
    const initialState = someExpensiveComputation(props)
    return initialState
  })
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(0)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
    </>
  )
}
```

**注意：**set 方法不会像类组件的 setState 一样做 merge，所以建议：

- 如果数据结构简单，可以将变量根据数据结构需要放在不同的 useState 中，避免放入一个对象中大量使用类似`{...state, value}`形势。
- 如果数据结构复杂，建议使用 useReducer 管理组件的 state。



### 1.2 useReducer

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init)
```

- useReducer 接收三个参数，**第一个参数为一个 reducer 函数**，**第二个参数是reducer的初始值**，**第三个参数为可选参数，值为一个函数，可以用来惰性提供初始状态**。这意味着我们可以使用使用一个 `init` 函数来计算初始状态/值，而不是显式的提供值。如果初始值可能会不一样，这会很方便，最后会用计算的值来代替初始值。
  - reducer 接受两个参数一个是 state 另一个是 action ，用法原理和 redux 中的 reducer 一致。
- useReducer 返回一个数组，数组中包含一个 state 和 dispath**，state 是返回状态中的值，而 dispatch 是一个可以发布事件来更新 state 的函数**。

**注意：**React 不使用 `state = initialState` 这一由 Redux 推广开来的参数约定。有时候初始值依赖于 props，因此需要在调用 Hook 时指定。如果你特别喜欢上述的参数约定，可以通过调用 `useReducer(reducer, undefined, reducer)` 来模拟 Redux 的行为，但不鼓励你这么做。

```jsx
function init(initialCount) {	
    return {count: initialCount};	
}	

function reducer(state, action) {	
    switch (action.type) {	
        case 'increment':	
            return {count: state.count + 1};	
        case 'decrement':	
            return {count: state.count - 1};	
        case 'reset':	
            return init(action.payload);	
        default:	
            throw new Error();	
    }	
}	

function Counter({initialCount}) {	
    const [state, dispatch] = useReducer(reducer, initialCount, init);	
    return (	
        <>	
        Count: {state.count}	
<button	
    onClick={() => dispatch({type: 'reset', payload: initialCount})}>	
    Reset	
</button>	
<button onClick={() => dispatch({type: 'increment'})}>+</button>	
<button onClick={() => dispatch({type: 'decrement'})}>-</button>	
</>	
);	
}	

function render () {	
    ReactDOM.render(<Counter initialCount={0} />, document.getElementById('root'));	
}
```

**同时，useReucer 也是 useState 的内部实现，useState 和 useReucer 的实现原理：**

```jsx
let memoizedState
function useReducer(reducer, initialArg, init) {
    let initState = void 0
    if (typeof init === 'function') {
        initState = init(initialArg)
    } else {
        initState = initialArg
    }
    function dispatch(action) {
        memoizedState = reducer(memoizedState, action)
        // React的渲染
        // render()
    }
    memoizedState = memoizedState || initState
    return [memoizedState, dispatch]
}

function useState(initState) {
    return useReducer((oldState, newState) => {
        if (typeof newState === 'function') {
            return newState(oldState)
        }
        return newState
    }, initState)
}
```

在某些场景下，useReducer 比 useState 更加适用。Kent C. Dodds 提供了一个 `useReducer` 的最佳实践：**当你一个元素中的状态，依赖另一个元素中的状态，最好使用 useReducer。**



## 2.Effect Hooks 

### 2.1 useEffect

```jsx
useEffect(effect, array);
```

useEffect 接收两个参数，没有返回值。

- 第一个参数为 effect 函数，该函数将在 componentDidMmount 时触发和 componentDidUpdate 时有条件触发（该添加为 useEffect 的第二个数组参数）。同时该 effect 函数可以返回一个函数（returnFunction），returnFunction 将会**在 componentWillUnmount 时触发**和**在 componentDidUpdate 时先于 effect 有条件触发（先执行 returnFuncton 再执行 effect，比如需要做定时器的清除）**。
  **注意：**与 componentDidMount 和 componentDidUpdate 不同之处是，effect 函数触发时间为在浏览器完成渲染之后。 如果需要在渲染之前触发，需要使用 useLayoutEffect。
- 第二个参数 array 作为有条件触发情况时的条件限制：
  - 如果不传，则每次 componentDidUpdate 时都会先触发 returnFunction（如果存在），再触发 effect。
  - 如果为空数组`[]`，componentDidUpdate 时不会触发 returnFunction 和 effect。
  - 如果只需要在指定变量变更时触发 returnFunction 和 effect，将该变量放入数组。



### 2.2 useLayoutEffect

```jsx
useLayoutEffect(effect, array);
```

与 useEffect 使用方法一样，只是执行回调函数的时机有着略微区别，运行时机更像是 componentDidMount 和 componentDidUpdate。但是要注意的是，该方法是同步方法，在浏览器 paint 之前执行，会阻碍浏览器 paint，只有当我们需要进行DOM的操作时才使用该函数（比如设定 DOM 布局尺寸，这样可以防抖动）。



**useLayoutEffect 与 useEffect**

正常情况用默认的 useEffect 钩子就够了，这可以保证状态变更不阻塞渲染过程，但如果 effect 更新（清理）中涉及 DOM 更新操作，用 useEffect 就会有意想不到的效果，这时我们最好使用 useLayoutEffect 。

比如逐帧动画 requestAnimationFrame ，要做一个 useRaf hook 就得用上后者，需要保证同步变更。这也符合作者说到的 **useEffect的时期是非常晚，可以保证页面是稳定下来再做事情**。

**钩子的执行顺序：**`useLayoutEffect > requestAnimationFrame > useEffect`



## 3.Context Hooks

要理解 Context Hooks 中的 api，首先需要了解 context 和其使用场景。

**设计目的：**context 设计目的是为共享那些被认为对于一个组件树而言是“全局”的数据。

**使用场景：**context 通过组件树提供了一个**传递数据**的方法，从而**避免了在每一个层级手动的传递 props 属性**。

**注意点：**不要仅仅为了避免在几个层级下的组件传递 props 而使用 context，它是被用于在多个层级的多个组件需要访问相同数据的情景。

### 3.1 createContext

```jsx
const {Provider, Consumer} = React.createContext(defaultValue, calculateChangedBits)
```

- 该方法创建一对`{ Provider, Consumer }`。当 React 渲染 context 组件 Consumer 时，它将从组件树的上层中最接近的匹配的 Provider 读取当前的 context 值。Consumer 是 Provider 提供数据的使用者。
- 如果上层的组件树没有一个匹配的 Provider，而此时你需要渲染一个 Consumer 组件，那么你可以用到 defaultValue 。这有助于在不封装它们的情况下对组件进行测试。例如：

    ```jsx
    import React, { useContext} from 'react';
    import ReactDOM from 'react-dom';
    /*  结果读取为123,因为没有找到Provider */
    const { Provider, Consumer } = React.createContext(123);
    function Bar() {
      return <Consumer>{color => <div>{color}</div>}</Consumer>;
    }
    function Foo() {
      return <Bar />;
    }
    function App() {
      return (
          <Foo />
      );
    }
    ReactDOM.render(
        <App />,
        document.getElementById('root')
    )
    ```

#### 3.1.1 Provider

React 组件允许 `Consumers 订阅 context 的改变`。而 Provider 就是发布这种状态的组件，该组件`接收一个 value 属性`传递给 Provider 的后代 Consumers。`一个 Provider 可以联系到多个 Consumers`。Providers 可以被嵌套以覆盖组件树内更深层次的值。

```jsx
export const ProviderComponent = props => {
  return (
    <Provider value={}>
      {props.children}
    </Provider>
  )
}
```

在`createContext()`函数中的第二个参数为`calculateChangedBits`，它是一个接受 newValue 与 oldValue 的函数，返回值作为 changedBits，在 Provider 中，当 changedBits = 0，将不再触发更新。而在 Consumer 中有一个不稳定的 props，unstable_observedBits，若 Provider 的`changedBits & observedBits = 0`，也将不触发更新。

```jsx
const Context = React.createContext({foo: 0, bar: 0}, (a, b) => {
    let result = 0
    if (a.foo !== b.foo) {
        result |= 0b01
    }
    if (a.bar !== b.bar) {
        result |= 0b10
    }
    return result
})
```



#### 3.1.2 Consumer

```jsx
<Consumer>
  {value => /* render something based on the context value */}
</Consumer>
```

- 一个可以订阅 context 变化的 React 组件。当 context 值发生改变时，Consumer 值也会改变
- 接收一个 函数作为子节点，该函数接收当前 context 的值并返回一个 React 节点。**传递给函数的 value 将等于组件树中上层 context 的最近的 Provider 的 value 属性**。如果 context 没有 Provider ，那么 value 参数将等于被传递给 `createContext()` 的 defaultValue 。

每当 Provider 的值发生改变时, 作为 Provider 后代的所有 Consumers 都会重新渲染。 从 Provider 到其后代的Consumers 传播不受 shouldComponentUpdate 方法的约束，因此即使祖先组件退出更新时，后代Consumer也会被更新。

```jsx
// 创建一个 theme Context,  默认 theme 的值为 light
const ThemeContext = React.createContext('light');

function ThemedButton(props) {
    // ThemedButton 组件从 context 接收 theme
    return (
        <ThemeContext.Consumer>
            {theme => <Button {...props} theme={theme} />}
        </ThemeContext.Consumer>
    )
}

// 中间组件
function Toolbar(props) {
    return (
        <div>
            <ThemedButton />
        </div>
    )
}

class App extends React.Component {
    render() {
        return (
            <ThemeContext.Provider value="dark">
                <Toolbar />
            </ThemeContext.Provider>
        )
    }
}
```



### 3.2 useContext

```jsx
const context = useContext(Context)
```

使用效果和 Consumer 类似，但是是函数式的使用方式，仍然需要与 Provider 配合使用。

**该函数接收一个 Context 类型的参数（就是包裹了 Provider 和 Consumer 的那个对象），返回 Provider 中的 value 属性对象的值。**

```jsx
const Context = React.createContext('light');

// Provider
class Provider extends Component {
  render() {
    return (
      <Context.Provider value={'dark'}>
        <DeepTree />
      </Context.Provider>
    )
  }
}
```

```jsx
// Consumer
function Consumer(props) {
  const context = useContext(Context)
  return (
    <div>
      {context} // dark
    </div>
  )
}
```



### 3.3 配合 useReducer 使用

```jsx
// Color.jsx
import React, { createContext, useReducer } from 'react'

export const ColorContext = createContext()
export const UPDATE_COLOR = 'UPDATE_COLOR'

function reducer(state, action) {
  switch (action.type) {
    case UPDATE_COLOR:
      return action.color
    default:
      return state
  }
}

export const Color = props => {
  const [color, dispatch] = useReducer(reducer, 'blue')
  return (
    <ColorContext.Provider value={{ color, dispatch }}>
      {props.children}
    </ColorContext.Provider>
  )
}
```

```jsx
// Button.jsx
import React, { useContext } from 'react'
import { ColorContext, UPDATE_COLOR } from './Color'
function Buttons() {
  const { dispatch } = useContext(ColorContext)
  return (
    <div>
      <button
        onClick={() => {
          dispatch({ type: UPDATE_COLOR, color: 'red' })
        }}
      >
        red
      </button>
      <button
        onClick={() => {
          dispatch({ type: UPDATE_COLOR, color: 'yellow' })
        }}
      >
        yellow
      </button>
    </div>
  )
}

export default Buttons
```

```jsx
// ShowArea.jsx
import React, { useContext } from 'react'
import { ColorContext } from './Color'
function ShowArea() {
  const { color } = useContext(ColorContext)
  return <div style={{ color }}>color:{color}</div>
}

export default ShowArea
```

```jsx
// index.jsx
import React from 'react'
import ShowArea from './ShowArea'
import Buttons from './Buttons'
import { Color } from './Color'
function Demo() {
  return (
    <div>
      <Color>
        <ShowArea />
        <Buttons />
      </Color>
    </div>
  )
}

export default Demo
```



## 4.Ref Hooks

### 4.1 useRef

```jsx
const RefElement = createRef(initialValue)
```

#### 4.1.1 组件引用

useRef 可以需要传递一个参数，该参数一般是用于 useRef 的另一种用法，如果是引用元素对象一般不传参数，返回一个可变的 ref 对象，该对象下面有一个 current 属性指向被引用对象的实例。

要说到 useRef，我们需要说到 createRef ，以及为什么要有这个 api 出现。（createRef 使用方法和 useRef 一致，返回的是一个 ref 对象）

两者当做 ref 正常使用时效果基本完全一样：

- createRef

  ```jsx
  import { React, createRef } from 'react'
  
  const FocusInput = () => {
    const inputElement = createRef()
    const handleFocusInput = () => {
      inputElement.current.focus()
    }
    return (
      <>
        <input type='text' ref={inputElement} />
        <button onClick={handleFocusInput}>Focus Input</button>
      </>
    )
  }
  
  export default FocusInput
  ```

- useRef

  ```jsx
  import { React, useRef } from 'react'
  
  const FocusInput = () => {
    const inputElement = useRef()
    const handleFocusInput = () => {
      inputElement.current.focus()
    }
    return (
      <>
        <input type='text' ref={inputElement} />
        <button onClick={handleFocusInput}>Focus Input</button>
      </>
    )
  }
  
  export default FocusInput
  ```

但是，这两者对应 ref 的引用其实是有着本质区别的：**createRef 每次渲染都会返回一个新的引用，而 useRef 每次都会返回相同的引用。**

像这样：

```jsx
const App = () => {
  const [renderIndex, setRenderIndex] = React.useState(1)
  const refFromUseRef = React.useRef()
  const refFromCreateRef = createRef()

  if (!refFromUseRef.current) {
    refFromUseRef.current = renderIndex
  }

  if (!refFromCreateRef.current) {
    refFromCreateRef.current = renderIndex
  }

  return (
    <>
      <p>Current render index: {renderIndex}</p>
      <p>
        <b>refFromUseRef</b> value: {refFromUseRef.current}
      </p>
      <p>
        <b>refFromCreateRef</b> value:{refFromCreateRef.current}
      </p>

      <button onClick={() => setRenderIndex(prev => prev + 1)}>
        Cause re-render
      </button>
    </>
  )
}
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2dpZi9aV1Z4clE3RzBXU2RpYmRpY1Zwanc1S1g3S0JMVkNzNGszMmV2SVN6NG1DOFRqWVhFNDZFeVdHeXFhc0UxTnc2NGdzM0p3QjZmeUZBTjRPVkx1aWFMVEFWUS82NDA?x-oss-process=image/format,png)

因为一直都存在 refFromUseRef.current，所以并不会改变值。

#### 4.1.2 替代 this

**那么，为什么要赋予 useRef 这种特性，在什么场景下我们需要这种特性呢？**

一个经典案例：

```jsx
import React, { useRef, useState } from 'react'

function App() {
  const [count, setCount] = useState()
  function handleAlertClick() {
    setTimeout(() => {
      alert(`Yout clicked on ${count}`)
    }, 3000)
  }
  return (
    <div>
      <p>You click {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <button onClick={handleAlertClick}>Show alert</button>
    </div>
  )
}

export default App
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2dpZi9aV1Z4clE3RzBXU2RpYmRpY1Zwanc1S1g3S0JMVkNzNGszQmxGWnl0OTRjYVl0dG95ckQ5cnE4VmtCYlAxM212ZFFpYXM0NXQwRlZNM3JGSEtBYWppYURWdWcvNjQw?x-oss-process=image/format,png)

当我们更新状态的时候, **React 会重新渲染组件, 每一次渲染都会拿到独立的 count 状态,  并重新渲染一个  handleAlertClick  函数.  每一个 handleAlertClick 里面都有它自己的 count。**

你会发现，count 的值并不能够实时的显示更新的数据，这个是由于 JS 中一值就存在的闭包机制导致的，当点击显示弹窗的按钮时，此时的 count 的值已经确定，并且传入到了`alert`方法的回调中，形成闭包，后续值的改变不会影响到定时器的触发。

而如果在类组件中，如果我们使用的是`this.state.count`，得到的结果又会是实时的，因为它们都是指向的同一个引用对象。

在函数组件中，我们可以使用 useRef 来实现实时得到新的值，这就是 useRef 的另外一种用法，它还相当于 this , 可以存放任何变量。useRef 可以很好的解决闭包带来的不方便性。

```jsx
import React, { useRef, useState } from 'react'

function App() {
  const [count, setCount] = useState(0)
  const lastestCount = useRef()
  lastestCount.current = count
  function handleAlertClick() {
    setTimeout(() => {
      alert(`You clicked on ${lastestCount.current}`) // 实时的结果
    }, 3000)
  }
  return (
    <div>
      <p>Yout click {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <button onClick={handleAlertClick}>Show alert</button>
    </div>
  )
}

export default App
```

要值得注意的是，如果我们在 useRef 中传入参数（一般 useRef 中传值就用在这里），使用下面这种方法来访问值，结果又会不同：

```jsx
import React, { useRef, useState } from 'react'

function App() {
  const [count, setCount] = useState(0)
  
  const lastestCount = useRef(count) // 直接传入count
  
  function handleAlertClick() {
    setTimeout(() => {
      alert(`You clicked on ${lastestCount.current}`)
    }, 3000)
  }
  return (
    <div>
      <p>Yout click {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
      <button onClick={handleAlertClick}>Show alert</button>
    </div>
  )
}

export default App
```

点击的时候我们会发现弹出来的值永远是0，正如我们所说，useRef 返回的都是相同的引用，参数在第一个传入进去的时候已经赋值给了 current 属性，返回了一个实例回来，后续因为已经有了实例了，所以会直接将原来的实例返回，传入的参数也就不再起作用了。



### 4.2 forwardRef

```jsx
forwardRef((props, ref) => {
    // dosomething
    return (
    	<div ref={ref}></div>
    )
})
```

forwardRef 准确来说不是 hooks 中的内容，但是如果我们要使用 useImperativeHandle，就需要使用它来进行搭配。

该方法的作用是：**引用父组件的 ref 实例，成为子组件的一个参数，可以引用父组件的 ref 绑定到子组件自身的节点上。**

该方法可以看做是一个高阶组件，本身 props 只带有 children 这个参数，它能将从父组件拿到的 ref 和 props 传入给子组件，由子组件来调用父组件传入的 ref。

**传入的组件会接收到两个参数，一个是父组件传递的 props，另一个就是 ref 的引用。**

```jsx
// 我们可以使用三层组件嵌套，把传入forwardRef的函数看成传值的中间层
function InputWithLabel(props) {
  // 这里的myRef为通过外部打入的父级ref节点
  const { label, myRef } = props
  const [value, setValue] = useState("")
  const handleChange = e => {
    const value = e.target.value
    setValue(value)
  }

  return (
    <div>
      <span>{label}:</span>
      <input type="text" ref={myRef} value={value} onChange={handleChange} />
    </div>
  )
}

// 这里用forwardRef来承接得到父级传入的ref节点，并将其以参数的形式传给子节点
const RefInput = React.forwardRef((props, ref) => (
  <InputWithLabel {...props} myRef={ref} />
))

// 调用该RefInput的过程
function App() {
  // 通过useRef hook 获得相应的ref节点
  const myRef = useRef(null)

  const handleFocus = () => {
    const node = myRef.current
    console.log(node)
    node.focus()
  }

  return (
    <div className="App">
      <RefInput label={"姓名"} ref={myRef} />
      <button onClick={handleFocus}>focus</button>
    </div>
  )
}
```



### 4.3 useImperativeHandle

```jsx
useImperativeHandle(ref, () => ({
    a:1,
    b:2,
    c:3
}))
```

**官方建议useImperativeHandle和forwardRef同时使用，减少暴露给父组件的属性，避免使用 ref 这样的命令式代码。**

useImperativeHandle 有三个参数：

- 第一个参数，接收一个通过 forwardRef 引用父组件的 ref 实例
- 第二个参数一个回调函数，返回一个对象，对象里面存储需要暴露给父组件的属性或方法
- 第三个参数为一个可选参数，该参数是一个依赖项数组，就像 useEffect 那样

```jsx
function Example(props, ref) {
    const inputRef = useRef()
    useImperativeHandle(ref, () => ({
        // 父组件可以通过this.xxx.current.focus的方式使用子组件传递出去的focus方法
        focus: () => {
            inputRef.current.focus()
        }
    }))
    return <input ref={inputRef} />
}

export default forwardRef(Example)
```

```jsx
class App extends Component {
  constructor(props){
      super(props)
      this.inputRef = createRef()
  }
  
  render() {
    return (
        <>
            <Example ref={this.inputRef}/>
            <button onClick={() => {this.inputRef.current.focus()}}>Click</button>
        </>
    )
  }
}
```



## 5.性能优化

### 5.1 memo

```jsx
MemoComponent = memo(Component)
```

我们都知道，对于类组件来说，有 PureComponent 可以通过判断父组件传入的 props 是否进行改变来优化渲染性能。所以，在函数式组件中，React 也有一个类似 PureComponent 功能的高阶组件 memo，效果同 PureComponent，都会判断父组件传入的 props 是否发生改变来重新渲染当前组件。

使用方法很简单：

```jsx
import React, { memo } from 'react'

function Demo(props){
    return (
    	<div>{props.name}</div>
    )
}

export default memo(Demo)
```



### 5.2 useMemo

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])
```

useMemo 是 React 推出用于优化函数式组件性能的 hooks，它可以传入两个参数：

- 第一个参数为一个工厂函数，返回一个缓存的值，也就是仅当重新渲染时数组中的值发生改变时，回调函数才会重新计算缓存数据，这可以使得我们避免在每次重新渲染时都进行复杂的数据计算。
- 第二个参数为一个依赖项数组，只有依赖项中的数据发生改变时才重新计算值，用法同 useEffect 的依赖项数组

```jsx
import React, { useState, useMemo } from 'react'

function Child({ color }) {
	// color值不发生改变不会打印console，但是依旧会触发重新渲染，如果连这个函数都不执行，在最外层加上memo
    const actionColor = useMemo(() => {
        console.log('color update')
        return color
    }, [color])

    return <div style={{ actionColor }}>{actionColor}</div>
}

function MemoCount() {
    const [count, setCount] = useState(0)
    const [color, setColor] = useState('blue')
    return (
        <div>
            <button
                onClick={() => {
                    setCount(count + 1)
                }}
                >
                Update Count
            </button>
            <button
                onClick={() => {
                    setColor('green')
                }}
                >
                Update Color
            </button>
            <div>{count}</div>
            <Child color={color} />
        </div>
    )
}

export default MemoCount
```

上面的例子其实并不是 useMemo 最常用的场景，就像之前说的，在 props 发生改变的时候才会触发被 memo 的组件的重新渲染，但是如果只是 props 的引用对象发生改变，实际的值并没有发生改变，组件还是会被重新渲染。就像下面这样：

```jsx
import React, { useState, memo } from 'react'

const Child = memo(({ config }) => {
	console.log(config)
    return <div style={{ color:config.color }}>{config.text}</div>
})

function MemoCount() {
    const [count, setCount] = useState(0)
    const [color, setColor] = useState('blue')
    const config = {
        color,
        text:color
    }
    return (
        <div>
            <button
                onClick={() => {
                    setCount(count + 1)
                }}
                >
                Update Count
            </button>
            <button
                onClick={() => {
                    setColor('green')
                }}
                >
                Update Color
            </button>
            <div>{count}</div>
            <Child config={config} />
        </div>
    )
}

export default MemoCount
```

当我们改变 count 值的时候，我们发现这其实和 config 对象是无关的，但是 Child 组件依旧会重新渲染，因为由于父组件的重新渲染，config 被重新赋值了新的对象，虽然新的对象里面的值都是相同的，但由于是引用类型对象，所以依旧会改变值，要改变这种状况，我们需要：

```jsx
// 使用useMemo
import React, { useState,useMemo, memo } from 'react'

const Child = memo(({ config }) => {
	console.log(config)
    return <div style={{ color:config.color }}>{config.text}</div>
})

function MemoCount() {
    const [count, setCount] = useState(0)
    const [color, setColor] = useState('blue')
    // 只会根据color的改变来返回不同的对象，否则都会返回同一个引用对象
    const config = useMemo(()=>({
        color,
        text:color
    }),[color])
    
    return (
        <div>
            <button
                onClick={() => {
                    setCount(count + 1)
                }}
                >
                Update Count
            </button>
            <button
                onClick={() => {
                    setColor('green')
                }}
                >
                Update Color
            </button>
            <div>{count}</div>
            <Child config={config} />
        </div>
    )
}

export default MemoCount
```

这样，当 count 的值发生改变时，子组件就不会再重新渲染了。



### 5.3 useCallback

```jsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b)
  },
  [a, b],
)
```

useCallback 的用法和 useMemo 类型，是专门用来缓存函数的 hooks，也是接收两个参数，同时，我们第一个参数传入额回调函数就是要缓存的函数。

**注意：**第二个参数目前只用于指定需要判断是否变化的参数，并不会作为形参传入回调函数。**建议回调函数中使用到的变量都应该在数组中列出。**

要在回调函数中传入参数，我们可以使用高阶函数的方法，useCallback 会帮我们缓存这个高阶函数，如上所示。

当然，同样可以在`callback`中写入形参：

```js
const memoizedCallback = useCallback(
  (a, b) => {
    doSomething(a, b)
  },
  [],
)
// memoizedCallback 其实就是传入的回调函数
```

可以看出，都是当依赖项方式改变时，才触发回调函数。因此，我们可以认为：`useCallback(fn, input)` 等同于 `useMemo(() => fn, input)`

```jsx
// useCallback的实现原理
let memoizedState = null
function useCallback(callback, inputs) {
  const nextInputs =
        inputs !== undefined && inputs !== null ? inputs : [callback]
  const prevState = memoizedState;
  if (prevState !== null) {
    const prevInputs = prevState[1]
    if (areHookInputsEqual(nextInputs, prevInputs)) {
      return prevState[0]
    }
  }
  memoizedState = [callback, nextInputs]
  return callback
}

// useMemo的实现原理
function useMemo(callback, inputs){
  return useCallback(callbak(),inputs)
}
```

更多情况，useCallback一般用于在 React 中给事件绑定函数并需要传入参数的时候：

```jsx
// 下面的情况可以保证组件重新渲染得到的方法都是同一个对象，避免在传给onClick的时候每次都传不同的函数引用
import React, { useState, useCallback } from 'react'

function MemoCount() {
    const [count, setCount] = useState(0)
    
    memoSetCount = useCallback(()=>{
        setCount(count + 1)
    },[count])
    
    return (
        <div>
            <button
                onClick={memoSetCount}
                >
                Update Count
            </button>
            <div>{count}</div>
        </div>
    )
}

export default MemoCount
```



## 6.Debug

### 6.1 useDebugValue

```jsx
useDebugValue(value)
// or
useDebugValue(date, date => date.toDateString());
```

useDebugValue 可用于在 React 开发者工具中显示自定义 hook 的标签。

useDebugValue 接收两个参数，根据传入参数数量的不同有不同的使用方式：

- 直接传 debug 值

    ```jsx
    function useFriendStatus(friendID) {
      const [isOnline, setIsOnline] = useState(null);

      // ...

      // 在开发者工具中的这个 Hook 旁边显示标签
      // e.g. "FriendStatus: Online"
      useDebugValue(isOnline ? 'Online' : 'Offline');

      return isOnline;
    }
    ```

- 延迟格式化 debug 值

  ```jsx
  const date = new Date()
  useDebugValue(date, date => date.toDateString())
  ```



## 7.自定义 Hooks

**自定义 Hook 是一个函数，其名称以`use`开头，函数内部可以调用其他的 Hook**

```jsx
// myhooks.js
// 下面自定义了一个获取窗口长宽值的hooks
import React, { useState, useEffect, useCallback } from 'react'

function useWinSize() {
  const [size, setSize] = useState({
    width: document.documentElement.clientWidth,
    height: document.documentElement.clientHeight
  })
  const onResize = useCallback(() => {
    setSize({
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight
    })
  }, [])

  useEffect(() => {
    window.addEventListener('resize', onResize)
    return () => {
      window.removeEventListener('reisze', onResize)
    }
  }, [onResize])
  return size
}

export const useWinSize
```

```jsx
import { useWinSize } from './myhooks'
function MyHooksComponent() {
  const size = useWinSize()
  return (
    <div>
      页面Size:{size.width}x{size.height}
    </div>
  )
}

export default MyHooksComponent
```



## 参考

- [你不知道的 useRef](https://zhuanlan.zhihu.com/p/105276393)
- [React Hooks 入门教程 - 阮一峰](http://www.ruanyifeng.com/blog/2019/09/react-hooks.html)
- [React Hooks 官方文档](https://reactjs.org/docs/hooks-intro.html)


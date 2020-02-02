# React Hooks

- hooks概念在React Conf 2018被提出来，并将在未来的版本中被引入，hooks遵循函数式编程的理念，主旨是在函数组件中引入类组件中的状态和生命周期，并且这些状态和生命周期函数也可以被抽离，实现复用的同时，减少函数组件的复杂性和易用性。
- 函数组件 (functional component) 内部能够”钩住“ React 内部的 state 和 life-cycles。
- 真正功能强大的地方是使我们能够更轻松地复用组件逻辑（custom hooks）
- 让FunctionalComponent具有ClassComponent的功能

**设计Hooks主要是解决ClassComponent的几个问题：**

- 很难复用逻辑（只能用HOC，或者render props），会导致组件树层级很深
- 会产生巨大的组件（指很多代码必须写在类里面）
- 类组件很难理解，比如方法需要bind，this指向不明确

## 1.主要的Hooks

- useState： setState
- useReducer： setState
- useRef: ref
- useImperativeMethods: ref
- useContext: context
- useCallback: 可以对setState的优化
- useMemo: useCallback的变形
- useLayoutEffect: 类似componentDidMount/Update, componentWillUnmount
- useEffect: 类似于setState(state, cb)中的cb，总是在整个更新周期的最后才执行



**useLayoutEffect与useEffect**

正常情况用默认的 useEffect 钩子就够了，这可以保证状态变更不阻塞渲染过程，但如果 effect 更新（清理）中涉及 DOM 更新操作，用 useEffect 就会有意想不到的效果。

比如逐帧动画 requestAnimationFrame ，要做一个 useRaf hook 就得用上后者，需要保证同步变更。这也符合作者说到的“useEffect的时期是非常晚，可以保证页面是稳定下来再做事情。”

钩子的执行顺序：`useLayoutEffect > requestAnimationFrame > useEffect`



**使用注意：**

- 不能将 hooks 放在循环、条件语句或者嵌套方法内。react是根据hooks出现顺序来记录对应状态的
- 只在 function 组件和自定义 hooks 中使用 hooks。
- 命名规范
  + useState 返回数组的第二项以 set 开头（仅作为约定）
  + 自定义 hooks 以 use 开头（可被 lint 校验）



## 2.useState

- **useState**
  useState 可传任意类型的变量或者返回任意类型变量的 function。
  useState 返回数组的第二个参数(setter)，可传任意类型的变量，或者一个接收 state 旧值的 function，其返回值作为 state 新值

  ```
  function Counter({ initialCount }) {
    const [count, setCount] = useState(initialCount);
    // Lazy initialization
    const [state, setState] = useState(() => {
      const initialState = someExpensiveComputation(props);
      return initialState;
    });
    return (
      <>
        Count: {count}
        <button onClick={() => setCount(0)}>Reset</button>
        <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
        <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      </>
    );
  }
  ```

  set 方法不会像 setState 一样做 merge，所以建议如果数据结构简单，可以将变量根据数据结构需要放在不同的 useState 中，避免大量使用类似{...state, value}形势。如果数据结构复杂，建议使用 useReducer 管理组件的 state

- **useEffect**

  ```
  useEffect(effect, array);
  ```

  effect 函数将在 componentDidAmount 时触发和 componentDidUpdate 时有条件触发。可以返回一个函数(returnFunction)，returnFunction 将会在 componentWillUnmount 时触发和在 componentDidUpdate 时先于 effect 有条件触发。
  与 componentDidAmount 和 componentDidUpdate 不同之处是，effect 函数触发时间为在浏览器完成渲染之后。 如果需要在渲染之前触发，需要使用 useLayoutEffect。
  第二个参数 array 作为有条件触发情况时的条件限制。

  - 如果不传，则每次 componentDidUpdate 时都会先触发 returnFunction（如果存在），再触发 effect。
  - 如果为空数组[]，componentDidUpdate 时不会触发 returnFunction 和 effect。
  - 如果只需要在指定变量变更时触发 returnFunction 和 effect，将该变量放入数组。

- **useContext**
  和consumer类似，仍然需要与Provider配合使用

  ```
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

  ```
  // Consumer
  function Consumer(props) {
    const context = useContext(Context);
    return (
      <div>
        {context} // dark
      </div>
    );
  }
  ```

- **useReducer**
  用于管理复杂结构的状态对象，与redux的核心逻辑一致。

  ```
  const [state, dispatch] = useReducer(reducer, initialState, {
    type: 'reset',
    payload: initialCount
  });
  ```

  ```
  // demo
  const TodosDispatch = React.createContext(null);
  
  function TodosApp() {
    // Tip: `dispatch` won't change between re-renders
    const [todos, dispatch] = useReducer(todosReducer, initialState);
  
    return (
      <TodosDispatch.Provider value={dispatch}>
        <DeepTree todos={todos} />
      </TodosDispatch.Provider>
    );
  }
  ```

- **useCallback**
  useCallback和下面的useMemo是非常实用的提升性能的小工具。

  ```
  const memoizedCallback = useCallback(
    () => {
      doSomething(a, b);
    },
    [a, b]
  );
  ```

  useCallback返回一个[memoized](https://en.wikipedia.org/wiki/Memoization)函数，在参数a和b都没有改变时，总是返回同一个函数。
  其具体实现逻辑基本如下：

  ```
  let memoizedState = null;
  function useCallback(callback, inputs) {
    const nextInputs =
      inputs !== undefined && inputs !== null ? inputs : [callback];
    const prevState = memoizedState;
    if (prevState !== null) {
      const prevInputs = prevState[1];
      if (areHookInputsEqual(nextInputs, prevInputs)) {
        return prevState[0];
      }
    }
    memoizedState = [callback, nextInputs];
    return callback;
  }
  ```

  注：第二个参数目前只用于指定需要判断是否变化的参数，并不会作为形参传入回调函数。建议回调函数中使用到的变量都应该在数组中列出。以后的版本可能会将第二项数组参数移除，自动判断回调函数中使用到的变量是否变化来判断返回结果。

- **useMemo**

  ```
  const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
  ```

  与useCallback类似，返回一个memoized函数执行结果。
  useMemo(() => fn, inputs) 等价于 useCallback(fn, inputs)
  useMemo可用于实现PureComponent子组件:

  ```
  function Parent({ a, b }) {
    // Only re-rendered if `a` changes:
    const child1 = useMemo(() => <Child1 a={a} />, [a]);
    // Only re-rendered if `b` changes:
    const child2 = useMemo(() => <Child2 b={b} />, [b]);
    return (
      <>
        {child1}
        {child2}
      </>
    )
  }
  ```

- **useRef**

  ```
  const refContainer = useRef(initialValue);
  ```

  类似 react.createRef()。
  在使用hooks的function component中，useRef不仅可以用来做DOM的引用，还可以做来作为类似class的实例属性，因为相同位置的useRef()每次返回的都是同一个对象。

  ```
  function Timer() {
    const intervalRef = useRef();
  
    useEffect(() => {
      const id = setInterval(() => {
        // ...
      });
      intervalRef.current = id;
      return () => {
        clearInterval(intervalRef.current);
      };
    });
  
    // ...
  }
  ```

  利用useRef()的这种特性，还可以做很多其他有趣的事情，例如获取previous props或previous state：

  ```
  function Counter() {
    const [count, setCount] = useState(0);
  
    const prevCountRef = useRef();
    useEffect(() => {
      prevCountRef.current = count;
    });
    const prevCount = prevCountRef.current;
  
    return <h1>Now: {count}, before: {prevCount}</h1>;
  }
  ```

- **useImperativeMethods**

  ```
  function FancyInput(props, ref) {
    const inputRef = useRef();
    useImperativeMethods(ref, () => ({
      focus: () => {
        inputRef.current.focus();
      }
    }));
    return <input ref={inputRef} ... />;
  }
  FancyInput =  React.forwardRef(FancyInput);
  ```

  useImperativeMethods提供了父组件直接调用子组件实例方法的能力。
  上例中，一个包含 `<FancyInput ref={fancyInputRef} />` 的父组件，就可以调用 fancyInputRef.current.focus().
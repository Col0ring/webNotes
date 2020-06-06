# dva

## 1.Models

models 是 dva 最核心的概念，通过 models，我们才能实现 dva 的数据流向的概念。

**models 共有 5 个属性：**

- namespace：model 的命名空间，同时也是他在全局 state 上的属性，只能用字符串，不支持通过 `.` 的方式创建多层命名空间。

- state：当前命名空间内的初始值，优先级低于传给 `dva()` 的 `opts.initialState`。

  ```js
  import dva from 'dva'
  const app = dva({
    initialState: { count: 1 },
  });
  app.model({
    namespace: 'count',
    state: 0,
  });
  ```

- reducers：以 key/value 格式定义 reducer。用于处理同步操作，唯一可以修改 `state` 的地方。由 `action` 触发。

  格式为 `(state, action) => newState` 或 `[(state, action) => newState, enhancer]`。

- effects：以 key/value 格式定义 effect。用于处理异步操作和业务逻辑，不直接修改 `state`。由 `action` 触发，可以触发 `action`，可以和服务器交互，可以获取全局 `state` 的数据等等。

  格式为 `*(action, effects) => void` 或 `[*(action, effects) => void, { type }]`。

  type 类型有：

  - `takeEvery`：在发起（dispatch）到 Store 并且匹配 pattern 的每一个 action 上派生一个 saga。**也就是每 dispatch 一次都会出出发 effects**
  - `takeLatest`：在发起到 Store 并且匹配 pattern 的每一个 action 上派生一个 saga。并自动取消之前所有已经启动但仍在执行中的 saga 任务。**也就是每次都只会出发最新的 effects，会将前面的 effects 抛弃掉。**
  - `throttle`：跟函数防抖一样，在发起到 Store 并且匹配 pattern 的一个 action 上派生一个 saga。 它在派生一次任务之后，仍然将新传入的 action 接收到底层的 buffer 中，至多保留（最近的）一个。但与此同时，它在 ms 毫秒内将暂停派生新的任务 —— 这也就是它被命名为节流阀（throttle）的原因。其用途，是在处理任务时，无视给定的时长内新传入的 action。**该 type 后面还需要在传入一个 ms 属性作为参数，该属性为`int`类型，代表需要间隔的毫秒数。**
  - `watcher`：**可以理解为只会在初始化的执行一次，后续的 dispatch 也不会触发。**

- subscriptions：以 key/value 格式定义 subscription。subscription 是订阅，用于订阅一个数据源，然后根据需要 dispatch 相应的 action。在 `app.start()` 时被执行，数据源可以是当前的时间、服务器的 websocket 连接、keyboard 输入、geolocation 变化、history 路由变化等等。

  格式为 `({ dispatch, history }, done) => unlistenFunction`。

  **注意：**如果要使用 `app.unmodel()`，subscription 必须返回 unlisten 方法，用于取消数据订阅。该方法是我们自己定制的。

```js
import dva from 'dva'
const app = dva()
app.model({
  namespace: 'todo',
  state: [],
  reducers: {
    add(state, { payload: todo }) {
      // 保存数据到 state
      return [...state, todo];
    },
  },
  effects: {
    *save({ payload: todo }, { put, call }) {
      // 调用 saveTodoToServer，成功后触发 `add` action 保存到 state
      yield call(saveTodoToServer, todo);
      yield put({ type: 'add', payload: todo });
    },
  },
  subscriptions: {
    setup({ history, dispatch }) {
      // 监听 history 变化，当进入 `/` 时触发 `load` action
      // history会返回对应的 unlisten 函数
      return history.listen(({ pathname }) => {
        if (pathname === '/') {
          dispatch({ type: 'load' });
        }
      });
    },
  },
});
```

### 1.1 State

```typescript
type State = any
```

State 表示 Model 的状态数据，通常表现为一个 javascript 对象（当然它可以是任何值）；操作的时候每次都要当作不可变数据（immutable data）来对待，保证每次都是全新对象，没有引用关系，这样才能保证 State 的独立性，便于测试和追踪变化。

在 dva 中你可以通过 dva 的实例属性 `_store` 看到顶部的 state 数据，但是通常你很少会用到:

```javascript
const app = dva();
console.log(app._store); // 顶部的 state 数据
```



### 1.2 Action

```typescript
type AsyncAction = any
```

Action 是一个普通 javascript 对象，它是改变 State 的唯一途径。无论是从 UI 事件、网络回调，还是 WebSocket 等数据源所获得的数据，最终都会通过 dispatch 函数调用一个 action，从而改变对应的数据。action 必须带有 `type` 属性指明具体的行为，其它字段可以自定义，如果要发起一个 action 需要使用 `dispatch` 函数；需要注意的是 `dispatch` 是在组件 connect Models以后，通过 props 传入的。

```js
dispatch({
  type: 'add',
});
```



### 1.3 dispatch 函数

```typescript
type dispatch = (a: Action) => Action
```

dispatching function 是一个用于触发 action 的函数，action 是改变 State 的唯一途径，但是它只描述了一个行为，而 dipatch 可以看作是触发这个行为的方式，而 Reducer 则是描述如何改变数据的。

在 dva 中，connect Model 的组件通过 props 可以访问到 dispatch，可以调用 Model 中的 Reducer 或者 Effects，常见的形式如：

```javascript
dispatch({
  type: 'user/add', // 如果在 model 外调用，需要添加 namespace
  payload: {}, // 需要传递的信息
});
```



### 1.4 Reducer

```typescript
type Reducer<S, A> = (state: S, action: A) => S
```

Reducer（也称为 reducing function）函数接受两个参数：之前已经累积运算的结果和当前要被累积的值，返回的是一个新的累积结果。该函数把一个集合归并成一个单值。

Reducer 的概念来自于是函数式编程，很多语言中都有 reduce API。如在 javascript 中：

```javascript
[{x:1},{y:2},{z:3}].reduce(function(prev, next){
    return Object.assign(prev, next);
})
//return {x:1, y:2, z:3}
```

在 dva 中，reducers 聚合积累的结果是当前 model 的 state 对象。通过 actions 中传入的值，与当前 reducers 中的值进行运算获得新的值（也就是新的 state）。需要注意的是 Reducer 必须是[纯函数](https://github.com/MostlyAdequate/mostly-adequate-guide/blob/master/ch3.md)，所以同样的输入必然得到同样的输出，它们不应该产生任何副作用。并且，每一次的计算都应该使用[immutable data](https://github.com/MostlyAdequate/mostly-adequate-guide/blob/master/ch3.md#reasonable)，这种特性简单理解就是每次操作都是返回一个全新的数据（独立，纯净），所以热重载和时间旅行这些功能才能够使用。



### 1.5 Effect

Effect 被称为副作用，在我们的应用中，最常见的就是异步操作。它来自于函数编程的概念，之所以叫副作用是因为它使得我们的函数变得不纯，同样的输入不一定获得同样的输出。

dva 为了控制副作用的操作，底层引入了[redux-sagas](http://superraytin.github.io/redux-saga-in-chinese)做异步流程控制，由于采用了[generator的相关概念](http://www.ruanyifeng.com/blog/2015/04/generator.html)，所以将异步转成同步写法，从而将effects转为纯函数。至于为什么我们这么纠结于 **纯函数**，如果你想了解更多可以阅读[Mostly adequate guide to FP](https://github.com/MostlyAdequate/mostly-adequate-guide)，或者它的中文译本[JS函数式编程指南](https://www.gitbook.com/book/llh911001/mostly-adequate-guide-chinese/details)。

```js
app.model({
  namespace: 'todos',
  effects: {
    *addRemote({ payload: todo }, { put, call }) {
      yield call(addTodo, todo);
      yield put({ type: 'add', payload: todo });
    },
  },
});
```

effect 第二个参数带有三个 effects：

- put：用于触发 action 。

  ```javascript
  yield put({ type: 'todos/add', payload: 'Learn Dva' });
  ```

- call：用于调用异步逻辑，支持 promise 。

  ```javascript
  const result = yield call(fetch, '/todos');
  ```

- select：用于从 state 里获取数据。

  ```javascript
  const todos = yield select(state => state.todos);
  ```



### 1.6 Subscription

Subscriptions 是一种从 **源** 获取数据的方法，它来自于 elm。

Subscription 语义是订阅，用于订阅一个数据源，然后根据条件 dispatch 需要的 action。数据源可以是当前的时间、服务器的 websocket 连接、keyboard 输入、geolocation 变化、history 路由变化等等。

```javascript
import key from 'keymaster';
...
app.model({
  namespace: 'count',
  subscriptions: {
    keyEvent({dispatch}) {
      key('⌘+up, ctrl+up', () => { dispatch({type:'add'}) });
    },
  }
});
```



## 2.Router

dva 中的 router 实际上是对`react-router`与`react-router-redux`的一层封装，可以通过引入拿到这两者的内部元素。

```jsx
import dva from 'dva';
import { Router, Route, routerRedux } from 'dva/router';
const app = dva();
app.router(({history}) =>
  <Router history={history}>
    <Route path="/" component={HomePage} />
  </Router>
);
```

### 2.1 绑定数据

如果我们需要使用上面的 model，我们就需要将其绑定到对应的组件上，我们可以通过`connect`进行数据绑定。

```js
import { connect } from 'dva';
function App() {}

function mapStateToProps(state, ownProps) {
  return {
    users: state.users,
  };
}
export default connect(mapStateToProps)(App);
```

然后在 App 里就有了 `dispatch` 和 `users` 两个属性。dispath 是用来派发 action 的，而 users 是用户自己定义的 state



### 2.2 Route Component

按照官方的说法，Route Components 是指 `./src/routes/` 目录下的文件，他们是 `./src/router.js` 里匹配的 Component。在 dva 中，通常需要 connect Model的组件都是 Route Components，组织在`/routes/`目录下，而`/components/`目录下则是纯组件（Presentational Components）。

所以，我们可以理解为 RouteComponent 表示 Router 里匹配路径的 Component，通常会绑定 model 的数据，Presentational Component 是独立的纯粹的，例如`ant.design UI`组件的 react 实现，每个组件跟业务数据并没有耦合关系，只是完成自己独立的任务，需要的数据通过 props 传递进来，需要操作的行为通过接口暴露出去。 而 Container Component 更像是状态管理器，它表现为一个容器，订阅子组件需要的数据，组织子组件的交互逻辑和展示。

**需要注意的是：Router 里匹配路径的 Component 会有额外的 props 用以获取路由信息。**

- location
- params
- children

具体内容同`react-router`。



### 2.3 使用 routerRedux

我们还可以直接使用编程式导航基于 action 直接进行跳转。

```js
import { routerRedux } from 'dva/router';

// Inside Effects
yield put(routerRedux.push('/logout'));

// Outside Effects
dispatch(routerRedux.push('/logout'));

// With query
routerRedux.push({
  pathname: '/logout',
  query: {
    page: 2,
  },
});
```

除 `push(location)` 外还有更多方法，详见 [react-router-redux](https://github.com/reactjs/react-router-redux#pushlocation-replacelocation-gonumber-goback-goforward)



## 3.dva API

### 3.1 app = dva(opts)

创建应用，返回 dva 实例。(注：dva 支持多实例)

`opts` 包含：

- `history`：指定给路由用的 history，默认是 `hashHistory`
- `initialState`：指定初始数据，优先级高于 model 中的 state，默认是 `{}`

如果要配置 history 为 `browserHistory`，可以这样：

```js
import createHistory from 'history/createBrowserHistory';
const app = dva({
  history: createHistory(),
  initialState:{ count: 1 }
});
```

另外，出于易用性的考虑，`opts` 里也可以配所有的 [hooks](https://dvajs.com/api/#appusehooks) ，下面包含全部的可配属性：

```js
const app = dva({
  history,
  initialState,
  onError,
  onAction,
  onStateChange,
  onReducer,
  onEffect,
  onHmr,
  extraReducers,
  extraEnhancers,
});
```



### 3.2 app.use(hooks)

**配置 hooks 或者注册插件。（插件最终返回的是 hooks ）**

比如注册 [dva-loading](https://github.com/dvajs/dva-loading) 插件的例子：

```js
import createLoading from 'dva-loading';
...
app.use(createLoading(opts));
```

`hooks` 包含：

- `onError((err, dispatch) => {})`

    `effect` 执行错误或 `subscription` 通过 `done` 主动抛错时触发，可用于管理全局出错状态。

    注意：`subscription` 并没有加 `try...catch`，所以有错误时需通过第二个参数 `done` 主动抛错。例子：

    ```js
    app.model({
      subscriptions: {
        setup({ dispatch }, done) {
          done(e);
        },
      },
    });
    ```

    如果我们用 antd，那么最简单的全局错误处理通常会这么做：

    ```js
    import { message } from 'antd';
    const app = dva({
      onError(e) {
        message.error(e.message, /* duration */3);
      },
    });
    ```

- `onAction(fn | fn[])`

    在 action 被 dispatch 时触发，用于注册 redux 中间件。支持函数或函数数组格式。

    例如我们要通过 [redux-logger](https://github.com/evgenyrodionov/redux-logger) 打印日志：

    ```js
    import createLogger from 'redux-logger';
    const app = dva({
      onAction: createLogger(opts),
    });
    ```

- `onStateChange(fn)`

    `state` 改变时触发，可用于同步 `state` 到 localStorage，服务器端等。

- `onReducer(fn)`

    封装 reducer 执行。比如借助 [redux-undo](https://github.com/omnidan/redux-undo) 实现 redo/undo ：

    ```js
    import undoable from 'redux-undo';
    const app = dva({
      onReducer: reducer => {
        return (state, action) => {
          const undoOpts = {};
          const newState = undoable(reducer, undoOpts)(state, action);
          // 由于 dva 同步了 routing 数据，所以需要把这部分还原
          return { ...newState, routing: newState.present.routing };
        },
      },
    });
    ```

- `onEffect(fn)`

    封装 effect 执行。比如 [dva-loading](https://github.com/dvajs/dva-loading) 基于此实现了自动处理 loading 状态。

- `onHmr(fn)`

    热替换相关，目前用于 [babel-plugin-dva-hmr](https://github.com/dvajs/babel-plugin-dva-hmr) 。

- `extraReducers`

    指定额外的 reducer，比如 [redux-form](https://github.com/erikras/redux-form) 需要指定额外的 `form` reducer：

    ```js
    import { reducer as formReducer } from 'redux-form'
    const app = dva({
      extraReducers: {
        form: formReducer,
      },
    });
    ```

  - `extraEnhancers`

    指定额外的 [StoreEnhancer](https://github.com/reactjs/redux/blob/master/docs/Glossary.md#store-enhancer) ，比如结合 [redux-persist](https://github.com/rt2zz/redux-persist) 的使用：

    ```js
    import { persistStore, autoRehydrate } from 'redux-persist';
    const app = dva({
      extraEnhancers: [autoRehydrate()],
    });
    persistStore(app._store);
    ```

### 3.3 app.model(model)

注册 model，详见上面第一大点。

### 3.4 app.unmodel(namespace)

取消 model 注册，清理 reducers, effects 和 subscriptions。subscription 如果没有返回 unlisten 函数，使用 `app.unmodel` 会给予警告⚠️。

### 3.5 app.replaceModel(model)

> 只在app.start()之后可用

替换model为新model，清理旧model的reducers, effects 和 subscriptions，但会保留旧的state状态，对于HMR非常有用。subscription 如果没有返回 unlisten 函数，使用 `app.unmodel` 会给予警告⚠️。

如果原来不存在相同namespace的model，那么执行`app.model`操作

### 3.6 app.router(({ history, app }) => RouterConfig)

注册路由表。

通常是这样的：

```jsx
import { Router, Route } from 'dva/router';
app.router(({ history }) => {
  return (
    <Router history={history}>
      <Route path="/" component={App} />
    </Router>
  );
});
```

推荐把路由信息抽成一个单独的文件，这样结合 [babel-plugin-dva-hmr](https://github.com/dvajs/babel-plugin-dva-hmr) 可实现路由和组件的热加载，比如：

```js
app.router(require('./router'));
```

而有些场景可能不使用路由，比如多页应用，所以也可以传入返回 JSX 元素的函数。比如：

```js
app.router(() => <App />);
```

### 3.7 app.start(selector?)

启动应用。`selector` 可选，如果没有 `selector` 参数，会返回一个返回 JSX 元素的函数。

```js
app.start('#root');
```

那么什么时候不加 `selector`？常见场景有测试、node 端、react-native 和 i18n 国际化支持。

比如通过 react-intl 支持国际化的例子：

```jsx
import { IntlProvider } from 'react-intl';
...
const App = app.start();
ReactDOM.render(<IntlProvider><App /></IntlProvider>, htmlElement);
```
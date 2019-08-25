# 	Redux

> 参考自阮一峰的[《Redux 入门教程》](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

- Redux是一个独立专门用于做状态的JS库（不是react的插件库，与react无关）
- Redux可以在任意的项目中使用，包括angular、react、vue，不过基本Redux是配合react使用的
- 该插件在react中的作用是用于集中式管理react应用中多个组件共享的状态

**把Redux管理的状态比喻成一个图书馆，流程图如下：**

![img](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1894865766,1121191065&fm=26&gp=0.jpg)

## 1.何时使用Redux

**使用Redux的总体原则是：能不使用就不使用，除非是到了不用Redux项目完成起来会非常吃力**

**不必使用的场景：**

- 用户的使用方式非常简单
- 用户之间没有协作
- 不需要与服务器大量交互，也没有使用 WebSocket
- 视图层（View）只从单一来源获取数据



**具体的使用场景：**

- 某个组件的状态需要共享时
- 某个状态需要在任何地方都可以拿到
- 一个组件需要改变全局状态
- 一个组件需要改变另外一个组件的状态



## 2.基本概念和API

**使用Redux需要下载Redux的包**

```shell
npm i Redux -S
```

### 2.1 Store

Store 就是保存数据的地方，可以把它看成一个容器。整个应用只能有一个 Store。Redux 提供`createStore`这个函数，用来生成 Store

```js
 import { createStore } from 'Redux';
 const store = createStore(fn);
// createStore函数接受另一个函数作为参数，返回新生成的 Store 对象，这个函数就是下文提到的reducer
```



### 2.2 State

`Store`对象包含所有数据。**如果想得到某个时点的数据，就要对 Store 生成快照。**这种时点的数据集合，就叫做 State。**当前时刻的 State，可以通过`store.getState()`拿到**

```js
import { createStore } from 'Redux';
const store = createStore(fn);
const state = store.getState();
```

**Redux 规定， 一个 State 对应一个 View。只要 State 相同，View 就相同。知道 State，就知道 View 是什么样，反之亦然**



### 2.3 Action

State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，**State 的变化必须是 View 导致的。**Action 就是 View 发出的通知，表示 State 应该要发生变化了

**Action 是一个对象。其中的`type`属性是必须的，表示 Action 的名称。**其他属性可以自由设置，在Redux社区有一个[规范](https://github.com/acdlite/flux-standard-action)可以进行参考

```js
const action = {
    type: 'ADD_TODO',
    payload: 'Learn Redux'
};
// Action 的名称是 ADD_TODO，它携带的信息是字符串 Learn Redux
```

可以这样理解，Action 描述当前发生的事情。**改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store**



### 2.4 Action Creator

View 要发送多少种消息，就会有多少种 Action。如果都手写，会很麻烦。可以定义一个函数来生成 Action，这个函数就叫 Action Creator。

```js
const ADD_TODO = '添加 TODO';

function addTodo(text) {
    return {
        type: ADD_TODO,
        text
    }
}

const action = addTodo('Learn Redux')
// addTodo函数就是一个 Action Creator
```



### 2.5 store.dispatch()

**`store.dispatch()`是 View 发出 Action 的唯一方法**

```js
import { createStore } from 'Redux';
const store = createStore(fn);

store.dispatch({
    type: 'ADD_TODO',
    payload: 'Learn Redux'
});
// store.dispatch接受一个 Action 对象作为参数，将它发送出去
```

**结合 Action Creator，这段代码可以改写如下**

```js
store.dispatch(addTodo('Learn Redux'));
```



### 2.6 Reducer

Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。**Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State**

```js
const reducer = function (state, action) {// action就是通过dispatch传入的对象
    // ...
    return new_state;
};
```

**注：**整个应用的初始状态，可以作为 State 的默认值

```js
const defaultState = 0;
const reducer = (state = defaultState, action) => {
    switch (action.type) {
        case 'ADD':
            return state + action.payload;
        default: 
            return state;
    }
};

const state = reducer(1, {
    type: 'ADD',
    payload: 2
});
/*
reducer函数收到名为ADD的 Action 以后，就返回一个新的 State，作为加法的计算结果。其他运算的逻辑（比如减法），也可以根据 Action 的不同来实现
*/
```

**注意：**实际应用中，**Reducer 函数不用像上面这样手动调用，`store.dispatch`方法会触发 Reducer 的自动执行。**为此，Store 需要知道 Reducer 函数，做法就是**在生成 Store 的时候，将 Reducer 传入`createStore`方法**

```js
import { createStore } from 'Redux';
const store = createStore(reducer);
/*
createStore接受 Reducer 作为参数，生成一个新的 Store。以后每当store.dispatch发送过来一个新的 Action，就会自动调用 Reducer，得到新的 State
*/
```

**同时，为什么这个函数叫做 Reducer 呢？因为它可以作为数组的`reduce`方法的参数**

```js
// 一系列 Action 对象按照顺序作为一个数组
const actions = [
    { type: 'ADD', payload: 0 },
    { type: 'ADD', payload: 1 },
    { type: 'ADD', payload: 2 }
];

const total = actions.reduce(reducer, 0); // 3
/*
数组actions表示依次有三个 Action，分别是加0、加1和加2。数组的`reduce`方法接受 Reducer 函数作为参数，就可以直接得到最终的状态3
*/
```

#### 2.6.1 纯函数

**Reducer 函数最重要的特征是，它是一个纯函数。也就是说，只要是同样的输入，必定得到同样的输出**

纯函数是函数式编程的概念，**必须遵守以下一些约束**:

- 不得改写参数
- 不能调用系统 I/O 的API
- 不能调用`Date.now()`或者`Math.random()`等不纯的方法，因为每次会得到不一样的结果

**由于 Reducer 是纯函数，就可以保证同样的State，必定得到同样的 View。但也正因为这一点，Reducer 函数里面不能改变 State，必须返回一个全新的对象**

```js
// State 是一个对象
function reducer(state, action) {
    return Object.assign({}, state, { thingToChange });
    // 或者
    return { ...state, ...newState };
}

// State 是一个数组
function reducer(state, action) {
    return [...state, newItem];
}

```

**注：**最好把 State 对象设成只读。你没法改变它，要得到新的 State，唯一办法就是生成一个新对象。这样的好处是，任何时候，与某个 View 对应的 State 总是一个不变的对象



#### 2.6.2 store.subscribe()

**Store 允许使用`store.subscribe`方法设置监听函数，一旦 State 发生变化，就自动执行这个函数**

```js
import { createStore } from 'Redux';
const store = createStore(reducer);

store.subscribe(listener);
```

于是，只要把 View 的更新函数（对于 React 项目，就是组件的`render`方法或`setState`方法）放入`listen`，就会实现 View 的自动渲染

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import {createStore} from 'redux'
import App from './App'
import {reducer1,reducer2} from './reducers'// 因为不只管理一个状态

const sotre = createStore(reducer1)// 内部会第一次调用reducer函数得到初始的state
function render(){
    ReactDOM.render(<App store={store} />,document.getElementById('root'))
}
//初始化渲染
render()

store.subscribe(render)
```

**`store.subscribe`方法返回一个函数，调用这个函数就可以解除监听**

```js
let unsubscribe = store.subscribe(
    () =>
    console.log(store.getState())
);

unsubscribe();
```



## 3.Store的实现

**可以发现 Store 提供了三个方法：**

- store.getState()
- store.dispatch()
- store.subscribe()

```js
import { createStore } from 'redux';
let { subscribe, dispatch, getState } = createStore(reducer);
```

`createStore`方法还可以接受**第二个参数，**表示 State 的最初状态。这通常是**服务器给出的**

```js
let store = createStore(todoApp, window.STATE_FROM_SERVER)
/*
window.STATE_FROM_SERVE`就是整个应用的状态初始值，如果提供了这个参数，它会覆盖 Reducer函数的默认初始值
*/
```

```js
// 一个createStore方法的简单实现
const createStore = (reducer) => {
    let state;
    let listeners = [];

    const getState = () => state;

    const dispatch = (action) => {
        // 进行了分派操作并且改变state并重新渲染页面
        state = reducer(state, action);
        listeners.forEach(listener => listener());
    };

    const subscribe = (listener) => {
        listeners.push(listener);
        // 监听返回过滤后的listener，此时这个函数应该已经结束不会再启用，listener已经存入了内存
        return () => {
            listeners = listeners.filter(l => l !== listener);
        }
    };

    dispatch({});

    return { getState, dispatch, subscribe };
};
```



## 4.Reducer的拆分

Reducer 函数负责生成 State。由于整个应用只有一个 State 对象，包含所有数据，对于大型应用来说，这个 State 必然十分庞大，导致 Reducer 函数也十分庞大

**现在看下面的例子：**

```js
const chatReducer = (state = defaultState, action = {}) => {
    const { type, payload } = action;
    switch (type) {
        case ADD_CHAT:
            return Object.assign({}, state, {
                chatLog: state.chatLog.concat(payload)
            });
        case CHANGE_STATUS:
            return Object.assign({}, state, {
                statusMessage: payload
            });
        case CHANGE_USERNAME:
            return Object.assign({}, state, {
                userName: payload
            });
        default: return state;
    }
};
```

**上面代码中，三种 Action 分别改变 State 的三个属性：**

- ADD_CHAT：`chatLog`属性
- CHANGE_STATUS：`statusMessage`属性
- CHANGE_USERNAME：`userName`属性

**这三个属性之间没有联系，这提示我们可以把 Reducer 函数拆分。**不同的函数负责处理不同属性，最终把它们合并成一个大的 Reducer 即可

```js
const chatReducer = (state = defaultState, action = {}) => {
    return {
        chatLog: chatLog(state.chatLog, action),
        statusMessage: statusMessage(state.statusMessage, action),
        userName: userName(state.userName, action)
    }
};
// Reducer 函数被拆成了三个小函数，每一个负责生成对应的属性
```

这样一拆，Reducer 就易读易写多了。而且，这种拆分与 React 应用的结构相吻合：一个 React 根组件由很多子组件构成。**这就是说，子组件与子 Reducer 完全可以对应**

### 4.1 combineReducers

Redux 提供了一个`combineReducers`方法，用于 Reducer 的拆分。你只要定义各个子 Reducer 函数，然后用这个方法，将它们合成一个大的 Reducer

```js
import { combineReducers } from 'redux';

const chatReducer = combineReducers({
    chatLog,
    statusMessage,
    userName
})

export default chatReducer
// combineReducers方法将三个子 Reducer 合并成一个大的函数
```

**注：**这种写法有一个前提，就是 State 的属性名必须与子 Reducer 同名。如果不同名，就要采用下面的写法

```js
const reducer = combineReducers({
    a: doSomethingWithA,
    b: processB,
    c: c
})

// 等同于
function reducer(state = {}, action) {
    return {
        a: doSomethingWithA(state.a, action),
        b: processB(state.b, action),
        c: c(state.c, action)
    }
}
```

总之，`combineReducers()`做的就是产生一个整体的 Reducer 函数。该函数根据 State 的 key 去执行相应的子 Reducer，并将**返回结果合并成一个大的 State 对象（所以，如果我们想获取到对应的State,可以通过getState().key获取到）**

```js
// combineReducer的简单实现
const combineReducers = reducers => {
    // 通过循环返回一个总的对象
    return (state = {}, action) => {
        return Object.keys(reducers).reduce(
            (nextState, key) => {
                nextState[key] = reducers[key](state[key], action);
                return nextState;
            },
            {} 
        );
    };
};
```

**也可以把所有子 Reducer 放在一个文件里面，然后统一引入**

```js
import { combineReducers } from 'redux'
import * as reducers from './reducers'
// reducers包含了所有的action
const reducer = combineReducers(reducers)
```

**注意：**每个传入 combineReducers 的 reducer 都需满足以下规则：

- 所有未匹配到的 action，必须把它接收到的第一个参数也就是那个 state 原封不动返回
- 永远不能返回 undefined。当过早 return 时非常容易犯这个错误，为了避免错误扩散，遇到这种情况时 combineReducers 会抛异常。
- 如果传入的 state 就是 undefined，一定要返回对应 reducer 的初始 state。根据上一条规则，初始 state 禁止使用 undefined。使用 ES6 的默认参数值语法来设置初始 state 很容易，但你也可以手动检查第一个参数是否为 undefined
- 虽然 combineReducers 自动帮你检查 reducer 是否符合以上规则，但你也应该牢记，并尽量遵守



## 5.计数器案例

```jsx
const Counter = ({ value }) => (
    <h1>{value}</h1>
);

const render = () => {
    ReactDOM.render(
        <Counter value={store.getState()}/>,
        document.getElementById('root')
    );
};

store.subscribe(render);
render();
/*
上面是一个简单的计数器，唯一的作用就是把参数value的值，显示在网页上。Store 的监听函数设置为render，每次 State 的变化都会导致网页重新渲染
*/
```

```jsx
// 下面加入一点变化，为`Counter`添加递增和递减的 Action
// 当然，下面的模块如果复杂应该分模块引入
const Counter = ({ value, onIncrement, onDecrement }) => (
    <div>
        <h1>{value}</h1>
        <button onClick={onIncrement}>+</button>
        <button onClick={onDecrement}>-</button>
    </div>
);

const reducer = (state = 0, action) => {
    switch (action.type) {
        case 'INCREMENT': return state + 1;
        case 'DECREMENT': return state - 1;
        default: return state;
    }
};

const store = createStore(reducer);

const render = () => {
    ReactDOM.render(
        <Counter
            value={store.getState()}
            onIncrement={() => store.dispatch({type: 'INCREMENT'})}
            onDecrement={() => store.dispatch({type: 'DECREMENT'})}
            />,
        document.getElementById('root')
    );
};

render();
store.subscribe(render);
```



## 6.中间件

在Redux创建Store的过程中还支持传入中间件，在Redux中，中间件就是一个函数，对`store.dispatch`方法进行了改造，在发出 Action 和执行 Reducer 这两步之间，添加了其他功能，就如Redux需要的异步等功能也是通过中间键的方式来进行添加的

**注：**中间件在node的两个著名框架中都有着及其广泛的以应用，这里就只说如何使用中间件

```js
// 为redux提供日志中间件
// 在redux中有个专门为添加中间件进行解析执行的函数applyMiddleware，在后面详细说明，这里先理解有这个函数
import { applyMiddleware, createStore } from 'redux';
import logger from 'redux-logger';

const store = createStore(
    reducer,
    applyMiddleware(logger)
);
/*
将引入的logger放在applyMiddleware方法之中，传入createStore方法，就完成了store.dispatch()的功能增强
*/
```

**注意：**

- `createStore`方法可以接受整个应用的初始状态作为参数，那样的话，`applyMiddleware`就应该是第三个参数了

  ```js
  const store = createStore(
      reducer,
      initial_state,
      applyMiddleware(logger)
  );
  ```

- 要注意中间件的次序，中间件的执行次序也是依照参数的位置依次执行的，如果两个中间件有依耐关系，就需要自己调整好中间件的位置

  ```js
  const store = createStore(
      reducer,
      applyMiddleware(thunk, promise, logger)
  );
  /*
  applyMiddleware方法的三个参数，就是三个中间件。有的中间件有次序要求，使用前要查一下文档。比如，logger就一定要放在最后，否则输出结果会不正确
  */
  ```

### 6.1 applyMiddlewares()

applyMiddlewares 是 Redux 的原生方法，作用是将所有中间件组成一个数组，依次执行

```js
// applyMiddlewares 源码
export default function applyMiddleware(...middlewares) {
    return (createStore) => (reducer, preloadedState, enhancer) => {
        var store = createStore(reducer, preloadedState, enhancer);
        var dispatch = store.dispatch;
        var chain = [];

        var middlewareAPI = {
            getState: store.getState,
            dispatch: (action) => dispatch(action)
        };
        chain = middlewares.map(middleware => middleware(middlewareAPI));
        dispatch = compose(...chain)(store.dispatch);

        return {...store, dispatch}
    }
}
/*
所有中间件被放进了一个数组chain，然后嵌套执行，最后执行store.dispatch。可以看到的是，因为循环遍历了传进来的中间件数组，所有的中间件内部都传入了middlewareAPI对象，于是可以拿到getState和dispatch这两个方法
*/
```



## 7.异步操作

**Redux的异步操作需要借助中间件来完成，所以要先弄清楚Redux的中间件**

### 7.1 基本思路

在Redux中，**同步操作只要发出一种 Action 即可，异步操作的差别是它要发出两种 Action（因为成功和失败不可能同时发出）**

- 操作发起时的 Action
- 操作成功时的 Action（如果失败了就不会发出）
- 操作失败时的 Action（如果成功了就不会发出）

```js
// 以向服务器取出数据为例，三种 Action （这里的三种就说的是前面所有的action了）可以有两种不同的写法

// 写法一：名称相同，参数不同
{ type: 'FETCH_POSTS' }
{ type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
{ type: 'FETCH_POSTS', status: 'success', response: { ... } }

// 写法二：名称不同
{ type: 'FETCH_POSTS_REQUEST' }
{ type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
{ type: 'FETCH_POSTS_SUCCESS', response: { ... } }
```

**除了 Action 种类不同，异步操作的 State 也要进行改造，反映不同的操作状态**

```js
let state = {
    // ... 
    isFetching: true,
    didInvalidate: true,
    lastUpdated: 'xxxxxxx'
};
/*
State 的属性isFetching表示是否在抓取数据。didInvalidate表示数据是否过时，lastUpdated表示上一次更新时间
*/
```

**于是，这就是整个异步操作的思路：**

- 操作开始时，送出一个 Action，触发 State 更新为"正在操作"状态，View 重新渲染
- 操作结束后，再送出一个 Action，触发 State 更新为"操作结束"状态，View 再一次重新渲染



### 7.2 redux-thunk 中间件

异步操作至少要送出两个 Action：用户触发第一个 Action，这个跟同步操作一样，不必深究，但是如何才能在操作结束时，系统自动送出第二个 Action 确是一个很难的问题

**使用[`redux-thunk`](https://github.com/gaearon/redux-thunk)中间件可以让我们在store.dispatch中传入一个函数作为参数（正常情况下只能传入对象作为参数），这样我们就能在传入的函数中进行异步操作**

```shell
npm i redux-thunk -S
```

```js
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import reducer from './reducers';

// Note: this API requires redux@>=3.1.0
// 然后直接引用中间件就好
const store = createStore(
    reducer,
    applyMiddleware(thunk)
);
```

**有了可以传入函数的dispath，奥妙就在 Action Creator 之中，在使用了`redux-thunk`中间件以后，store.dispatch()可以传入一个函数作为参数，在该函数内可以执行异步操作，这就是异步的 Action Creator**

```js
class AsyncApp extends Component {
    componentDidMount() {
        const { dispatch, selectedPost } = this.props
        // 分发一个函数
        dispatch(fetchPosts(selectedPost))
    }
}
/*
加载成功后（componentDidMount方法），它送出了（dispatch方法）一个 Action，向服务器要求数据 fetchPosts(selectedSubreddit)。这里的fetchPosts就是 Action Creator
*/
```

```js
// 下面为fetchPosts函数
/*
同步的action都是返回一个对象，而异步的action则是返回一个函数，这就是dispatch传参的第二种参数
*/
const fetchPosts = postTitle => (dispatch, getState) => {
  // 返回一个函数，在函数中可以执行异步代码
  // 异步action一样要执行一个与之匹配的同步的action来修改state
  dispatch(requestPosts(postTitle));
  return fetch(`/some/API/${postTitle}.json`)
    .then(response => response.json())
    .then(json => dispatch(receivePosts(postTitle, json)));
  };
};

// 使用方法一
store.dispatch(fetchPosts('reactjs'));
// 使用方法二
store.dispatch(fetchPosts('reactjs')).then(() =>
  console.log(store.getState())
);
/*
fetchPosts是一个Action Creator（动作生成器），返回一个函数。这个函数执行后，先发出一个Action（requestPosts(postTitle)），然后在函数内部就可以进行异步操作。拿到结果后，先将结果转成 JSON 格式，然后再发出一个 Action（ receivePosts(postTitle, json)）
*/
```

**注意：**

- `fetchPosts`返回了一个函数，而普通的 Action Creator 默认返回一个对象
- 返回的函数的参数是`dispatch`和`getState`这两个 Redux 方法，普通的 Action Creator 的参数是 Action 的内容
- 在返回的函数之中，先发出一个 Action（`requestPosts(postTitle)`），表示操作开始
- 异步操作结束之后，再发出一个 Action（`receivePosts(postTitle, json)`），表示操作结束

**于是，这样就解决了dispath中异步发送第二个 Action 的问题。**因此，异步操作的第一种解决方案就是，**写出一个返回函数的 Action Creator，然后使用`redux-thunk`中间件改造`store.dispatch`**



### 7.3 redux-promise 中间件

既然 Action Creator 可以返回函数，当然也可以返回其他值。**另一种异步操作的解决方案，就是让 Action Creator 返回一个 Promise 对象**

**此时需要使用`redux-promise`中间件**

```shell
npm i redux-promise -S
```

```js
import { createStore, applyMiddleware } from 'redux';
import promiseMiddleware from 'redux-promise';
import reducer from './reducers';

const store = createStore(
    reducer,
    applyMiddleware(promiseMiddleware)
); 
```

`redux-promise`使得`store.dispatch`方法可以接受 Promise 对象作为参数。**此时的 Action Creator 有两种写法:**

- 返回值是一个 Promise 对象

  ```js
  const fetchPosts = 
        (dispatch, postTitle) => new Promise(function (resolve, reject) {
            dispatch(requestPosts(postTitle));
            return fetch(`/some/API/${postTitle}.json`)
                .then(response => ({
                type: 'FETCH_POSTS',
                payload: response.json()
            }));
        });
  ```

- Action 对象的`payload`属性是一个 Promise 对象。这需要从[`redux-actions`](https://github.com/acdlite/redux-actions)模块引入`createAction`方法

  ```shell
  npm i redux-actions -S
  ```

  ```js
  import { createAction } from 'redux-actions';
  
  class AsyncApp extends Component {
      componentDidMount() {
          const { dispatch, selectedPost } = this.props
          // 发出同步 Action
          dispatch(requestPosts(selectedPost));
          // 发出异步 Action
  	  	//写法要变成这样
          dispatch(createAction(
              'FETCH_POSTS', 
              fetch(`/some/API/${postTitle}.json`)
              .then(response => response.json())1
          ));
      }
  /*
  第二个dispatch方法发出的是异步 Action，只有等到操作结束，这个 Action 才会实际发出
  */
  ```

  **注意：**`createAction`的第二个参数必须是一个 Promise 对象

```js
// redux-promise源码
export default function promiseMiddleware({ dispatch }) {
  return next => action => {
    if (!isFSA(action)) {
      return isPromise(action)
        ? action.then(dispatch)
        : next(action);
    }

    return isPromise(action.payload)
      ? action.payload.then(
          result => dispatch({ ...action, payload: result }),
          error => {
            dispatch({ ...action, payload: error, error: true });
            return Promise.reject(error);
          }
        )
      : next(action);
  };
}
```

**可以看出：**

- 如果 Action 本身是一个 Promise，它 resolve 以后的值应该是一个 Action 对象，会被`dispatch`方法送出（`action.then(dispatch)`），但 reject 以后不会有任何动作
- 如果 Action 对象的`payload`属性是一个 Promise 对象，那么无论 resolve 和 reject，`dispatch`方法都会发出 Action



## 8.React-Redux

React-Redux是专门针对React做的一个Redux库，是React的插件，该插件作用是减少React与Redx的耦合度，使得编码更加的简洁

### 8.1 组件分类

React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）

**注：**建议将UI组件专门建立一个`components`文件夹进行存放，容器组件建立一个`containers`文件夹进行存放

#### 8.1.1 UI 组件

**UI 组件有以下几个特征：**

- 只负责 UI 的呈现，不带有任何业务逻辑
- 没有状态（即不使用`this.state`这个变量）
- 所有数据都由参数（`this.props`）提供
- 不使用任何 Redux 的 API

```jsx
// 如下就是一个 UI 组件
const Title = value => <h1>{value}</h1>;
```

因为不含有状态，UI 组件又称为"纯组件"（或者应该叫做无状态组件），即它纯函数一样，纯粹由参数决定它的值



#### 8.1.2 容器组件

**容器组件的特征与 UI 组件恰恰相反：**

- 负责管理数据和业务逻辑，不负责 UI 的呈现
- 带有内部状态
- 使用 Redux 的 API



**总之：UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑**

如果一个组件既有 UI 又有业务逻辑，**那就将它拆分成外面是一个容器组件，里面包了一个UI 组件。**前者负责与外部的通信，将数据传给后者，由后者渲染出视图

**React-Redux 规定，所有的 UI 组件都由用户提供，容器组件则是由 React-Redux 自动生成。也就是说，用户负责视觉层，状态管理则是全部交给它**



### 8.2 connect()

**React-Redux 提供`connect`方法，用于从 UI 组件生成容器组件**

```js
import { connect } from 'react-redux'
/*
很奇特的用法,connect本身是一个函数，返回了一个函数，然后立刻传入了TodOlIST作为参数并且立即执行返回一个包装后的组件
*/
const VisibleTodoList = connect()(TodoList);
/*
TodoList是 UI 组件，VisibleTodoList就是由 React-Redux 通过connect方法自动生成的容器组件
*/
```

当然，上面只是 UI 组件的一个单纯的包装层。**在使用时还需要定义业务逻辑**，需要给出下面两方面的信息：

- **输入逻辑：**外部的数据（即`state`对象）如何转换为 UI 组件的参数
- **输出逻辑：**用户发出的动作如何变为 Action 对象，从 UI 组件传出去

```js
import { connect } from 'react-redux'
// connect方法的完整 API 应该像下面这样
const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
```

**`connect`方法接受两个参数：`mapStateToProps`和`mapDispatchToProps`**

这两个参数定义了 UI 组件的业务逻辑。前者负责输入逻辑，即将`state`映射到 UI 组件的参数（`props`），后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action

#### 8.2.1 mapStateToProps()

**`mapStateToProps`是一个函数。它的作用就是像它的名字那样，建立一个从（外部的）`state`对象到（UI 组件的）`props`对象的映射关系**

```js
// mapStateToProps执行后应该返回一个对象，里面的每一个键值对就是一个映射
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
/*
mapStateToProps是一个函数，它接受state作为参数，返回一个对象。这个对象有一个todos属性，代表 UI 组件的同名参数，后面的getVisibleTodos也是一个函数，可以从state算出 todos的值
*/
```

```js
const getVisibleTodos = (todos, filter) => {
    switch (filter) {
        case 'SHOW_ALL':
            return todos
        case 'SHOW_COMPLETED':
            return todos.filter(t => t.completed)
        case 'SHOW_ACTIVE':
            return todos.filter(t => !t.completed)
        default:
            throw new Error('Unknown filter: ' + filter)
    }
}
```

**注意：**

- `mapStateToProps`会订阅 Store，每当`state`更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染
- `mapStateToProps`还可以接受第二个参数。第一个参数总是`state`对象，还可以使用第二个参数，代表容器组件的`props`对象，使用了该对象后，如果容器组件的参数发生变化，也会引发 UI 组件重新渲染

```js
/* 
容器组件的代码
    <FilterLink filter="SHOW_ALL">
      All
   </FilterLink>
*/
const mapStateToProps = (state, ownProps) => {
    return {
        active: ownProps.filter === state.visibilityFilter
    }
}
```

**注 ：**`connect`方法可以省略`mapStateToProps`参数，那样的话，UI 组件就不会订阅Store，就是说 Store 的更新不会引起 UI 组件的更新



#### 8.2.2 mapDispatchToProps()

**`mapDispatchToProps`是`connect`函数的第二个参数，用来建立 UI 组件的参数到`store.dispatch`方法的映射。也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store。**该参数可以是一个函数，也可以是一个对象

- 如果`mapDispatchToProps`是一个函数，会得到`dispatch`和`ownProps`（容器组件的`props`对象）两个参数

  ```js
  const mapDispatchToProps = (
      dispatch,
      ownProps
  ) => {
      return {
          onClick: () => {
              dispatch({
                  type: 'SET_VISIBILITY_FILTER',
                  filter: ownProps.filter
              });
          }
      };
  }
  ```

  **`mapDispatchToProps`作为函数，应该返回一个对象，该对象的每个键值对都是一个映射，定义了 UI 组件的参数怎样发出 Action**

- 如果`mapDispatchToProps`是一个对象，它的每个键名也是对应 UI 组件的同名参数，键值应该是一个函数，会被当作 Action creator ，返回的 Action 会由 Redux 自动发出

  ```js
  // 上面的mapDispatchToProps写成对象就是下面这样
  const mapDispatchToProps = {
    onClick: (filter) => {
      type: 'SET_VISIBILITY_FILTER',
      filter: filter
    };
  }
  ```



### 8.3 Provider 组件

`connect`方法生成容器组件以后，需要让容器组件拿到`state`对象，才能生成 UI 组件的参数。传统的解决方法是将`state`对象作为参数，传入容器组件。但是，这样做比较麻烦，尤其是容器组件可能在很深的层级，一级级将`state`传下去就很麻烦

**所以，React-Redux 提供`Provider`组件，可以让容器组件拿到`state`**

```jsx
// 引入Provider
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import todoApp from './reducers'
import App from './components/App'

let store = createStore(todoApp);

render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
)
/*
Provider在根组件外面包了一层，这样一来，App的所有子组件就默认都可以拿到state了
*/
```

#### 8.3.1 实现原理

**它的原理是`React`组件的[`context`](https://facebook.github.io/react/docs/context.html)属性**

```js
// Provider源码
class Provider extends Component {
    getChildContext() {
        return {
            store: this.props.store
        };
    }
    render() {
        return this.props.children;
    }
}

Provider.childContextTypes = {
    store: React.PropTypes.object
}
```

**`store`放在了上下文对象`context`上面。然后，子组件就可以从`context`拿到`store`**

```js
// 大致代码如下
class VisibleTodoList extends Component {
    componentDidMount() {
        const { store } = this.context;
        this.unsubscribe = store.subscribe(
            () =>
            this.forceUpdate()
        );
    }

    render() {
        const props = this.props;
        const { store } = this.context;
        const state = store.getState();
        // ...
    }
}

VisibleTodoList.contextTypes = {
    store: React.PropTypes.object
}
```

`React-Redux`自动生成的容器组件的代码，就类似上面这样，从而拿到`store`



### 8.4 计数器案例

```jsx
// 下面是一个计数器组件，它是一个纯的 UI 组件
class Counter extends Component {
    render() {
        const { value, onIncreaseClick } = this.props
        return (
            <div>
                <span>{value}</span>
                <button onClick={onIncreaseClick}>Increase</button>
            </div>
        )
    }
}
/*
这个 UI 组件有两个参数：value和onIncreaseClick。前者需要从state计算得到，后者需要向外发出 Action
*/
```

```js
// 定义value到state的映射，以及onIncreaseClick到dispatch的映射
function mapStateToProps(state) {
  return {
    value: state.count
  }
}

function mapDispatchToProps(dispatch) {
  return {
    onIncreaseClick: () => dispatch(increaseAction)
  }
}

// Action Creator
const increaseAction = { type: 'increase' }
```

```js
// 使用connect方法生成容器组件
const App = connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter)
```

```js
// 定义这个组件的 Reducer
// Reducer
function counter(state = { count: 0 }, action) {
    const count = state.count
    switch (action.type) {
        case 'increase':
            return { count: count + 1 }
        default:
            return state
    }
}
```

```jsx
// 生成store对象，并使用Provider在根组件外面包一层
import { loadState, saveState } from './localStorage';

const persistedState = loadState();
const store = createStore(
    todoApp,
    persistedState
);

store.subscribe(throttle(() => {
    saveState({
        todos: store.getState().todos,
    })
}, 1000))

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);
```



**完整代码：**

```jsx
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import ReactDOM from 'react-dom'
import { createStore } from 'redux'
import { Provider, connect } from 'react-redux'

// React component
class Counter extends Component {
  render() {
    const { value, onIncreaseClick } = this.props
    return (
      <div>
        <span>{value}</span>
        <button onClick={onIncreaseClick}>Increase</button>
      </div>
    )
  }
}

Counter.propTypes = {
  value: PropTypes.number.isRequired,
  onIncreaseClick: PropTypes.func.isRequired
}

// Action
const increaseAction = { type: 'increase' }

// Reducer
function counter(state = { count: 0 }, action) {
  const count = state.count
  switch (action.type) {
    case 'increase':
      return { count: count + 1 }
    default:
      return state
  }
}

// Store
const store = createStore(counter)

// Map Redux state to component props
function mapStateToProps(state) {
  return {
    value: state.count
  }
}

// Map Redux actions to component props
function mapDispatchToProps(dispatch) {
  return {
    onIncreaseClick: () => dispatch(increaseAction)
  }
}

// Connected Component
const App = connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter)

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```



### 8.5 使用React-Router 

使用`React-Router`的项目，与其他项目没有不同之处，也是使用`Provider`在`Router`外面包一层，**毕竟`Provider`的唯一功能就是传入`store`对象**

```jsx
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path="/" component={App} />
    </Router>
  </Provider>
);
```



## 9.调试Redux

- 首先需要在Chrome安装Redux调试扩展程序`Redux Devtools`

- 然后下载调试的工具依耐包

  ```shell
  npm i redux-devtools-extension -D
  ```

  ```js
  import {createStore, applyMiddleware} from 'redux'
  import {composeWithDevTools} from 'redux-devtools-extension'
  import {reducers1} from './reducers'
  import thunk from 'redux-thunk'
  
  const store = createStore(
  	reducer1,
      composeWithDevTools(applyMiddleware(thunk))
  )
  ```



## 10.模块化

一般使用Redux推荐使用Vuex规范的构建模式，将每一个模块进行分层管理，创建一个redux目录来专门装这些模块

- **action-type.js**

  ```js
  // action-type.js
  /*
  	包含所有action的type名称常量
  */
  ```

- **action.js**

  ```js
  // action.js
  import {type1,type2} from './action-type.js'// action操作通常引用action-type
  /*
  	包含所有action  creator（action的工厂函数）
  */
  ```

- **reducers.js**

  ```js
  // reducers.js
  import {type1,type2} from './action-type.js'// reducers也通常引用action-type与action对应
  import { combineReducers } from 'redux'
  /*
  	包含多个reducer函数（根据旧的state和action返回一个新的state）
  */
  // 如果要合并
  export default combineReducers({
  	reducers1,reducers2
  })
  // 该文件的内容通常会被引入到store.js中
  ```

- **store.js**

  ```js
  // store.js
  /*
  	redux最核心的管理对象
  */
  import {createStore, applyMiddleware} from 'redux'
  import {composeWithDevTools} from 'redux-devtools-extension' // redux调试工具
  import {reducers1} from './reducers'// 这个只是有一个reducers的情况，并且还没有合并reducers
  /*
  	如果有多个reducers建议使用combinReducers进行合并
  	import reducers from './reducers'引入
  */
  import thunk from 'redux-thunk'
  
  export default createStore(
  	reducer1,// reducers
      composeWithDevTools(applyMiddleware(thunk))
  )
  ```

**在实际中的应用**

```jsx
// index.js，入口文件
import React from 'react'
import ReactDOM from 'react-dom'
import {Provider} from 'react-redux'
import store from './redux/store'
import App from './containers/App' //App组件变为了容器组件
import * as serviceWorker from './serviceWorker'
import './index.css'

ReactDOM.render(<App />, document.getElementById('root'))

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister()
```

```jsx
// index.js，入口文件
import React, {Component} from 'react'
import PropTypes from 'prop-types'
import {connect} from 'react-redux'
import store from './redux/store'

import {actions1,actions2} from '../../redux/actions'


Class App extends Component {
    static propTypes = {
        data:PropTypes.array.isRequired, // 举个例子，data为一个数组
        action1:PropTypes.func.isRequired,// 一般映射到的组件类的方法都与actions类的一致
        action1:PropTypes.func.isRequired
    }
}

export default connect(
	state=>({data:state}),// state就是一个data的数组，如果使用了combineReducers使用state.data映射
    {actions1,actions2}// 组件的属性与action的名称一致
)(App) 
```


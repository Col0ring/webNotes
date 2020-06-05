# dva

## 1.modals

modals 是 dva 最核心的概念，通过 modals，我们才能实现 dva 的数据流向的概念。

**modals 共有 5 个属性：**

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


# dva 插件

## 1.dva-loading

dva-loading 是 dva 中非常有名的一个插件，该插件可以自动监听 dav 中的异步加载状态，即可以用来监听全局的加载状态，也可以对局部的状态进行更新。

### 1.1 使用配置

- 在 dva 项目中：

   在 dva 项目的入口文件引入 dva-loading：

  ```jsx
  import createLoading from 'dva-loading';
  const app = dva();
  app.use(createLoading());
  ```

  配置完成后，在所有连接了的 Route Component 中就都会有一个 loading 对象：

  ```jsx
  export default connect(({ app, loading }) => ({ app, loading }))(App);
  ```
  
  loading 对象：
  
  ```js
  {
    global: false,
    models: {app: false},
    effects: {app: false}
  }
  ```
  
  该 loading 对象对应的是三种不同的 loading 状态，第一个是全局的 loading，只有在 dva 内有新的异步请求产生，该状态就会变为 true，同理第二个是包含某个 model 内部全部的异步请求的 loading 状态，而第三个如 `loading.effects['user/query']` 为监听单一异步请求状态，该监听的状态对应的 effect 同使用 dispatch 时分发的 effect 。
  
- 在 umi 项目中：具体用法同上，只是不需要配置，umi 内部已经集成了 dva 和 dva-loading 插件
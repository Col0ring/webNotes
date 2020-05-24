# hash路由与history路由

## 1.网页url的组成部分

```js
// http://127.0.0.1:8080/test.html?a=123&b=456#/a/b
console.log(location.protocol) // http:
console.log(location.hostname) // 127.0.0.1
console.log(location.host) // 127.0.0.1:8080
console.log(location.port) // 8080
console.log(location.pathname) // /test.html
console.log(location.search) // ?a=123&b=456
console.log(location.hash) // #/a/b
```



## 2.hash路由

**hash路由的特点：**

- hash变化会触发网页跳转，即浏览器的前进、后退
- hash变化不会刷新页面，这是SPA必需的特点
- hash永远不会提交到server端，只会在前端产生在前端消除

**hash变化包含：**

- JS修改url
- 手动修改url的hash
- 浏览器的前进、后退

```js
// 监听hash变化
window.onhashchange = e => {
    console.log(e.oldURL)
    console.log(e.newURL)
    console.log(location.hash)
}
// 页面加载完成打印hash
document.addEventListener('DOMContentLoaded', () => {
    console.log(location.hash)
})
// js控制hash变化
document.getElementById('btn').onclick = () => {
 	location.href = '#/user'   
}
```



## 3.history路由

**history路由的特点：**

- 使用url规范的路由，但是跳转时不会刷新页面
- 使用H5提供的新api类似history.pushState与window.onpopstate结合实现路由跳转
- 需要server端重定向进行支持

```js
// 页面加载完成打印path
document.addEventListener('DOMContentLoaded', () => {
    console.log(location.pathname)
})
// js控制hash变化
document.getElementById('btn').onclick = () => {
 	const state = {name:'page'}
    /*
    pushState方法接受三个参数，依次为：state：一个与指定网址相关的状态对象，popstate事件触发时，该对象会传入回调函数。如果不需要这个对象，此处可以填null。title：新页面的标题，但是所有浏览器目前都忽略这个值，因此这里可以填null。url：新的网址，必须与当前页面处在同一个域，浏览器的地址栏将显示这个网址。
    */
    history.pushState(state, '', 'page')
}

// 监听浏览器的前进、后退
window.onpopstate = e => {
// state为pushState中传入的state，只要使用pushState，浏览器就会有记录，所以即使使用浏览器的前进后退也会有值
    console.log(e.state, location.pathname)
}
```


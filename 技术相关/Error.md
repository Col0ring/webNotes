# Error

## 1.错误类型

**所有的错误类型都是来源于同一个父类型`Error`，同时在这个父类型下有4个子错误类型`ReferenceError`、`TypeError`、`RangeError`、`SyntaxError`**

- Error：所有错误的父类型

- ReferenceError：引用的变量不存在

  ```js
  console.log(a) // a并没有被定义
  ```

- TypeError：数据类型不正确

  ```js
  let a = null
  console.log(a.a) // 数据类型错误，null类型没有a属性
  ```

- RangeError：数据值不在其所允许的范围内

  ```js
  // 没有跳出判断的递归
  function fn() {
      fn() // 超过最大调用栈堆大小
  }
  fn()
  ```

- SyntaxError：语法错误

  ```js
  let a = """" // 不期待的字符串，应该是双引号包含单引号
  ```

  

## 2.错误处理

**主要有两种错误处理的方式：**

- 捕获错误：`try...catch`

  ```js
  try {
      console.log(a) // a未定义
  } catch(err) {
      console.log('出错了：',err.messsage) // 打印错误信息
  }
  ```

- 抛出错误`throw new Error()`

  ```js
  let a = Date.now()
  if(a%2 === 0){
      throw new Error('a为偶数，出错啦')
  }
  ```

  **注：**如果使用`try...catch`来捕获错误一样不会抛出错误



## 3.错误对象

**一个错误对象有两个属性：**

- message：错误相关信息
- stack：函数调用栈记录信息

```js
try {
    console.log(a) // a未定义
} catch(err) {
    console.log('出错了：',err.messsage) // 打印错误信息
    console.log('出错了：',err.stack) // 打印错误栈记录信息
}
```


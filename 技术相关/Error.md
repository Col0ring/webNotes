# Error

> 本文只探讨 JS 中标准的错误类型和属性方法。

`Error`是 JS 的标准内置对象之一。通过`Error`的构造器可以创建一个错误对象。当运行时错误产生时，`Error`的实例对象会被抛出，如果没有在外层对抛出的错误对象进行捕获则会造成程序的终止。同时`Error`对象允许用户基于其进行扩展创建用户自定义对象。

## 1.错误类型

**所有的错误类型都是来源于同一个父类型`Error`，同时在这个父类型下有6个子错误类型：`EvalError`、`ReferenceError`、`TypeError`、`RangeError`、`SyntaxError`、`URIError`**。

- Error：所有错误的父类型。

- EvalError：与`eval()`函数有关的错误，但是需要注意的是，因为`eval()`函数最终还是会将字符串编译为 JS，所以会被抛出其他类型的错误，此错误不再会被 JS 抛出。

- ReferenceError：引用的变量不存在。

  ```js
  console.log(a) // a并没有被定义
  ```

- TypeError：数据类型不正确。

  ```js
  let a = null
  console.log(a.a) // 数据类型错误，null类型没有a属性
  ```

- RangeError：数据值不在其所允许的范围内。

  ```js
  // 没有跳出判断的递归
  function fn() {
      fn() // 超过最大调用栈堆大小
  }
  fn()
  ```

- SyntaxError：语法错误。

  ```js
  let a = """" // 不期待的字符串，应该是双引号包含单引号
  ```

- URIError：使用全局 URI 处理函数产生的错误。

  全局的 URI 处理函数：

  - `decodeURI()`
  - `decodeURIComponent()`
  - `encodeURI()`
  - `encodeURIComponent()`

  ```js
  decodeURI('%')
  decodeURIComponent('%');
encodeURI('\uD800')
  encodeURIComponent('\uD800')
  ```
  



## 2.创建错误

### 2.1 被动创建

一般来说被动创建的错误都是代码在运行时出现 bug 造成的，**此类错误都是子类错误并且会被立刻抛出**，可能的错误触发条件在上面已经列举出，在这里就不再多说了。



### 2.1 主动创建

除了代码报错出现错误外，还可以手动创建错误（只创建错误不抛出并不会造成程序的终止，当然一般手动创建的错误都会选择抛出，不过这种都会使用捕获错误来达到简化操作的目的，后面再进行细讲）。主要的方式是直接通过错位类进行创建：

```js
// 错误类可以传入一个 message 参数，用于显示自定义的错误描述信息
const e = new Error('Error')
const e1 = new EvalError('EvalError')
const e2 = new RangeError('RangeError')
const e3 = new ReferenceError('ReferenceError')
const e4 = new SyntaxError('SyntaxError')
const e5 = new TypeError('TypeError')
const e6 = new URIError('URIError')
```

与一般的类不同，错误类的实例除了通过构造函数的形式使用`new`关键字创建外，还可以直接将错误类作为函数使用来创建，并且这两者的结果是一致的。

```js
const e = Error('Error')
const e1 = EvalError('EvalError')
const e2 = RangeError('RangeError')
const e3 = ReferenceError('ReferenceError')
const e4 = SyntaxError('SyntaxError')
const e5 = TypeError('TypeError')
const e6 = URIError('URIError')
```



## 3.错误处理

**主要有两种错误处理的方式：**

- 捕获错误：`try...catch`。

  ```js
  try {
      console.log(a) // a 未定义
  } catch(err) {
      console.log('出错了：',err.message) // 打印错误信息
  }
  ```

- 抛出错误：使用`throw`关键词， `throw new Error()`。

  ```js
  let a = Date.now()
  if(a % 2 === 0){
      throw new Error('a为偶数，出错啦')
  }
  ```
  **注意：**错误本身并不会造成程序的终止，真正使得程序终止的是`throw`关键词，只是被动创建的错误都会默认使用`throw`将其抛出。

  ```js
  try {
    throw 1 // 即使不抛出错误类型也会被捕获
  } catch (error) {
    console.log(error) // 1
  }
  ```




## 4.错误对象

**一个错误对象有三个属性：**

- name：error 类型的名称，初始值为`"Error"`，可以通过后续改变实例的值进行修改。

- message：错误相关信息，默认为空字符串，手动创建的时候可以通过传入错误对象的第一个参数赋值。

- stack：函数调用栈记录信息，默认会**对抛出的错误位置进行函数式的追踪**（无论这个函数以怎样的顺序被调用，处于什么模式，来自于哪一行或者哪个文件，有着什么样的参数。这个栈产生于最近一次调用最早的那次调用，返回原始的全局作用域调用）。**虽然该值可以被修改，但是不建议对其进行改变。**

  **注意：**该属性是非标准的，在不同浏览器下的信息可能会不同，并且在默认的情况下不同的浏览器会在不同时期设置这个值。

```js
try {
    console.log(a) // a未定义
} catch(err) {
    console.log('出错了：',err.name) // 打印错误名：ReferenceError
    console.log('出错了：',err.message) // 打印错误信息：a is not defined
    console.log('出错了：',err.stack) // 打印错误栈记录信息：ReferenceError: a is not defined...
}
```

```js
try {
  const e = new Error('错误信息')
  e.name = '错误'
  // 修改错误栈信息
  e.stack = '错误栈'
  throw e
} catch (error) {
  console.log(error.name) // 错误
  console.log(error.message) // 错误信息
  // 没有修改 e.stack
 	// console.log(error.stack) // 错误: 错误信息...
  // 修改 e.stack
  console.log(error.stack) // 错误栈
}
```

### 4.1 关于错误对象的 toString 方法

错误实例的`toString`方法自身做了相应修改，当我们调用实例的`toString`方法时，会返回以`error.name: error.message`组成的字符串（该字符串也是默认的错误栈最开始的错误信息）：

```js
try {
  const e = new Error('错误信息')
  e.name = '错误'
  throw e
} catch (error) {
  console.log(error.toString()) // 错误: 错误信息
}

```



## 5.错误捕获

### 5.1 try...catch

在前面已经初步说了
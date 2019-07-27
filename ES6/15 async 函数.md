# async 函数

## 含义

ES2017 标准引入了 async 函数，使得异步操作变得更加方便。

async 函数是 Generator 函数的语法糖。

> 什么是语法糖？
>
> 意指那些没有给计算机语言添加新功能，而只是对人类来说更“甜蜜”的语法。语法糖往往给程序员提供了更实用的编码方式，有益于更好的编码风格，更易读。不过其并没有给语言添加什么新东西。
>
> 反向还有语法盐：
>
> 主要目的是通过反人类的语法，让你更痛苦的写代码，虽然同样能达到避免代码书写错误的效果，但是编程效率很低，毕竟提高了语法学习门槛，让人齁到忧伤。。。

`async`函数使用时就是将 Generator 函数的星号（`*`）替换成`async`，将`yield`替换成`await`，仅此而已。

`async`函数对 Generator 函数的区别：

（1）内置执行器。

Generator 函数的执行必须靠执行器，而`async`函数自带执行器。也就是说，`async`函数的执行，与普通函数一模一样，只要一行。

（2）更好的语义。

`async`和`await`，比起星号和`yield`，语义更清楚了。`async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。

（3）正常情况下，`await`命令后面是一个 Promise 对象。如果不是，会被转成一个立即`resolve`的 Promise 对象。

（4）返回值是 Promise。

`async`函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用`then`方法指定下一步的操作。

进一步说，`async`函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而`await`命令就是内部`then`命令的语法糖。

## 错误处理

如果`await`后面的异步操作出错，那么等同于`async`函数返回的 Promise 对象被`reject`。

```js
async function f() {
  await new Promise(function (resolve, reject) {
    throw new Error('出错了');
  });
}

f()
.then(v => console.log(v))
.catch(e => console.log(e))
// Error：出错了

```

上面代码中，`async`函数`f`执行后，`await`后面的 Promise 对象会抛出一个错误对象，导致`catch`方法的回调函数被调用，它的参数就是抛出的错误对象。具体的执行机制，可以参考后文的“async 函数的实现原理”。

防止出错的方法，也是将其放在`try...catch`代码块之中。

```js
async function f() {
  try {
    await new Promise(function (resolve, reject) {
      throw new Error('出错了');
    });
  } catch(e) {
  }
  return await('hello world');
}
```

如果有多个`await`命令，可以统一放在`try...catch`结构中。

```js
async function main() {
  try {
    const val1 = await firstStep();
    const val2 = await secondStep(val1);
    const val3 = await thirdStep(val1, val2);

    console.log('Final: ', val3);
  }
  catch (err) {
    console.error(err);
  }
}
```

## 应用

```js
var fn = function (time) {
  console.log("开始处理异步");
  setTimeout(function () {
    console.log(time);
    console.log("异步处理完成");
    iter.next();
  }, time);

};

function* g(){
  console.log("start");
  yield fn(3000)
  yield fn(500)
  yield fn(1000)
  console.log("end");
}

let iter = g();
iter.next();
```

下面是async函数的写法

```js
var fn = function (time) {
  return new Promise(function (resolve, reject) {
    console.log("开始处理异步");
    setTimeout(function () {
      resolve();
      console.log(time);
      console.log("异步处理完成");
    }, time);
  })
};

var start = async function () {
  // 在这里使用起来就像同步代码那样直观
  console.log('start');
  await fn(3000);
  await fn(500);
  await fn(1000);
  console.log('end');
};

start();
```
























## let命令

**基本用法**

跟使用es5的var一样

> BUT

**不存在变量提升**

`var`会存在变量提升现象，

`let`和`const`则不会有这种情况

**暂时性死区  简称 TDZ** 

只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

```js
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

ES6 明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

**不允许重复申明**

`let`不允许在相同作用域内，重复声明同一个变量。

```js
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}
```

因此，不能在函数内部重新声明参数。

```js
function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}
```

### 块级作用域

**为什么需要块作用域**

ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

第一种场景，内层变量可能会覆盖外层变量。

```js
var tmp = 123;

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
```

第二种场景，用来计数的循环变量泄露为全局变量。

```js
var s = 'hello';
for (var i = 0; i < s.length; i++) {  
  console.log(s[i]);
}
console.log(i); // 5
```

**ES6 的块级作用域** 

```js
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```

ES6 允许块级作用域的任意嵌套。

块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。

```js
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```

**块级作用域不返回值，除非`t`是全局变量。**

## const

`const`声明一个只读的常量。

**`const`除了以下两点与`let`不同外，其他特性均与`let`相同:**

1. `const`一旦声明变量，就必须立即初始化，不能留到以后赋值。
2. 一旦声明，常量的值就不能改变。

**本质**

const限定的是赋值行为。

也就是说

```js
const a = 1;
a = 2;//报错

const arr = [];
arr.push(1) //[1] 
//在声明引用型数据为常量时，const保存的是变量的指针，只要保证指针不变就不会保存。下面的行为就会报错

arr = [];//报错 因为是赋值行为。变量arr保存的指针改变了。
```

## 顶层对象属性与全局变量

顶层对象，在浏览器环境指的是`window`对象，在 Node 指的是`global`对象。ES5 之中，顶层对象的属性与全局变量是等价的。

```js
window.a = 1;
a // 1

a = 2;
window.a // 2
```

顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。

**为了解决这个问题，es6引入的`let` 、`const`和`class`声明的全局变量不再属于顶层对象的属性。**

而同时为了向下兼容，var和function声明的变量依然属于全局对象的属性

```js
var a = 1;
window.a // 1

let b = 1;
window.b // undefined
```


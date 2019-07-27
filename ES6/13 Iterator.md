# Iterator

## 概念

迭代器是一种接口、是一种机制。

为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

Iterator 的作用有三个：

1. 为各种数据结构，提供一个统一的、简便的访问接口；
2. 使得数据结构的成员能够按某种次序排列；
3. 主要供`for...of`消费。

**Iterator本质上，就是一个指针对象。**

过程是这样的：

（1）创建一个指针对象，指向当前数据结构的起始位置。

（2）第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员。

（3）第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员。

（4）不断调用指针对象的`next`方法，直到它指向数据结构的结束位置。



**普通函数实现Iterator**

```js
function myIter(obj){
  let i = 0;
  return {
    next(){
      let done = (i>=obj.length);
      let value = !done ? obj[i++] : undefined;
      return {
        value,
        done,
      }
    }
  }
}
```

原生具备 Iterator 接口的数据结构如下。

* Array
* Map
* Set
* String
* 函数的 arguments 对象
* NodeList 对象

下面的例子是数组的`Symbol.iterator`属性。

```js
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();

iter.next() // { value: 'a', done: false }
iter.next() // { value: 'b', done: false }
iter.next() // { value: 'c', done: false }
iter.next() // { value: undefined, done: true }
```

下面是另一个类似数组的对象调用数组的`Symbol.iterator`方法的例子。

```js
let iterable = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3,
  [Symbol.iterator]: Array.prototype[Symbol.iterator]
};
for (let item of iterable) {
  console.log(item); // 'a', 'b', 'c'
}
```

注意，普通对象部署数组的`Symbol.iterator`方法，并无效果。

```js
let iterable = {
  a: 'a',
  b: 'b',
  c: 'c',
  length: 3,
  [Symbol.iterator]: Array.prototype[Symbol.iterator]
};
for (let item of iterable) {
  console.log(item); // undefined, undefined, undefined
}
```

字符串是一个类似数组的对象，也原生具有 Iterator 接口。

```js
var someString = "hi";
typeof someString[Symbol.iterator]
// "function"

var iterator = someString[Symbol.iterator]();

iterator.next()  // { value: "h", done: false }
iterator.next()  // { value: "i", done: false }
iterator.next()  // { value: undefined, done: true }
```


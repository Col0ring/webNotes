# Symbol

## 为啥需要Symbol

ES5里面对象的属性名都是字符串，如果你需要使用一个别人提供的对象，你对这个对象有哪些属性也不是很清楚，但又想为这个对象新增一些属性，那么你新增的属性名就很可能和原来的属性名发送冲突，显然我们是不希望这种情况发生的。所以，我们需要确保每个属性名都是独一无二的，这样就可以防止属性名的冲突了。因此，ES6里就引入了Symbol，用它来产生一个独一无二的值。

## Symbol是什么

Symbol实际上是ES6引入的一种原始数据类型，除了Symbol，JavaScript还有其他5种数据类型，分别是Undefined、Null、Boolean、String、Number、对象，这5种数据类型都是ES5中就有的。

## 怎么生成一个Symbol类型的值

既然我们已经知道了Symbol是一种原始的数据类型，那么怎么生成这种数据类型的值呢？Symbol值是通过Symbol函数生成的，如下：

```js
let s = Symbol();
console.log(s);  // Symbol()
typeof s;  // "symbol"
```

## Symbol函数前不能用new

Symbol函数不是一个构造函数，前面不能用new操作符。所以Symbol类型的值也不是一个对象，不能添加任何属性，它只是一个类似于字符型的数据类型。如果强行在Symbol函数前加上new操作符，会报错，如下：

```js
let s = new Symbol();
// Uncaught TypeError: Symbol is not a constructor(…)
```

## Symbol函数的参数

### 字符串作为参数

用上面的方法生成的Symbol值不好进行区分，Symbol函数还可以接受一个字符串参数，来对产生的Symbol值进行描述，方便我们区分不同的Symbol值。

```js
let s1 = Symbol('s1');
let s2 = Symbol('s2');
console.log(s1);  // Symbol(s1)
console.log(s2);  // Symbol(s2)
s1 === s2;  //  false
let s3 = Symbol('s2');
s2 === s3;  //  false
```

1. 给Symbol函数加了参数之后，控制台输出的时候可以区分到底是哪一个值；
2. Symbol函数的参数只是对当前Symbol值的描述，因此相同参数的Symbol函数返回值是不相等的；

### 对象作为参数

如果Symbol函数的参数是一个对象，就会调用该对象的toString方法，将其转化为一个字符串，然后才生成一个Symbol值。所以，说到底，Symbol函数的参数只能是字符串。

### Symbol值不可以进行运算

既然Symbol是一种数据类型，那我们一定想知道Symbol值是否能进行运算。告诉你，Symbol值是不能进行运算的,不仅不能和Symbol值进行运算，也不能和其他类型的值进行运算，否则会报错。
Symbol值可以显式转化为字符串和布尔值，但是不能转为数值。

```js
var mysym1 = Symbol('my symbol');

mysym1.toString() //  'Symbol('my symbol')'

String(mysym1)  //  'Symbol('my symbol')'

var mysym2 = Symbol();

Boolean(mysym2);  // true

Number(mysym2)  // TypeError: Cannot convert a Symbol value to a number(…)
```

### Symbol作属性名

Symbol就是为对象的属性名而生，那么Symbol值怎么作为对象的属性名呢？有下面几种写法：

```js
let a = {};
let s4 = Symbol();
// 第一种写法
a[s4] = 'mySymbol';
// 第二种写法
a = {
    [s4]: 'mySymbol'
}
// 第三种写法
Object.defineProperty(a, s4, {value: 'mySymbol'});
a.s4;  //  undefined
a.s4 = 'mySymbol';
a[s4]  //  undefined
a['s4']  // 'mySymbol'
```

1. 使用对象的Symbol值作为属性名时，获取相应的属性值不能用点运算符；
2. 如果用点运算符来给对象的属性赋Symbol类型的值，实际上属性名会变成一个字符串，而不是一个Symbol值；
3. 在对象内部，使用Symbol值定义属性时，Symbol值必须放在方括号之中，否则只是一个字符串。

## Symbol值作为属性名的遍历

使用for...in和for...of都无法遍历到Symbol值的属性，Symbol值作为对象的属性名，也无法通过Object.keys()、Object.getOwnPropertyNames()来获取了。但是，不同担心，这种平常的需求肯定是会有解决办法的。我们可以使用Object.getOwnPropertySymbols()方法获取一个对象上的Symbol属性名。也可以使用Reflect.ownKeys()返回所有类型的属性名，包括常规属性名和 Symbol属性名。

```js
let s5 = Symbol('s5');
let s6 = Symbol('s6');
let a = {
    [s5]: 's5',
    [s6]: 's6'
}
Object.getOwnPropertySymbols(a);   // [Symbol(s5), Symbol(s6)]
a.hello = 'hello';
Reflect.ownKeys(a);  //  ["hello", Symbol(s5), Symbol(s6)]

```

利用Symbol值作为对象属性的名称时，不会被常规方法遍历到这一特性，可以为对象定义一些非私有的但是又希望只有内部可用的方法。

## Symbol.for()和Symbol.keyFor()

Symbol.for()函数也可以用来生成Symbol值，但该函数有一个特殊的用处，就是可以重复使用一个Symbol值。

```js
let s1 = Symbol.for("s11");
let s2 = Symbol.for("s22");

console.log(s1===s2)//false

let s3 = Symbol("s33");
let s4 = Symbol("s33");

console.log(s3===s4)//false

console.log(Symbol.keyFor(s3))//undefined
console.log(Symbol.keyFor(s2))//"s22"
console.log(Symbol.keyFor(s1))//"s11"
```

**Symbol.for()**函数要接受一个字符串作为参数，先搜索有没有以该参数作为名称的Symbol值，如果有，就直接返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值。


**Symbol.keyFor()**函数是用来查找一个Symbol值的登记信息的，Symbol()写法没有登记机制，所以返回undefined；而Symbol.for()函数会将生成的Symbol值登记在全局环境中，所以Symbol.keyFor()函数可以查找到用Symbol.for()函数生成的Symbol值。

## 内置Symbol值


ES6提供了11个内置的Symbol值，分别是Symbol.hasInstance 、Symbol.isConcatSpreadable 、Symbol.species 、Symbol.match 、Symbol.replace 、Symbol.search 、Symbol.split 、Symbol.iterator 、Symbol.toPrimitive 、Symbol.toStringTag 、Symbol.unscopables 等。

有兴趣的可以自行了解：[ 地址 ](http://es6.ruanyifeng.com/#docs/symbol)
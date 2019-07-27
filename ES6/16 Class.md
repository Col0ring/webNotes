# Class 

## 用法

**class跟let、const一样：不存在变量提升、不能重复声明...**

es5面向对象写法跟传统的面向对象语言（比如 C++ 和 Java）差异很大，很容易让新学习这门语言的程序员感到困惑。

ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过`class`关键字，可以定义类。

ES6 的`class`可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

```js
//es5
function Fn(x, y) {
  this.x = x;
  this.y = y;
}

Fn.prototype.add = function () {
  return this.x + this.y;
};
//等价于
//es6
class Fn{
  constructor(x,y){
    this.x = x;
    this.y = y;
  }
  
  add(){
    return this.x + this.y;
  }
}

var F = new Fn(1, 2);
console.log(F.add()) //3
```

构造函数的`prototype`属性，在 ES6 的“类”上面继续存在。事实上，类的所有方法都定义在类的`prototype`属性上面。

```js
class Fn {
  constructor() {
    // ...
  }

  add() {
    // ...
  }

  sub() {
    // ...
  }
}

// 等同于

Fn.prototype = {
  constructor() {},
  add() {},
  sub() {},
};
```

类的内部所有定义的方法，都是不可枚举的（non-enumerable），这与es5不同。

```js
//es5
var Fn = function (x, y) {
  // ...
};

Point.prototype.add = function() {
  // ...
};

Object.keys(Fn.prototype)
// ["toString"]
Object.getOwnPropertyNames(Fn.prototype)
// ["constructor","add"]

//es6
class Fn {
  constructor(x, y) {
    // ...
  }

  add() {
    // ...
  }
}

Object.keys(Fn.prototype)
// []
Object.getOwnPropertyNames(Fn.prototype)
// ["constructor","add"]
```

## 严格模式

类和模块的内部，默认就是严格模式，所以不需要使用`use strict`指定运行模式。只要你的代码写在类或模块之中，就只有严格模式可用。

考虑到未来所有的代码，其实都是运行在模块之中，所以 ES6 实际上把整个语言升级到了严格模式。

## constructor

`onstructor`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。

```js
class Fn {
}

// 等同于
class Fn {
  constructor() {}
}
```

`constructor`方法默认返回实例对象（即`this`），完全可以指定返回另外一个对象。

```js
class Foo {
  constructor() {
    return Object.create(null);
  }
}

new Foo() instanceof Foo
// false
//constructor函数返回一个全新的对象，结果导致实例对象不是Foo类的实例。
```

## 类必须使用new调用

类必须使用`new`调用，否则会报错。这是它跟普通构造函数的一个主要区别，后者不用`new`也可以执行。

```js
class Foo {
  constructor() {
    return Object.create(null);
  }
}

Foo()
// TypeError: Class constructor Foo cannot be invoked without 'new'
```

## Class 表达式

与函数一样，类也可以使用表达式的形式定义。

```js
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
```

上面代码使用表达式定义了一个类。需要注意的是，这个类的名字是`MyClass`而不是`Me`，`Me`只在 Class 的内部代码可用，指代当前类。

```js
let inst = new MyClass();
inst.getClassName() // Me
Me.name // ReferenceError: Me is not defined
```

如果类的内部没用到的话，可以省略`Me`，也就是可以写成下面的形式。

```js
const MyClass = class { /* ... */ };

```

采用 Class 表达式，可以写出立即执行的 Class。

```js
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName(); // "张三"

```

上面代码中，`person`是一个立即执行的类的实例。

## 私有方法和私有属性

私有方法/私有属性是常见需求，但 ES6 不提供，只能通过变通方法模拟实现。（以后会实现）

通常是在命名上加以区别。

```js
class Fn {

  // 公有方法
  foo () {
    //....
  }

  // 假装是私有方法（其实外部还是可以访问）
  _bar() {
    //....
  }
}
```

## 原型的属性

class定义类时，只能在constructor里定义属性，在其他位置会报错。

如果需要在原型上定义方法可以使用：

1. Fn.prototype.prop = value;
2. Object.getPrototypeOf()获取原型，再来扩展
3. Object.assign(Fn.prototype,{在这里面写扩展的属性或者方法})

## Class 的静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。

如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

ES6 明确规定，Class 内部只有静态方法，没有静态属性。

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function

//静态属性只能手动设置
class Foo {
}

Foo.prop = 1;
Foo.prop // 1
```

## get、set

存值函数和取值函数，不多说，看代码

```js
class Fn{
	constructor(){
		this.arr = []
	}
	get bar(){
		return this.arr;
	}
	set bar(value){
		this.arr.push(value)
	}
}


let obj = new Fn();

obj.menu = 1;
obj.menu = 2;

console.log(obj.menu)//[1,2]
console.log(obj.arr)//[1,2]
```

## 继承

### 用法

```js
class Fn {
}

class Fn2 extends Fn {
}
```

### 注意

1. 子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。这是因为子类没有自己的`this`对象，而是继承父类的`this`对象，然后对其进行加工。如果不调用`super`方法，子类就得不到`this`对象。

```js
class Point { /* ... */ }

class ColorPoint extends Point {
  constructor() {
    super()//必须调用
  }
}

let cp = new ColorPoint(); // ReferenceError
```

2. 父类的静态方法也会被继承。

> 嗯！就是这么让人绝望

## Object.getPrototypeOf()

`Object.getPrototypeOf`方法可以用来从子类上获取父类。

```js
Object.getPrototypeOf(Fn2) === Fn
// true
```

因此，可以使用这个方法判断，一个类是否继承了另一个类。

## super 关键字

`super`这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

第一种情况，`super`作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次`super`函数。

> 作为函数时，`super()`只能用在子类的构造函数之中，用在其他地方就会报错。

```js
class A {}

class B extends A {
  constructor() {
    super();
  }
}

```

上面代码中，子类`B`的构造函数之中的`super()`，代表调用父类的构造函数。这是必须的，否则 JavaScript 引擎会报错。

注意，`super`虽然代表了父类`A`的构造函数，但是返回的是子类`B`的实例，即`super`内部的`this`指的是`B`，因此`super()`在这里相当于`A.prototype.constructor.call(this)`。

第二种情况，`super`作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

```js
class A {
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2
  }
}

let b = new B();
```

上面代码中，子类`B`当中的`super.p()`，就是将`super`当作一个对象使用。这时，`super`在普通方法之中，指向`A.prototype`，所以`super.p()`就相当于`A.prototype.p()`。

由于`this`指向子类，所以如果通过`super`对某个属性赋值，这时`super`就是`this`，赋值的属性会变成子类实例的属性。

```js
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}

let b = new B();

```

上面代码中，`super.x`赋值为`3`，这时等同于对`this.x`赋值为`3`。而当读取`super.x`的时候，读的是`A.prototype.x`，所以返回`undefined`。
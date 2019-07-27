# JS编码风格指南

> 内容引用于《编写可维护的JavaScript》附录A

## 1.缩进

每一行的层级由4个空格组成，避免使用制表符缩进(就是tab键缩进)

**注:**现在大多数编译器的tab键默认也是用的空格进行缩进,但是还是有编译器的tab键是制表符



## 2.行的长度

每行长度不超过80个字符，如果超过，应当在一个运算符（逗号，加号等）后换行。下一行应当增加两级缩进（8个空格）

```js
//好的写法
doSomething(arguments1, arguments2, arguments3, arguments4,
        arguments5);
```



## 3.运算符的行间距

二元运算符前后必须使用一个空格来保持表达式的整洁。操作符包括赋值运算符和逻辑运算符

```js
var found = (values[0] === item);

if(found && (count > 10)){
    
}

for(var i = 0; i < count; i++){
    
}
```



## 4.括号间距

当使用括号时，紧接左括号之后和紧接右括号之前不应该有空格



## 5.对象直接量(字面量)

**对象直接量应当使用如下格式**

- 起始左花括号应当同表达式保持一行
- 每个属性的名值对应当保持一个缩进，第一个属性应当在左花括号后另起一行
- 每个属性的名值对应当使用不含引号的属性名，其后紧跟一个冒号（之前不含空格），而后是值
- 倘若属性值是函数类型，函数体应当在属性名之下另起一行，而且其前后就均保留一个空格
- 一组相关的属性前后可以插入空行以提升代码的可读性
- 结束的右花括号应当独占一行

```js
var obj = {
  
    key1: value1,
    
    key2: value2,
  
    func: function() {
        
    },
  
    key3: value3
};
```

**当对象字面量作为函数参数时，如果值是变量，起始花括号应当同函数名在同一行，所有起始花括号之前列出的规则同样适用**

```js
//好的写法
doSomething({
    key1: value1,
    key2: value2
})
//不好的写法
doSomething({key1: value1,key2: value2})
```



## 6.注释

**频繁地使用注释有助于他人理解你的代码，如下情况应当使用注释**

- 代码晦涩难懂
- 可能被误认为错误的代码
- 必要但并不明显的针对特定浏览器的代码
- 对于对象、方法或者属性，生成文档是有必要的（使用恰当的文档注释）



### 6.1 单行注释

**单行注释应当用来说明一行或者一组相关的代码。单行注释可能有如下三种使用方式**

- 独占一行的注释，用来解释下一行代码
- 在代码行的尾部注释，用来解释它之前的代码
- 多行，用来注释掉一个代码块

```js
//好的写法
if(condition){
  
  //如果代码执行到这里，则表明通过了所有的安全检查
  allowed()
}

//不好的写法:注释前没有空行
if(condition){
  //如果代码执行到这里，则表明通过了所有的安全检查
  allowed()
}
//不好的写法:错误的缩进
if(condition){

//如果代码执行到这里，则表明通过了所有的安全检查
  allowed()
}
```

**对于行尾的情况，代码和注释间至少一个空格**

```js
//好的写法
var a = 10; // 这里是注释
//不好的写法
var a = 10;// 这里是注释
```



### 6.2 多行注释

**多行注释应当在代码需要更多的文字去解释的时候使用。每个多行注释应当有如下三行**

- 首行仅仅包含/*注释开始，该行不能有其他文件*
- 接下来的行开头并保持左对齐。这些行可以有文字描述
- 最后一行以*/结束并与先前行对齐。也不应再有其他文字

**多行注释应当保持同它描述的代码一样的缩进，后续每个*后加一个空格**

```js
//好的写法
if (condition) {
  
    /*
    * 这里是说明
    * 这里是说明
    * 这里是说明
    */
    allowed();
}
```



## 7.变量声明

使用var声明变量时,所有变量在使用前都应当事先定义

```js
//好的写法
var a = 1,
    b = 2,
    c = 3,
    d = 4;
//不好的写法
var a = 1;
var b = 2;
var c = 3;
var d = 4;
//不好的写法
var a = 1,
  b = 2,
  c = 3,
  d = 4;
```

**未初始化值的变量在后面**

```js
//好的写法
var a = 1,
    b,
    c;
// 不好的写法
var b,
    c,
    a = 1;
```



## 8 .函数声明

函数应当在使用前提前定义，不是作为对象的方法的函数，应当使用函数声明的格式（function声明）

```js
// 好的写法
function fn(arg1, arg2) {
   
}
// 不好的写法：不恰当的空格
function fn (arg1, arg2){
  
}
// 不好的写法：花括号位置不对
function fn(arg1, arg2) 
{
   
}
```

**函数内的函数声明，应当在var等声明符声明后，立即定义**

```js
//好的写法
function outer() {
  
    var count = 10,
        name = 'fengyu',
        age = 18,
        empty;
    
    function inner() {
        
    }
}
```

**匿名函数的自执行**

```js
// 推荐使用
(function (){}());

// 其他的都不推荐。
```



## 9.命名

变量和函数命名时要小心。命名应仅限于数字字母字符。某些情况可以使用下划线。最好不好用美元符号('$')和反斜杠('')

变量命名采用驼峰式，首字母小写，每个单词首字母大写，变量名的第一个单词应当是一个名词（而非动词）以避免和函数混淆，不要再变量名中使用下划线。

```js
// 好的写法
var accountNumber = "1234567"；
// 不好的写法：大写字母开头
var AccountNumber = "1234567"；
// 不好的写法：动词开头
var getAccountNumber = "1234567"；
//不好的写法：使用下划线
var account_Number = "1234567";
```

**函数名也使用驼峰，第一个单词应当是动词（而非名词），也不要使用下划线**

```js
// 好的写法
function doSomething() {
  
}
// 不好的写法:首字母大写
function DoSomething() {
  
}
// 不好的写法:名词
function car() {
  
}
// 不好的写法：下划线
function do_something() {
  
}
```

**构造函数才能首字母大写，名称应当非动词开头**

```js
// 好的写法
function MyObject() {
  
}
// 不好的写法：首字母小写
function myObject() {
  
}
// 不好的写法：动词开头
function getMyObject() {
  
}
```

**常量（值不会被改变的变量）命名应当是全部大写，不同单词用下划线隔开**

```js
// 好的写法(ES5版本)
var TOTAL_COUNT = 10;
// 其他都是不好的写法

/*
在ES6时虽然有了const来命名常量,但是还是推荐命名全部大写
*/
```

**对象的属性同变量的命名规则相同。对象的方法同函数的命名规则相同，如果属性或者方法是私有（不希望别人访问），应当在之前加一个下划线**

```js
// 好的写法
var object = {
    _count: 10,
  
    _getCount: function (){
        return this._count;
    }
}
```



## 10.严格模式

严格模式应当仅限在函数内部使用，千万不要在全局使用

```js
// 不好的写法：全局使用严格模式
"use strict";

function doSomething() {
    //code
};

// 好的写法
function doSomething() {
    "use strict";
    
    //code
};
```

**如果期望在多个函数里使用严格模式而不需要多次声明“use strict”，可以使用立即执行的函数**

```js
(function (){
    "use strict";
    
  function doSomething() {
    
  };
  
  function doSomethingElse() {
    
  };
}());
```



## 11.赋值

当给变量赋值时，如果右侧时含有比较语句的表达式，需要使用圆括号包裹

```js
// 好的写法
var flag = (i < count);

//不好的写法
var flag = i < count;
```



## 12.等号运算符

使用===（严格等于）和！==（严格不相等）代替==（相等）和!=（不等）来避免弱类型转换错误

```js
// 好的写法
var same = (a === b);

//不好的写法
var same = (a == b);
```



## 13.三元运算符

三元运算符应当仅仅用在条件赋值语句中，而不要作为if的替代品

```js
// 好的写法
var value = condition ? value1 : valule2;

// 不好的写法
condition ? doSomething() : doSomethingElse();
```



## 14.语句

### 14.1 简单语句

每行最多只包含一条语句。所有简单的语句都应该以分号结束

```js
// 好的写法
count++;
a = b;

// 不好的写法
count++; a = b;
```



### 14.2 返回语句

返回语句当返回一个值得时候不应当使用圆括号包裹，除非在某些情况下这么做可以让返回值更容易理解

```js
return;
return arr.length();
return (length > 18 ? length : 18);
```



### 14.3 复合语句

**复合语句时大括号括起来得语句列表**

- 括起来得语句应当较符合语句多缩进一个层级
- 开始得大括号应当在符合语句所在行的末尾；结束的大括号应当独占一行且同复合语句的开始保持一样的缩进
- 当语句是控制结构的一部分时，诸如if或者for语句，所有的语句都需要用大括号括起来，也包括单个语句。这个约定是的我们更加方便地添加语句而不用担心忘记加括号而引起bug。
- 像if一样的语句的开始的关键词，其后应该紧跟一个空格，起始大括号应当在空格之后

#### 14.3.1  if语句

if语句应当是下面的格式

```js
if (condition) {
    statements
}

if (condition) {
    statements1
} else {
    statements2
}

if (condition1) {
    statements1
} else if (condition2) {
    statements2
} else {
    statements3
}
```

**绝不允许在if中省略花括号**

```js
// 好的写法
if (condition) {
    doSomething();
}
//不好的写法：不恰当的空格
if(condition){
    doSomething();
}
// 不好的写法：省略花括号
if(condition)
    doSomething();
// 不好的写法：代码在一行
if (condition) {doSomething();}
// 不好的写法：在一行还省略花括号
if (condition) doSomething();
```



#### 14.3.2 for语句

for类型的语句应当是下面的格式

```js
for (initialization; condition; Step) {
    statements
}

for (variable in object) {
    statements
}
```

**for语句的初始化部分不应当有变量声明**

```js
// 好的写法
var i,
    len;
for (i = 0, len = 10; i < len; i++) {
    //code
}
//在ES6中如果是用作块作用域推荐用let声明
for (let i = 0, let len = 10; i < len; i++) {
    //code
}

// 不好的写法
for (var i = 0, len = 10; i < len; i++) {
    //code
}
for (var prop in object) {
    //code
}
```



#### 14.3.3 while语句

while类的语句应当是下面格式

```js
while (condition) {
    //code       
}
```



#### 14.3.4 do...while语句

do类语句应当是下面格式

```js
do {
    //code
} while (condition);
```



#### 14.3.5 switch语句

格式如下

```js
//除了第一个case之外，包括default在内的每一个case之前都应当有一个空行
switch (expression) {
    case exp1:
        //code
        break;
    
    case exp2:
        //code
        break;
    
    default:
        //code
        break;
}
```

**如果一个switch不包含default的情况，应当用注释代替**

```js
switch (expression) {
    case exp1:
        //code
        break;
    
    case exp2:
        //code
        break;
    
    // 没有default
}
```



## 15.留白

**在逻辑相关的代码块之间添加空行可以提高代码的可读性**

**两行空行仅限在如下情况中使用**

- 在不同的源代码文件之间
- 在类和接口定义之间



**单行空行仅限在如下情况中使用**

- 方法之间
- 方法中局部变量和第一行语句中间
- 多行或者单行注释之前
- 方法中逻辑代码块之间以提升代码的可读性



**空格应当在如下情况中使用**

- 关键词后跟括号的情况应当用空格隔开
- 参数列表中逗号之后应当留一个空格
- 所有的除了点(.)之外的二元运算符，其操作数都应当用空格隔开。单目运算符的操作数事件不应该用空白隔开，诸如一元减号，递增(++)，递减(--)
- for语句中的表达式之间应当用空格隔开



## 16.分号

在JS编写中其实是可以不用在最后添加分号的,同时也推荐尽量不要加分号,但是需要注意当一行代码以**[**,**(**,**`**开头的时候,需要在其前方或者上一段代码的后方补上分号来避免一些语法解析错误

**注意:**除了分号,也可以使用`！`,`~`等符号作为分割符来使用



## 17.避免使用

- 切勿使用像String一类的原始包装类型创建新的对象
- 避免使用eval()
- 避免使用with语句（其实该语句已经被废除了）



## 总结

**代码风格风格其实多种多样,就拿缩进来说**

- jQuery核心风格指南明确规定使用制表符缩进
- Dauglas Crockford的JavaScript代码规范规定使用4个空格字符缩进
- SproutCore风格指南规定使用2个空格缩进
- Google的Javascript风格指南规定使用2个空格缩进
- Dojo编程风格指南规定使用制表符缩进

**总之,大概的了解一下每种代码风格特色,一切以所在的团队风格为准**
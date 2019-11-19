# TypeScript

> 使用的TypeScript版本为`3.5.2`，如果有的地方不一样可能是版本报错的问题，注意对应版本修改即可

## 1.什么是TypeScript?

- TypeScript是微软开发的一款开源的编程语言
- TypeScript是JavaScript的超级,遵循最新的ES6和ES5规范。TypeScript扩展立了JavaScript的语法
- TypeScript更新后更像Java、C#这样的面向对象语言,可以让JavaScript开发大型企业项目
- 现如今的流行框架都可以继承TypeScript



## 2.TypeScript的安装及编译

### 2.1 安装

**通过下面的方式在全局安装TypeScript**

```shell
cnpm install -g typescript
# 或 yarn add typescript -g
```



### 2.2 编译

- **安装TypeScript后通过内置的命令tsc就能将ts文件编译为对应的js文件**

  ```shell
  tsc helloworld.ts
  ```

- **上面的方法太麻烦,可以在编译器(这里只说在vscode中的配置)中自动监视**
  
  + 在项目目录中运行`tsc --init`生成tsconfig.json
  + 可以修改tsconfig.json中的`outDir`选项为`"outDir": "./js"`,以后所有的js代码都会在这个文件夹中编译
  + 然后在vscode中打开命令面板(Ctrl+Shift+P)选择输入`task`,选择`Run Task`,监听tsconfig.json,就可以vscode中实时监控TypeScript编译



## 3.TypeScript的数据类型

> **该节如果从头开始看只需要看基本类型,最后再回来看高级类型**

**TypeScipt中为了使编写的代码更加规范,更有利于维护,增加了类型校验,所有以后写ts代码必须要指定类型**

**注:**在TypeScript后面可以不指定类型,但是后期不能改变其类型,不然会报错,但是只会报错,不会阻止代码编译,因为JS是可以允许的

### 3.1 基本类型

- **布尔类型(boolean)**

  ```typescript
  let flag:boolean=true;
  //let flag=true 也是可以的
  flag=123;//错误,会报错,因为不符合类型
  flag=false;//正确写法,boolean类型只能写true和false
  console.log(flag);
  ```

- **数字类型(number)**

  ```typescript
  let num:number=123;
  console.log(num);
  ```

- **字符串类型(string)**

  ```typescript
  let str:string="string";
  console.log(str);
  ```

- **数组类型(Array)**

  **在TypeScript中有两种定义数组的方法**

  ```typescript
  //第一种定义数组方法
  let arr1:number[]=[1.2.3];//一个全是数字的数组
  
  //第二种定义数组方法
  let arr2:Array<number>=[1,2,3];
  
  //第三种定义数组方法(用了下面的元组类型)
  let arr3:[number,string]=[123,"string"];
  
  //第四种定义数组方法(用了下面的任意类型)
  let arr4:any[]=[1,"string",true,null];
  ```

- **只读数组类型(ReadonlyArray)**

  **只读数组中的数组成员和数组本身的长度等属性都不能够修改,并且也不能赋值给原赋值的数组**

  ```typescript
  let a:number[]=[1,2,3,4];
  let ro:ReadonlyArray<number>=a;//其实只读数组只是一个内置定义的泛型接口
  ro[1]=5;//报错
  ro.length=5;//报错
  a=ro;//报错,因为ro的类型为Readonly,已经改变了类型
  a=ro as number[];//正确,不能通过上面的方法复制,但是可以通过类型断言的方式,类型断言见下面
  ```

  **注：**TypeScript 3.4引入了一种新语法，该语法用于对数组类型`ReadonlyArray`使用新的`readonly`修饰符

  ```typescript
  function foo(arr: readonly string[]) {
      arr.slice();        // okay
      arr.push("hello!"); // error!
  }
  ```

  

- **元组类型(tuple)**

  元组类型属于数组的一种,上面一种数组里面只能有一种类型,否则会报错,而元组类型内部可以有多种类型

  ```typescript
  //元组类型可以为数组中的每个成员指定一个对应的类型
  let arr:[number,string]=[123,"string"];//注意如果类型不对应也会报错
  arr[2]=456;//报错
  //因为元组类型在声明时是一一对应的,只能有2个成员
  ```

  **注意：**与数组一样，元祖也可以使用`readonly`修辞了，但是，尽管出现了`readonly`类型修饰符，但类型修饰符只能用于数组类型和元组类型的语法

  ```typescript
  let err1: readonly Set<number>; // error!
  let err2: readonly Array<boolean>; // error!
  
  let okay: readonly boolean[]; // works fine
  ```

- **枚举类型(enum)**

  ```typescript
  /*
  	通过:
      enum 枚举名{
      	标识符=整形常数,
      	标识符=整形常数,
      	......
      	标识符=整形常数
      };
      定义一个枚举类型
  */
  enum Color {
    red=1,
    blue=3,
    oragne=5
  }
  
  let color:Color = Color.red;//color为Color枚举类型,值为Color中的red,也就是1
  console.log(color);//1
  ```

  **注意:**

  + 如果没有给标识符赋值,那么标识符的值默认为索引值

  + 如果在期间给某个标识符进行了赋值,而之后的标识符没有赋值,那么之后表示符的索引依次为前面的值加1

  + 枚举可以引用内部的标识符的值或者外部变量的值

    ```typescript
    const o = 5
    enum Color {
      red=1,
      blue=red, // 引用内部标识符
      oragne=o // 引用外部变量
    }
    ```

  + 可以把标识符用引号括起来,效果不受影响

  + 还可以通过反过来选择索引值来获取字符串标识符（实际上就是将枚举的变量赋值为一个对象，该对象分别有**键对应属性和属性对应键**两种相对应的属性）

  ```typescript
  enum Color {
    red,
    blue=3,
    "oragne"
  }
  let color1:Color = Color.red;
  let color2:Color = Color.blue;
  let color3:Color = Color.oragne;
  let color4:string=Color[1];//通过获取索引得到标识符
  console.log(color1);//0
  console.log(color2);//3
  console.log(color3);//4
  console.log(color4);//red
  ```

- **任意类型(any)**

  **任意类型的数据就像JS一样,可以随意改变其类型**

  ```typescript
  let num:any=123;
  num="string";
  console.log(num);//string
  ```

  **具体用法**

  ```typescript
  //如果在ts中想要获取DOM节点,就需要使用任意类型
  let oDiv:any=document.getElementsByTagName("div")[0];
  oDiv.style.backgroundColor="red";
  //按道理说DOM节点应该是个对象,但是TypeScript中没有对象的基本类型,所以必须使用any类型才不会报错
  ```

- **unknown类型**

  TypeScript 3.0 引入了新的`unknown` 类型，它是 `any` 类型对应的安全类型。就像所有类型都可以被归为 `any`，**所有类型也都可以被归为 `unknown`。**这使得 `unknown` 成为 TypeScript 类型系统的另一种顶级类型（另一种是 `any`）。`unknown` 和 `any` 的主要区别是 `unknown` 类型会更加严格：**在对 `unknown` 类型的值执行大多数操作之前，我们必须进行某种形式的检查。**而在对 `any` 类型的值执行操作之前，我们不必进行任何检查

  ```typescript
  // unknown可以被赋值为任意类型
  let value: unknown;
  value = true;             // OK
  value = 42;               // OK
  value = "Hello World";    // OK
  value = [];               // OK
  value = {};               // OK
  value = Math.random;      // OK
  value = null;             // OK
  value = undefined;        // OK
  value = new TypeError();  // OK
  value = Symbol("type");   // OK
  ```

  ```typescript
  // 但是 unknown 类型只能被赋值给 any 类型和 unknown 类型本身，如果没有类型喜欢的话
  let value: unknown;
  // value = 123 如果这样就是类型
  let value1: unknown = value;   // OK
  let value2: any = value;       // OK
  let value3: boolean = value;   // Error
  let value4: number = value;    // Error
  let value5: string = value;    // Error
  let value6: object = value;    // Error
  let value7: any[] = value;     // Error
  let value8: Function = value;  // Error
  ```

  直观的说，这是有道理的：**只有能够保存任意类型值的容器才能保存 `unknown` 类型的值**

  ```typescript
  // 当然也不能直接进行操作，必须要进行类型细化
  let value: unknown;
   
  value.foo.bar;  // Error
  value.trim();   // Error
  value();        // Error
  new value();    // Error
  value[0][1];    // Error
  ```

  **注：**

  + `unknown`类型也可以和any一样通过`typeof`、`instanceof`或自定义类型保护来缩小`unknown`的范围

  + 在联合类型中，`unknown` 类型会吸收任何类型。这就意味着如果任一组成类型是 `unknown`，联合类型也会相当于 `unknown`，只有与`any`进行联合才会不同

    ```typescript
    type UnionType1 = unknown | null;       // unknown
    type UnionType2 = unknown | undefined;  // unknown
    type UnionType3 = unknown | string;     // unknown
    type UnionType4 = unknown | number[];   // unknown
    type UnionType5 = unknown | any;  // any
    ```

  + 在交叉类型中，任何类型都可以吸收 `unknown` 类型。这意味着将任何类型与 `unknown` 相交不会改变结果类型

    ```typescript
    type IntersectionType1 = unknown & null;       // null
    type IntersectionType2 = unknown & undefined;  // undefined
    type IntersectionType3 = unknown & string;     // string
    type IntersectionType4 = unknown & number[];   // number[]
    type IntersectionType5 = unknown & any;        // any
    ```
    
  + `never`类型是`unknown`类型的子类型

    ```typescript
    type t = never extends unknown? true:false // true
    ```

  + `keyof unknown`等于类型`never`

    ```typescript
    type t = keyof unknown // never
    ```

  + 只能对`unknown`类型进行等于或非等于的运算符操作，不能进行其他的操作

    ```typescript
    let value:unknown
    let value2:number = 1
    console.log(value === value2)
    console.log(value !== value2)
    ```

  + 使用映射类型时如果遍历的是`unknown`类型，不会映射任何属性

    ```typescript
    type Types<T> = {
        [P in keyof T]:number
    }
    type t = Types<unknown> // t = {}
    ```

    

- **undefined和null类型** 

  **虽然为变量指定了类型,但是如果不赋值的话默认该变量还是undefined类型,如果没有指定undefined直接使用该变量的话会报错,只有自己指定的是undefined类型才不会报错**

  ```typescript
  let flag1:undefined;
  /*
  	也可以let flag1:undefined=undefined;
  */
  console.log(flag);//undefined
  
  let flag2:null=null;//如果不指定值为null那么打印的时候也会报错
  console.log(flag2);//null
  ```

  **为变量指定多种可能的类型**

  ```typescript
  let flag:number|undefined;//这种写法就不会在没有赋值的时候报错了,因为设置了可能为undefined
  console.log(flag);//undefined
  flag=123;
  console.log(flag);//123 也可以改为数值类型
  
  //也可以设定多个类型
  let flag1:number|string|null|undefined;
  flag1=123;
  console.log(flag1);//123
  flag1=null;
  console.log(flag1);//null
  flag1="string";
  console.log(flag1);//string
  ```

- **void类型**

  **TypeScript中的void表示没有任何类型,一般用于定义方法的时候没有返回值**,虽然也能给变量指定类型,但是void类型只能被赋值undefined和null,没有什么实际意义

  **注:**在TypeScript中函数的类型表示其返回值的类型,函数类型为void表示其没有返回值

  ```typescript
  function run():void{
      console.log(123);
      //return 不能有返回值,否则报错
  }
  
  function run1():number{
      console.log(456);
      return 123;//必须有返回值,并且返回值为number类型,否则报错
  }
  
  function run2():any{
      console.log(789);//因为any是任意类型,所以也可以不要返回值
  }
  ```

- **never类型**

  **never类型是其他类型(包括null和undefine)的子类型,代表着从来不会出现的值,意为这声明never类型的变量只能被never类型所赋值**

  ```typescript
  let a:undefined;
  a=undefined;
  
  let b:null;
  b=null;
  
  let c:never;//c不能被任何值赋值,包括null和undefiend,指不会出现的值
  c=(()=>{
      throw new Error("error!!");
  })();//可以这样赋值
  
  // 返回never的函数必须存在无法达到的终点
  function error(message: string): never {
      throw new Error(message);
  }
  
  // 推断的返回值类型为never
  function fail() {
      return error("Something failed");
  }
  
  // 返回never的函数必须存在无法达到的终点
  function infiniteLoop(): never {
      while (true) {
      }
  }
  ```

- **object类型**

  `object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`（TypeScript中的Symbol同JS完全一样，在这里是没有讲的），`null`或`undefined`之外的类型

  ```typescript
  let stu:object={name:"张三",age:18};
  console.log(stu);//{name: "张三", age: 18}
  //也可以
  let stu: {} = { name: "张三", age: 18 };
  console.log(stu);
  ```

  ```typescript
  declare function create(o: object | null): void;
  //declare是一个声明的关键字,可以写在声明一个变量时解决命名冲突的问题
  create({ prop: 0 }); // OK
  create(null); // OK
  
  create(42); // 报错
  create("string"); // 报错
  create(false); // 报错
  create(undefined); // 报错
  ```
  
  **注：**一般对象类型不会这样申明，而是直接写让TypeScript做自动类型判断或者更加精确的指示，如接口等，在后面都有介绍到
  
- **函数类型**

  函数类型有多种声明方式，最简单直观的是用`Function`表示函数类型，在写函数表达式的时候可以直接写声明是一个函数类型（不过一般我们不这样做，要么是不在表达式左边直接写`Function`而是靠类型推断，要么是直接将一个函数的类型写全），具体介绍在后面函数中单独介绍

  ```typescript
  let a:Function
  a = function():void = {
      console.log("function")
  } 
  ```


#### 3.1.1 枚举类型

由于枚举类型的比较特殊，这里单独再拿出来说。在前面已经知道了枚举的基本使用，同时枚举类型相互获取键值的特性也已经清楚，这里说几个特殊情况：

- **字符串枚举：**枚举的值除了使用整数外还可以是纯的字符串，在这里主要利用其键值相互获取的特性

  ```typescript
  enum Message {
      Error = 'error',
      Success = 'success',
      Failed = 'failed'
  }
  // 字符串枚举依然可以使用内部的变量，但不能使用外部的变量
  const f = 'faliled'
  enum Message {
      Error = 'error',
      Success = 'success',
      Failed = Success
      // Failed = f 报错
  }
  ```

- **异构枚举：**简单来说就是既包含数字又包含字符串的枚举类型

  ```typescript
  // 异构枚举也可以使用内部的变量，但不能使用外部的变量
  enum Message {
      Error = 0,
      Success = 'success',
      Failed = 'failed'
  }
  ```

- **枚举作为类型使用：**当满足一定条件时，枚举对象本身和其中的成员都可以当做是类型来使用

  - `enum E { A }`：无初始值，但是这种类型的成员必须前一个是一个数值类型的枚举成员
  - `enum E { A = 'a' }`：字符串枚举
  - `enum E { A = 1 }`：基本的有初始化的枚举类型

  ```typescript
  enum Animals {
    Dog = 1,
    Cat = 2
  }
  
  // type的值只能是Animals中的Dog成员，也就是type只能是1
  interface Dog {
    type: Animals.Dog
  }
  const dog: Dog = {
    type: Animals.Dog 
    // type:1，type也可以直接赋值为数值，这里是所有的数值，但是U币能是Cat，要说到后面的类型兼容
  }
  
  // 如果直接使用枚举变量，那么相当于高级类型的字符自变量类型
  enum Animals {
    Dog = 1,
    Cat = 2
  }
  
  interface Animal {
    type: Animals // type的值为1或2
  }
  const dog: Dog = {
    type: Animals.Dog // type:1
    // type: Animals.Cat // type:2
  }
  
  ```

- **编译枚举变量：**在正常情况下，枚举不像是`type`定义的类型这样编译后是不会存在的，枚举类型创建后就是默认开辟了一个枚举变量的空间，并且枚举值我们一般用作提高代码的可读性：

  ```typescript
  enum Status {
      Success:200,
  }
  console.log(res.status === Status.Success) // 通常我们会这样为响应对象提高代码可读性
  ```

  如果不想要枚举变量真实存在，只是想自己创建一个别值，那么可以在枚举变量前加上`const`关键字

  ```typescript
  // 加了const关键字以后相当于原来的枚举变量只是个占位符
  const enum Status {
      Success:200,
  }
  ```

  

### 3.2 高级类型

#### 3.2.1 keyof与[]

keyof是索引类型查询操作符。假设T是一个类型，那么keyof T产生的类型是T的属性名称字符串字面量类型构成的联合类型。而[]是索引访问操作符，可以单独使用相当于是对象书写的`[]`形式，也可以与`keyof`操作符一起使用

**注意：**T是数据类型，并非数据本身

```typescript
interface Itest{
  webName:string;
  age:number;
  address:string
}

type ant=keyof Itest;// 在编译器中会提示ant的类型是webName、age和address这三个字符串
let a:ant = 'webName'// a只能是三个字符串中的一个
```

**使用[]**

```typescript
interface Type {
  a: string
  b: number
  c: boolean
  d: undefined
  e: null
  f: never
  h: object
}
type Test = Type['a'] // string
```

**如果T是一个带有字符串索引签名的类型，那么`keyof T`是string类型，并且`T[string]`为索引签名的类型**

```typescript
interface Map<T> {
  [key: string]: T;
}
let keys: keyof Map<number>;
//string|number，因为可索引接口的类型如果是string的话可以传可以为string或者number
/*
interface Map<T> {
  [key: number]: T;
}
let keys: keyof Map<number>; //可索引接口为number则keys的类型只能是number
*/
let value: Map<number>['name'];//number，Map<number>['name']这是一个类型，最后结果是接口里属性的值
```

**`keyof`操作符也可以配合type的类型别名来实现类似字面量类型的结果**

```typescript
interface Type {
  a: string
  b: number
  c: boolean
  d: undefined
  e: null
  f: never
  h: object
}
type Test = Type[keyof Type] // string | number | boolean | object
// 与上面的用法相对应
```

**注意：**使用`keyof`或类似`keyof`这样需要在一些类型中选择类型时会将`null`、`undefined`、`never`类型排除掉

**`keyof`操作符经常与extends关键字一起作为泛型的约束来使用**

```typescript
// K只能是T的索引属性组成的一个数组，并且返回一个T属性值对应值组成的数组
function getValue<T, K extends keyof T>(
  obj: T,
  names: K[]
): Array<T[K]> /* 可以写成T[K][] */ {
  return names.map(n => obj[n])
}
let obj = {
  name: '张三',
  age: 18
}
getValue(obj, ['name', 'age'])
```



#### 3.2.2 交叉类型

**交叉类型是将多个类型合并为一个类型。这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性**

```typescript
function extend<T, U>(first: T, second: U): T & U {
  let result:T & U = <T & U>{} //只能通过类型断言来做联合类型
  for (let id in first) {
    ;(<any>result)[id] = (<any>first)[id]
  }
  for (let id in second) {
    if (!(result as any).hasOwnProperty(id)) {// 如果这里不断言为any会报错的，因为不能确保T&U类型有方法
      ;(<any>result)[id] = (<any>second)[id]
    }
  }
  return result
}
/*
1.简便一点的写法
function extend<T, U>(first: T, second: U): T & U {
  let result: any = {
  for (let id in first) {
    result[id] = first[id]
  }
  for (let id in second) {
    if (!result.hasOwnProperty(id)) {
      result[id] = second[id]
    }
  }
  return result as T & U
}
2.更简便的
function extend<T, U>(first: T, second: U): T & U {
  let result = {} as T & U
  result = Object.assign(first, second) // 注意要开启es6
  return result
}
*/


class Person {
  constructor(public name: string) {}
}
interface Loggable {
  log(): void
}
class ConsoleLogger implements Loggable {
  log() {
    // ...
  }
}
var jim = extend(new Person('Jim'), new ConsoleLogger())
var n = jim.name
jim.log()
```



#### 3.2.3 联合类型

**联合类型与交叉类型很有关联,但是使用上却完全不同,我们使用的用竖线隔开每一个类型参数的时候使用的就是联合类型**

联合类型表示一个值可以是几种类型之一。 用竖线( `|`)分隔每个类型,所以 `number | string | boolean`表示一个值可以是 `number`,`string`,或 `boolean`

```typescript
function padLeft(value: string, padding: string | number) {
    // ...
}

let indentedString = padLeft("Hello world", true); // errors,只能是string或number
```

**注意:**如果一个值是联合类型,我们只能访问此联合类型的所有类型里共有的成员

```typescript
interface Bird {
    fly();
    layEggs();
}

interface Fish {
    swim();
    layEggs();
}

function getSmallPet(): Fish | Bird {//这的报错先不管
    // ...
}

let pet = getSmallPet();//因为没有明确返回哪个类型,所以只能确定时Fish和Bird类型中的一个
pet.layEggs(); // 我们可以调用共有的方法
pet.swim();    // errors,不能调用一个类型的方法,因为万一是Bird类型就不行了
```



#### 3.2.4 类型保护

**联合类型适合于那些值可以为不同类型的情况。 但当我们想确切地了解是否为 `Fish`时,JavaScript里常用来区分2个可能值的方法是检查成员是否存在,但是在TypeScript中必须要先使用类型断言**

```typescript
let pet = getSmallPet();

if ((<Fish>pet).swim) {
    (<Fish>pet).swim();
}
else {
    (<Bird>pet).fly();
}
```

- **用户自定义类型保护**

    ```typescript
    function isFish(pet: Fish | Bird): pet is Fish {
        return (<Fish>pet).swim !== undefined;//返回一个布尔值,然后TypeScript会缩减该变量类型
    }
    /*
    pet is Fish就是类型谓词。谓词为 parameterName is Type这种形式,parameterName必须是来自于当前函数签名里的一个参数名
    */
    ```

    ```typescript
    // 'swim' 和 'fly' 调用都没有问题了,因为缩减了pet的类型
    if (isFish(pet)) {
        pet.swim();
    }
    else {
        pet.fly();
    }
    ```

    **注意:**TypeScript不仅知道在 `if`分支里 `pet`是 `Fish`类型,它还清楚在 `else`分支里，一定 不是 `Fish`类型，一定是 `Bird`类型

- **typeof类型保护**

    ```typescript
    function padLeft(value: string, padding: string | number) {
        if (typeof padding === "number") {
            return Array(padding + 1).join(" ") + value;
        }
        if (typeof padding === "string") {
            return padding + value;
        }
        throw new Error(`Expected string or number, got '${padding}'.`);
    }
    ```

    **注意:**这些 `typeof`类型保护只有两种形式能被识别:`typeof v === "typename"`和 `typeof v !== "typename"`，`"typename"`必须是 `"number"`， `"	"`， `"boolean"`或 `"symbol"`。 但是TypeScript并不会阻止你与其它字符串比较,语言不会把那些表达式识别为类型保护，如使用`(typeof str).includes('string')`是没有类型保护的

- **instanceof类型保护**

    ```typescript
    interface Padder {
        getPaddingString(): string
    }
    
    class SpaceRepeatingPadder implements Padder {
        constructor(private numSpaces: number) { }
        getPaddingString() {
            return Array(this.numSpaces + 1).join(" ");
        }
    }
    
    class StringPadder implements Padder {
        constructor(private value: string) { }
        getPaddingString() {
            return this.value;
        }
    }
    
    function getRandomPadder() {
        return Math.random() < 0.5 ?
            new SpaceRepeatingPadder(4) :
            new StringPadder("  ");
    }
    
    // 类型为SpaceRepeatingPadder | StringPadder
    let padder: Padder = getRandomPadder();
    
    if (padder instanceof SpaceRepeatingPadder) {
        padder; // 类型细化为'SpaceRepeatingPadder'
    }
    if (padder instanceof StringPadder) {
        padder; // 类型细化为'StringPadder'
    }
    ```

    **注意:**`instanceof`的右侧要求是一个构造函数,TypeScript将细化为:

    - 此构造函数的 `prototype`属性的类型，如果它的类型不为 `any`的话
    - 构造签名所返回的类型的联合

- **可以为null的类型**

    如果没有在vscode中,直接编译的话是可以给一个其他类型的值赋值为undefined或者null的,但是如果编译时使用了`--strictNullChecks`标记的话,就会和vscode一样不能赋值了,**并且可选参数和可以选属性会自动加上`|undefined`类型**

    **类型保护与类型断言**

    由于可以为null的类型是通过联合类型实现,那么需要使用类型保护来去除 `null`

    ```typescript
    function f(sn: string | null): string {
        if (sn == null) {
            return "default";
        }
        else {
            return sn;
        }
    }
    //也可以使用下面的形式
    function f(sn: string | null): string {
        return sn || "default";
    }
    ```

    **如果编译器不能够去除 `null`或 `undefined`,可以使用类型断言手动去除。 语法是添加 `!`后缀：`identifier!`从 `identifier`的类型里去除了 `null`和 `undefined`**

    ```typescript
    function broken(name: string | null): string {
      function postfix(epithet: string) {
        return name.charAt(0) + '.  the ' + epithet; // error, 'name' is possibly null
      }
      name = name || "Bob";
      return postfix("great");
    }
    //上面的函数会报错，因为嵌套太深了
    function fixed(name: string | null): string {
      function postfix(epithet: string) {
         //这里的name强制断言不是null或者undefined,因为在嵌套函数中不知道name是否为null
        return name!.charAt(0) + '.  the ' + epithet; // ok
      }
      name = name || "Bob";//这里已经明确表明返回的name肯定是字符串
      return postfix("great");//但是由于是嵌套函数,这里还不知道
    }
    /*
    嵌套函数中:因为编译器无法去除嵌套函数的null(除非是立即调用的函数表达式)。因为它无法跟踪所有对嵌套函数的调用,尤其是将内层函数做为外层函数的返回值。如果无法知道函数在哪里被调用,就无法知道调用时name的类型
    */
    ```



#### 3.2.5 类型别名

**类型别名会给一个类型起个新名字。类型别名有时和接口很像，甚至可以相互兼容，但是类型别名可以作用于原始值，联合类型，元组以及其它任何需要手写的类型，同时类型别名无法像接口一样扩展**

**注意:**别名不会创建一个新的类型,而是创建了一个新的名字来引用那个类型

```typescript
type Name = string;//通过type关键词创建一个别名
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    }
    else {
        return n();
    }
}
```

**同接口一样,类型别名也可以是泛型,可以添加类型参数并且在别名声明的右侧传入**

```typescript
type Container<T> = { value: T };

/*
我们也可以使用类型别名来在属性里引用自己，定义一种可以无限嵌套的树状结构，注意这个应该是可选参数，不然无限引用了
*/
type Tree<T> = {
    value: T;
    left?: Tree<T>;
    right?: Tree<T>;
}

//与交叉类型一起使用，我们可以创建出一些十分稀奇古怪的类型
type LinkedList<T> = T & { next: LinkedList<T> };

interface Person {
    name: string;
}

var people: LinkedList<Person>;
var s = people.name;//注意这种写法在vscode中会报错,因为更加严格,必须先赋值再使用
var s = people.next.name;
var s = people.next.next.name;
var s = people.next.next.next.name;
```

**注:**类型别名不能直接出现在声明右侧的任何地方

```typescript
type Yikes = Array<Yikes>; 
// 报错，提示无限引用本身，只可以在对象属性中引用自己，因为这样才能可选，不然无限嵌套
```



#### 3.2.6 字面量类型

**字面量类型允许指定类型必须的固定值。在实际应用中,字面量类型可以与联合类型,类型保护和类型别名很好的配合。通过结合使用这些特性,可以实现类似枚举类型**

```typescript
type Easing = "ease-in" | "ease-out" | "ease-in-out";
class UIElement {
    animate(dx: number, dy: number, easing: Easing) {
        if (easing === "ease-in") {
            // ...
        }
        else if (easing === "ease-out") {
        }
        else if (easing === "ease-in-out") {
        }
        else {
            // error! should not pass null or undefined.
        }
    }
}

let button = new UIElement();
button.animate(0, 0, "ease-in");
button.animate(0, 0, "uneasy"); // error: "uneasy" is not allowed here
//只能从三种允许的字符中选择其一来做为参数传递,传入其它值则会产生错误
```

**字面量类型还可以用于区分函数重载**

```typescript
function createElement(tagName: "img"): HTMLImageElement;
function createElement(tagName: "input"): HTMLInputElement;
// ... more overloads ...
function createElement(tagName: string): Element {
    // ... code goes here ...
}
```

**注：**上面只用了字符串做例子，其余的所有类型的固定值也都可适应这个规则



#### 3.2.7 可辨识联合

可辨识联合具有两要素：

- 具有普通的单例类型属性
- 一个类型别名包含了这些类型的联合

```typescript
interface Square {
  kind: 'square'
  size: number
}
interface Rectangle {
  kind: 'rectangle'
  height: number
  width: number
}

interface Circle {
  kind: 'circle'
  radius: number
}

type Shape = Square | Rectangle | Circle
function asserNever(value: never): never {
  throw new Error('Unexpected object:' + value)
}
function getArea(s: Shape): number {
  // s.kind属性就是可辨识联合的索引
  switch (s.kind) {
    // 下面都会自动识别的
    case 'square':
      return s.size * s.size
    case 'rectangle':
      return s.width * s.height
    case 'circle':
      return Math.PI * s.radius * 2
    // 可以定义一个如果都不是就会报错的默认选项，定义了这个后如果没有写上面3个的任意一个都会有清楚的提示
    default:
      return asserNever(s)
  }
}

let a: any = 1
getArea(a) // 抛出错误，注意不是编译器报错
```



#### 3.2.8 this类型

**this类型主要用于链式调用中**，著名的`jQuery`就使用了大量返回当前对象`this`创建链式调用的功能

```typescript
class BasicCalculator {
  public constructor(protected value: number = 0) { }
  
  public currentValue(): number {
    return this.value;
  }
  
  public add(operand: number) {
    this.value += operand;
    return this;
  }
  
  public subtract(operand: number) {
    this.value -= operand;
    return this;
  }
  
  public multiply(operand: number) {
    this.value *= operand;
    return this;
  }
  
  public divide(operand: number) {
    this.value /= operand;
    return this;
  }
}
// 链式调用
let v = new BasicCalculator(2).multiply(5).add(1).currentValue();
```

在类继承后也可以正常链式调用并且所有的放都会整的反应出来

```typescript
class BasicCalculator {
  public constructor(protected value: number = 0) { }
  
  public currentValue(): number {
    return this.value;
  }
  
  public add(operand: number) {
    this.value += operand;
    return this;
  }
  
  public subtract(operand: number) {
    this.value -= operand;
    return this;
  }
  
  public multiply(operand: number) {
    this.value *= operand;
    return this;
  }
  
  public divide(operand: number) {
    this.value /= operand;
    return this;
  }
}

class ScientificCalculator extends BasicCalculator {
  public constructor(value = 0) {
    super(value);
  }
  
  public square() {
    this.value = this.value ** 2;
    return this;
  }
  
  public sin() {
    this.value = Math.sin(this.value);
    return this;
  }
}

let v = new caScientificCalculatorlc(0.5).square().divide(2).sin().currentValue();
```

**注：**在之前，上面调用父类的方法将会报错，只能使用子类的放，TypeScript1.7增加了this类型，那么divide()返回值类型将会被推断为this类型。这就展现了this类型的多态，不但可以是父类类型，也可以是子类类型，也可以是实现的接口类型。比如**this 类型在描述一些使用了 mixin 风格继承的库 (比如 Ember.js) 的交叉类型**

```typescript
interface MyType {
  extend<T>(other: T): this & T;
}
```



#### 3.2.9 映射类型

**映射类型可以理解为我们想要对一个已知的类型进行装饰，类似数组的`map`方法**

**注：**映射类型一般是给自变量联合类型使用的

一个常见的任务是将一个已知的类型每个属性都变为可选的：

```typescript
interface PersonPartial {
    name?: string;
    age?: number;
}
```

或者想要变成只读的类型：

```typescript
interface PersonReadonly {
    readonly name: string
    readonly age: number
}
```

TypeScript提供了从旧类型中创建新类型的一种方式 — **映射类型**。 在映射类型里，新类型以相同的形式去转换旧类型里每个属性

```typescript
// 只读
type Readonly<T> = {
    readonly [P in keyof T]: T[P]
}

// 可选
type Partial<T> = {
    [P in keyof T]?: T[P]
}
```

```typescript
type PersonPartial = Partial<Person>;
type ReadonlyPerson = Readonly<Person>;
```

**下面来看看最简单的映射类型和它的组成部分：**

```typescript
type Keys = 'option1' | 'option2';
/*
k in Keys 类似对象的遍历，只是将联合类型中的每一个类型都遍历出来
*/
type Flags = { [K in Keys]: boolean };
```

它的语法与索引签名的语法类型，内部使用了 `for .. in`。 具有三个部分：

- 类型变量 `K`，它会依次绑定到每个属性。
- **字符串字面量联合**的 `Keys`，它包含了要迭代的属性名的集合。
- 属性的结果类型。

```typescript
// 上面的答案，会对联合类型进行类型的转换为字面量类型
type Flags = {
    option1: boolean;
    option2: boolean;
}
```



**注意：**`k in keyof keys`与`k in keys`是有区别的，前者是对类似对象类型的类型使用，后者是对联合类型使用，前面的keyof只是把类似对象类型的类型的属性名转变为联合类型再进行后者的操作。并且不能直接对泛型或者类似对象类型使用`k in keys`这样的语法，会报错，而对联合类型如果使用前者不会报错，但是会造成非正常业务需求的类型错误，因为这样其实keyof是将联合类型中的每一个类型中隐藏的TypeScript中定义的属性或方法取了出来

```typescript
// 只读
type Readonly<T> = {
    readonly [P in T]: T[P] // 会报错
    readonly [P in keyof T]: T[P] // 正常业务需求
}
```

```typescript
type Pick<T, K extends keyof T> = {
    [P in K]:T[P] // 正常业务需求，返回T中包含K中带有的属性名
}
```

```typescript
type Keys = 'option1' | 'option2';
type Flags = { [K in Keys]: boolean }; // 正常业务需求
type Flags = { [K in keyof Keys]: boolean }; // 非正常业务需求
/*
因为k in Keys就是遍历类型的属性名，而keyof Keys也是拿到属性名，而属性名是字符串类型（和直接keyof string是一样的结果），所以直接遍历的是出来的其实是TypeScript封装的字符串类型中的一系列属性和方法
*/
```

在真正的应用里，可能不同于上面的 `Readonly`或 `Partial`。 它们会基于一些已存在的类型，且按照一定的方式转换字段。 这就是 **`keyof`**和**索引访问类型**要做的事情：

```typescript
type NullablePerson = { [P in keyof Person]: Person[P] | null }
type PartialPerson = { [P in keyof Person]?: Person[P] }
// keyof Person 代表 
```

但它更有用的地方是可以有一些通用版本。

```typescript
type Nullable<T> = { [P in keyof T]: T[P] | null }
type Partial<T> = { [P in keyof T]?: T[P] }
```

在这些例子里，属性列表是 `keyof T`且结果类型是 `T[P]`的变体。 这是使用通用映射类型的一个好模版。 因为这类转换是 同态]的，映射只作用于 `T`的属性而没有其它的。 编译器知道在添加任何新属性之前可以拷贝所有存在的属性修饰符。 例如，假设 `Person.name`是只读的，那么 `Partial<Person>.name`也将是只读的且为可选的

下面是另一个例子， `T[P]`被包装在 `Proxy<T>`类里：

```typescript
type Proxy<T> = {
    get(): T;
    set(value: T): void;
}
type Proxify<T> = {
    [P in keyof T]: Proxy<T[P]>;
}
function proxify<T>(o: T): Proxify<T> {
   // ... wrap proxies ...
}
let proxyProps = proxify(props);
```

注意 `Readonly<T>`和 `Partial<T>`用处不小，因此它们与 `Pick`和 `Record`一同被包含进了TypeScript的标准库里：

```typescript
// 四个内置的映射类型

// 只读
type Readonly<T> = {
    readonly [P in keyof T]: T[P]
}

// 可选
type Partial<T> = {
    [P in keyof T]?: T[P]
}
// 获取T中包含K或K数组的属性
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
}
// 改变类型，在使用的时候值的类型也会改变
type Record<K extends string, T> = {
    [P in K]: T;
}
```

`Readonly`， `Partial`和 `Pick`是同态的，但 `Record`不是。 因为 `Record`并不需要输入类型来拷贝属性，所以它不属于同态

**同态：**两个相同代数结构之间的结构保持映射，也就是说进入和出去的值应该是一样的，而`Record`进入和出去的值发生了改变

```typescript
type ThreeStringProps = Record<'prop1' | 'prop2' | 'prop3', string>
```

**非同态类型本质上会创建新的属性，因此它们不会从它处拷贝属性修饰符**

##### 3.2.9.1 对keyof的升级

在2.9版本中keyof操作符已经支持在映射类型的属性值上使用`string`、`number`和`symbol`类型

```typescript
const stringIndex = 'a'
const numberIndex = 1
const symbolIndex = Symbol()
type Obj = {
  [stringIndex]: string
  [numberIndex]: number
  [symbolIndex]: symbol
}

type keyType = keyof Obj

let obj: Readonly<Obj> = {
  a: 'aa',
  1: 11,
  [symbolIndex]: Symbol()
}
```



##### 3.2.9.2 对于元祖和数组的支持

在3.1版本中TypeScript支持将元祖和数组会映射为新的元祖和数组，并不会映射为新的类型

```typescript
type MapToPromise<T> = {
  [K in keyof T]: Promise<T[K]>
}

type Tuple = [number, string, boolean]

type promiseTuple = MapToPromise<Tuple>

const tuple: promiseTuple = [
  new Promise(resolve => resolve(1)),
  new Promise(resolve => resolve('a')),
  new Promise(resolve => resolve(true))
]
```



#### 3.2.10 由映射类型进行推断

现在了解了如何包装一个类型的属性，那么接下来就是如何拆包。 其实这也非常容易：

```typescript
function unproxify<T>(t: Proxify<T>): T {
    let result = {} as T;
    for (const k in t) {
        result[k] = t[k].get();
    }
    return result;
}

let originalProps = unproxify(proxyProps);
```

**注意：**这个拆包推断只适用于同态的映射类型。 如果映射类型不是同态的，那么需要给拆包函数一个明确的类型参数



#### 3.2.11 增加移除修饰符

**通过在修辞符前写`+`和`-`的方法就能够增加和移除修辞符**

**注意：**

- 这里的修辞符只是`readony`和`?`这两个能用在所有数据结构地方的修辞符
- 只能在上面使用了泛型中的`K in keyof T`这种模式属性周围才能使用增加移除修辞符，也就是必须要`in keyof`操作符

其实在映射类型中在前面直接添加`readonly`和在后面加`?`就是增加修辞符，只是省略了`+`号

```typescript
type ReadonlyAndPartial<T> = {
  +readonly [P in keyof T]+?: T[P]
}
type WritableAndPartial<T> = {
  -readonly [P in keyof T]-?: T[P]
}
```



#### 3.2.12 条件类型

条件类型的定义类似TypeScript语法中的三元操作符，使用extends操作符判断前者是否是后者的子类型之一，语法类似为`T extends U ? X:Y`

```typescript
type t<T> =	T extends string ? string:number
```

##### 3.2.12.1 分布式条件类型

当检测的类型为联合类型时，该类型就被叫做分布式条件类型，在实例化的时候TypeScript会自动分化为联合类型

```typescript
type TypeName<T> = T extends any? T:never
type Tyoe = TypeName<string|number> // string | number
```

在很多时候这种条件类型会能够帮助我们解决很多事情

```typescript
type TypeName<T> = 
T extends string ? string : 
T extends number ? number : 
T extends boolean ? boolean :
T extends undefined ? undefined : 
T extends () => void ? () => void :
object
type Type = TypeName<() => void> // () => void
type Type = TypeName<(string[]> // object
type Type = TypeName<(() => void) | string[]> // () => void | object
```



##### 3.2.12.2 条件类型推断

我们现在要实现一个功能，为一个泛型的类型中传入一个类型，如果是数组类型就返回数组中的一个元素的类型，如果不是数组类型就返回该类型

```typescript
type Type<T> = T extends any[] ? T[number] : T
type Test = Type<string[]> // string
type Test2 = Type<string> // string
```

而在TypeScript中，有一个专门做条件类型推断的关键字`infer`，`infer`是用来用做类型推断并赋值的，后面通常跟一个泛型变量，推断后的返回类型交给后面的泛型变量。`infer`可以专门用来推断出数组中元素的类型

```typescript
type Type<T> = T extends Array<infer U> ? U : T // 如果成立infer能推断出数组中元素的类型并且赋值给U
type Test = Type<string[]> // string 
type Test2 = Type<string> // string
// 效果同上
```



##### 3.2.12.3 预定义的条件类型

TypeScript 2.8 在`lib.d.ts`里增加了一些预定义的有条件类型：

- `Exclude<T, U>` -- 从`T`中剔除可以赋值给`U`的类型。
- `Extract<T, U>` -- 提取`T`中可以赋值给`U`的类型。
- `NonNullable<T>` -- 从`T`中剔除`null`和`undefined`。
- `ReturnType<T>` -- 获取函数返回值类型。
- `InstanceType<T>` -- 获取构造函数类型的实例类型。

```typescript
type T00 = Exclude<"a" | "b" | "c" | "d", "a" | "c" | "f">;  // "b" | "d"
type T01 = Extract<"a" | "b" | "c" | "d", "a" | "c" | "f">;  // "a" | "c"

type T02 = Exclude<string | number | (() => void), Function>;  // string | number
type T03 = Extract<string | number | (() => void), Function>;  // () => void

type T04 = NonNullable<string | number | undefined>;  // string | number
type T05 = NonNullable<(() => string) | string[] | null | undefined>;  // (() => string) | string[]

function f1(s: string) {
    return { a: 1, b: s };
}

class C {
    x = 0;
    y = 0;
}

type T10 = ReturnType<() => string>;  // string
type T11 = ReturnType<(s: string) => void>;  // void
type T12 = ReturnType<(<T>() => T)>;  // {}
type T13 = ReturnType<(<T extends U, U extends number[]>() => T)>;  // number[]
type T14 = ReturnType<typeof f1>;  // { a: number, b: string }
type T15 = ReturnType<any>;  // any
type T16 = ReturnType<never>;  // any
type T17 = ReturnType<string>;  // Error
type T18 = ReturnType<Function>;  // Error

type T20 = InstanceType<typeof C>;  // C
type T21 = InstanceType<any>;  // any
type T22 = InstanceType<never>;  // any
type T23 = InstanceType<string>;  // Error
type T24 = InstanceType<Function>;  // Error
```

**注意：**`Exclude`类型是建议的`Diff`类型的一种实现。使用`Exclude`这个名字是为了避免破坏已经定义了`Diff`的代码，并且感觉这个名字能更好地表达类型的语义。没有增加`Omit<T, K>`类型，因为它可以很容易的用`Pick<T, Exclude<keyof T, K>>`来表示(删除T中的K有的属性)，**但要注意TypeScript中3.5中已经加入了`Omit<T, K>`类型**



### 3.3 类型断言

**类型断言好比其它语言里的类型转换,但是不进行特殊的数据检查和解构, 它没有运行时的影响,只是在编译阶段起作用,TypeScript会假设程序员已经检查过**

**正则断言有两种方式:**

- **尖括号写法**

  ```typescript
  let someValue: any = "this is a string";
  //如果写的是any找长短的时候编译器是找不到length属性的,类型断言后就可以找到
  let strLength: number = (<string>someValue).length;
  ```

- **as语法**

  ```typescript
  let someValue: any = "this is a string";
  let strLength: number = (someValue as string).length;
  ```

**注意:**两种形式是等价的,但是当在TypeScript里使用JSX时,只有`as`语法断言是被允许的

#### 3.3.1 const断言

TypeScript 3.4引入了一种用于文字值的新构造，称为*const*断言。它的语法是类型断言，`const`代替类型名（例如`123 as const`）。当我们使用`const`断言构造新的文字表达式时，我们可以向语言发出信号：

- 该表达式中的任何文字类型都不应扩展（例如，不得从`"hello"`到`string`）
- 对象文字获取`readonly`属性
- 数组文字变成`readonly`元组

```ts
// Type '"hello"'
let x = "hello" as const;

// Type 'readonly [10, 20]'
let y = [10, 20] as const;

// Type '{ readonly text: "hello" }'
let z = { text: "hello" } as const;
```

**注意：**

- `const`断言只能立即应用于简单的文字表达式

    ```ts
    // Error! A 'const' assertion can only be applied to a
    // to a string, number, boolean, array, or object literal.
    let a = (Math.random() < 0.5 ? 0 : 1) as const;

    // Works!
    let b = Math.random() < 0.5 ?
        0 as const :
        1 as const;
    ```

- `const`上下文不会立即将表达式转换为完全不可变的

    ```ts
    let arr = [1, 2, 3, 4];

    let foo = {
        name: "foo",
        contents: arr,
    } as const;

    foo.name = "bar";   // error!
    foo.contents = [];  // error!

    foo.contents.push(5); // ...works!
    ```



### 3.4 类型推论

**TypeScript里,在有些没有明确指出类型的地方,类型推论会帮助提供类型**

```typescript
let x = 3;
//变量x的类型被推断为数字.这种推断发生在初始化变量和成员,设置默认参数值和决定函数返回值时
```

#### 3.4.1 最佳通用类型

**当需要从几个表达式中推断类型时候,会使用这些表达式的类型来推断出一个最合适的通用类型**

```typescript
let x = [0, 1, null];//这时x被推断为(null|number)[]
```

**由于最终的通用类型取自候选类型,有些时候候选类型共享相同的通用类型,但是却没有一个类型能做为所有候选类型的类型**

```typescript
class Animal{}
class Rhino extends Animal{}
class Elephant extends Animal{}
class Snake extends Animal{}

let zoo = [new Rhino(), new Elephant(), new Snake()];
//被推断为(Rhino | Elephant | Snake)[]类型
//我们想让zoo被推断为Animal[]类型,但是这个数组里没有对象是Animal类型的,因此不能推断出这个结果
//为了更正,需要明确指定类型
let zoo: Animal[] = [new Rhino(), new Elephant(), new Snake()];
```



#### 3.4.2 上下文类型

**TypeScript类型推论也可能按照相反的方向进行。 这被叫做“按上下文归类”。按上下文归类会发生在表达式的类型与所处的位置相关时**

```typescript
window.onmousedown = function(mouseEvent) {
//这个例子会报错,因为window.onmousedown已经为event对象设置过类型检查
    console.log(mouseEvent.button);  //<- Error
};
/*
	TypeScript类型检查器使用window.onmousedown函数的类型来推断右边函数表达式的类型。 因此,就能推断出mouseEvent参数的类型了。如果函数表达式不是在上下文类型的位置,mouseEvent参数的类型需要指定为any,这样也不会报错了
*/
window.onmousedown = function(mouseEvent: any) {//手动指定了参数类型上下文类型就会被忽略
    console.log(mouseEvent.button);  //<- Now, no error is given
};
```

**上下文归类会在很多情况下使用到。通常包含函数的参数,赋值表达式的右边,类型断言,对象成员和数组字面量和返回值语句。 上下文类型也会做为最佳通用类型的候选类型**

```typescript
function createZoo(): Animal[] {//在这里面Animal就是被作为最佳通用类型
    return [new Rhino(), new Elephant(), new Snake()];
}
```



### 3.5 类型兼容

> **该小节需要先把后面的知识学习完全再回来看**

**在TypeScript中另一大特性为类型兼容，TypeScript类型兼容性是基于结构子类型的，同时结构类型只使用其成员来描述类型**

#### 3.5.1 属性兼容性

在为对象赋值时，会检测对象中是否含有应该有的属性，同时也会检验额外的属性，**如果直接使用对象自变量进行赋值会报错，而如果先赋值给其他变量再给对象赋值就能通过检测。**

**官方说法：**如果认为S相对于T具有额外属性，首先S是一个`fresh object literal type`，并且S中含有T不期望存在的属性。我们这里可以简单的理解为`fresh object literal type`就是一个直接的对象自变量

**`fresh object literal types`失去freshness情况如下:**

- fresh类型数据被widened，结果数据的类型失去freshness
- 类型断言后产生的数据类型类型失去freshness

```typescript
interface Info {
  name: string
}

let info: Info
const info1 = { name: '张三' }
const info2 = { age: 18 }
const info3 = { name: '张三', age: 18 }

info = info1
info = info2 //报错，因为没有name字段
info = info3 // 不会报错
info = { name: '张三', age: 18 } // 直接给会报错
info = { name: '张三', age: 18 } as any // 失去freshness，不会报错
```

**注：**这种检测为递归检测，会对对应的变量的值进行检测

```typescript
interface Info {
  name: string
  info: { age: number }
}

let info: Info
const info1 = { name: '张三', info: { age: 18 } }
const info2 = { age: 18 }
const info3 = { name: '张三', age: 18 }

info = info1
// info = info2 报错，因为没有name字段
info = info3 // 报错，递归建材到info属性对象没有age属性
```



#### 3.5.2 函数兼容性

- **参数兼容**

  - **个数兼容：**函数的参数个数不同能够向下兼容，也就是参数个数少的函数能赋值给参数个数多的函数。但是反之就不能成立

      ```typescript
      let funcX = (num: number) => 0
      let funcY = (num: number, str: string) => 0

      funcY = funcX
      funX = funcY // 报错
      ```

      比如在TypeScript中我们使用数组的`forEach`方法的时候，我们传入的回调函数可以传1~3个参数，如果传多了就会报错，这就是使用回调函数赋值给`forEach`方法内部使用的案列

      ```typescript
      let arr: number[] = [1, 2, 3]

      arr.forEach((v, i, a) => {
        console.log(v)
      })

      arr.forEach(v => {
        console.log(v)
      })
      ```
    
  - **名称与类型兼容：**参数的名称没必要是相同的，只要与对应的参数类型一致就行，所以要确保参数的类型一致才能赋值
  
      ```typescript
      let funcX = (n: number) => 0
      let funxY = (num: number, str: string) => 0
       
      funxY = funcX
      ```
  
      ```typescript
      let funcX = (n: string) => 0
      let funxY = (num: number, str: string) => 0
       
      funxY = funcX // 报错，因为对应参数类型不一致
      ```
  
  - **可选参数兼容：**被赋值变量上有额外的可选参数不会出错，赋值变量的可选参数在被复制变量里没有对应的参数也不会出错
  
      ```typescript
      let funcX = (ant: number,address?:string,target?:string) => 0
      let funcY = (num: number, str?: string) => 0
        
      funcY = funcX // funcX虽然有两个额外参数，但它们都是可选的，所以不会报错
      ```
  
  - **参数双向协变兼容：**默认是兼容的，可以开启严格模式使其不兼容参数双向协变
  
      ```typescript
      let funcX = (num: string | number) => 0
      let funcY = (num: number) => 0
      
      // 默认是兼容的，可以开启严格模式使其不兼容参数双向协变，因为在使用时默认是使用的原来创建函数的类型，funcY只能传入number类型，而funcX是可以接受string和number类型的
      funcY = funcX
      funcX = funcY
      ```
  
- **返回值类型兼容：**函数的返回值同新版的参数双向协变一样，需要源函数的返回值需要能够赋值给目标函数返回值类型

  ```typescript
  let funcX = (): string | number => 0
  let funxY = (): number => 0
  
  funcX = funxY
  funY = funcX // 报错
  ```

  **注：**如果目标函数的返回值类型是void，那么源函数返回值可以是任意类型

  ```typescript
  let funcX = (): void => {}
  let funxY = (): number => 0
  
  funcX = funxY // 不会报错
  ```

- **函数重载：**对于有重载的函数，源函数的每个重载都要在目标函数上找到对应的函数签名（当然也包括必须要函数重载的情况数量一样），规则同上

  ```typescript
  function merge(arg1: number, arg2: number): number
  function merge(arg1: string, arg2: string): string
  function merge(arg1: any, arg2: any): any {
    return arg1 + arg2
  }
  
  function sum(arg1: number, arg2: number): number
  function sum(arg1: any, arg2: any): any {
    return arg1 + arg2
  }
  
  let func = merge
  func = sum // 报错
  ```



#### 3.5.3 枚举兼容性

枚举兼容性在之前已经有提到过，枚举的兼容性可以体现在除了可以被赋值为枚举成员，还能够被赋值给纯数字（**必须要枚举成员中有数字类型的成员存在**），但是不能被赋值为其他的枚举变量或者字符串

```typescript
enum Status {
  On,
  Off
}
enum Animal {
  Dog,
  Cat
}
let s = Status.On
s = 1 // 不会报错
s = Animal.Cat // 报错
s = "1" // 报错
```



#### 3.5.4 类兼容性

TypeScript类和接口的兼容性非常类似，但是类分实例部分和静态部分。比较两个类类型数据时，**只有实例成员会被比较，静态成员和构造函数不会比较**

```typescript
class Animal {
  public static age: number
  constructor(public name: string) {}
}

class People {
  public static age: string
  constructor(public name: string) {}
}

class Food {
  constructor(public name: number) {}
}

let animal: Animal = new People('张三') // 正确，只会比较实例属性方法
let animal2: Animal = new Food('食物') // 报错，因为实例成员类型不正确

```

**注：**实例属性方法比较时不会检验多余的属性和方法

```typescript
class Animal {
  public static age: number

  constructor(public name: string) {}
}

class People {
  public static age: string
  public gender: string = '男' // 多余的属性
  constructor(public name: string) {}
}

let animal: Animal = new People('张三') // 不会报错
```

##### 3.5.4.1 类私有成员兼容

私有成员会影响兼容性判断，如果目标类型包含一个私有成员（或受保护类型），那么源类型必须包含来自同一个类的这个私有成员。 **允许子类赋值给父类，但是不能赋值给其它有同样类型的类**

```typescript
class Parent {
  constructor(private age: number) {}
}

class Child extends Parent {
  constructor(age: number) {
    super(age)
  }
}

class Other {
  constructor(private age: number) {}
}

let c: Parent = new Child(18) 
let other: Parent = new Other(18) // 报错
// 上面的private换成protected也一样
```



#### 3.5.5 泛型兼容性

TypeScript是结构性的类型系统，泛型的类型参数影响数据的成员，所以即使进行了泛型的限定如果没有真正的使用给结构中的元素是对数据没有任何影响的

```typescript
interface Empty<T> {
}
let obj:Empty<number> = {} // 不会报错
```

所以能够这样：

```typescript
interface Empty<T> {}
let x: Empty<number>
let y: Empty<string> = {}
x = y
```

但是如果在内部使用了给了成员就无法赋值了

```typescript
interface Data<T> {
  data: T
}
let x: Data<number>
let y: Data<string> = { data: 'str' }

x = y
```



### 3.6 声明合并

TypeScript中有些独特的概念可以在类型层面上描述JavaScript对象的模型。 这其中尤其独特的一个例子是`“声明合并”`的概念。`“声明合并”`是指编译器将针对同一个名字的两个独立声明合并为单一声明。 合并后的声明同时拥有原先两个声明的特性。 任何数量的声明都可被合并，并不局限于两个声明

#### 3.6.1 声明的概念

**TypeScript中的声明会创建以下三种实体之一：命名空间，类型或值。** 创建命名空间的声明会新建一个命名空间，它包含了用`.`符号来访问时使用的名字。 创建类型的声明是用声明的模型创建一个类型并绑定到给定的名字上。 最后，创建值的声明会创建在JavaScript输出中看到的值

| Declaration Type（声明类型） | Namespace（创建了命名空间） | Type（创建了类型） | Value（创建了值） |
| :--------------------------- | :-------------------------: | :----------------: | :---------------: |
| Namespace                    |              √              |                    |         √         |
| Class                        |                             |         √          |         √         |
| Enum                         |                             |         √          |         √         |
| Interface                    |                             |         √          |                   |
| Type Alias（类型别名）       |                             |         √          |                   |
| Function                     |                             |                    |         √         |
| Variable                     |                             |                    |         √         |



#### 3.6.2 接口合并

**最简单也最常见的声明合并类型是接口合并。 从根本上说，合并的机制是把双方的成员放到一个同名的接口里**

```typescript
interface Box {
    height: number;
    width: number;
}

interface Box {
    scale: number;
}

let box: Box = {height: 5, width: 6, scale: 10};
```

**注意：**

- 接口的非函数的成员应该是唯一的。如果它们不是唯一的，那么它们必须是相同的类型。如果两个接口中同时声明了同名的非函数成员且它们的类型不同，则编译器会报错

    ```typescript
    interface Box {
        height: number;
    }

    interface Box {
        height: string; // 这样是报错的，height只能是number
        width: number;
    }
    ```

- 对于函数成员，每个同名函数声明都会被当成这个函数的一个重载

  **注意：**当接口 `A`与后来的接口 `A`合并时，后面的接口具有更高的优先级

  ```typescript
  interface Cloner {
      clone(animal: Animal): Animal;
  }
  
  interface Cloner {
      clone(animal: Sheep): Sheep;
  }
  
  interface Cloner {
      clone(animal: Dog): Dog;
      clone(animal: Cat): Cat;
  }
  ```

  **合并后的接口：**

  ```typescript
  interface Cloner {
      clone(animal: Dog): Dog;
      clone(animal: Cat): Cat;
      clone(animal: Sheep): Sheep;
      clone(animal: Animal): Animal;
  }
  ```

  可以看出，**每组接口里的声明顺序保持不变，但各组接口之间的顺序是后来的接口重载出现在靠前位置**

  但是，这个规则有一个例外是当出现特殊的函数签名时。 如果签名里有一个参数的类型是**单一的字符串字面量（比如，不是字符串字面量的联合类型）**，那么它将会被提升到重载列表的最顶端

  ```typescript
  interface Document {
      createElement(tagName: any): Element;
  }
  interface Document {
      createElement(tagName: "div"): HTMLDivElement;
      createElement(tagName: "span"): HTMLSpanElement;
  }
  interface Document {
      createElement(tagName: string): HTMLElement;
      createElement(tagName: "canvas"): HTMLCanvasElement;
  }
  ```

  **合并后的接口：**

  ```typescript
  interface Document {
      createElement(tagName: "canvas"): HTMLCanvasElement;
      createElement(tagName: "div"): HTMLDivElement;
      createElement(tagName: "span"): HTMLSpanElement;
      createElement(tagName: string): HTMLElement;
      createElement(tagName: any): Element;
  }
  ```



#### 3.6.3 命名空间合并

**对于命名空间的合并，模块导出的同名接口进行合并，构成单一命名空间内含合并后的接口。**对于命名空间里值的合并，如果当前已经存在给定名字的命名空间，那么后来的命名空间的导出成员会被加到已经存在的那个模块里

```typescript
namespace Animals {
    export class Zebra { }
}

namespace Animals {
    export interface Legged { numberOfLegs: number; }
    export class Dog { }
}
```

**等同于：**

```typescript
namespace Animals {
    export interface Legged { numberOfLegs: number; }

    export class Zebra { }
    export class Dog { }
}
```

**注意：** 非导出成员仅在其原有的（合并前的）命名空间内可见。这就是说合并之后，从其它命名空间合并进来的成员无法访问非导出成员

```typescript
namespace Animal {
    let haveMuscles = true;

    export function animalsHaveMuscles() {
        return haveMuscles;
    }
}

namespace Animal {
    export function doAnimalsHaveMuscles() {
        return haveMuscles;  // Error, because haveMuscles is not accessible here
    }
}
/*
因为 haveMuscles并没有导出，只有 animalsHaveMuscles函数共享了原始未合并的命名空间可以访问这个变量。 doAnimalsHaveMuscles函数虽是合并命名空间的一部分，但是访问不了未导出的成员
*/
```



#### 3.6.4 命名空间与类和函数和枚举类型合并

**命名空间可以与其它类型的声明进行合并。 只要命名空间的定义符合将要合并类型的定义。合并结果包含两者的声明类型。** TypeScript使用这个功能去实现一些JavaScript里的设计模式。

- **合并命名空间和类：**这让我们可以表示内部类

    ```ts
class Album {
        label: Album.AlbumLabel;
    }
    namespace Album {
        export class AlbumLabel { }
    }
    ```
    
    **合并规则与上面合并命名空间的规则一致**，我们必须导出 `AlbumLabel`类，好让合并的类能访问。 合并结果是一个类并带有一个内部类。 也可以使用命名空间为类增加一些**静态属性**

    ```typescript
class Person {
        constructor(public name:string){
    	}
    }
    namespace Person {
        export age = 18
    }
console.log(Person.age) // 18
    ```
    
- **合并命名空间和函数：**TypeScript使用声明合并来达到这个目的并保证类型安全

    ```ts
    function buildLabel(name: string): string {
        return buildLabel.prefix + name + buildLabel.suffix;
    }
    
    namespace buildLabel {
        export let suffix = "";
        export let prefix = "Hello, ";
    }
    
    console.log(buildLabel("Sam Smith"));
    ```

- **合并命名空间和枚举：**名空间可以用来扩展枚举型

    ```ts
    enum Color {
        red = 1,
        green = 2,
        blue = 4
    }
    // 给Color枚举对象多添加了一个属性mixColor，在这里的效果与给对象赋值是一样的
    namespace Color {
        export function mixColor(colorName: string) {
            if (colorName == "yellow") {
                return Color.red + Color.green;
            }
            else if (colorName == "white") {
                return Color.red + Color.green + Color.blue;
            }
            else if (colorName == "magenta") {
                return Color.red + Color.blue;
            }
            else if (colorName == "cyan") {
                return Color.green + Color.blue;
            }
        }
    }
    ```
    
- **接口和类：**接口可以和类进行合并，合并后类的实例需要具有接口定义的属性或方法

    ```typescript
    class Person {}
    
    interface Person {
      name: string
    }
    // 这种声明合并常用在使用类装饰器的时候消除报错
    const c = new Person()
    console.log(c.name) // 不报错，因为已经合并了
    console.log(c.age) // 报错，Person实例没有age属性
    ```

    

## 4.TypeScript中的函数

### 4.1 函数声明

- **函数声明法**

  ```typescript
  function fun1():string{//指定返回类型
      console.log("string");
      return "string";
  }
  fun1();
  
  function fun2():void{
      console.log("void");
  }
  fun2();
  ```

- **函数表达式**

  ```typescript
  let fun=function():number{ //其实只对一般进行了校验
      console.log(123);
      return 123;
  }
  /*
  	完整写法：在后面细讲规则
  	let fun:()=>number = function():number{
           console.log(123);
           return 123;
  	}
  */
  
  fun();
  ```

- **匿名函数**

  ```typescript
  let result:number=(function():number{
      return 10;
  })()
  console.log(result);//10
  ```

**注意:**

- TypeScript中的函数也是可以做类型校验的,不过在返回返回值时与箭头函数返回返回值类似

    ```typescript
    //(str:string)=>number 就是对于函数的类型校验，是函数类型Function的深度校验的写法
    let fun: (str:string) => number = function(str:string): number {
      console.log(str);
      return 123;
    };
    fun("string");

    //返回一个具有返回值的函数
    let fun: (str:string) => (()=>number) = function(): number {
      return ()=>{
        return 123;
      };
    };
    /*
        但是一般都只写一边,因为TypeScript有根据上下文的类型推断机制,可以写更少的代码
    */
    ```
    
- TypeScript中的函数即使没有返回值也应该显示声明返回值为`void`类型，否则为`any`类型

    ```typescript
    const a = function():void = {
        console.log("function")
    } 
    ```

    

### 4.2 函数传参

**在TypeScript中对函数传参,形参也需要指定对应类型,如果实参传入类型错误会报错,但是形参也可以指定多种数据类型,也可以是类**

```typescript
function fun(name:string,age:number):string{
    return `${name}----${age}`;
}
console.log(fun("张三",18));
```

```typescript
function fun(name:string,age:number|string):string{
    return `${name}----${age}`;
}
console.log(fun("张三","18"));
```

#### 4.2.1 可选参数

**在JS中函数的形参和实参可以不一样,但是在TypeScript中必须要一一对应,如果不一样就必须要配置可选参数**

**注意:**可选参数必须配置到参数的最后面,可以有多个可选参数

```typescript
//通过给最后的参数加上?来设置可选参数
function fun(name:string,age?:number):string{
    if(age){
     	return `${name}----${age}`;   
    }else{
        return `${name}----年龄保密`;
    }
}
console.log(fun("张三",18));//张三----18
console.log(fun("李四"));//李四----年龄保密
```



#### 4.2.2 默认参数

**在TypeScript中默认参数的传入和JS中一样,如果没有传入该参数那么默认就会使用默认参数**

**注意:**默认参数和可选参数不能在同一个形参变量上使用,默认参数可以不用写在参数的最后，但是如果不是写在最后而是写在前面的参数上又想要使用默认参数，可以给可选参数的位置传入`undefined`，这样函数就会使用默认参数

```typescript
//直接给形参赋值
function fun(name: string = "王五", age?: number): string {
  if (age) {
    return `${name}----${age}`;
  } else {
    return `${name}----年龄保密`;
  }
}
console.log(fun("张三", 18)); //张三----18
console.log(fun()); //王五----年龄保密
```

```typescript
//直接给形参赋值
function fun(name: string = "王五", age: number): string {
    return `${name}----${age}`;
}
console.log(fun(undefined, 18)); //王五----18
```



#### 4.2.3 剩余参数

**在TypeScript中的剩余参数也是和JS中的一样,通过扩展运算符(...)接受剩余参数**

**注：**剩余参数可以看做是多个可选参数组成的数组

```typescript
//通过扩展运算符传入参数
function sum(a:number,b:number,...result: number[]): number {
  let sum: number = a+b;
  sum = result.reduce(function(prev: number, next: number): number {
    return prev + next;
  });
  return sum;
}

console.log(sum(0,1,1, 2, 3, 4, 5));//16
```



### 4.3 函数重载

**在TypeScript中通过为同一个函数提供多个函数类型定义来实现多种功能的目的**

**注：**

- 在JS中,如果出现了同名方法,在下面的方法会替换掉上面的方法
- 在TypeScript中的函数重载不同于`Java`、`C++`这种，而是要在最后一个函数（该函数被叫做函数实体）中通过判断类型来做到

```typescript
//函数重载
function fun(name:string):string;
function fun(age:number):string;
function fun(str:any):any{//如果要进行函数重载判断,那么这个形参和返回值的类型必须要包含上面函数
    //根据传入参数的不同进入不同的重载函数中,虽然这里的类型为any,但主要是为了包含上面,传参由上面判断
    if(typeof str==="string"){
        return "我是"+str;
    }else{
        return "我的年龄为"+str;
    }
}
//fun函数能传入的参数只能是string和number,传入其他参数会报错
console.log(fun("张三"));//我是张三
console.log(fun(18));//我的年龄为18
console.log(fun(true));//错误写法,报错
```

```typescript
//函数重载
function fun(name:string):number;//返回不同的类型
function fun(name:string,age:number):string;
function fun(name:any,age?:any):any{//也可以传入可选参数,根据参数的不同进入不同的重载函数
    if(age){
        return "我是"+name+",年龄为"+age;
    }else{
        return 123;
    }
}

console.log(fun("张三",18));//我是张三,年龄为18
console.log(fun("张三"));//123
console.log(fun("张三",true));//错误写法,在重载函数中无法找到对应函数
```



### 4.4 箭头函数

**在TypeScript中箭头也是和JS中的一样的用法**

```typescript
setTimeout(():void=>{
    console.log(123);
})
```



### 4.5 this

**TypeScript中的this是可以在函数传参数时手动指定其类型限定,这个类型限定可以为TypeScript提供推断依据**

```typescript
class Animal{
    name:string="动物";
    /* 
	一般来说可以直接指定this的指向，在以前的版本如果不知道TypeScript是不知道this应该有什么属性的，this显示为any类型，只有在编译时才会报错（在新版中已经可以不用对this进行类型指向了，默认是当前的函数所指向的对象）
	*/
    eat(this:Animal,food:string){// 注意this不会占据参数的位置，这个函数实际只有一个参数，只用传入一个参数
        console.log(this.name);
        console.log(food);
        console.log(this.age);
        // TypeScript会报错，因为Anmial的实例没有age属性
    }
}

let a = new Animal();
a.eat("food");
```

```typescript
class Animal {
  name: string = "动物";
  eat(this: void) {//而如果指定的void再调用下面的则会报错,因为类型不匹配了
    console.log(this.name);
  }
}

let a = new Animal();
a.eat();
```



## 5.TypeScript中的类

### 5.1 类的定义

**TypeScript中的类和JS中类的定义基本一样，只是做了额外的类型检验**

#### 5.1.1 实例类类型

```typescript
class Person {
  name: string;//和JAVA类似,要先在这声明对象中拥有的属性和类型
  /*该属性定义相当于public name:string;,只不过省略了public,下面再做解释*/
  constructor(n: string) {
    this.name = n;//和ES6一样,传入构造函数,只不过需要在前面先定义this的属性
  }

  run(): void {
    console.log(this.name);
  }
}
/*
	类也可以写成表达式写法：
	const Person = class {
	}
*/

let p:Person = new Person("张三");//可以对实例类型变量进行类型的定义,也可以默认不写为any
p.age=18;//报错,类只允许有name属性
p.run();//张三
```

**注意:**

- **constructor函数后面不能加上返回值的修辞符**,否则会报错,可以看作是固定写法
- 函数定义完成后不需要加上符号分割



#### 5.1.2 静态类类型

**上面直接把类作为类型的修辞符只是用作将该类型限定为实例的类类型，如果要限定为类本身需要使用`typeof`**

```typescript
class Person {
  static max:number = 100;
  name: string;
  constructor(n: string) {
    this.name = n;
  }

  run(): void {
    console.log(this.name);
  }
}

let Person2:typeof Person=Person;//把Person类赋值Person2
// 当然这种赋值实际上是把Person对象给了Person2
console.log(Person === Person2);// true

//typeof Person是表明该对象是一个Person类的类型,而不是Person的实例类型
Person2.max = 150; 
// 可以直接在类上修改原有的属性，这样是修改静态属性
Person2.min = 0;// 报错,因为Person类没有min静态属性
let p2:Person=new Person2("李四");//因为Person2也是Person类型的类,所以可以这样实例对象
p2.run();
console.log(Person2.max);// 150
```



### 5.2 类的继承

**类的继承和接口不同,一个类只能继承一个父类**

```typescript
class Person {
    name: string;
    constructor(n: string) {
        this.name = n;
    }
    run(): void {
        console.log(this.name);
    }
}

class Student extends Person {//类的继承可以说和ES6完全一样,只是constructor需要指定类型
    age:number;
    constructor(name: string,age:string) {
        super(name);
        this.age=age;
    }
    work() {
        console.log(this,age);
    }
}
let s = new Student("李四",18);
s.run();
s.work();//18
```

**注意:**如果子类里的方法和父类方法名一致,那么在使用的时候会使用子类的方法,不会使用父类的方法了

```typescript
class Person {
  name: string;
  constructor(n: string) {
    this.name = n;
  }
  run(): void {
    console.log(this.name);
  }
}

class Student extends Person {
  constructor(name: string) {
    super(name);
  }
  run() {
    super.run(); //李四,super可以在子类中代表父类
    console.log(this.name + "子类方法");
  }
}
let s = new Student("李四");
s.run(); //李四子类方法
```



### 5.3 类的修辞符

**在TypeScript中定义属性或方法的时候为我们提供了四种修辞符**

- **public:**公有类型,在类、子类、类外部都可以对其访问

  **注:**这里的在类外部访问就是在实例化过后能在类的外界单独打印该属性,而不是只在内部的方法中使用该属性

- **protected:**保护类型,在类、子类里可以对其进行访问,但是在类的外部无法进行访问

- **private:**私有类型,在类里面可以访问,在子类和类的外部都无法访问，在JS中要使用私有属性一般只有用`_属性/方法`、`模块外部定义内部使用`和`Symbol定义属性的方法来使用`，而在TypeScript中更加简便

  + TypeScript使用的是结构性类型系统,当我们比较两种不同的类型时,并不在乎它们从何处而来,如果所有成员的类型都是兼容的,我们就认为它们的类型是兼容的
  + 当我们比较带有 `private`或 `protected`成员的类型的时候,如果其中一个类型里包含一个`private`成员，那么只有当另外一个类型中也存在这样一个 `private`成员,并且它们都是来自同一处声明时,我们才认为这两个类型是兼容的。对于 `protected`成员也使用这个规则

  ```typescript
  class Animal {
      private name: string;
      constructor(theName: string) { this.name = theName; }
  }
  
  class Rhino extends Animal {
      constructor() { super("Rhino"); }
  }
  
  class Employee {
      private name: string;
      constructor(theName: string) { this.name = theName; }
  }
  
  let animal = new Animal("Goat");
  let rhino = new Rhino();
  let employee = new Employee("Bob");
  
  animal = rhino;//能够赋值,因为两者兼容,都是同一个私有属性name
  animal = employee; // 错误: Animal 与 Employee 不兼容,name的私有属性不兼容
  ```

- **readonly:**只读类型,可以使用 `readonly`关键字将属性设置为只读的, 只读属性必须在声明时或构造函数里被初始化。同时`readonly`修辞符是可以和其他三个修辞符一起存在的，注意`readonly`必须要放在第二个位置，只写`readonly`默认在前面加了`public`

  ```typescript
  class Octopus {
      readonly name: string;
      readonly numberOfLegs: number = 8;
      constructor (theName: string) {
          this.name = theName;
      }
  }
   
  let dad = new Octopus("Man with the 8 strong legs");
  dad.name = "Man with the 3-piece suit"; // 错误! name 是只读的.
  ```

  

**注意：**

- 如果属性不添加修饰符,默认为公有属性(public)

    ```typescript
    //public
    class Person {
        public name: string;
        constructor(n: string) {
            this.name = n;
        }
        public run(): void {
            console.log(this.name);
        }
    }

    class Student extends Person {
        constructor(name: string) {
            super(name);
        }
    }
    let s = new Student("李四");
    console.log(s.name);//李四
    s.run();//李四
    ```

    ```typescript
    //protected
    class Person {
        protected name: string;
        constructor(n: string) {
            this.name = n;
        }
        public run(): void {//如果这个方法是protected下面的s.sun()也会报错
            console.log(this.name);
        }
    }

    class Student extends Person {
        constructor(name: string) {
            super(name);
        }
    }
    let s = new Student("李四");
    console.log(s.name);//报错
    s.run();//李四
    ```

* 如果构造函数也可以被标记成 `protected`, 意味着这个类不能在包含它的类外被实例化,但是能被继承

    ```typescript
    class Person {
        protected name: string;
        protected constructor(theName: string) { this.name = theName; }
    }
    // Employee 能够继承 Person
    class Employee extends Person {
        private department: string;

        constructor(name: string, department: string) {
            super(name);
            this.department = department;
        }

        public getElevatorPitch() {
            return `Hello, my name is ${this.name} and I work in ${this.department}.`;
        }
    }
    let howard = new Employee("Howard", "Sales");
    let john = new Person("John"); // 错误:因为'Person' 的构造函数是被保护的.
    ```

    ```typescript
    //private
    class Person {
        private name: string;
        constructor(n: string) {
            this.name = n;
        }
        run(): void {
            console.log(this.name);
        }
    }

    class Student extends Person {
        constructor(name: string) {
            super(name);
        }
        work():void{
            console.log(this.name);
        }
    }
    let s = new Student("李四");
    console.log(s.name);//报错
    s.work();//报错
    s.run();//李四,因为run方法是Person内部的,可以使用私有属性
    ```

- **在子类中通过`super`调用父类原型的属性和方法时也只能够访问到父类的`public`和`protected`方法，否则会报错**



#### 5.3.1 参数属性

参数属性通过给构造函数参数前面添加一个访问限定符来声明。 使用 `private`限定一个参数属性会声明并初始化一个私有成员,对于 `public`和 `protected`和`readonly`来说也是一样

**总的来说,这种写法是上面先声明又赋值属性的简便写法,可以直接通过这种写法改写上方先先在前面声明属性的写法,构造函数中也可以什么都不写**

- 声明了一个构造函数参数及其类型
- 声明了一个同名的公共属性
- 当我们 new 出该类的一个实例时,把该属性初始化为相应的参数值

```typescript
class Octopus {
    readonly numberOfLegs: number = 8;
    constructor(readonly name: string) {//通过这种写法改变上面对应readonly的例子
    }
}
```



### 5.3.2 可选属性

**与函数的可选参数一样，在类中也可以定义类的可选属性**

```typescript
class Person {
  name?: string
  constructor(n?: string) {
    this.name = n
  }
  run(): void {
    console.log(this.name)
  }
}
/* 等同下面的写法 */
class Person {
  name: string | undefined
  constructor(n?: string) {
    this.name = n
  }
  run(): void {
    console.log(this.name)
  }
}
```



### 5.4 寄存器

**TypeScript中也可以对一个属性时用get和set方法对一个属性内部的获取和赋值进行拦截**

```typescript
let passcode = "secret passcode";

class Employee {
    private _fullName: string;
    get fullName(): string {//对fullName属性进行拦截
        return this._fullName;
    }
    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    alert(employee.fullName);
}
```

**注意:只带有 `get`不带有 `set`的存取器自动被推断为`readonly`类型的属性**



### 5.5 静态方法和属性

```typescript
class Person {
  public name: string;
  constructor(n: string) {
    this.name = n;
  }
  run(): void {
    console.log(this.name);
  }
}

class Student extends Person {
  static name1: string; //设置静态属性
  constructor(name: string) {
    super(name);
    Student.name1 = this.name; //赋值
  }
  static work(): void {//静态方法
    console.log(Student.name1);
  }
  work(): void {
    console.log(Student.name1);
  }
}
let s = new Student("李四");
console.log(Student.name1); //李四
Student.work();
s.work(); //李四
```



### 5.6 抽象类

**TypeScript中的抽象类是提供其他类继承的基类,不能直接被实例化,只能被其他类所继承**

**用`abstract`关键字定义抽象类和抽象类中的抽象方法或属性**,抽象类中的抽象方法**不包含具体实现**,但是必须要在派生类,也就是**继承的类中实现抽象方法,抽象属性不需要赋值**.并且**继承的类不能够扩展自己的方法和属性**

**总的来说,抽象类和抽象方法只是用来定义一个标准,而在其子类在必须要实现这个标准,并且不能扩展抽象类中没有的标准,否则会报错**

**注意:**用`abstract`声明的抽象方法只能放在抽象类中,否则会报错

```typescript
abstract class Department {
	abstract age:number;
    constructor(public name: string) {//参数属性的一个应用
        this.name = name;
    }
    
    printName(): void {
        console.log('Department name: ' + this.name);
    }

    abstract printMeeting():void;// 必须在派生类中实现
}

class AccountingDepartment extends Department {
    public age:number=18;
    constructor() {
        super('Accounting and Auditing');// 在派生类的构造函数中必须调用 super()
    }
    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }
    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}

let department: Department; // 允许创建一个对抽象类型的引用
department = new Department(); // 错误: 不能创建一个抽象类的实例
department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值
console.log(department.age);//18
department.printName();
department.printMeeting();
/*
错误: 方法在声明的抽象类中不存在（因为department是抽象类型，如果是直接写的AccountingDepartment类型是不会报错的）
*/
department.generateReports(); 
```



## 6.TypeScript中的接口

**接口是在面向对象编程中一种规范的定义,它定义了行为和动作的规范,起一种限制的作用,只限制传入到接口的数据**

TypeScript中的接口类似于JAVA,同时还增加了更灵活的接口类型,包括属性、函数、可索引和类等

**注意：**不要把接口看做是一个对象字面量，而更像是一个代码块，在其中每个人属性或方法的限制可以用逗号、分号甚至是直接用换行（不写分号逗号，但是必须要隔行书写）隔开，如果写在一行就必须用逗号或分号隔开

### 6.1 属性类型接口

- **属性类接口一般用作对于json对象的约束(下面的代码还没有使用接口)**

  ```typescript
  //ts定义方法中传入参数就是一种接口
  function print1(str: string): void {
    console.log(str); //约束只能且必须传入一个字符串参数
  }
  print1("string");
  
  /*
  对json对象进行约束，这是用了带有调用签名的对象字面量，其实仔细一看就像是匿名接口
  */
  function print2(obj: { name: string; age: number }): void {
    console.log(obj); //约束只能传有带有name和age属性的对象
  }
  print2({ name: "张三", age: 18 });
  
  function print3(obj: { name: string; age: 18 }): void {
    console.log(obj); //约束只能传有带有name和age属性的对象,并且age必须为18
  }
  print3("张三",19);//报错
  print3("张三",18);
  ```

- **对批量方法进行约束:使用接口**

  通过`interface`关键词对接口进行定义

  ```typescript
  interface FullName {
    firstName: string; //注意这里要;
    secondName: string;
  }
  /*
  	加入一个用法
  	let a: FullName['firstName'];//显示a为string类型，因为接口中的值可以单独获取来得到类型
  */
  
  function printName(name: FullName): void {
    console.log(name.firstName, name.secondName);
  }
  let obj={//属性的位置可以不一样
      firstName:"张",
      secondName:"三"
  }
  printName(obj);//传入对象必须有firstName和secondName
  
  let obj2={
      firstName:"李",
      secondName:"四",
      age:18
  }
  function printInfo(info: FullName): void {//使用接口可以对批量的函数进行约束,并且内部职能
    console.log(info.firstName + info.secondName + info.age);
  }
  // 使用这种方式TypeScript不会进行类型检验
  printInfo(obj2);//原则上只能传入只含有firstName和secondName的对象,但是如果写在外面传入也不会报错
  /*
  	但是上面这种方法在传入参数的时候不会报错,但是在函数内部使用info.age的时候就会报错,因为接口内部没有设置age属性,如果不想报错,函数内部使用形参的属性必须是只能有接口定义的
  */
  printInfo({
      firstName:"李",
    secondName:"四",
      age:18
});//通过这种方式传入就会直接报错
  ```
  
  **可选属性接口:**和函数的传参一样,可以在接口处用`?`代表可选接口属性
  
  ```typescript
  interface FullName {
    firstName: string; //注意这里要;结束
    secondName?: string;
  }
  function printName(name: FullName): void {
    console.log(name.firstName, name.secondName);//张 undefined
  }
  
  printName({
        firstName:"张"
  })
  ```

  **案例:**利用TS封装ajax请求

  ```typescript
  interface Config {
    type: string;
    url: string;
    data?: string;
    dataType?: string;
  }
  
  function ajax(config: Config) {
    let xhr: XMLHttpRequest = new XMLHttpRequest();
    xhr.open(config.type, config.url, true);
    xhr.send(config.data);
  
    xhr.onreadystatechange = function() {
      if (xhr.readyState === 4 && xhr.status === 20) {
        if (config.dataType.toLowerCase() === "json") {
          console.log(JSON.parse(xhr.responseText));
        } else {
          console.log(xhr.responseText);
        }
      }
    };
  }
  //由于接口的要求,必须传入type和url
  ajax({
    type: "get",
    data: "name=张三",
  url: "http://www.baidu.com/",
    dataType: "json"
  });
  ```

  

### 6.2 函数类型接口

**函数类型接口用于对方法传入的参数和返回值进行约束,可通过接口进行批量约束**

```typescript
//加密的函数类型接口
interface encrypt{
    (key:string,value:string):string
    a:string
}

let md5:encrypt=function(key:string,value:string):string{
    //函数必须是两个参数,并且类型对应接口,同时返回值必须是接口的返回值string
    return key+"---"+value;
}

console.log(md5("name","张三"))
```

**注:**函数类接口和类类接口类型区别在于函数类接口不用写函数名,只需要现在后面的参数返回值等,而类类型接口需要限制方法名和属性等



### 6.3 可索引类型接口

**可索引接口通常用作对数组和对象进行约束(但是这个接口不常用)**

```typescript
//对数组使用
interface Arr{
    [index:number]:string;//定义索引必须为number,值为string,否则会报错
}
let arr:Arr=["123","456"];
/*
	其实该接口的用法同数组指定类型的定义
	let arr:number[]=[1,2,3]
	let arr:Array<string>=["123","456"]
*/
//对对象使用,想要约束对象的属性值时可以使用
interface Obj{
    [index:string]:string;//定义索引必须为string,值为string,否则会报错
}
let obj:Obj={name:"张三",age:"20"};//age不能是number
```

```typescript
//可索引接口也可以用来对一个属性接口进行额外对象的属性检验，可以用这种方式来跳过属性检查
interface Arr{//该接口可以限制一个对象必须有color和width,还可以有其他的属性
    color:string;
    width:number;
    [propName:string]:any;
}
```

**注意：**在同时使用`string`和`number`类型的可索引接口时，**number类型的索引对应的值必须是string类型的子类型或同级类型，**否则会报类型出错误

```typescript
class Animal {
  name: string
  constructor(n: string) {
    this.name = n
  }
}

class Dog extends Animal {
  breed: string
  constructor(n: string, b: string) {
    super(n)
    this.breed = b
  }
}

class Cat extends Animal {
  breed: string
  constructor(n: string, b: string) {
    super(n)
    this.breed = b
  }
}

class Cat extends Animal {
  breed: string
  constructor(n: string, b: string) {
    super(n)
    this.breed = b
  }
}

interface NotOkay {
  [x: number]: Animal // 在这会报错，数字索引类型“Animal”不能赋给字符串索引类型“Dog”
  [x: string]: Dog
}
/*
	实际上两者都存在数字索引最后是被转换为了string类型的，比如传入1实际上时'1'，相当于将Animal类型的值转换为了Dog，而Dog是Animal类型的子类型，当然不能够父类型转换为子类型，而如果数字索引为其他的string、number等基础类型一样会报错，因为不是Dog的子类型
*/
// 下面两种都不会报错
interface NotOkay {
  [x: number]: Cat // 这里不会报错是因为Cat拥有Dog相同的方法
  [x: string]: Dog
}
/*
interface NotOkay {
  [x: number]: Bird // 报错，没有breed方法
  [x: string]: Dog
}
*/

interface NotOkay {
  [x: number]: Dog
  [x: string]: Animal
}
```



### 6.4 类类型接口

#### 6.4.1 实例类接口

**实例类类型接口主要用于对类的约束,和抽象类相似**

**注意：**使用实例类接口只会对实例的属性进行限制，不过对类的静态属性进行限制（包括构造器函数constructor，即使写了想对应的限制属性也不会起到作用，要限制需要使用构造器类接口）

```typescript
interface Animal {//其实这个说是属性类型也没错.因为eat也可以说是一个属性
  name: string;
  eat(str: string): void;//这个接口也可以用作对生成的类的实例化对象的检验
}

class Dog implements Animal {
  //类类型接口通过这种写法限制类
  constructor(public name: string) {}//类中必须有name属性和eat方法
  eat(str:string) {
    console.log(this.name + "吃"+str);
  }
}
let dog: Dog = new Dog("狗");
dog.eat("狗粮");

class Cat implements Animal {
  private age: number = 2;//也可以有其他的属性,这点和抽象类相同
  constructor(public name: string) {}
  eat() {
   /*
	  如果接口要字符串类型的参数,这里可以不传参可以传字符串类型的参数,如果接口要求不传参,这里就不能传		  参,否则报错
   */
    console.log(this.name + "吃鱼");
    return 123;//接口为void或者any或者number时可以返回number,否则会报错,其余类型对应
  }
  public showAge() {//也可以有其他的方法
    console.log(this.age);
  }
}

let cat: Cat = new Cat("猫");
console.log(cat.eat());//123
cat.showAge();//2
```



#### 6.4.2 构造器与静态类接口

**实例类接口类型主要是对于类返回的实例进行限制，而构造器类接口就是对类使用`new`时来对构造器函数进行限制**

```typescript
interface AnimalBehavior {
  eat(str: string): void
}
// 限定一个类有一个构造器接收name与age同时返回的实例对象符合AnimalBehavior接口
interface Animal {
  new (name: string, age: number): AnimalBehavior
  a:string // a就是一个静态的属性，也就是函数上的属性
}
// 这里的ctor必须有constructor方法并且返回一个AnimalBehavior实例且还有一个静态的a属性
function createAnimal(ctor: Animal, name: string, age: number): AnimalBehavior {
  // 这边的return其实已经是由最后返回值得AnimalBehavior来进行限制的，new所做的工作已经结束了
  return new ctor(name, age)
}

class Dog implements AnimalBehavior {
  constructor(name: string, age: number) {}
  static a = "A" // 必须要有这个静态的属性，否则下面的createAnimal函数会报错
  eat(str: string) {
    console.log('eat ' + str)
  }
}

let d = createAnimal(Dog, 'dog', 2)
d.eat('meat')
```



### 6.5 混合类型接口

**混合类型接口是讲多种类型接口混合从而合成一个集成多种条件限制的接口**

```typescript
//如将函数类型与属性类型混用,创建一个含有属性的函数
interface Counter {
  (start: number): number;
  interval: number;
  reset(): void;
}

function getCounter(): Counter {
  let counter: Counter = function(start: number): number {
    return start++;
  } as Counter;//必须进行断言,将这个函数当做一个Couter类型,否则会报错
  counter.interval = 123;

  counter.reset = function() {
    this.interval = 0;
  };
  return counter;
}

let c = getCounter();
//这个混合类型限制的变量本身是个函数,但是有reset方法和interval属性
c(10);
c.reset();
console.log(c.interval);
c.interval = 5;
console.log(c.interval);

```



### 6.6 接口扩展

**接口扩展与类的继承类似,可以用子接口扩展父接口,从而拿到多个接口的限制条件**

```typescript
interface Animal {
  eat(): void;
}

interface Person extends Animal {//继承父接口的限制条件
  name: string;
  work(): void;
}

class Student implements Person {//接口会同时将前面两者的接口限制合并
  constructor(public name: string) {}
  eat() {
    console.log(this.name + "吃饭");
  }
  work() {
    console.log(this.name + "上学");
  }
}

let stu: Student = new Student("小明");
stu.eat();
stu.work();
```

```typescript
//接口和继承相结合
interface Animal {
  eat(): void;
}
interface Plant{
    wait():void;
}
//也可以继承多个接口,用逗号隔开
interface Person extends Animal,Plant {
  name: string;
  work(): void;
}

class YoungPerson {
  constructor(public name: string) {}
  drink() {
    console.log(this.name + "喝水");
  }
}
//混合继承和接口限制的类
class Student extends YoungPerson implements Person {
  constructor(name: string) {
    super(name);
  }
  eat() {
    console.log(this.name + "吃饭");
  }
  work() {
    console.log(this.name + "上学");
  }
  wait(){
    console.log(this.name+"停下");
  }
}

let stu: Student = new Student("小明");
stu.eat();
stu.drink();
stu.work();
stu.wait();
```



### 6.7 继承类类型接口

**TypeScript允许类也可以当作接口来使用,所以也可以被接口所继承**

```typescript
class Control {
  private state: any;
}

//继承类的接口可以继承到一个类的私有和包含属性,接口会检验一个类是否继承有该父类的这两个属性
interface SelectableControl extends Control {
  select(): void;
}
//一个继承Control类的Button类,虽然state是private类型不能再内部调用.但是确实继承了这个属性,不报错
class Button extends Control implements SelectableControl {
  select(): void {}
}
//只继承了Control类,内部可以定义其他方法
class Radio extends Control{
  select(): void {}
}
//这个类会报错,因为没有继承Control类,没有state属性
class Input implements SelectableControl {
  select(): void {}
}
//即使写了private的state也会报错,因为state是在上一个类中是私有的,不能在外部访问,两个state是不同的
class Input2 implements SelectableControl {
  private state=123;
  select(): void {}
}
/*
	如果上面的Control类型是public,那么在Input2中的state只要是设置为public类型就不会报错,设置为其	  他类型会和接口不符合,则会报错
*/
```



## 7.TypeScript中的泛型

**泛型就是解决类、接口等方法的复用性问题,以及对不特定数据的支持问题的类型**

**如:**我们想通过传入不同类型的值而返回对应类似的值,在TypeScript中可以通过any类型的返回值解决返回值的不同,但是不能解决规定同一个函数传入指定不同类型参数的问题,而且用any作为返回类型性能没有泛型高,并且不符合规范

### 7.1 泛型函数

**可以使用TypeScript中的泛型来支持函数传入不特定的数据类型,要求传入的参数和返回的参数一致**

``` typescript
function fun<T>(value: T): T { //一般用T代表泛型,当然也可以是其他的非关键字和保留字,可以在函数内用
  let data: T; //T就代表着泛型函数要使用的泛型,通过后期的传入来使用
  data = value;
  return data;
}

console.log(fun<boolean>(true));
console.log(fun(123));
/*
如果不传泛型参数会利用类型推论自动推导出来，这里或推断出来是number类型,如果没有指定泛型类型的泛型参数，会把所有泛型参数当成any类型比较
*/
```

**注意：**如果编译器不能够自动地推断出类型的话，只能像上面那样明确的传入T的类型，在一些复杂的情况下，这是可能出现的。在大部分情况下，都是通过泛型的自动推断来约束用户的参数是否正确



### 7.2 泛型类

**通过泛型类可以实现对类内部不同类型变量的分别管理**

```typescript
//如:有个最小堆算法,需要同时支持返回数字和字符串两种类型,可以通过类的泛型来实现
class MinNum<T> {
  public list: T[] = [];
  add(value: T): void {
    this.list.push(value);
  }
  min(): T {
    let minNum = this.list[0];
    for (let i in this.list) {
      if (minNum > this.list[i]) {
        minNum = this.list[i];
      }
    }
    return minNum;
  }
}
let min1 = new MinNum<number>();//通过泛型实现类不同变量类型的内部算法,比any类型效率更高
min1.add(1);
min1.add(2);
min1.add(996);
min1.add(7);

console.log(min1.min()); //1

let min2 = new MinNum<string>();
min2.add("a");
min2.add("c");
min2.add("e");
console.log(min2.min()); //a
```

#### 7.2.1 把类当做参数的泛型类

```typescript
//将类当做传参的约束条件,只允许指定的类的实例作为参数传入
class Person {
  name: string | undefined;
  //这里如果没有写或者为undefined会报错,因为TypeScript怕定义了却不赋值,除非在construct中进行了赋值
  age: number | undefined;
}

class Student {
  show(info: Person): boolean {//参数只允许传入Person类的对象
    console.log(info);
    return true;
  }
}

let per = new Person();
per.name = "张三";
per.age = 18;
let stu = new Student();

stu.show(per);
```

```typescript
//使用泛型类可以手动的对不同种类的条件进行约束
//将类当做传参的约束条件,只允许指定的类的实例作为参数传入
class Person {
  name: string | undefined;
  age: number | undefined;
}

class User {
  userName: string | undefined;
  password: string | undefined;
}

class Student<T> {
  show(info: T): void {
    //参数只允许传入Person类的对象
    console.log(info);
  }
}

let per = new Person();
per.name = "张三";
per.age = 18;
let stu = new Student<Person>();//T在这传入的是泛型类,作为show方法的校验
stu.show(per);

let user = new User();
user.password = "123456";
user.userName = "张三";

let stu2 = new Student<User>();//可以写入不同的类
stu2.show(user);
```



**案例**

```typescript
/*
功能:定义一个操作数据库的库,支持Mysql,Mysql,MongoDb
要求:Mysql、Mssql、MongoDb功能一样,都有add、updata、delete、get方法
注意:约束统一的规范、以及代码重用
解决方案:需要约束规范所以要定义接口,需要代码重用所以用泛型
*/
interface DBI<T> {
  add(info: T): boolean;
  update(info: T, id: number): boolean;
  delete(info: T): boolean;
  get(id: number): any[];
}

//定义一个操作mysql数据库的类
//注意:要实现泛型接口,这个类应该是个泛型类
class MysqlDb<T> implements DBI<T> {
  add(info: T): boolean {
    console.log(info);
    return true;
  }
  update(info: T, id: number): boolean {
    throw new Error("Method not implemented.");
  }
  delete(info: T): boolean {
    throw new Error("Method not implemented.");
  }
  get(id: number): any[] {
    return [
      {
        title: "xxx",
        desc: "xxxxx",
        id: id
      },
      {
        title: "xxx",
        desc: "xxxxx",
        id: id
      }
    ];
  }
}

//定义一个操作mssql数据库的类
class MssqlDb<T> implements DBI<T> {
  add(info: T): boolean {
    throw new Error("Method not implemented.");
  }
  update(info: T, id: number): boolean {
    throw new Error("Method not implemented.");
  }
  delete(info: T): boolean {
    throw new Error("Method not implemented.");
  }
  get(id: number): any[] {
    throw new Error("Method not implemented.");
  }
}

//操作用户表,定义一个User类和数据表做映射
class User {
  username: string | undefined;
  password: string | undefined;
}

let u = new User();
u.username = "张三";
u.password = "123456";

let oMysql = new MysqlDb<User>(); //类作为约束条件

oMysql.add(u);
console.log(oMysql.get(10));
```



#### 7.2.2 在泛型里使用构造器类类型

**在TypeScript使用泛型创建工厂函数时，需要引用构造函数的类类型**

```typescript
// create函数的参数是一个Class，返回值是这个Class的实例
function create<T>(c: {new(): T; }): T {
    return new c();
}
```

**注：**`c:T`的意思是，c 的类型是 T，**但这个函数的目的不是要求 c 的类型是 T，而是要求 c 就是 T**

```typescript
// 下面这种作比较
let num = new Number(1);
fn(Number)
fn(num)
```

**更高级的简写的用法来使用原型属性推断并约束构造函数与类实例的关系**

```typescript
class BeeKeeper {
    hasMask: boolean;
}

class ZooKeeper {
    nametag: string;
}

class Animal {
    numLegs: number;
}

class Bee extends Animal {
    keeper: BeeKeeper;
}

class Lion extends Animal {
    keeper: ZooKeeper;
}

function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}

createInstance(Lion).keeper.nametag;  // typechecks!
createInstance(Bee).keeper.hasMask;   // typechecks!
```

**解析：**

- **`c:{new():T}`里的`new`是构造函数的方法**，意思是这个c是一个有着构造函数方法的对象，**下面的`return new c();`里的`new`是创建一个新的实例的`new`，**二者是不同的东西

- `c:new()=>T`和`c:{new():T}`是一样的，前者是后者的简写，意即c的类型是对象类型且这个对象包含返回类型是T的构造函数

  **注意：**这里的`=>`不是箭头函数，只是用来标明函数返回类型



### 7.3 泛型接口

**通过对接口使用泛型,通过对函数和类接口的使用来自己实现对于传入参数调节的限制**

```typescript
//因为类和函数接口差距不大,所以这里就只写函数类泛型接口
//第一种写法
interface encrypt {
  <T>(value: T): T;
}

let md5: encrypt = function<T>(value: T): T {
  //通过泛型函数赋值
  return value;
};

console.log(md5<string>("张三"));//泛型声明刚好和接口内部的顺序相呼应
console.log(md5<boolean>(true));

//第二种写法
interface encrypt<T> {
  (value: T): T;
}
//在将接口给变量的时候就指定类型给
let md5: encrypt<string> = function<T>(value: T): T {
  //通过泛型函数赋值
  return value;
};
//在这就可以直接使用函数,而不需要指定泛型
console.log(md5("张三"));

/*
其实两种方法根据对于接口<T>写的位置的不同可以大致推断出其泛型声明指定的位置，一般来说第二种在工业编程中用的是最多的
*/
```



### 7.4 泛型限定

因为泛型可以是任意的类型，而如果想要对泛型的类型进行相应的约束时，可以使用使用`extends`关键字对其进行约束

**注意:**这里的`extends`不再是继承这类意思，而且起到限定与约束作用

```typescript
function identity<T>(arg: T): T {
  console.log(arg.length); // 这里会报错，因为T是任意类型，所有不一定有length属性
  return arg;
}
```

```typescript
// 写成这样是不会报错的，因为参数为一个泛型组成的数组
function identity<T>(arg: T[]): T[] {
  console.log(arg.length); // 这里会报错，因为T是任意类型，所有不一定有length属性
  return arg;
}
```

```typescript
// 我们可以对泛型进行限定来解决报错
interface Lengthwise {
    length: number;
}
// 约束传入的参数必须要带有length属性
function identity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length); 
  return arg;
}
identity(5) // 报错，5是number类型，不具有length属性
identity("string") // 字符串具有length属性
```

#### 7.4.1 在泛型约束中使用类型参数

可以声明一个类型参数，且它被另一个类型参数所约束。 比如，现在我们想要用属性名从对象里获取这个属性。 并且我们想要确保这个属性存在于对象 `obj`上，因此我们需要在这两个类型之间使用约束

```typescript
// 让K被约束为T的key，keyof是索引类型查询操作符
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key]
}

let x = { a: 1, b: 2, c: 3, d: 4 }

getProperty(x, 'a') // okay
getProperty(x, 'm') // error: Argument of type 'm' isn't assignable to 'a' | 'b' | 'c' | 'd'.
```

**注：**上面的这种写法就不能再调用函数的时候来手动写一下约束条件了，只能让它自动推断出来



## 8.TypeScript中的模块

**与ES6一样,TypeScript也引入了模块化的概念,TypeScript也可以使用ES6中的export、export default和import导出和引入模块类的数据,从而实现模块化**

**ES6标准与Common.js的区别**

- **require:**node和es6都支持的引入
- **export和import:**ES6支持的导出引入,在浏览器和node中也不支持(node 8.x版本以后已经支持),需要babel转换,而且在node中会被转换为exports,**但是在TypeScipt中使用编译出来的JS代码可以在node中运行,因为会被编译为node认识的exports**
- **module.exports和exports:**只有node支持的导出

**注：**ES6的模块不是对象，`import`命令会被 JavaScript 引擎静态分析，在编译时就引入模块代码，而不是在代码运行时加载，所以无法实现条件加载

### 8.1 导出

#### 8.1.1 导出声明

**任何声明(比如变量、函数、类、类型别名或接口)都能够通过添加`export`关键字来导出**

```typescript
export interface StringValidator {
    isAcceptable(s: string): boolean;
}
```

```typescript
export const numberRegexp = /^[0-9]+$/;

export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}
```



#### 8.1.2 导出语句

```typescript
//上面的语句可以直接通过导出语句来写
const numberRegexp = /^[0-9]+$/;
interface StringValidator {
    isAcceptable(s: string): boolean;
}
class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}
export { ZipCodeValidator };
export { ZipCodeValidator as mainValidator };//as能够改变导出变量的名字,在外部接收时使用
```



#### 8.1.3 默认导出

每个模块都可以有一个`default`导出,默认导出使用 `default`关键字标记，并且一个模块只能够有一个`default`导出。需要使用一种特殊的导入形式来导入 `default`导出。**通过`export default`导出的值可以用任意变量进行接收**

**注:**

- 类和函数声明可以直接被标记为默认导出,标记为默认导出的类和函数的名字是可以省略的
  ```typescript
  //ZipCodeValidator.ts
  export default class ZipCodeValidator {
    static numberRegexp = /^[0-9]+$/;
    isAcceptable(s: string) {
        return s.length === 5 && ZipCodeValidator.numberRegexp.test(s);
    }
  }
  ```

  ```typescript
  import validator from "./ZipCodeValidator";
  let myValidator = new validator();
  ```

- `export default`导出也可以是一个值

  ```typescript
  //OneTwoThree.ts
  export default "123";
  ```

  ```typescript
  import num from "./OneTwoThree";
  console.log(num); // "123"
  ```



#### 8.1.4 导出模块

TypeScript提供了`export =`语法,`export =`语法定义一个模块的导出`对象`

**注意:**

- 这里的`对象`一词指的是类、接口、命名空间、函数或枚举
- 若使用`export =`导出一个模块，则必须使用TypeScript的特定语法`import module = require("module")`来导入此模块

```typescript
//ZipCodeValidator.ts
let numberRegexp = /^[0-9]+$/;
class ZipCodeValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}
export = ZipCodeValidator;
```

```typescript
import zip = require("./ZipCodeValidator");

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validator = new zip();

// Show whether each string passed each validator
strings.forEach(s => {
  console.log(`"${ s }" - ${ validator.isAcceptable(s) ? "matches" : "does not match" }`);
});
```



### 8.2 导入

**模块的导入操作与导出一样简单,可以使用以下 `import`形式之一来导入其它模块中的导出内容**

```typescript
import { ZipCodeValidator } from "./ZipCodeValidator";
let myValidator = new ZipCodeValidator();
```

```typescript
//可以对导入内容重命名
import { ZipCodeValidator as ZCV } from "./ZipCodeValidator";
let myValidator = new ZCV();
```

```typescript
//将整个模块导入到一个变量，并通过它来访问模块的导出部分
import * as validator from "./ZipCodeValidator";
let myValidator = new validator.ZipCodeValidator();
```

```typescript
//导入默认模块
//可以对导入内容重命名
import ZCV  from "./ZipCodeValidator";
let myValidator = new ZCV();
```

**当然,也可以直接使用`import`导入一个不需要进行赋值的模板,该模板会自动进行内部的代码**

```typescript
import "./my-module.js";
```

#### 8.2.1 动态导入

**import的导入导出默认是静态的，如果要动态的导入导出可以使用ES6新增的`import()`函数实现类似`require()`动态导入的功能**

**注：**

- 使用`import()`函数返回的是Promise对象
- 如果是`commonjs`格式的模块需要我们手动调用`default()`方法获得默认导出

```typescript
async function getTime(format:string){
    const momment = await import('moment')
    return moment.default().format(format)
}
// 使用async的函数本身的返回值是一个Promise对象
getTime('L').then(res=>{
    console.log(res)
})
```



### 8.3 export = 和 import = require()

CommonJS和AMD的环境里都有一个`exports`变量，这个变量包含了一个模块的所有导出内容。CommonJS和AMD的`exports`都可以被赋值为一个`对象`, 这种情况下其作用就类似于 es6 语法里的默认导出，即 `export default`语法了。**虽然作用相似，但是 `export default` 语法并不能兼容CommonJS和AMD的`exports`**

**为了支持CommonJS和AMD的`exports`, TypeScript提供了`export =`语法。**`export =`语法定义一个模块的导出`对象`。 这里的`对象`一词指的是类，接口，命名空间，函数或枚举

而`import module = require("module")`也是TypeScript新增的一种导入格式，该格式的导入可以兼容所有的导入格式，但是注意如果是引入的ES6特有的导出会默认把导出的模块转换为对象（因为module只能够接受一个值，默认应该要获取到所有的导出），同时该对象会多一个`__esModule`值为`true`的属性（），而其他的所有属性会加载这个对象中

**注：**即使使用的是`export default`在也会是同样的效果，不过会把默认导出添加到一个`default`属性上

**注意：**

- `export = `在一个模块中只能使用一次，所以是与`CommonJS`一样基本都是用于导出一个对象出来
- `ES6`的`import ... from ...`的默认导出的语法不能作用在`export =`导出的对象，因为没有`default`对象，就像`CommonJS`的`module.exports`一样（虽然最后是转换为这个），而`ES6`的`export default`转换为`CommonJS`就是为其添加一个`default`属性
- 若使用`export =`导出一个模块，则**必须使用TypeScript的特定语法`import module = require("module")`来导入此模块**
- 除了`import module = require("module")`导入`ES6`的模块有区别之外，在导入CommonJS和AMD效果类似，如果在都支持的模块中（UMD模块为代表），该导入相当于是导入了ES6模块中的`default`

```typescript
// ZipCodeValidator.ts
let numberRegexp = /^[0-9]+$/;
class ZipCodeValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}
export = ZipCodeValidator;
```

```typescript
// Test.ts
import zip = require("./ZipCodeValidator");

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validator = new zip();

// Show whether each string passed each validator
strings.forEach(s => {
  console.log(`"${ s }" - ${ validator.isAcceptable(s) ? "matches" : "does not match" }`);
});
```

#### 8.3.1 生成模块代码

在之前说到的`import module = require("module")`的区别的原因是根据编译时指定的模块目标参数，编译器会生成相应的供Node.js ([CommonJS](http://wiki.commonjs.org/wiki/CommonJS))，Require.js ([AMD](https://github.com/amdjs/amdjs-api/wiki/AMD))，[UMD](https://github.com/umdjs/umd)，[SystemJS](https://github.com/systemjs/systemjs)或[ECMAScript 2015 native modules](http://www.ecma-international.org/ecma-262/6.0/#sec-modules) (ES6)模块加载系统使用的代码。如：

- SimpleModule.ts

    ```ts
    import m = require("mod");
    export let t = m.something + 1;
    ```

- AMD / RequireJS SimpleModule.js

    ```js
    define(["require", "exports", "./mod"], function (require, exports, mod_1) {
        exports.t = mod_1.something + 1;
    });
    ```

- CommonJS / Node SimpleModule.js

    ```js
    let mod_1 = require("./mod");
    exports.t = mod_1.something + 1;
    ```

- UMD SimpleModule.js

    ```js
    (function (factory) {
        if (typeof module === "object" && typeof module.exports === "object") {
            let v = factory(require, exports); if (v !== undefined) module.exports = v;
        }
        else if (typeof define === "function" && define.amd) {
            define(["require", "exports", "./mod"], factory);
        }
    })(function (require, exports) {
        let mod_1 = require("./mod");
        exports.t = mod_1.something + 1;
    });
    ```

- System SimpleModule.js

    ```js
    System.register(["./mod"], function(exports_1) {
        let mod_1;
        let t;
        return {
            setters:[
                function (mod_1_1) {
                    mod_1 = mod_1_1;
                }],
            execute: function() {
                exports_1("t", t = mod_1.something + 1);
            }
        }
    });
    ```

- Native ECMAScript 2015 modules SimpleModule.js

    ```js
    import { something } from "./mod";
    export let t = something + 1;
    ```



#### 8.3.2 可选的模块加载

有时候，你只想在某种条件下才加载某个模块。 在TypeScript里，使用下面的方式来实现它和其它的高级加载场景，我们可以直接调用模块加载器并且可以保证类型完全。

编译器会检测是否每个模块都会在生成的JavaScript中用到。 如果一个模块标识符只在类型注解部分使用，并且完全没有在表达式中使用时，就不会生成 `require`这个模块的代码。略掉没有用到的引用对性能提升是很有益的，并同时提供了选择性加载模块的能力

```typescript
import a = require('./a') // 如果只写这句话是不会引入a模块的
console.log(a) // 必须要使用过才会真正引入
```

这种模式的核心是`import id = require("...")`语句可以让我们访问模块导出的类型。 模块加载器会被动态调用（通过 `require`），就像下面`if`代码块里那样。 它利用了省略引用的优化，所以模块只在被需要时加载。 为了让这个模块工作，一定要注意 `import`定义的标识符只能在表示类型处使用（不能在会转换成JavaScript的地方）

为了确保类型安全性，我们可以使用`typeof`关键字。 `typeof`关键字，当在表示类型的地方使用时，会得出一个类型值，这里就表示模块的类型

```typescript
// 如下面这样就可以在node.js环境实现可选模块加载
declare function require(moduleName: string): any;

import { ZipCodeValidator as Zip } from "./ZipCodeValidator";

if (needZipValidation) {
    let ZipCodeValidator: typeof Zip = require("./ZipCodeValidator");
    let validator = new ZipCodeValidator();
    if (validator.isAcceptable("...")) { /* ... */ }
}
```



### 8.4 模块转换问题

**TypeScript中默认是将所有代码转换为`CommonJS`模块代码，相对于模块有不同的代码转换规则**

- **ES6模块：**

  - `import * as ... from ...`，这种写法是最接近`CommonJS`中`require`的写法，将所有导出的模块装维一个对象，所以最后也会变为`var ... = require('...')`

  - `import {...} from ...`，同上一种一样，不过相当于是用了取对象符

  - `import ... from ...`，因为这种写法是取出export的默认导出，而默认导出其实是模块的一个叫作`default`的属性，所以也是用了取对象符`var ... = require('...').default`

    **注意：**这样导入的模块一般是需要对应`ES6`的`export default`语法的，因为要获取`default`属性，而使用的`CommonJS`和`export = `的写法是直接导出一整个对象，如果不给这些导出的对象设置`default`属性会得到`undefined`

  - `export`单独导入同`CommonJS`中的`exports.xxx`语法，只需要主要`export default`等同于`exports.default = xxx`

- **CommonJS模块：**因为是转为这种语法的，所以没有兼容性可说

- **TypeScript模块：**

  - `import ... = require('...')`，等同于`CommonJS`的`require`语法，只是可以支持AMD模块，而原生的`require`是不支持的
  - `export = `，等同于`CommonJS`的`module.exports =`



## 9.命名空间

**在代码量较大的情况下,为了避免各种变量命名相冲突,可以将类似功能的函数、类、接口等放置到命名空间中**

在TypeScript中的命名空间中的对象、类、函数等可以通过export暴露出来通过`命名空间名.类名`等来使用

**注意:**这个暴露是暴露在命名空间外,不是将其在模块中暴露出去

**命名空间和模块的区别:**

- **命名空间:**内部模块,主要用于组织代码,避免命名冲突
- **模块:**TypeScript的外部模块的简称,侧重代码的复用,一个模块里可能会有多个命名空间

```typescript
namespace Validation {//通过namespace关键词创建一个命名空间
    export interface StringValidator {
        isAcceptable(s: string): boolean;//类类型接口
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {//要在外部使用必须导出
        isAcceptable(s: string) {//函数内部可以不导出
            return lettersRegexp.test(s);
        }
    }

    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// 在外界就可以直接通过Validation.StringValidator访问命名空间内部导出的接口
let validators: { [s: string]: Validation.StringValidator; } = {};
//上面接口的意思是一个对象,对象中的每个成员都是有isAcceptable接口方法的实例化对象
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}                                   
```

### 9.1 多文件中的命名空间

如果命名空间相同，多个文件内部的代码会合并到同一个命名空间中，其实就是使用`var`声明字重复定义变量，如果内部没有导出的变量依然只能在内部使用，而暴露的变量就会合并

**注：**如果导出变量有重名，后面的文件会覆盖掉前面的

- **通过export和import进行使用**

  ```typescript
  //module.ts
  export namespace A {
    interface Animal {
      name: string;
      eat(): void;
    }
    export class Dog implements Animal {
      name: string;
      constructor(theName: string) {
        this.name = theName;
      }
      eat(): void {
        console.log(this.name + "吃狗粮");
      }
    }
  }
  ```

  ```typescript
  // A在JS中就被转换为了一个对象
  import { A } from "./module";
  let dog = new A.Dog("狗");//传入命名空间
  dog.eat();
  ```

- **通过三斜线指令引入**

  **三斜线指令:**包含单个XML标签的单行注释,注释的内容会做为编译器指令使用,三斜线引用告诉编译器在编译过程中要引入的额外的文件

  **注意:**三斜线指令仅可放在包含它的文件的最顶端。 一个三斜线指令的前面只能出现单行或多行注释,这包括其它的三斜线指令。 如果它们出现在一个语句或声明之后,那么它们会被当做普通的单行注释,并且不具有特殊的涵义

  这里只用`///<reference path=""/>`,其余用法在 [TypeScript中文文档](<https://www.tslang.cn/docs/handbook/namespaces.html>) 查看

  `/// <reference path="..." />`指令是三斜线指令中最常见的一种,它用于声明文件间的 依赖,三斜线引用告诉编译器在编译过程中要引入的额外的文件,也就是会引入对应path的文件

  ```typescript
  //Validation.ts
  namespace Validation {
      export interface StringValidator {
          isAcceptable(s: string): boolean;
      }
  }
  ```

  ```typescript
  //LettersOnlyValidator.ts
  /// <reference path="Validation.ts" />
  namespace Validation {
      const lettersRegexp = /^[A-Za-z]+$/;
      export class LettersOnlyValidator implements StringValidator {
          isAcceptable(s: string) {
              return lettersRegexp.test(s);
          }
      }
  }
  ```

  ```typescript
  //ZipCodeValidator.ts
  /// <reference path="Validation.ts" />
  namespace Validation {
      const numberRegexp = /^[0-9]+$/;
      export class ZipCodeValidator implements StringValidator {
          isAcceptable(s: string) {
              return s.length === 5 && numberRegexp.test(s);
          }
      }
  }
  ```

  ```typescript
  /// <reference path="Validation.ts" />
  /// <reference path="LettersOnlyValidator.ts" />
  /// <reference path="ZipCodeValidator.ts" />
  
  // Some samples to try
  let strings = ["Hello", "98052", "101"];
  
  // Validators to use
  let validators: { [s: string]: Validation.StringValidator; } = {};
  validators["ZIP code"] = new Validation.ZipCodeValidator();
  validators["Letters only"] = new Validation.LettersOnlyValidator();
  
  // Show whether each string passed each validator
  for (let s of strings) {
      for (let name in validators) {
          console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
      }
  }
  ```

  

### 9.2 别名

**别名是另一种简化命名空间操作的方法是使用`import q = x.y.z`给常用的对象起一个短的名字,不要与用来加载模块的`import x = require('name')`语法弄混了,这里的语法是为指定的符号创建一个别名** 

**注:**可以用这种方法为任意标识符创建别名,也包括导入的模块中的对象

```typescript
namespace Shapes {
    export namespace Polygons {
        export class Triangle { }
        export class Square { }
    }
}

import polygons = Shapes.Polygons;//用polygons代替Shapes.Polygons,相当于C语言的define
let sq = new polygons.Square(); // Same as "new Shapes.Polygons.Square()"
```

**注意：**并没有使用`require`关键字,而是直接使用导入符号的限定名赋值,与使用 `var`相似，但它还适用于类型和导入的具有命名空间含义的符号。 重要的是,对于值来讲, `import`会生成与原始符号不同的引用,所以**改变别名的`var`值并不会影响原始变量的值**



## 10.TypeScript中的装饰器

> **此内容是面向未来的，可以跳过**

**装饰器是一种特殊类型的声明,它能够被附加到类声明,方法,属性或参数上,可以修改类的行为**,通俗来讲装饰器就是一个方法,可以注入到类、方法、属性参数上来扩展类、属性、方法、参数的功能

**装饰器已经是ES7的标准特性之一**

**常见的装饰器**

- 类装饰器
- 属性装饰器
- 方法装饰器
- 参数装饰器

**装饰器的写法**

- 普通装饰器(无法传参)
- 装饰器工厂(可传参)

**注意：**装饰器是一项实验性特性，因为装饰器只是个未来期待的用法,所以默认是不支持的,如果想要使用就要打开tsconfig.json中的`experimentalDecorators`,否则会报语法错误

**命令行**:

```shell
tsc --target ES5 --experimentalDecorators
```

**tsconfig.json**:

### 10.1 类装饰器

**类装饰器在类声明之前被声明(紧跟着类声明),类装饰器应用于类`构造函数,`可以用来监视,修改或替换类定义,需要传入一个参数**

#### 10.1.1 普通装饰器

```typescript
function logClass(target: any) {
  console.log(target);
  //target就是当前类,在声明装饰器的时候会被默认传入
  target.prototype.apiUrl = "动态扩展的属性";
  target.prototype.run = function() {
    console.log("动态扩展的方法");
  };
}

@logClass
class HttpClient {
  constructor() {}
  getData() {}
}
//这里必须要设置any,因为是装饰器动态加载的属性,所以在外部校验的时候并没有apiUrl属性和run方法
let http: any = new HttpClient();
console.log(http.apiUrl);
http.run();
```



#### 10.1.2 装饰器工厂

**如果要定制一个修饰器如何应用到一个声明上,需要写一个装饰器工厂函数。 装饰器工厂就是一个简单的函数,它返回一个表达式,以供装饰器在运行时调用**

**注：**装饰器工厂是将内部调用的函数作为真正的装饰器返回的，所以装饰器工厂需要和函数用法一样通过`()`来调用，内部可以接收参数

```typescript
function color(value: string) { // 这是一个装饰器工厂
    return function (target:any) { //这是装饰器,这个装饰器就是上面普通装饰器默认传入的类
        // do something with "target" and "value"...
    }
}
```

```typescript
function logClass(value: string) {
  return function(target: any) {
    console.log(target);
    console.log(value);
    target.prototype.apiUrl = value;//将传入的参数进行赋值
  };
}

@logClass("hello world")//可传参数的装饰器
class HttpClient {
  constructor() {}
  getData() {}
}

let http: any = new HttpClient();
console.log(http.apiUrl);
```



#### 10.1.3 类装饰器重构构造函数

类装饰器表达式会在运行时当作函数被调用,类的构造函数作为其唯一的参数**,如果类装饰器返回一个值,它会使用提供的构造函数来替换类的声明**，通过这种方法我们可以很轻松的继承和修改原来的父类，定义自己的属性和方法

**注意：** 如果要返回一个新的构造函数，必须注意处理好原来的原型链

```typescript
/*
通过返回一个继承的类实现一个类的属性和方法的重构,换句话说就是在中间层有一个阻拦,然后返回的是一个新的继承了父类的类,这个类必须有父类的所有属性和方法,不然会报错
*/
function logClass(target: any) {
  // 返回一个继承原来类的新的类
  return class extends target {//可以当做是固定写法吧
    apiUrl: string = "我是修改后的数据";
    getData() {
      console.log(this.apiUrl);
    }
  };
}
//重构属性和方法
@logClass
class HttpClient {
  // 如果不在这声明TypeScript的检测器检测不出来，在下面的使用都会报错，可以使用接口的声明合并来消除
  constructor(public apiUrl = "我是构造函数中的数据") {}
  getData() {
    console.log(123);
  }
}
/*
    interface HttpClient {
      apiUrl: string
      getData(): void
    }
*/

let http: any = new HttpClient();
console.log(http.apiUrl);//我是修改后的数据
http.getData();//我是修改后的数据
```



#### 10.1.4 装饰器求值

**类中不同声明上的装饰器将按以下规定的顺序应用：**

1. *参数装饰器*，然后依次是*方法装饰器*，*访问符装饰器*，或*属性装饰器*应用到每个实例成员
2. *参数装饰器*，然后依次是*方法装饰器*，*访问符装饰器*，或*属性装饰器*应用到每个静态成员
3. *参数装饰器*应用到构造函数
4. *类装饰器*应用到类



### 10.2 方法装饰器

**方法装饰器声明在一个方法的声明之前（紧靠着方法声明）**

**注意：**

- 它会被应用到方法的属性描述符上，可以用来监视，修改或者替换方法定义
-  方法装饰器不能用在声明文件( `.d.ts`)，重载或者任何外部上下文（比如`declare`的类）中

**方法装饰器被应用到方法的属性描述符上,可以用来监视,修改或替换方法定义,传入三个参数(都是自动传入的):**

- 对于静态成员来说是类的构造函数,对于实例成员是类的原型对象

- 成员的名字(只是个string类型的字符串,没有其余作用)

- 成员的属性描述符,是一个对象,里面有真正的方法本身

  **注：** 如果代码输出目标版本小于`ES5`，属性描述符将会是`undefined`

**注意：**如果方法装饰器返回一个值，它会被用作方法的**属性描述符**，如果代码输出目标版本小于`ES5`返回值会被忽略

```typescript
function get(value: any) {
  // PropertyDescriptor是TypeScript中内置的属性描述符的类型限定，包含了类型修辞符的所有属性
  return function(target: any, methodName: string, desc: PropertyDescriptor) {
    console.log(target); //HttpClient类
    console.log(methodName); //getData方法名,一个字符串
    console.log(desc); //描述符
    console.log(desc.value); //方法本身就在desc.value中
    target.url = 123; //也能改变原实例
  };
}

class HttpClient {
  public url: any | undefined;
  constructor() {}
  @get("hello world")
  getData() {
    console.log(this.url);
  }
}

let http = new HttpClient();
console.log(http.url); //123
```

```typescript
function get(value: any) {
   // PropertyDescriptor是TypeScript中内置的属性描述符的类型限定
  return function(target: any, methodName: string, desc: PropertyDescriptor) {
    let oMethod = desc.value
    desc.value = function(...args: any[]) {
      //因为用了方法装饰器,所以实际调用getData()方法的时候会调用desc.value来实现,通过赋值可以实现重构方法
      //原来的方法已经赋值给oMethod了,所以可以改变
      args = args.map(
        //这个段代码是将传入的参数全部转换为字符串
        (value: any): string => {
          return String(value)
        }
      )
      console.log(args) //因为方法重构了,所以原来的getData()中的代码无效了,调用时会打印转换后参数
      /*
            如果想依然能用原来的方法,那么写入下面的代码,相当于就是对原来的方法进行了扩展
        */
      oMethod.apply(target, args) //通过这种方法调用可以也实现原来的getData方法
    }
  }
}

class HttpClient {
  public url: any | undefined
  constructor() {}
  @get('hello world')
  getData(...args: any[]) {
    console.log(args) //[ '1', '2', '3', '4', '5', '6' ]
    console.log('我是getData中的方法')
  }
}

let http = new HttpClient()
http.getData(1, 2, 3, 4, 5, 6) //[ '1', '2', '3', '4', '5', '6' ]
```

```typescript
function get(bool: boolean): any {
  return (target: any, prop: string, desc: PropertyDescriptor) => {
    // 通过返回值修改属性描述符
    return {
      value() {
        return 'not age'
      },
      enumerable: bool
    }
  }
}

class Test {
  constructor(public age: number) {}
  @get(false)
  public getAge() {
    return this.age
  }
}
const t = new Test(18)
console.log(t.getAge()) // not age，getAge()函数的值以及被修改了
for (const key in t) {
  console.log(key) // 只有age属性，如果上面@get传入的是true就还有getAge()方法
}
```



#### 10.3.1 属性描述符

在ES5之前，JavaScript 没有内置的机制来指定或者检查对象某个属性(property)的特性(characteristics)，比如某个属性是只读(readonly)的或者不能被枚举(enumerable)的。但是在 ES5之后，JavaScript 被赋予了这个能力，所有的对象属性都可以通过属性描述符(Property Descriptor)来指定

```typescript
interface obj {
  [key: string]: any
}
let myObject: obj = {}

Object.defineProperty(myObject, 'a', {
  value: 2,
  writable: true, // 可写
  configurable: true, // 可配置
  enumerable: true // 可遍历
})
// 上面的定义等同于 myObject.a = 2;
// 所以如果不需要修改这三个特性，我们不会用 `Object.defineProperty`

console.log(myObject.a) // 2
```

**属性描述符的六个属性**

- value：属性值

- writable：是否允许赋值，**true** 表示允许，否则该属性不允许赋值

  ```typescript
  interface obj {
    [key: string]: any
  }
  let myObject: obj = {}
  
  Object.defineProperty(myObject, 'a', {
    value: 2,
    writable: false, // 不可写
    configurable: true,
    enumerable: true
  })
  
  myObject.a = 3 // 写入的值将会被忽略
  console.log(myObject.a) // 2
  ```

- get：返回属性值的函数。如果为 **undefined** 则直接返回描述符中定义的 **value** 值

- set：属性的赋值函数。如果为 **undefined** 则直接将赋值运算符右侧的值保存为属性值

- configurable：如果为 **true**，则表示该属性可以重新使用（`Object.defineProperty(...)` ）定义描述符，或者从属性的宿主删除。缺省为 `true`

  ```typescript
  let myObject = {
    a: 2
  }
  
  Object.defineProperty(myObject, 'a', {
    value: 4,
    writable: true,
    configurable: false, // 不可配置!
    enumerable: true
  })
  
  console.log(myObject.a) // 4
  myObject.a = 5
  // 因为最开始writable时true，所以不会影响到赋值
  console.log(myObject.a) // 5
  
  Object.defineProperty(myObject, 'a', {
    value: 6,
    writable: true,
    configurable: true,
    enumerable: true
  }) // TypeError
  ```

  **注：**一旦某个属性被指定为 `configurable: false`，那么就不能从新指定为`configurable: true` 了，这个操作是单向，不可逆的

  **这个特性还会影响`delete` 操作的行为**

  ```typescript
  let myObject = {
    a: 2
  }
  
  Object.defineProperty(myObject, 'a', {
    value: 4,
    writable: true,
    configurable: false, // 不可配置!
    enumerable: true
  })
  delete myObject.a
  console.log(myObject.a) // 4
  ```

- enumerable：如果为 **true**，则表示遍历宿主对象时，该属性可以被遍历到（比如 `for..in` 循环中）。缺省为 `true`

  ```typescript
  interface obj {
    [key: string]: any
  }
  let myObject: obj = {}
  
  Object.defineProperty(
    myObject,
    'a',
    // make `a` enumerable, as normal
    { enumerable: true, value: 2 }
  )
  
  Object.defineProperty(
    myObject,
    'b',
    // make `b` NON-enumerable
    { enumerable: false, value: 3 }
  )
  console.log(myObject.b) // 3
  console.log('b' in myObject) // true
  myObject.hasOwnProperty('b') // true
  
  // .......
  // 无法被遍历到
  for (let k in myObject) {
    console.log(k, myObject[k])
  }
  // "a" 2
  
  myObject.propertyIsEnumerable('a') // true
  myObject.propertyIsEnumerable('b') // false
  
  Object.keys(myObject) // ["a"]
  Object.getOwnPropertyNames(myObject) // ["a", "b"]
  ```

  可以看出，`enumerable: false` 使得该属性从对象属性枚举操作中被隐藏，但`Object.hasOwnProperty(...)` 仍然可以检测到属性的存在。另外，`Object.propertyIsEnumerable(..)` 可以用来检测某个属性是否可枚举,`Object.keys(...)` 仅仅返回可枚举的属性，而`Object.getOwnPropertyNames(...)` 则返回该对象上的所有属性，包括不可枚举的

**注：**Object有专门操作属性的方法，在这里就不再多讲了



### 10.3 方法参数装饰器

**参数装饰器声明在一个参数声明之前（紧靠着参数声明）。** 参数装饰器应用于类构造函数或方法声明。

**注意：**参数装饰器不能用在声明文件（.d.ts），重载或其它外部上下文（比如 `declare`的类）里

**参数装饰器被表达式会在运行时当作函数被调用,可以使用参数装饰器为类的原型增加一些元素数据,传入三个参数(都是自动传入的):**

- 对于静态成员来说是类的构造函数,对于实例成员是类的原型对象
- 方法的名字(只是个string类型的字符串,没有其余作用)
- 参数在函数参数列表中的索引

**注：**

-  参数装饰器只能用来监视一个方法的参数是否被传入
- 参数装饰器的返回值会被忽略

```typescript
//这个装饰器很少使用
function logParams(value: any) {
  return function(target: any, methodName: any, paramsIndex: any) {
    console.log(target);
    console.log(methodName);//getData
    console.log(paramsIndex);//1,因为value在下面是第二个参数
  };
}

class HttpClient {
  public url: any | undefined;
  constructor() {}
  getData(index: any, @logParams("hello world") value: any) {
    console.log(index);
    console.log(value);
  }
}

let http: any = new HttpClient();
http.getData(0, "123"); //我是修改后的数据
```



### 10.4 访问器装饰器

**访问器装饰器声明在一个访问器的声明之前（紧靠着访问器声明）。 访问器装饰器应用于访问器的属性描述符并且可以用来监视，修改或替换一个访问器的定义。** 

**注意：**

- 访问器装饰器不能用在声明文件中（.d.ts），或者任何外部上下文（比如 `declare`的类）里
- TypeScript不允许同时装饰一个成员的`get`和`set`访问器。取而代之的是，**一个成员的所有装饰的必须应用在文档顺序的第一个访问器上**。这是因为，**在装饰器应用于一个属性描述符时，它联合了`get`和`set`访问器，而不是分开声明的**

**访问器装饰器表达式会在运行时当作函数被调用，传入下列3个参数(都是自动传入的)：**

- 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象

- 成员的名字

- 成员的属性描述符

  **注：**如果代码输出目标版本小于`ES5`，*Property Descriptor*将会是`undefined`

**注意：**如果访问器装饰器返回一个值，它会被用作**方法的属性描述符**。如果代码输出目标版本小于`ES5`返回值会被忽略

```typescript
function configurable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.configurable = value;
    };
}

class Point {
    private _x: number;
    private _y: number;
    constructor(x: number, y: number) {
        this._x = x;
        this._y = y;
    }

    @configurable(false)
    get x() { return this._x; }

    @configurable(false)
    get y() { return this._y; }
}
```



### 10.5 属性装饰器

**属性装饰器声明在一个属性声明之前（紧靠着属性声明）。**

**注意：**属性装饰器不能用在声明文件中（.d.ts），或者任何外部上下文（比如 `declare`的类）里。

**属性装饰器表达式在运行时当作函数被调用,传入两个参数(都是自动传入的):**

- 对应静态成员来说是类的构造函数,对于实例成员来说是类的原型对象
- 成员的名字

**注：**属性描述符不会做为参数传入属性装饰器，这与TypeScript是如何初始化属性装饰器的有关。 因为目前没有办法在定义一个原型对象的成员时描述一个实例属性，并且没办法监视或修改一个属性的初始化方法。**返回值也会被忽略。**因此，属性描述符只能用来监视类中是否声明了某个名字的属性

```typescript
function logProperty(value: string) {
  return function(target: any, attr: string) {
   //target为实例化的成员对象,attr为下面紧挨着的属性
    console.log(target);
    console.log(attr);
    target[attr] = value;//可以通过修饰器改变属性的值
  };
}

class HttpClient {
  @logProperty("hello world")//修饰器后面紧跟着对应要修饰的属性
  public url: string | undefined;
  constructor() {}
  getData() {
    console.log(this.url);
  }
}

let http: any = new HttpClient();
http.getData();//hello world
```



### 10.5.5 返回值总结

- 属性和方法参数装饰器的返回值会被忽略
- 访问器和方法装饰器的返回值都会被用做方法的属性描述符（低于`Es5`版本会被忽略）
- 类装饰器的返回值会返回一个新的构造函数



### 10.6 装饰器的执行顺序

**我们可以对同一个对象使用多个装饰器，装饰器的执行顺序是从后往前执行的**

- **书写在同一行上**

  ```typescript
  @f @g x
  ```

- **书写在多行上**

  ```typescript
  @f
  @g
  x
  ```

**在TypeScript里，当多个装饰器应用在一个声明上时会进行如下步骤的操作：**

1. 由上至下依次对装饰器表达式求值。
2. 求值的结果会被当作函数，由下至上依次调用。

**简单的说就是：**如果是装饰器工厂修饰的（不是只有一个函数，是通过返回函数来实现），会从上到下按照代码的顺序先执行装饰器工厂生成装饰器，然后再从下往上执行装饰器

**特别提醒：**如果方法和方法参数装饰器在同一个方法出现,参数装饰器先执行 

```typescript
function f() {
    console.log("f(): evaluated");
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("f(): called");
    }
}

function g() {
    console.log("g(): evaluated");
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {
        console.log("g(): called");
    }
}

class C {
    @f()
    @g()
    method() {}
}
```

```shell
# 在控制台中打印
f(): evaluated
g(): evaluated
g(): called
f(): called
```



## 11.Mixins混入

### 11.1 对象的混入

**和JS一样，TypeScript中混入对象也是使用`Object.assign()`方法来实现，不多最后的结果会多了一个交叉类型的类型定义，同时包含了所有混入对象的属性**

```typescript
interface ObjectA {
  a: string
}

interface ObjectB {
  b: string
}

let A: ObjectA = {
  a: 'a'
}

let B: ObjectB = {
  b: 'b'
}

let AB: ObjectA & ObjectB = Object.assign(A, B) // 及时左边没有类型定义也会自动被定义为交叉类型
console.log(AB)
```



### 11.2 类的混入

**对于类的混入，我们需要理解下面这个例子：**

```typescript
// Disposable Mixin
class Disposable {
    isDisposed: boolean;
    dispose() {
        this.isDisposed = true;
    }

}

// Activatable Mixin
class Activatable {
    isActive: boolean;
    activate() {
        this.isActive = true;
    }
    deactivate() {
        this.isActive = false;
    }
}

class SmartObject implements Disposable, Activatable {
    constructor() {
        setInterval(() => console.log(this.isActive + " : " + this.isDisposed), 500);
    }

    interact() {
        this.activate();
    }

    // Disposable
    isDisposed: boolean = false;
    dispose: () => void;
    // Activatable
    isActive: boolean = false;
    activate: () => void;
    deactivate: () => void;
}
applyMixins(SmartObject, [Disposable, Activatable]);

let smartObj = new SmartObject();
setTimeout(() => smartObj.interact(), 1000);

////////////////////////////////////////
// In your runtime library somewhere
////////////////////////////////////////

function applyMixins(derivedCtor: any, baseCtors: any[]) {
    baseCtors.forEach(baseCtor => {
        Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
            derivedCtor.prototype[name] = baseCtor.prototype[name];
        });
    });
}
```

代码里首先定义了两个类，它们将做为mixins。 可以看到每个类都只定义了一个特定的行为或功能。 稍后我们使用它们来创建一个新类，同时具有这两种功能

```typescript
// Disposable Mixin
class Disposable {
    isDisposed: boolean;
    dispose() {
        this.isDisposed = true;
    }

}

// Activatable Mixin
class Activatable {
    isActive: boolean;
    activate() {
        this.isActive = true;
    }
    deactivate() {
        this.isActive = false;
    }
}
```

然后我们需要创建一个类来使用他们作为接口进行限制。**没使用`extends`而是使用`implements`。 把类当成了接口，仅使用Disposable和Activatable的类型而非其实现。** 这意味着我们需要在类里面实现接口。 但是这是我们在用mixin时想避免的。

我们可以这么做来达到目的，**为将要mixin进来的属性方法创建出占位属性。 这告诉编译器这些成员在运行时是可用的。 这样就能使用mixin带来的便利，虽说需要提前定义一些占位属性。**

```typescript
class SmartObject implements Disposable, Activatable {
    constructor() {
        setInterval(() => console.log(this.isActive + " : " + this.isDisposed), 500);
    }

    interact() {
        this.activate();
    }

    // Disposable
    isDisposed: boolean = false;
    dispose: () => void;
    // Activatable
    isActive: boolean = false;
    activate: () => void;
    deactivate: () => void;
}
```

创建帮助函数，帮我们做混入操作。 它会遍历mixins上的所有属性，并复制到目标上去，把之前的占位属性替换成真正的实现代码

```typescript
function applyMixins(derivedCtor: any, baseCtors: any[]) {
    baseCtors.forEach(baseCtor => {
        Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
            derivedCtor.prototype[name] = baseCtor.prototype[name];
        })
    });
}
```

最后，把mixins混入定义的类，完成全部实现部分

```typescript
applyMixins(SmartObject, [Disposable, Activatable]);
```


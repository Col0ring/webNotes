# TypeScript

## 1.什么是TypeScript?

- TypeScript是微软开发的一款开源的编程语言
- TypeScript是avaScript的超级,遵循最新的ES6和ES5规范。TypeScript扩展立了JavaScript的语法
- TypeScript更新后更像Java、C#这样的面向对象语言,可以让JavaScript开发大型企业项目
- 现如今的流行框架都可以继承TypeScript



## 2.TypeScript的安装及编译

### 2.1 安装

**通过下面的方式在全局安装TypeScript**

```shell
cnpm install -g typescript
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

- **元组类型(tuple)**

  元组类型属于数组的一种,上面一种数组里面只能有一种类型,否则会报错,而元组类型内部可以有多种类型

  ```typescript
  //元组类型可以为数组中的每个成员指定一个对应的类型
  let arr:[number,string]=[123,"string"];//注意如果类型不对应也会报错
  arr[2]=456;//报错
  //因为元组类型在声明时是一一对应的,只能有2个成员
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
  + 可以把标识符用引号括起来,效果不受影响
  + 还可以通过反过来选择索引值来获取字符串标识符

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
  
  let c:never;//c不能被任何值赋值,包括null和undefiend,只不会出现的值
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

  `object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型

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



### 3.2 高级类型

#### 3.2.1 交叉类型

**交叉类型是将多个类型合并为一个类型。这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性**

```typescript
function extend<T, U>(first: T, second: U): T & U {
  let result = <any>{}; //官方这里是T&U,但是会报错,因为不知T和U是不是对象,必须any才可以
  for (let id in first) {
    (<any>result)[id] = (<any>first)[id];
  }
  for (let id in second) {
    if (!result.hasOwnProperty(id)) {
      (<any>result)[id] = (<any>second)[id];
    }
  }
  return result;
}

class Person {
  constructor(public name: string) {}
}
interface Loggable {
  log(): void;
}
class ConsoleLogger implements Loggable {
  log() {
    // ...
  }
}
var jim = extend(new Person("Jim"), new ConsoleLogger());
var n = jim.name;
jim.log();
```



#### 3.2.2 联合类型

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



#### 3.2.3 类型保护

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

    **注意:**这些 `typeof`类型保护只有两种形式能被识别:`typeof v === "typename"`和 `typeof v !== "typename"`，`"typename"`必须是 `"number"`， `"string"`， `"boolean"`或 `"symbol"`。 但是TypeScript并不会阻止你与其它字符串比较,语言不会把那些表达式识别为类型保护

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

    如果没有在vscode中,直接编译的话是可以给一个其他类型的值赋值为undefined或者null的,但是如果编译时使用了`--strictNullChecks`标记的话,就会和vscode一样不能赋值了,并且可选参数和可以选属性会自动加上`|undefined`类型

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

    **如果编译器不能够去除 `null`或 `undefined`,你可以使用类型断言手动去除。 语法是添加 `!`后缀：`identifier!`从 `identifier`的类型里去除了 `null`和 `undefined**`

    ```typescript
    function broken(name: string | null): string {
      function postfix(epithet: string) {
        return name.charAt(0) + '.  the ' + epithet; // error, 'name' is possibly null
      }
      name = name || "Bob";
      return postfix("great");
    }
    //上面的函数会报错
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



#### 3.2.4 类型别名

**类型别名会给一个类型起个新名字。类型别名有时和接口很像,但是可以作用于原始值，联合类型，元组以及其它任何需要手写的类型**

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

//我们也可以使用类型别名来在属性里引用自己
type Tree<T> = {
    value: T;
    left: Tree<T>;
    right: Tree<T>;
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

**注:**类型别名不能出现在声明右侧的任何地方

```typescript
type Yikes = Array<Yikes>; //报错
```



#### 3.2.5 字符串自变量类型

**字符串字面量类型允许指定字符串必须的固定值。在实际应用中,字符串字面量类型可以与联合类型,类型保护和类型别名很好的配合。通过结合使用这些特性,可以实现类似枚举类型的字符串**

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

**字符串字面量类型还可以用于区分函数重载**

```typescript
function createElement(tagName: "img"): HTMLImageElement;
function createElement(tagName: "input"): HTMLInputElement;
// ... more overloads ...
function createElement(tagName: string): Element {
    // ... code goes here ...
}
```





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
window.onmousedown = function(mouseEvent) {//这个例子会报错
    console.log(mouseEvent.button);  //<- Error
};
/*
	TypeScript类型检查器使用window.onmousedown函数的类型来推断右边函数表达式的类型。 因此,就能推	 断出mouseEvent参数的类型了。如果函数表达式不是在上下文类型的位置,mouseEvent参数的类型需要指定为	   any,这样也不会报错了
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
  let fun=function():number{
      console.log(123);
      return 123;
  }
  fun();
  ```

- **匿名函数**

  ```typescript
  let result:number=(function():number{
      return 10;
  })()
  console.log(result);//10
  ```

**注意:**TypeScript中的函数也是可以做类型校验的,不过在返回返回值时与箭头函数返回返回值类似

```typescript
//(str:string)=>number 就是对于函数的类型校验
let fun: (str:string) => number = function(): number {
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

**注意:**默认参数和可选参数不能在同一个形参变量上使用,而且两者同时存在的时候传入参数的顺序

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



#### 4.2.3 剩余参数

**在TypeScript中的剩余参数也是和JS中的一样,通过扩展运算符(...)接受剩余参数**

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

**注:**在JS中,如果出现了同名方法,下载下面的方法会替换掉上面的方法

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
class Animale{
    name:string="动物";
    eat(this:Animal){
        console.log(this.name);
    }
}
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

**TypeScript中的类和ES6中类的定义类似,但是也有区别**

```typescript
class Person {
  name: string;//和JAVA类似,要先在这声明对象中拥有的属性和类型,这里的分号很重要
  /*该属性定义相当于public name:string;,只不过省略了public,下面再做解释*/
  constructor(n: string) {
    this.name = n;//和ES6一样,传入构造函数,只不过需要在前面先定义this的属性
  }

  run(): void {
    console.log(this.name);
  }
}

let p:Person = new Person("张三");//可以对实例类型变量进行类型的定义,也可以默认不写为any
let Person2:typeof Person=Person;//把Person类赋值Person2
//typeof Person是表明该对象是一个Person类的类型,而不是Person的实例类型
let p2:Person=new Person2();//因为Person2也是Person类型的类,所以可以这样实例对象
p.age=18;//报错,类只允许有name属性
p.run();//张三
```

**注意:**

- **对象属性设置的时候需要在后面打上分号表示结尾**,不像JS中对象写法
- **constructor函数后面不能加上返回值的修辞符**,否则会报错,可以看作是固定写法
- 函数定义完成后不需要加上符号分割



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

- **private:**私有类型,在类里面可以访问,在子类和类的外部都无法访问

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

- **readonly:**只读类型,可以使用 `readonly`关键字将属性设置为只读的, 只读属性必须在声明时或构造函数里被初始化

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

  

**注意:**如果属性不添加修饰符,默认为公有属性(public)

```typescript
//public
class Person {
    public name: string;//
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
    protected name: string;//
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

**注意:**如果构造函数也可以被标记成 `protected`, 意味着这个类不能在包含它的类外被实例化,但是能被继承

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
    private name: string;//
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



**参数属性**

参数属性通过给构造函数参数前面添加一个访问限定符来声明。 使用 `private`限定一个参数属性会声明并初始化一个私有成员,对于 `public`和 `protected`来说也是一样

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
    }
    
    printName(): void {
        console.log('Department name: ' + this.name);
    }

    abstract printMeeting():void;// 必须在派生类中实现
}

class AccountingDepartment extends Department {
    public age:number=18；
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
department.generateReports(); // 错误: 方法在声明的抽象类中不存在
```



## 6.TypeScript中的接口

**接口是在面向对象编程中一种规范的定义,它定义了行为和动作的规范,起一种限制的作用,只限制传入到接口的数据**

TypeScript中的接口类似于JAVA,同时还增加了更灵活的接口类型,包括属性、函数、可索引和类等

### 6.1 属性类型接口

- **属性类接口一般用作对于json对象的约束(下面的代码还没有使用接口)**

  ```typescript
  //ts定义方法中传入参数就是一种接口
  function print1(str: string): void {
    console.log(str); //约束只能且必须传入一个字符串参数
  }
  print1("string");
  
  //对json对象进行约束
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
  
  function printName(name: FullName): void {
    console.log(name.firstName, name.secondName);
  }
  let obj:object={//属性的位置可以不停
      firstName:"张",
      secondName:"三"
  }
  printName(obj);//传入对象必须有firstName和secondName
  
  let obj2:object={
      firstName:"李",
      secondName:"四",
      age:18
  }
  function printInfo(info: FullName): void {//使用接口可以对批量的函数进行约束,并且内部职能
    console.log(info.firstName + info.secondName+info.age);
  }
  
  printInfo(obj2);//原则上只能传入只含有firstName和secondName的对象,但是如果写在外面传入也不会报错
  /*
  	但是上面这种方法在传入参数的时候不会报错,但是在函数内部使用info.age的时候就会报错,因为接口内部	没有设置age属性,如果不想报错,函数内部使用形参的属性必须是只能有接口定义的
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
//可索引接口也可以用来对一个属性接口进行额外对象的属性检验
interface Arr{//该接口可以限制一个对象必须有color和width,还可以有其他的属性
	color:string;
    width:number;
    [propName:string]:any;
}
```



### 6.4 类类型接口

**类类型接口主要用于对类的约束,和抽象类相似**

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
  private age: number = 2;//也可以有其他的属性,这点和抽象类不同
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

```typescript
function fun<T>(value: T): T {//一般用T代表泛型,当然也可以是其他的非关键字和保留字,可以在函数内用
  let data: T;//T就代表着泛型函数要使用的泛型,通过后期的传入来使用
  data = value;
  return data;
}

console.log(fun<boolean>(true));
console.log(fun(123));//如果不对泛型函数指定泛型,默认为any类型
```



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

console.log(md5<string>("张三"));//泛型声明刚好和接口内部的顺序想呼应
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

//其实两种方法根据对于接口<T>写的位置的不同可以大致推断出其泛型声明指定的位置
```



## 8.TypeScript中的模块

**与ES6一样,TypeScript也引入了模块化的概念,TypeScript也可以使用ES6中的export、export default和import导出和引入模块类的数据,从而实现模块化**

**前后端的区别**

- **require:**node和es6都支持的引入
- **export和import:**ES6支持的导出引入,在浏览器和node中也不支持(node 8.x版本以后已经支持),需要babel转换,而且在node中会被转换为exports,**但是在TypeScipt中使用编译出来的JS代码可以在node中运行,因为会被编译为node认识的exports**
- **module.exports和exports:**只有node支持的导出

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

**当然,也可以直接使用`import`导入一个不需要进行赋值的模板,该模板会自动进行内部的代码**

```typescript
import "./my-module.js";
```



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

- **通过export和impot进行使用**

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
  
  import { A } from "./module";
  let dog = new A.Dog("狗");//传入命名空间
  dog.eat();
  ```

- **通过三斜线指令引入**

  **三斜线指令:**包含单个XML标签的单行注释,注释的内容会做为编译器指令使用,三斜线引用告诉编译器在编译过程中要引入的额外的文件

  **注意:**三斜线指令仅可放在包含它的文件的最顶端。 一个三斜线指令的前面只能出现单行或多行注释,这包括其它的三斜线指令。 如果它们出现在一个语句或声明之后,那么它们会被当做普通的单行注释,并且不具有特殊的涵义

  这里只用`///<reference path=""/>`,其余用法在 [TypeScript中文文档](<https://www.tslang.cn/docs/handbook/namespaces.html>) 查看

  `/// <reference path="..." />`指令是三斜线指令中最常见的一种,它用于声明文件间的 *依赖*,三斜线引用告诉编译器在编译过程中要引入的额外的文件,也就是会引入对应path的文件

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

**注意:**并没有使用`require`关键字,而是直接使用导入符号的限定名赋值,与使用 `var`相似，但它还适用于类型和导入的具有命名空间含义的符号。 重要的是,对于值来讲, `import`会生成与原始符号不同的引用,所以改变别名的`var`值并不会影响原始变量的值



## 10.TypeScript中的装饰器

> **此内容是面向未来的,可以跳过**

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

**注意:**因为装饰器只是个未来期待的用法,所以默认是不支持的,如果想要使用就要打开tsconfig.json中的`experimentalDecorators`,否则会报语法错误

### 10.1 类装饰器

**类装饰器在类声明之前被声明(紧跟着类声明),类装饰器应用于类构造函数,可以用来监视,修改或替换类定义,需要传入一个参数**

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

**类装饰器表达式会在运行时当作函数被调用,类的构造函数作为其唯一的参数,如果类装饰器返回一个值,它会使用提供的构造函数来替换类的声明**

```typescript
/*
通过返回一个继承的类实现一个类的属性和方法的重构,换句话说就是在中间层有一个阻拦,然后返回的是一个新的继承了父类的类,这个类必须有父类的所有属性和方法,不然会报错
*/
function logClass(target: any) {
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
  constructor(public apiUrl = "我是构造函数中的数据") {}
  getData() {
    console.log(123);
  }
}

let http: any = new HttpClient();
console.log(http.apiUrl);//我是修改后的数据
http.getData();//我是修改后的数据

```



### 10.2 属性装饰器

**属性装饰器表达式在运行时当作函数被调用,传入两个参数(都是自动传入的):**

- 对应静态成员来说是类的构造函数,对于实例成员来说是类的原型对象
- 成员的名字

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



### 10.3 方法装饰器

**方法装饰器被应用到方法的属性描述符上,可以用来监视,修改或替换方法定义,传入三个参数(都是自动传入的):**

- 对于静态成员来说是类的构造函数,对于实例成员是类的原型对象
- 成员的名字(只是个string类型的字符串,没有其余作用)
- 成员的属性描述符,是一个对象,里面有真正的方法本身

```typescript
function get(value: any) {
  return function(target: any, methodName: any, desc: any) {
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
  return function(target: any, methodName: any, desc: any) {
    let oMethod = desc.value;
    desc.value = function(...args: any[]) {
 //因为用了方法装饰器,所以实际调用getData()方法的时候会调用desc.value来实现,通过赋值可以实现重构方法
 //原来的方法已经赋值给oMethod了,所以可以改变
      args = args.map(//这个段代码是将传入的参数全部转换为字符串
        (value: any): string => {
          return String(value);
        }
      );
      console.log(args);//因为方法重构了,所以原来的getData()中的代码无效了,调用时会打印转换后参数
      /*
      	如果想依然能用原来的方法,那么写入下面的代码,相当于就是对原来的方法进行了扩展
      */
       oMethod.apply(target, args);//通过这种方法调用可以也实现原来的getData方法
    };
  };
}

class HttpClient {
  public url: any | undefined;
  constructor() {}
  @get("hello world")
  getData(...args: any[]) {
    console.log(args);//[ '1', '2', '3', '4', '5', '6' ]
    console.log("我是getData中的方法");
  }
}

let http = new HttpClient();
http.getData(1, 2, 3, 4, 5, 6);//[ '1', '2', '3', '4', '5', '6' ]
```



### 10.4 方法参数装饰器

**参数装饰器被表达式会在运行时当作函数被调用,可以使用参数装饰器为类的原型增加一些元素数据,传入三个参数(都是自动传入的):**

- 对于静态成员来说是类的构造函数,对于实例成员是类的原型对象
- 方法的名字(只是个string类型的字符串,没有其余作用)
- 参数在函数参数列表中的索引

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



### 10.5 装饰器的执行顺序

**装饰器的执行顺序大部分按照代码的执行顺序运行**

**注意:**

- 如果有多个同样的装饰器,会从后到前依次执行
- 如果方法和方法参数装饰器在同一个方法出现,参数装饰器先执行
# JS

## 1.认识JS

**js的三大组成部分:**

- **ECMAScript:**js语法的基础,定义了js的基本语法
- **BOM:**提供了操作浏览器这个软件的一些方法
- **DOM:**提供了操作标签元素的一些方法

**js的两个阶段**

- **编译期:**对代码进行静态分析,找到声明的变量和函数,检查语法是否符合规范
- **执行期:**运行js代码

### 1.1 如何在用js书写

**通过js可以实现三种书写形式**

- **document.write()**,这种形式是直接在页面中书写值,会把传入的值当作字符串传递在页面中,可以直观在页面看到
- **console.log()**,这种形式是在控制台中打印值,在页面中看不到,必须通过F12查看控制台才能看见
- **alert()**,这种形式是在打开页面时生成弹窗效果,这种方式也是最直观的,因为很容易被关注,但是只能在页面出现一次,不能一直存在



### 1.2 三种提示框

- **alert()警告框**,用法如上

- **confirm()确认框**,向其中传入参数代表要用户确认的内容,有确认和取消两个选项,如果用户选择确认则返回true，选择取消则返回false,可以用一个变量来接收返回值进行js判断

- **prompt()提示框**,传入的参数作为提示的内容,而用户可以在提示框中输入内容,返回值为用户输入结果转换的字符串，可以用一个变量接收

  **注意:**当想用两个提示框的返回值进行比较时,相当于比较的两个字符串,所以是用的Unicode编码进行比较,如果想比较的是两个数值,则需要在接收返回值时做强制类型转换

  ```js
  var a=+prompt("");//+号强制转换返回值为number类型
  ```

  

## 2.标识符及数据类型

### 2.1 标识符

- **标识符可以含有字母，数字，_,_$等;**

- **标识符不能以数字开头；**

- **标识符不能是JS中的关键字或保留字；**

- **标识符一般采用驼峰命名法：首字母小写，每个单词的开头字母大写，其余字母小写**
- **标识符就是JS中的变量(ES6有了常量的概念),变量使用前必须要先定义**
- **有3种命名符来命名表示符**
  - var，ES6之前用的命名符,但是作用域是函数作用域,并且支持在当前作用域中进行重复命名,有很大隐患

  - let，ES6新增命名变量方法,作用域和其他语言的作用域一样都在块级作用域中,ES6推荐使用

  - const,ES6新增的常量命名法,用这个命名符来命名的标识符为常量,只能用不能修改值,修改值就会报错,推荐在某一个量只用一次的时候用这个命名符

    **注意:**用const声明的常量不是不能赋值,而是不能通过=进行二次赋值,在不改变原来常量结构的情况下可以

    改变这个常量的属性值



### 2.2 数据类型

#### 2.2.1 六大数据类型

**注：ES6新增为7种,多了一种symbol符号的数据类型用作私有变量的使用**

**JS的数据类型分为5大基本数据类型和一个复杂数据类型object**

- **字符串string**

  - 字符串可以用""(单引号)或者''(双引号)或者``(反引号:ES6支持)

  - 单双引号没有区别，但是不要混合用
  - 在字符串中要使用js的变量有两种写法
    - ES5支持的用字符串拼接的方法

    - ES6中支持的必须用``(反引号)才支持的${变量名}的写法

      用``写的字符串又被称作**模板字符串**,能够更轻松的在字符串中插入变量

  - 当需要用一些特殊符号表示时，可以在特殊符号前加一个\和转义字符的用法基本相似比如用\ "表示"

    \ \ 表示 \

- **数值number**

  - JS中可以表示数值的最大值Number.MAX_VALUE

  - 如果超过了最大值,就会返回一个Infinity意为无穷,此时这个无穷是正无穷,而-Infinity为负无穷 

  - NaN也是一个特殊的数字，意为not a number,可以用很多方法得到这个数

    比如:两个字符串相乘返回NaN

  - Number.MIN_VALUE为最小值,但是是0以上的最小值,并不是负数
  - JS中的整数运算可以基本保证精确，浮点运算得到的结果不够精确

- **布尔值boolean**

  该类型只有两个值:true(正确),false(错误)

- **null**

  null的值就是null，专门用来表示空对象

- **undefined**

  - undefined的类型只有一个,就是undefined,当声明一个变量,却不给它赋值时,它的值就是undefined

  - 使用typeof来检验undefined也会返回undefined

- **object**

  这个类型的值最多,应为所有事物都可以看做是对象



#### 2.2.2 强制类型转换

-  **将其他数据类型转换为String**

  **1.调用被转换数据类型的toString()方法**

  **注:**

  **1.**该方法不能影响原来变量，而是有一个返回值

   **2.**null和undefined这两个值没有toString()方法，如果调用会报错

  ```js
  var a=123;
  a=a.toString();
  ```

  

  **原型prototype后说明toString()方法**

  当我们直接在页面中打印一个对象时，实际上是输出对象的toString()方法的返回值,所以直接打印一个对象和打印这个对象用toString后的值是一样的,如果我们希望在输出对象时不输出[Object Object],可以为对象的原型添加一个toString()方法来改变输出的值,所以可以在里面写入自己想要的提示(直接只写return就可以),如果用的构造函数的方法最好加上this

  

  **2.调用String()函数**

  使用String()函数做强制转换时，实际对于Number和Boolen等调用toString()方法,但是对于null和undefined会直接转换为"null"和"undefined"

  ```js
  var a=null; 
  a=String(a);
  ```

  

- **将其他数据类型转换Number**

  **1.使用Number()函数**

  - **字符串转换为数字**	

    - 如果是纯数字字符串，则直接转化为数字	

    - 如果字符串中有非数字字符，则返回NaN,只要有就会

    - 如果是一个空串或者一个全是空格的字符串则会返回0

      **如：**

      ```js
      var a="123";
      a=Number(a);//会返回123
      var b="abc"
      b=Number(b);//会返回NaN
      ```

  - **布尔值转换为数字**

    true为1,false为0

  - **null转化为数字为0**  

  - **undefined转化为数字为NaN** 	

  

  **2.使用parseInt()和parseFloat()函数**

  **注:该方法在ES6已经移植到Number对象下,作为Number的方法使用,与全局时的行为一样**

  该方法可以取出字符串的有效数字内容,从头开始取,parseInt是只要遇到非数字就会立刻结束,而parseFloat允许接收到一个小数点,遇到其他的非数字或者第二个小数点时会停止,想要把a="123px"中的123取出来的时候用这种方法才能达到效果​

  **将一个小数转换为整数可以对它使用parseInt()函数**	

  **注意:**

  - 这个方法最好专门对字符串用 如果对非String用回先把它转换为String再进行操作

    **如：**如果是布尔值等会转换为"true"在这个字符串再进行转换，这样会返回NaN

  - 这个方法现在是可以直接作为window的方法调用,所以可以事件省略,但是ES6规定必须要通过Number内建函数来调用,所以最好还是通过Number对象调用

    ```js
    var a="1.23px"
    console.log(Number.parseFloat(a));//1.23
    ```

    

  **对数值及parseInt函数实现进制转换的补充:**

  - 在JS表达16进制数字，需要以0(零)x开头 。如:a=0x10,输出的时候得到a=16

    任何用上方这种语法写的数字输出的时候都会转换为16进制的数字

  - 需要表示8进制的数字需要以0开头,用法如上

  - 2进制开头用0b表示(但不是所有浏览器都支持)

  - 实际上parseInt这个函数可以接收两个参数,第一个是要转换的字符串,第二个是需要转换的进制

    **注意:**把 a="070"转化为整数,a=parseInt(a)再输出有些浏览器会当成8进制，有些浏览器会省略掉前面的0直接变为10进制，如果想所有的浏览器一样，可以a=parseInt(a,10)，添加第二个参数，来指定数字的进制

  

- **将其他的数据类型转换为布尔值**

  **使用Boolean()函数**

  - 数字转换为布尔值，除了0和NaN都是true

  - 字符串转换为布尔值，除了空串都是true

  - null转换为布尔值，false

  - undefined转换为布尔值，false

  - 对象object也会转换为true

  **六大假值:0  NaN  undefined  null  ""  false**



#### 2.2.3 Symbol

##### 1.用法

**Symbol实际上是ES6引入的一种原始数据类型,Symbol引入出来的基础就是为了保存私有属性名,确保属性名发生冲突**,当引入一个别人创建的对象时,如果不清楚这个对象有哪些属性,但是又想要为这个对象添加一个属性时,这时两者如果有相同的属性名就会发生属性名的冲突问题,为了避免这种问题,所以引入了Symbol来创建一个独一无二的值

```js
//Symbol类型的值不能字面量创建,而是通过Symbol()函数来创建
var sym=Symbol();
console.log(sym);//Symbol();
console.log(typeof sym);//symbol
//现在当使用sym来做属性名时就不会与其他的属性名发生冲突
```

**注意:**

- **Symbo()函数不是一个构造函数,不能用new操作符。**所以Symbol类型的值也不是一个对象,不能添加任何属性,它只是一个类似于字符串的数据类型,如果在声明一个Symbol类型的值前面加上new就会报错

- **Symbol()类型的值不能够进行计算,**包括Symbol类型的任意数据类型都不可以,一旦进行计算就会报错

- **Symbol类型的值可以转换为字符串和布尔值,但是不能转换为数值,否则会报错,**并且转换成布尔值时永远都是true,不管内部是什么参数

  ```js
  var sym=Symbol();
  var sym1=Symbol(false);
  
  console.log(String(sym));"Symbol()"
  console.log(String(sym1));"Symbol(false)"
  console.log(Boolean(sym));//true
  console.log(Boolean(sym1));//true
  ```

- **因为是通过一个变量来接收的Symbol值,所以在使用Symbol值作为属性名时,获取对应的属性不能用点操作符,**如果用点操作符实际上时获取一个字符串,而不是Symbol值。同时**在对象内部用该变量创建属性名时也要要[]括起来**,否则该属性名依然只是一个字符串,而不是用的Symbol变量

  ```js
  //Symbol值作为属性名时有多种用法
  var name=Symbol();
  var a={};
  
  //第一种
  a[name]="孙悟空";
  
  //第二种
  a={
      [name]:"孙悟空";
  }
  
  //第三种
  Object.defineProperty(a,name,{value:"孙悟空"});//这里的name是变量
  
  //输出
  console.log(a.name);//undefined
  console.log(a[name]);//孙悟空
  console.log(a["name"]);//undefined
  ```

- **当遍历对象的属性值时,无法用for...in和for...of语句遍历到Symbol值的属性,也无法通过Object.keys() 、Object.getOwnPropertyNames()等函数来获取。**但是可以通过使用Object.getOwnPropertySymbols()函数获取一个对象上的Symbol属性名,也可以使用Reflect.ownKeys()函数返回所有类型的属性名,包括常规的属性名和Symbol属性名

  ```js
  var sym1=Symbol("sym1");
  var sym2=Symbol("sym2");
  var a={
      [sym1]:"sym1",
      [sym2]:"sym2",
      hello:"hello"
  }
  console.log(Object.getOwnPropertyNames(a));//[Symbol(sym1),Symbol(sym2)]
  console.log(Object.keys(a));//["hello",Symbol(sym1),Symbol(sym2)]
  ```

  **注:**正因为Symbol值作为对象的属性值无法被遍历到的这一特性,可以对对象定义一些非私有的但是又希望只有内部可用的方法



##### 2.Symbol()的参数

- **字符串作参数**

  **在创建一个symbol类型的值的时候,如果不传入任何参数,在调用变量的时候不好进行去区分,打印的值全都是`Symbol()`,传入一个字符串作为参数,可以对Symbol的值进行描述,方便我们区分不同的Symbo值**

  ```js
  var sym1=Symbol("sym1");
  var sym2=Symbol("sym2");
  var sym3=Symbol("sym1");
  console.log(sym1);//Symbol(sym1)
  console.log(sym2);//Symbol(sym2)
  console.log(sym1===sym3);//false
  
  /*
  	1.给Symbol()函数传入参数后可以在控制台输出的时候区分到底是哪一个值
  	2.Symbol()函数中传入的参数只是用来对当前的Symbol变量的描述,而没有其他意思,相同参数的
        Symbol()函数的返回值是不同的
  */
  ```

- **其他类型作参数**

  **当用其它类型的值作为参数传入到Symbol()函数中时,会自动将该参数转换为字符串类型,但是注意当传入的值为undefined的时候此时Symbol()函数中相当于没有传入参数,所以返回的依然是Symbol()**



##### 3.Symbol.for与Symbol.keyFor函数

- **Symbol.for()也函数可以用来生成Symbol值,但该函数有一个特殊的用处,该函数创建的Symbol值可以通过内部的参数重复使用一个Symbol值**

  Symbol.for()函数接收一个字符串作为参数,先搜索有没有以该参数为名称的Symbol值,**如果有直接返回这个Symbol值,如果没有就新建一个以给字符串为名称的Symbol值**

  ```js
  var sym=Symbol("sym1");
  var sym1=Symbol.for("sym1");
  var sym2=Symbol.for("sym2");
  var sym3=Symbol.for("sym1");
  
  console.log(sym===sym1);//fasle,因为用Symbol()创建的Symbol值没有计入Symbol值的登记中
  console.log(sym1===sym2);//false,因为参数标识不一样,重新创建了Symbol值
  console.log(sym1===sym3);//true,参数标识一样,相当于先前是sym3=sym1
  ```

- **Symbol.keyFor()函数用来查找一个Symbol值的登记信息(这个登记信息就是创建Symbol值时在函数中传入的参数)**,因为Symbol()函数创建的Symbol值没有登记机制,所以会返回undefined,而Symbol.for()函数会将生成的Symbol值的信息记录在全局环境中,所以Symbol.keyFor()函数可以查找到Symbol.for()函数创建的Symbol值，**该函数的返回值是字符串**

  ```js
  var sym1=Symbol.for("sym1");
  console.log(Symbol.keyFor(sym1));//"sym1"
  console.log(Symbol.keyFor(sym1)==="sym1");//true
  ```

  

### 2.3 解构赋值

**解构赋值为ES6的语法,ES6中,允许按照一定模式,从数数组和对象中提取值,对变量进行赋值,这种行为被称为解构**

```js
//右边为数组
var [a, b, c] = [1, 2, 3];
console.log(a, b, c);//1,2,3
//右边为对象
var {x:x,y:y,z:z} = {x: 1, y: 2, z: 3};
console.log(x, y, z);//1,2,3

var {x,y,z} = {x: 1, y: 2, z: 3};
console.log(x, y, z);//1,2,3
/*
注意:
1.当右边为对象时,对象中的属性也是模式的一部分,所以必须也要在左边写上相同的属性名
2.当左边的属性名和属性值的变量时相同时,可以只写属性值,这个属性值也是要用的变量名
*/

//模式不一致
var arr=[1,2,[3]];
var [a,b,c]=arr;
console.log(a,b,c);//1,2,[3]
//模式一致
var [a,b,[c]]=arr;
console.log(a,b,c);//1,2,[3]

var [json, arr, num, str] = [{ a: 1, b: 2 }, [1, 2, 3], 8, 'str'];
 console.log(json, arr, num, str);//{ a: 1, b: 2 }, [1, 2, 3], 8, 'str'
```

**注意:**

- 左右两边模式必须一样(都是数组或对象字面量)

- 声明和赋值赋值不能分开，必须在一句话里

  

## 3.运算符

**运算符不会对原来的值进行影响**

**运算符的运算时按住自身的层级进行顺序运算**

### 3.1 JS功能运算符

- **typeof,用于鉴定数据类型的时候使用,它会将值得类型以字符串的形式返回**

  ```js
  var a=123;
  var result=typeof a;
  console.log(result)/*这里会显示number*/
  /*
  	注意:typeof对null用时会返回object
  		typeof对函数function用时会返回function,实际上函数也是一个对象
  		typeof对symbol数据的返回值就是symbol
  */
  ```

- **in,用于检查一个对象中是否含有指定的属性,如果有则返回true,没有则返回false**

  ```js
  var obj=new Object();
  console.log("test2" in obj); //这里显示false,因为obj中没有这个属性
  							 //属性名实际上是字符串，所以要加""
  ```


- **instanceof,用于确定一个对象是否在另一个对象的原型链上,也就是一个对象是否属于另一个类对象的实例**

  ```js
  //可以使用instanceof可以检验一个对象是否是一个类的实例
  
  function Person(name){
  	this.name=name
  }		
    var per= new Person("孙悟空");
    console.log(per instanceof Person);//如果per是Person类的实例返回true，否则返回false
    
    console.log(per instanceof Object);
  //所有的对象都是Object的后代，所有任何对象和Object与instanceof检验都会返回true
  ```



### 3.2 算术运算符

**JS中的算术运算符和其他语言一样有+ - * / %等**

**注意:**

- 在用算术运算符进行运算的时候如果进行运算的两边有非number类型的,则会先转换为number类型,再进行运算,如:a=true a=-a会将a转化为number,所以可以利用这个特性用a=+a将一个其他类型的数很快的转化number类型
- 在ES6中新增了三点(扩展)运算符(...)和指数运算符(**)等



**+的特殊用法:**如果对两个字符串进行加法运算，则会做拼串并返回,任何值和字符串做加法运算，都会先转换为字

符串，然后再拼串

**注:**可以利用+的特殊性这一特点将任意数据类型转换为String，将任意数据类型+一个""空串转换为String，实质上也是调用的String函数

```js
var result=1+"2"+3//结果会是字符串123

var result=1++"2"+3//结果会是数字6，++"2"实际是是先计算的+"2"把字符2转换为数字2
```

**自增自减**

自增有两种:a++和++a,两种方法用完a都马上+1

自减和自增没有差别

**注意:**

a和a++不同,a是一个变量，而a++是一个表达式

a++等于原值,++a等于+1后的值，调用一次就直接用表达式进行运算

 

### 3.3 逻辑运算符

**逻辑运算符有:! && ||三种**

- !(非运算符)对一个数进行非运算，如果不是布尔值会被转换为布尔值(这种运算方法只适合JS)  

  **可以利用这一特性为任意数据类型取两次反将其转换为布尔值，原理与Boolean()一样**

  ```js
  var a="hello"
  a=!!a//true
  ```

- &&(与运算符)如果第一个值就是false就不会去看第二个值，第一个值是true会检查第二个值

- ||(或运算符)如果第一个值是true就不会检查第二个值,直接返回第一个值

  ```js
  true||alert("s")//不会执行弹窗
  false||alert("s")//执行弹窗
  ```

- ^(异或运算符),该运算符两边的值都为真或假时会返回false,只有一个为真一个为假时才会返回true

  ```js
  console.log(true^flase);//1
  console.log(true^true);//0
  console.log(false^false);//0
  ```

对于非布尔值进行运算，会先转换为布尔值，再进行运算，并返回原值，不会返回布尔值

**总结:**

- &&运算如果第一个值为true则直接返回第一个值，如果第一个值是false，则直接返回第一个值

- ||运算如果第一个值为true则直接返回第一个值，如果第一个值是false则直接返回第二个值

 

### 3.4 关系运算符

通过关系运算符比较两个值之间的大小关系,关系成立则返回true,关系不成立则返回false

**关系运算符有:\>   <    >=    <= 等**

**注意:**

- 关系运算符比较的一般都是number类型的值,如果不是number类型会转换为number类型

- 任何值和NaN做比较都会返回false

- 如果符合两测的值都是字符串时(如果有一方不是字符串就是全部转换为number类型再比较)，不会将其转换

  为数字进行比较，而会分别比较Unicode编码(UTF-8)，和C语言中的strcmp一样。所以，在比较两个字符串型

  的数字时，一定要转型，至少在其中一个前面加上+



**Unicode编码**

想输出一个Unicode编码需要在前面加上\和u 如\u1C00,在JS控制台中输入的编码是16进制数

如果要在网页中输入为&#+编码;这里的编码是10进制数，所以会和在JS控制台里的输入不一样

所以**在需要用到unicode编码是要清楚写在网页的代码和写在控制台代码的区别**

 

### 3.5 相等运算符

相等运算符通过判断两边是否相等来返回值,如果符合返回true,不符合返回flase

**相等运算符有==  ===  !=  !===等**

- **当==和!==两边类型不同时会进行类型转换再做比较，大部分情况都是转换为数字**

```js
console.log(true=="1")//将两边都转换为number,返回true

/*特殊情况*/
console.log(null==0)//按理说null转换为number应该是0结果是true，但是这个返回false
console.log(unfined==null)//undefined衍生于null，所以这两个值做相等判断时会返回true
```

- **当===(全等)和!==(不全等)两边类型不同时不会进行自动类型转换,直接返回false**

**注意:**

- 字符串相等依然是比较的unicode编码

- NaN不和任何值相等，包括它本身,所以不能通过相等运算符来判断一个值是否是NaN,可以通过isNaN()函数来

  判断

  ```js
  var a="abc";
  var b=NaN;
  console.log(isNaN(a));//true,先强制转换为数值类型
  console.log(isNaN(b));//返回true,如果b不是NaN返回false 
  ```

   **注意:**

  - 在ES5中该方法是全局变量的方法,而在ES6中该方法是Number内置对象的方法,需要通过Number.isNaN()来调用

  - 通过Number.isNaN()来调用该方法时不会强制转换类型,如果是非数值类型直接返回false

    ```js
    var a="abc";
    console.log(Number.isNaN(a));//false
    ```

    

**注:在ES6中引入了一个Object.is()方法作为判断,该方法能对NaN进行相等判断**

Object.is()方法可以接收两个参数,这两个参数为进行比较的两个值，该函数不会进行强制类型转换,相当于===

**与全等(===)的区别**

- +0不能用-0
- NaN等于自身

```js
console.log(Object.is(NaN,NaN));//true
console.log(Object.is(+0,-0));//false
console.log(Object.is(123,"abc"));//false
console.log(Object.is(123,"123"));//false
```



**拓展:**

- **Number.isFinite()方法用来检测一个数值是否为有限的(finite),该方法不会进行强制类型转换,非数值的类型将直接返回false**

  ```js
  console.log(Number.isFinite(1));//true
  console.log(Number.isFinite(NaN));//false
  console.log(Number.isFinite(Infinitey));//false
  console.log(Number.isFinite(-Infinitey));//false
  console.log(Number.isFinite(false));//false
  console.log(Number.isFinite(true));//false
  console.log(Number.isFinite("15"));//false
  console.log(Number.isFinite(Math.PI));//true，JS里的PI只保留了前几位小数
  ```

- **Number.isInteger()方法用来判断一个值是否为整数,也不会强制转换类型**

  ```js
  console.log(Number.isInterger(1));//true
  console.log(Number.isInterger(1.0));//true
  console.log(Number.isInterger(1.1));//false
  console.log(Number.isInterger("1"));//false
  console.log(Number.isInterger(true));//false
  ```

  **注意:**在JS内部,整数和浮点数时同样的储存方法,如1与1.0是同一个数



### 3.6 扩展运算符

**...为扩展运算符(ES6新增)，用作展开一些列表式的值(比如数组和JSON对象)**,会将列表以原本的方式展开

```js
var arr=[1,2,3];
var arr2=[...arr];//arr展开为1,2,3
var obj=["a":0,"b":1];
var obj2=[...obj]
console.log(arr2);//[1,2,3]
console.log(obj2);//["a":0,"b":1]
```

**注:其它用法在后面有详细说明**



### 3.7 运算符优先级

**js中的运算符优先级是一套规则,该规则在计算表达式时控制运算符执行的顺序,具有较高优先级的运算符先于较低优先级的运算符执行**

**下表按从最高到最低的优先级列出js运算符,具有相同优先级的运算符按从左至右的顺序求值**

| 运算符                             | 描述                                         |
| ---------------------------------- | -------------------------------------------- |
| . [] ()                            | 字段访问、数组下标、函数调用以及表达式分组   |
| ++ -- - ~ ! delete new typeof void | 一元运算符、返回数据类型、对象创建、未定义值 |
| * / %                              | 乘法、除法、取模                             |
| + - +                              | 加法、减法、字符串连接                       |
| << >> >>>                          | 移位                                         |
| < <= > >= instanceof               | 小于、小于等于、大于、大于等于、instanceof   |
| == != === !==                      | 等于、不等于、严格相等、非严格相等           |
| &                                  | 按位与                                       |
| ^                                  | 按位异或                                     |
| \|                                 | 按位或                                       |
| &&                                 | 逻辑与                                       |
| \|\|                               | 逻辑或                                       |
| ?:                                 | 条件                                         |
| = oP=                              | 赋值、运算赋值                               |
| ,                                  | 多重求值                                     |

转自https://www.cnblogs.com/yy-hh/p/4624792.html



## 4.逻辑语句

### 4.1 if语句

if语句对一个属性或者表达式进行判断,里面的数或表达式最后会转换为布尔值，如果为真才执行执行if语句里面的

代码

#### 4.1.1 if

```js
if(true){
    console.log(1);
}
//只判断是否为真
```



#### 4.1.2 if...else

```js
if(true){
    console.log(1);
}
else{
    consle.log(2);
}
//判断时否为真,如果为真执行上面的代码,如果为假执行下面的代码
```



#### 4.1.3 if....else if....else

```js
var a=1;
if(a===1){
    console.log(1);
}
else if(a===2){
	console.log(2);    
}
else{
    console,log(3);
}
//从上到下一次进行判断,如果第一次判断为真则执行第一个判断里面的代码,如果第一次为假第二次为真则执行第二个	判断里的代码,如果都为假则执行第三个判断里的代码

  //在这里else if可以写无数个,因为这个其实还是按照if...else的写法来写的
```



### 4.2 switch语句

```js
switch(x)
{
    case 1：；
    break;
    case 2: ;
    break;
  	default:
}
//括号里的字姆依次与下面做全等(===)比较
```

```js
可以在case里面用数字或者字符一起用，只是做全等比较而已，比如和以x="abc"
switch(x)
{
	case 1:;
    break;
	case 2:;
	break;
	case "abc":;
	break;
	default:
}
```

**注意:**

- **JS里面的switch中的case可以接受一个表达式**，因为swich语句在其他语句时case里面只能是数字或者字符

```js
var score=80;
switch(true){
case score>=60：
case score>=50:
}
```

-  switch语句中每一个case后面需要加上break来表示跳出选择,不然就会一直往下面运行case直到遇到break或

  将里面的选项全部运行完



## 5.循环语句

### 5.1 while与do...while语句

while和do...while都是循环语句,在条件允许的情况下会一直循环语句内的代码

```js
var i=0;
while(i>5){
    console.log(i);
}

do{
	console.log(i);
}while(i>5)
    
//两个语句循环功能类似，不同的是while是先判断后执行，do...while是先执行后判断
//所以在进行判断的时候while可以一次都不执行,而do...while至少会进行一次
```



### 5.2 for循环语句

```js
/*
for(单次表达式;{条件表达式};末尾循环体)
{
	中间循环体；
}
*/
for(var i=0;i<5;i++){
    console.log(i);
}
//单次表达式只会在开始的时候进行一次,后面将不会进行,可以在单次表达式声明变量,也可以在前面先声明变量
//执行末尾循环体后将再次进行条件判断，若条件还成立，则继续重复上述循环，当条件不成立时则跳出当下for循环
```

**注意:**

- 表达式皆可以省略，但分号不可省略，因为“;”可以代表一个空语句，省略了之后语句减少，即为语句格式发生

  变化

- 执行的中间循环体可以为一个语句，也可以为多个语句，当中间循环体只有一个语句时，其大括号{}可以省

  略，执行完中间循环体后接着执行末尾循环体

- 可以用break和continue等提前中断循环语句,但是这两个只是中断离得最近的循环语句

- 可以使用label(关联)加上break或continue来跳出多个循环(标记内容不一样是label单词，可以为任意符合标准的) 结束离标记最近的for循环,如果没有直接使用标记的话，break或continue结束的是当前的for循环 

  ```js
  for(var k = 0;k<5;k++){
      console.log( '我是第k:'+k+'个' );
      
      jump://结束下面最近的for循环
      for( var i = 0; i <3;i++ ){
          
          console.log( '我是第i:'+i+'个' )；
          
          for( var s = 0;s<4;s++ ){
              if( s === 2 ){
                  
                  break zhuang; //结束
              }
              
              console.log( '我是第s:'+s+'个' )；
          }
          
      }
      
  }
  ```


- 在用for循环循环一个数组的长度时,如果先在外边将数组的长度赋值,会提升程序性能

  ```js
  var arr=[1,2,3,4,5];
  for(let i=0,len=arr.length;i<len;i++){
      console.log(i);
  }
  ```

  

### 5.3 性能测试

用console.time()测试程序性能

```js
 /*
 console.time("计时器的名字")可以用来开启计时器，它需要一个字符串作为参数，这个字符串作为计时器的标识
 然后在需要测试的代码后面加console.timeEnd("计时器的名字")停止一个计时器
*/
console.time("test");
for(var i=0;i<5;i++){
    console.log(i);
}
console.timeEnd("test");
// 会在控制台中显示之间一共用了多长时间
```



## 6.普通对象

### 6.1 对象的分类

**JS中数据类型有String Number Boolean Null Undefined Object前五个为基本数据类型,Object对象为引用**

**数据类型,值只要不是前5种,都是对象**

```js
 //使用基本数据类型创建变量
 var name ="孙悟空";
 var gender="男";
 var age=18;
//使用基本数据类型的创建数据，我们所创建的变量都是独立，不能成为一个整体

//使用对象创建变量
var obj={name:"孙悟空",gender:"男",age:18};
//对象属于一个复合的数据类型，在对象中可以保存多个不同数据类型的属性
```

**对象的分类:**

- **内建对象**

  由es标准中定义的对象，在任何的ES的实现中都可以使用,如 Math String Number Boolean Function Object

  等

- **宿主对象**

  由js的运算环境提供的对象，目前来讲主要指由浏览器提供的对象如BOM DOM,如console.log中的console和

  document.write中的document就是一个对象

- **自定义对象**

  由开发人员自己创建的对象



### 6.2 创建对象

#### 6.2.1 构造函数创建对象

**使用new关键字调用的函数，是构造函数construstor,构造函数是专门用来创建对象的函数**

```js
var obj=new Object();//通过构造函数的方式创建对象
console.log(typeof obj)//使用typeof检查一个对象时，会返回object
console.log(obj)//这里obj是一个空的对象,之间打印这个对象会返回Oject{}
```



- **属性**

  在对象中保存的值称为属性,向对象中添加属性,语法:对象.属性名=属性值;或对象["属性名"]=属性值;

  ```js
  var obj=new Object();
  obj.name="孙悟空";
  obj.gender="男";
  obj.age=18;
  console.log(obj)//在这里obj中有值会打印出Object{name:"孙悟空",gender:"男",age:18}
  ```

  **注:**也可以使用Object.defineProperty()方法与Object.defineProperties()方法为对象设置一个或多个属性,并且能对设置的属性进行额外的操作

  

  - **读取对象中的属性**

    **语法:对象.属性名或对象["属性名"]**

    **注:**如果读取对象中没有的属性,不会报错而是会返回undefined

  - **修改对象的属性值**

    **语法:对象.属性名=新值或对象["属性名"]=新值**

  - **删除对象的属性**

    **语法:delete 对象.属性名或delete 对象["属性名"]**

    **注:**删除后的属性不再存在于整个对象中,如果再次读取会返回undefined

  - **验证对象中是否有该属性**

    **语法:"属性名" in 对象**

    **注:**属性名实质上时字符串,所以要加引号

  

- **属性名**

  **对象的属性名不强制要求遵守标识符的规范,但是使用时尽量按照标识符的规范去做**

  **如果在创建属性名时用的是.或者[]方法中的一个,在调用和修改删除时也要用同样的方式调用**

  **要使用特殊的属性名，不能使用.的方式，需要用   对象["属性名"]=属性值的方式**

  使用[]这种形式去操作属性更加的灵活，在**[]中甚至可以直接传递一个变量**,这样就能通过变量名来改变属性名,

  而**.只能跟去掉了""的字符串.的方式,不能使用变量**

  ```js
  var obj=new Object();
  var x="name";
  
  obj["name"]="孙悟空";
  obj["gender"]="男";
  
  console.log(obj[x]);//打印结果为孙悟空
  x="gender";
  console.log(obj[x]);//打印结果为男
  ```

  **注意:**

  - 如果[]中是一个变量就不要加引号,否则会认为是一个字符串
  - []里面除了字符串必须加引号，其他的可以不加引号，和基本数据类型相似



- **属性值**

  JS对象的属性值,可以是任意的数据类型,包括对象

  ```js
  var obj=new Object();
  obj.test=obj2;
  obj2=new Object();
  Obj2.name="猪八戒"；
  
  console.log(obj.test);//打印出obj2对象
  console.log(obj.test.name);//打印出猪八戒
  //由此可以说明对象的属性值可以是以对象来构成无限嵌套关系
  ```

  

- **枚举对象中的属性**

  for…in语句与for...of语句能够枚举对象中的属性，对象中有几个属性，循环体就会执行几次

  **注:**

  - for...of语句只能遍历数组,不能遍历JSON对象
  - for...in的性能很差,因为会遍历对象的原型对象

  ```js
  /*
  语法:
  for(var 变量 in/of 对象）{
  }
  */
  var obj={name:"孙悟空",gender:"男",age:18};
  for(var i in obj){
  	console.log(obj[i]);//打印出属性值
  }
  /*
  for…in语句
  每次执行时，会将对象中的属性名赋值给变量
  可以利用console.log(n);来获取属性值，n取得的属性值都会转化为字符串的形式,和前面的查找一个属性是否在字符串里面用in的情况不同
  */
  
  for(var i of obj){
  	console.log(i);//打印出属性值
  }
  /*
  for...of语句
  每次执行时,会将数组中的值赋值给变量，和for...in语句有所差别
  */
  ```




- **Object.defineProperty()与Object.defineProperties()方法**

  + **Object.defineProperty()方法接收三个参数,第一个为需要添加属性的对象,第二个为属性的属性名,第三个参数为一个对象,对象中有该属性需要的属性值和其他的一些对该属性的特殊属性**

    - `configurable`

      描述属性是否配置，以及可否删除,` **默认为 false**

    - `enumerable`

      描述属性是否可被枚举,会出现在for in 或者 Object.keys()的遍历中 ,**默认为 false**

    - `value`

      设置属性值,**默认为 undefined.**

    - `writable`

      是否可被修改值,**默认为 false**

    - `get`

      作为该属性的 getter 函数，如果没有 getter 则为`undefined`。函数返回值将被用作属性的值。 **默认为 undefined**

    - `set`

      作为属性的 setter 函数，如果没有 setter 则为`undefined`。函数将仅接受参数赋值给该属性的新值。 **默认为 undefined**

    ```js
    var obj = {};
    Object.defineProperty(obj,'property1',{
        value:"Hello"
    });
    ```

  + **Object.defineProperties()方法接收两个参数,第一个参数为要添加属性的对象,第二个参数为一个对象,内部可以设置多个属性,额外的属性效果同bject.defineProperty()**

    ```js
    var obj = {};
    Object.defineProperties(obj, {
      'property1': {
        value: true,
        writable: true
      },
      'property2': {
        value: 'Hello',
        writable: false
      }
      // etc. etc.
    });
    ```

    

#### 6.2.2 对象字面量创建对象

**语法:{属性名:属性值,属性名:属性值....}**

```js
var obj={name:"孙悟空",gender:"男",age:18};
```

**注意:**

- 属性名和属性值之间的=要变成:

- 对象字面量的属性名加引号也可以不加，建议不加。但是如果使用一些特殊非法的名字，必须加引号，汉字也

  可以使用，对象字面量里面也可以嵌套使用对象字面量

**对象字面量创建对象的简化操作**

- 在创建函数的时候可以省略`:function`,直接用函数名
- 可以使用[]对属性名进行操作

```js
var obj={
    ["a"+"b"]:123,
    
    /*
    say:function(){
    	alert("hello");
    }
    */
    //方法的简写
    say(){
        alert("hello");
    }
}
```



### 6.3 JSON

**JSON全称为JavaScript Object Notation(javaScript对象表示法)**,JS中的对象只有JS才能够解析并识别,其它语言都不能识别,所以如果需要做数据传输,就需要一个所有语言都能够识别的通用数据类型

**JSON就是一个特殊格式的字符串，这个字符串可以被任意的语言识别，并且可以转换为任意语言中的对象，JSON在开发中主要用来数据的交互**,JSON和JS中对象的格式一致，只不过JSON字符串中的属性名和属性值必须用双引号括起来,除此之外内部结构的语法和JS中的语法一致

#### 6.3.1 JSON分类

- 对象{}
- 数组[]

 **注意:**无论是对象还是数组都必须使用字面量形式的,并且在最外层都需要用引号包裹让整体成为字符串



#### 6.3.2 属性值

**在JSON中的属性值只允许以下六种**

- 字符串
- 数值
- 布尔值
- null
- 对象
- 数组

**注:**undefined,symbol值和函数都不能作为属性值,对象形式如果有这三种类型的值会在转换为JSON的时候被过滤掉,数组形式如果有会被转换为null传入JSON中



#### 6.3.3 JSON与对象的转换

JS为我们提供了一个工具类对象JSON,这个对象可以帮助我们将一个JSON转换为JS对象，也可以将一个JS对象转换为JSON

- **JSON-->JS对象**

  通过JSON.parse()函数可以将以JSON字符串转换为JS对象，该函数需要一个JSON字符串作为参数，会将JSON字符串转换为JS对象并返回

  ```js
  var json='{"name":"孙悟空","age":"18"}';
  var obj=JSON.parse(json);
  console.log(obj.name);//"孙悟空"
  ```

- **JS对象-->JSON**

  通过JSON.stringify()函数可以将一个JS对象转换为JSON字符串,该函数需要一个JS对象作为参数，会返回一个JSON字符串

  ```js
  var json={name:"孙悟空",age:"18"};
  var json=JSON.stringify(obj);
  console,log(json);//'{"name":"孙悟空","age":"18"}'
  ```

  **确认一个对象是否为空对象**

  ```js
  var obj={};
  var str=JSON.stringify(obj).replace(/\s|\r\n/g,"");
  if(str.length===2){
  	console.log("这是一个空对象")
  }
  ```

  

**注意:**JSON这个类对象在IE7及以下的浏览器中不支持，所以在这些浏览器中调用时会报错

**兼容方法**

- 使用eval()函数来替代JSON的类对象

  ```js
  /*
  	eval()函数
  		1.可以用来执行一段字符串形式的JS代码,并将指向结果返回
  		2.如果使用eval()函数执行的字符串中含有{},该函数会将{}当做代码块,如果不希望其被当成代码块			解析,则需要用()将字符串包裹
  */
  var str="alert('hello')";
  eval(str);//会出现弹框
  
  var json='{"name":"孙悟空","age":"18"}';
  var obj=eval("("+json+")");
  console.log(obj);//{name:"孙悟空",age:"18"}
  ```

  **注意:**eval()函数的功能很强大.可以直接执行一个字符串中的JS代码,但是在开发中尽量不要使用该函数,因为该函数不仅性能较差,而且存在非常大的安全隐患,容易被植入恶意代码造成损失

- 直接用外部引用的JSON



###  6.4 栈内存与堆内存

在讲内存之前先将讲数据,**在js中的数据分为两种,一种是引用型数据,一种是值类型数据**

**引用型数据只有object,而值类型数据有number,boolean,string,undefined,null。值类型数据在比较时,只比**

**较是否长的一样,而引用型数据在比较时是比较内存地址**



**关于栈内存与堆内存**

- **js中的变量都是保存到栈内存中**

- **基本数据类型的值在栈内存中存储，值与值之间是独立存在，修改一个变量不会影响其他变量**

- **对象是保存到堆内存中的**，每创建一个新的对象，就会在堆内存中开辟一个新的空间，而变量保存的是对象的

  内存地址(对象的引用)，如果两个变量保存的是同一个对象引用，当一个通过一个变量修改属性时，另一个也

  会受到影响

  ```js
  var obj={name:"孙悟空",gender:"男",age:18};
  var obj2=obj;
  
  console.log(obj.age)//打印结果为18
  obj2.age=19;
  console.log(obj.age)//打印结果为19
  ```

  **注意:**

  - 如果只是让obj2=null，obj并不会变化，因为这是让obj2的地址改变，而不是改变地址里面的值

  - 当比较两个基本数据类型的值时,就是比较值,而比较两个引用数据类型时,比较的是对象的地址,如果两个对

    象是一模一样的，但是地址不同，也会返回false



## 7.数组

**数组也是一个对象**,它和普通对象功能类似,也是用来存储值,不同的是普通对象使用字符串作为属性名，而数组是**使**

**用数字来作为属性名**操作属性值,所以数组的存储性比普通对象要好，在开发中经常使用数组来存储数据

**注意:**因为是使用数组索引来操作属性,所以不能使用.的方法来控制,只能用[]的形式



**数组的值**

数组的值可以是任何的数据类型,不一定是数值,也可以是一个对象,甚至一个函数,还可以是一个其他数组

```js
var arr=[{name:"孙悟空",gender:"男",age:18}];

var arr2=[function(){arlert("1")};
arr2[0]();

var arr3=[[1,2,3],[1,2,3]]；//这个就是我们说的二维数组
```



### 7.1 创建数组

创建数组时有四种方式,其中前三种都是通过构造函数来创建数组,只是传入的参数不同

- 声明或创建一个不指定长度的数组

  ```js
  var arr=new Array();//这时创建的数组里面没有值,可以向里面加值
  ```

- 声明或创建一个指定长度的数组

  ```js
  var arr=new Array(10);//创建一个length长度为10的数组,没有默认值
  ```

- .声明或创建一个带有默认值的数组

  ```js
  var arr=new Array(10,20,50);//创建一个length长度为3,带有值10,20,50的数组
  ```

- 使用字面量创建数组(推荐使用这种方式)

  ```js
  var arr=[1,2,3,4,5,10];//创建一个length长度为5,默认值为1,2,3,4,5,10的数组
  //也可以创建不定长度的数组
  var arr2=[];
  
  //数组字面量和对象字面量的区别在于数组字母量是用[]括起来,并且不用写属性值,默认使用的索引作为属性值
  ```

  

**数组中读取与添加元素**

- **读取:数组[索引]**

  **注意:**如果读取不存在的索引,不会报错而是返回undefined,但是数组的长度还是原来有内容的长度

- **添加:数组[索引]=值**

  **注意:**

  - 如果给已经存在值的索引添加一个新值,旧值会被覆盖掉,数组长度不变

  - 如果给不存在值的索引添加一个新值,**数组的长度会变为添加新值的索引的值-1**,中间没有值的索引中会存

    入empty(空)



### 7.2 length

**length是数组的一个属性,代表数组的长度**

- 对于连续的数组,使用length可以获取到数组的长度(值的个数)

- 对于非连续的数组,使用length会获取到数组的最大的索引加1(没有值的索引为empty),尽量不要创建非连续的

  数组



**查看数组长度:**数组名.length

```js
var arr=[1,2,3];
console.log(arr.length);
```

**注意:数组的length属性是能修改的**

- **如果length大于原长度，则多余的部分索引的值为empty**
- **如果length小于原长度，则多出的元素会被删除，所以可以修改length的长度为0来让数组为空**



**向数组的最后一个位置添加元素**

**用法:数组名[数组名.length]=值**

```js
var arr=new Array()
arr[arr.length]=70；// 因为length永远是最大索引加1
```



### 7.3 数组的方法

**数组的方法一般都不会使原数组发生变化,而是返回一个新的数组,只有改变原数组结果的方法才会使得原数组发生**

**改变**

#### 7.3.1 push与pop方法

- **push()方法可以向数组的末尾添加一个或多个值,并返回数组的新长度**

  - 添加一个值时,作用和用length向数组中最后添加一个值相同
  - 添加多个值时,参数用,隔开

  ```js
  var arr=[];
  //将要添加的值作为该方法的参数传递,添加的值会自动在数组的最后
  console.log(arr.push(1));//返回的值为1,返回值是这个数组的新长度
  console.log(arr);//值为1的数组
  
  console.log(arr.push(2,3));
  console.log(arr);//值为1,2,3的数组
  ```

- pop()方法可以删除数组的最后一个值，并将被删除的值作为返回值返回,不传入参数

  ```js
  var arr=[1,2,3];
  console.log(arr.pop());//返回3
  console.log(arr);//每使用一次数组长度length减少1
  ```

  

#### 7.3.2 unshift与shift方法

- **unshift()方法向数组开头添加一个或多个值,并返回新的数组长度**,添加多个值时,参数用,隔开。添加完成后其

  它值的索引一次向后调整对象的值

  **注意:**只能通过该方法在数组最前方添加新的值

  ```js
  var arr=[2,3];
  console.log(arr.unshift(1));//返回数组新长度3
  console.log(arr);//值为1,2,3的数组
  
  console.log(arr.unshift(-1,0));
  console.log(arr);//值为-1,0,1,2,3的数组
  ```

- shift()方法删除数组的第一个元素,并将被删除的元素作为返回值返回用法与pop()相反

  ```js
  var arr=[1,2,3];
  console.log(arr.shift());//返回1
  console.log(arr);//每使用一次数组长度length减少1,并且其他值的索引依次-1
  ```

  

####  7.3.3 forEach方法

**数组的遍历**

- for循环遍历数组,通过数组的length属性来获取数组的长度

  ```js
  var arr=[1,2,3,4,5];
  for(let i=0;i<arr.length;i++){
      console.log(i);
  }
  ```

- forEach()方法遍历数组(该方法只支持IE8以上的浏览器,如果要兼容IE8,用for循环)

  **forEach()方法需要一个函数作为参数才能使用,数组中有几个元素函数就会执行几次,不支持在函数中添加**

  **返回值**

  ```js
  var arr=[1,2,3];
  arr.forEach(function(){//这个函数只是我们创建,但是不需要我们调用,由浏览器自动调用,称为回调函数
  	console.log("hello");
  })
  ```

**每次执行时浏览器会将遍历到的数组以实参的形式传递进来,可以定义形参来读取这些内容**

**该方法有两个参数**

- 回调函数function(value,index,array)

  **浏览器在回调函数中传递了三个参数**

  - 第一个参数是遍历到的值value
  - 第二个参数是当前正在遍历元素的索引index

  - 第三个参数是正在遍历的数组(可选)


  ```js
  var arr=[1,2,3];
  arr.forEach(function(value,index,array){
      console.log(index+","+value);
      console.log(array);
  })
  ```

- 回调函数中this所指的对象,如果不写或者传入null和undefined,回调函数中的this为全局对象window(可选)



#### 7.3.4 map方法

**map(映射)()方法也是用做遍历数组,返回一个新数组,数组中的元素为原始数组元素调用函数处理后的值,并且按**

**照原始数组元素顺序依次处理元素**

**该方法有两个参数**

- 回调函数function(value,index,array){}(和forEach中的回调函数一样),但是可以在回调函数里加返回值

  **浏览器在回调函数中传递了三个参数**

  - 第一个参数是遍历到的值value
  - 第二个参数是当前正在遍历元素的索引index
  - 第三个参数是正在遍历的数组(可选)

- 回调函数中this所指的对象,如果不写或者传入null和undefined,回调函数中的this为全局对象window(可选)

```js
let arr = [12, 5, 8];
let result = arr.map(function (item) {
    return item*2;
})
let result2 = arr.map(item=>item*2);//map方法适合用箭头函数

console.log(result);//[ 24, 10, 16 ]
console.log(result2);//[ 24, 10, 16 ]

let score = [18, 86, 88, 24];
let result3 = score.map(item =>item>= 60?'及格':'不及格');
console.log(result3);//[ '不及格', '及格', '及格', '不及格']

let arr2=[1,2,3];
let result4=arr2.map(function(value,index,array),arr2){
    console.log(this);//this的指向为arr2
    return value+index;
}
```

 

#### 7.3.5 reduce方法

**reduce(汇总)()方法接收一个函数作为累加器，数组中的每个值(从左到右)开始缩减,最终计算为一个值**

**该方法有两个参数**

- 回调函数function(total, value, index, array)

  **浏览器在回调函数中传递了四个参数**

  - 第一个参数是每次计算结束后的返回值(第一次计算时默认值是0)
  - 第二个参数是遍历到的值value
  - 第三个参数是当前正在遍历元素的索引index
  - 第四个参数是正在遍历的数组(可选)

- 回调函数的初始值initialValue(用于设置上方第一个参数第一次计算时的值)(可选)

```js
//数组求和
var arr=[1,2,3,4,5];
var result=arr.reduce(function(prev,next){
      return prev+next;
 })
console.log(result);//值为15;

//对象求和
var ps = [{'p':1,'num':1},{'p':2,'num':2},{'p':3,'num':3},{'p':4,'num':4}];
ps.reduce(function(prev,next){
      return prev+next.p*next.num;
},10)//回调函数的第一次调用时，第一个参数是10，第二个参数是p[0]

//检验数组中出现字符串的数目
var arr=["HTML","JS","CSS","JAVA","CSS","HTML","HTML"];
var result=arr.reduce(function(back,prop){
	back[prop]=back[prop]?++back[prop]:1;
    return back;
},{})
console.log(result);//{HTML: 3, JS: 1, CSS: 2, JAVA: 1}
```



#### 7.3.6 filter方法

**filter(过滤器)()方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素**

**该方法有两个参数**

-  回调函数function(value,index,array)

  **浏览器在回调函数中传递了三个参数**

  - 第一个参数是遍历到的值value
  - 第二个参数是当前正在遍历元素的索引index
  - 第三个参数是正在遍历的数组(可选)

- 回调函数中this所指的对象,如果不写或者传入null和undefined,回调函数中的this为全局对象window(可选)

```js
//创建一个数组，判断数组中是否存在某个值
var newarr = [
  { num: 1, val: 'ceshi', flag: 'aa' },
  { num: 2, val: 'ceshi2', flag: 'aa2'  }
];
console.log(newarr.filter(item => item.num===2 ));

//去掉空数组空字符串、undefined、null
var arr = ['1',null,undefined, '3.jpg',''];
var newArr = arr.filter(item => item);//因为null,undefined和null转换为布尔值都是false
console.log(newArr);//只有1和3.jpg
```



#### 7.3.7 slice方法

**slice()方法可以用来从数组截取指定一部分值,这个方法不会影响原数组**

**该方法有两个参数**

- 截取开始的位置(必选)
- 截取结束的位置(查找时不包括结束位置)(可选),如果不写结束位置会从截取位置开始将后面所有值都截取

```js
var arr=[123,"孙悟空","猪八戒"]；
var result=arr.slice(0,2);
console.log(result);//值为123和孙悟空
```

**注意:**

- 该方法只能从左往右边截取,如果第一个数比第二个数大,则会返回空串
- 该方法接收负值,负值意味着从右往左数

 

#### 7.3.8 splice方法

**splice()方法可以用于删除数组中的指定元素，并向数组中添加新元素**,可以看作是push,pop和unshift,shift四种方

法的结合,**该方法会影响到原数组**,会将指定元素从元素组中删除,并将被删除的元素作为返回值返回

**注意:如果没有删除任何元素,返回空数组**

**该方法有任意多个参数,但是有两个必选参数**

- 第一个参数规定开始删除元素的索引位置
- 第二个参数规定应该删除的元素数量,值可以为0
- 第三个及以后的参数是要添加到数组中的新元素,这些元素将会自动插入到开始位置索引的前边

```js
var arr=[0,1,2,3,4];
var result=arr.splice(0，2);
console.log(arr);//值为2,3,4的数组
console.log(result);//值为0,1的数组
```

```js
//用作去除相等的数组中的数值时,可以用splice()方法来删除一个数组元素
//方法一
var arr=[1,2,3,5,3,3,6];
for(var i=0;i<arr.length-1;i++)
{
	for(var j=i+1;j<arr.length;j++)
	{
		if(arr[i]===arr[j])
		{
			arr.splice(j,1);
			j--;//当删除了当前的j所在元素以后,后面的元素会自动补位，此时将不会再比较这个元素，所以需
		}		//要自减才能比较		
	}
}
//方法二
var arr2=[1,2,3,5,3,3,6];
for(var i=arr2.length-1;i>=0;i--)//从后往前判断
{
	for(var j=i-1;j>=0;j--)
	{
		if(arr2[i]===arr2[j])
		{
			arr2.splice(j,1);
		}				
	}
}
//方法三(ES6方法去重)
var arr3=[1,2,3,5,3,3,6];
var arr4=[...new Set(arr3)];
/*
用new Set()会将传入的数组转换为Set结构数组,因为Set结构是不允许有重复的数据,所以会自动去重,最后通过	解构将Set结构数组重新转换为普通数组
*/
```



#### 7.3.9 concat方法

**concat()方法可以连接两个或多个数组,也可以连接单个的元素,并将新数组返回**,该方法不会对原数组产生影响

```js
var arr=[123,245];
var arr2=[566,156];

var result=arr.concat(arr2);//将两个数组连接起来
console.log(result);//result的值为123,245,566,156

//可以连接多个数组或值
var arr3=[1,2,3]
var result2=arr.concat(arr2,arr3,"999");
console.log(result2);//值为123,245,566,156,1,2,3,"999"
```

 **注意:**将一个数组放在另一个数组中最好的方法还是通过ES6的语法: ...数组名 来实现



#### 7.3.10 join方法

**join()方法可以将数组转换为一个字符串,并将转换后的字符串返回,**不会对原数组产生影响

**join中的参数为一个字符串,用这个字符串代替数组中的,(逗号)将数组分开**,默认值为,(逗号),与对整个数组使用

toString()方法效果一样

**注意:**如果在join()中传入了空串""作为参数,那么返回的字符串是整个数组合并后的值,相当于把数组中每个值用+连

接

```js
var arr=[1,2,3];
var str=arr.join();//默认用,分开
var str2=arr.join("");//空串将整个数组的值合并
var str3=arr.join("H");//用字符H隔开
console.log(str);//"1,2,3"
console.log(str2);//"123"
console.log(str3);//"1H2H3"
```



#### 7.3.11 reverse方法

**reverse()方法用作将数组里的值反转(左边的值到右边,右边的值到左边),该方法会直接修改原数组，返回值也是**

**修改后的数组**

```js
var arr=[1,2,3];
var result=arr.reverse();
console.log(arr);//值依次为3,2,1
console.log(result);//值依次为3,2,1
```

 

#### 7.3.12 sort方法

**sort()方法会对数组的元素进行排序,默认会按照Unicode编码进行排序(不是比较值的大小,即使对于纯数字的数**

**组,也会按照Unicode编码来排序),该方法会影响原数组**



**自己指定排序的规则**

**在sort()添加一个回调函数,回调函数中需要定义两个形参,浏览器将会分别使用数组中的元素作为实参去调用回**

**调函数，使用哪个元素调用不确定，但是肯定的是在数组中a一定在b的前面,浏览器会根据回调函数的返回值来决**

**定元素的顺序**

**如果返回一个大于0的值 ，则元素会交换位置，如果返回一个小于0的值，则元素位置不变，如果返回0，则认为两个元素相等，也不交换位置**

```js
var arr=[5,4,2,1,3,8,9]

arr.sort(function(a,b){
	if(a>b){
		return 1
	}
	else if(a<b){
		return -1;
	}
	else
	{
		return 0;
	}
})
console.log(arr);//[1,2,3,4,5,8,9]
//上面是升序排列，如果要降序排列只用改变返回值

arr.sort(function(a,b){
    return a-b//上面写法的简写
})
```



#### 7.3.13 indexOf与lastIndexOf方法

- **indexOf()方法可以检索一个数组中是否含有指定的元素**,找到返回该元素在数组中的索引位置,没有找到则返

  回-1

  **该方法可以指定第二个参数,第二个参数是指定开始查找的位置(0是第一个位置)**

  ```js
  var arr=[1,2,3];
  var result=arr.indexOf(1);
  console.log(result);//0
  result=str.indexOf(1,1);
  console.log(result);//-1
  ```

- lastIndexOf()方法用法和IndexOf()类似,不同的是检索数组时是**从右到左**检索,也可以指定开始查找的位置

  **注意:**虽然是从右往左检索,但是在设置第二个参数的时候指定开始查找的位置还是从左往右数的,只是查找的时

  候是从右往左



#### 7.3.14 valueOf方法

**valueOf()返回数组原始值(数组本身)**

```js
var arr=[1,2,3];
var result=str.valueOf();
console.log(str===result);//true
```



#### 7.3.15 fill方法

**fill() 方法用于将一个固定值替换数组的元素**,该方法有三个参数,会改变原数组

**参数**

- value,表示填充的值(必填)
- start,表示开始填充的位置(选填)
- end,表示停止填充的位置(选填,默认是整个数组长度)

```js
var arr=new Array(100);//创建长度为100的数组
arr.fill(1);//将1填充到所有的空位置
/*上面相当于
for(var i=0;i<arr.length;i++){
    arr[i]=1;
}
*/
console.log(arr);//[1,1,1,1,1,1....1]
```



#### 7.3.16 find与findIndex方法

- **find()方法返回通过测试的数组的第一个元素的值，**接收一个回调函数,不会改变原始数组

  **浏览器在回调函数中传递了三个参数**

  - 第一个参数是遍历到的值value
  - 第二个参数是当前正在遍历元素的索引index
  - 第三个参数是正在遍历的数组(可选)

  **find() 方法为数组中的每个元素都调用一次函数执行**

  - 当数组中的元素在测试条件时返回 true 时, find() 返回符合条件的元素，之后的值不会再调用执行函数
  - 如果没有符合条件的元素返回 undefined

- **findIndex() 方法返回传入一个测试条件中符合条件的数组第一个元素位置**,用法同find()方法相同,接收一个回调函数,不会改变原始数组

  **findIndex()方法为数组中的每个元素都调用一次函数执行**

  - 当数组中的元素在测试条件时返回 true 时, findIndex() 返回符合条件的元素的索引位置，之后的值不会再调用执行函数。
  - 如果没有符合条件的元素返回 -1

```js
var arr=[1,2,3,4];
var result1=arr.find(function(value,index,array){
    return value>2;
})
var result2=arr.findIndex(function(value,index,array){
    return value>2;
})
console.log(result1);//3
console.log(result2);//2
```



#### 7.3.17 some与every方法

- **some()方法用于检测数组中的元素是否满足指定条件,**接收一个回调函数, 不会改变原始数组

  **浏览器在回调函数中传递了三个参数**

  - 第一个参数是遍历到的值value
  - 第二个参数是当前正在遍历元素的索引index
  - 第三个参数是正在遍历的数组(可选)

  

  **some() 方法会依次执行数组的每个元素**

  - 如果有一个元素满足条件，则表达式返回true , 剩余的元素不会再执行检测

  - 如果没有满足条件的元素，则返回false

- **every() 方法用于检测数组所有元素是否都符合指定条件,**与some一样都接收一个回调函数,不改变原数组

  **every() 方法使用指定函数检测数组中的所有元素**

  - 如果数组中检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测。
  - 如果所有元素都满足条件，则返回 true。

```js
var arr=[1,2,3,4];
var result1=arr.some(function(value,index,array){
    return value>2;
})
var result2=arr.every(function(value,index,array){
    return value>2;
})

console.log(result1);//true
console.log(result2);//fasle
```



#### 7.3.18 copyWithin方法

**coptWithin()方法用于在当前数组内部将指定位置的元素复制到其他位置,并且会覆盖掉原来的元素,**返回当前数组,**此方法会改变原数组,**该方法有三个参数

**参数:**

- target,从该位置替换数据
- start,从该位置读取数据,默认值是0,可以接受负值,负值代表从右往左数
- end.从该位置前停止读取数据,默认是整个数组长度,可以接受负值,负值代表从右往左数

```js
var arr=[1,2,3,4,5];
var result=arr.copyWith(0,3);//从数组第一个位置开始替换数据,从第四个位置开始读取,读取后面所有值
console.log(arr);//[4,5,3,4,5]
console.log(result);//[4,5,3,4,5]
```



#### 7.3.19 keys,values与entries方法

**这三个方法都用作遍历数组,都返回一个遍历器对象,可以用for...of语句进行遍历**

- keys()方法,对数组的键名(索引)进行遍历,返回一个新的数组
- values()方法,对数组的键值(值)进行遍历,返回一个新的数组
- entries()方法,对数组的键值对进行遍历,该方法会返回一个二维数组,会将遍历到的键名和键值装到一个数组里作为二维数组的值

```js
for(var index of [1,2].keys()){
    console.log(index);//0 1
}
for(var value of [1,2].values()){
    console.log(value);//1 2
}

for(var arr of[1,2].entries()){
    console.log(arr);//[0,1] [1,2]
}
for(var [index,value] of [1,2].entries()){
    console.log(index,value);//0,1 1,2
}
```



**Object的keys,values与entries方法**

Object的上述方法需要传入一个对象作为参数,此时会直接分别将键名,键值,键值对装在一个数组中

```js
var obj=["a":0,"b":1];
console.log(Object.keys(obj));//["a","b"]
console.log(Object.values(obj));//[0,1]
console.log(Object.entries(obj));//[["a",0],["b",1]]
```



### 7.4 Array的方法

#### 7.4.1 Array.isArray方法

**Array.isArray()方法检验一个对象是否为真数组(不支持低版本IE)**,可以用instanceof功能符来做兼容

```js
var arr=[];
if(arr instanceof Array){
    console.log("1");
}
if(Array.isArray(arr)){
	console.log("2");
}
```



#### 7.4.2 Array.from方法

**Array.from()方法用于将两类对象转换为真数组,**该方法有三个参数

**参数:**

- 一个用于转换为真数组的类数组对象
- 一个回调函数,类似于数组的map()方法中的回调函数,用来对每个元素进行处理,将处理后的值放入返回的数组
- 回调函数中this所指的对象,如果不写或者传入null和undefined,回调函数中的this为全局对象window(可选)

```js
//Nodelist对象
var div=document.getElementByTagName("div");
Array.from(div).forEach(function(value){
    console.log(value);
})

//arguments对象
function fun(){
    var arr=Array.from(argumens);
    console.log(arr);
}
```



#### 7.4.3 Array.of方法

**Array.of()方法用于将一组值转换为数组,**这个方法的主要目的是弥补构造函数Arrray()的因为参数数量的不同而造成行为不同的不足

```js
console.log(Array());//[]
console.log(Array(3));//[,,,]
console.log(Array(1,2,3));//[1,2,3]

console.log(Array.of(3));//[3]
console.log(Array.of(1,2,3));//[1,2,3]
console.log(Array.of(3).length);//1
```



### 7.5 真数组与伪数组(类数组)

**真数组与伪数组都是对象,都有length属性,**而且真数组与一些伪数组的length属性都可以修改,所以不能用数组的length属性是否可变来判断一个数组是否是真数组



**真数组与伪数组的区别:**真数组能够使用数组的拓展方法,而伪数组没有这些方法

**用Array.isArray()可以检查一个对象是否为真数组**

```js
var arr=[1,2,3];
var obj=[0:1,1:2,2:3,length:3];//与真数组一样通过索引取值,有length属性

console.log(Array.isArray(arr));//true
console.log(Array.isArray(obj));//false
```



**真数组与伪数组的相互转换**

- 真数组转换为伪数组

  ```js
  var arr=[1,2,3];
  var obj={};
  [].push.apply(obj,arr);
  console.log(obj);
  ```

- 伪数组转换为真数组

  ```js
  //通过Array.from()函数
  var obj=[0:1,1:2,2:3,length:3];
  var arr=Array.from(obj);
  console.log(arr);
  //通过方法
  var arr1=[];
  var obj1=[0:1,1:2,2:3,length:3];
  [].push.apply(arr,obj)//IE8以上兼容
  console.log(arr);
  
  var arr2=[].slice.call(obj);//兼容IE8
  console.log(arr2);
  //通过循环
  var arr3=[];
  var obj2=[0:1,1:2,2:3,length:3];
  for(var i=0;i<obj.length;i++){
      arr3[i]=obj2[i];
  }
  console.log(arr3);
  //通过拓展运算符
  var obj3=[0:1,1:2,2:3,length:3];
  var arr4=[...obj3];
  console.log(arr4);
  ```

  

## 8.字符串方法

### 8.1 length

**在底层字符串是以字符数组的形式保存的,所以字符串其实是有length属性的,并且也可以使用索引(低版本IE不支持直接用索引,所以用charAt()代替)**

**注意:**与数组的length不同,字符串的length是只读的,不能修改



### 8.2 charAt,charCodeAt与fromCharCode方法

- **charAt()方法可以返回字符串中指定位置的字符,根据索引获取指定的字符**

```js
var str="Hello"
var result=str.charAt(0)
console.log(result)//会打印出H字符
```

- **charCodeAt()方法会返回指定字符串中指定字符的Unicode编码**

```js
var str="123"
var result=str.charCodeAt(0);
console.log(result)//打印出49(1的unicode编码)
```

- **fromCharCode()方法可以根据字符编码去获取字符**,这个方法由String来调用

```js
var result=String.fromCharCode(72)//result的值为字符串H
console.log(result);
```

 

### 8.3 concat方法

**concat()方法用于连接两个或多个字符串(中间,隔开),**作用和+一样,但是+更加简便,所以一般字符串拼接还是用+

```js
var str1="123";
var str2="456";
var str3=str1.concat(str2);//通str1+str2
//新字符串中的顺序还是先是调用者,然后后面是参数传入的顺序
```



### 8.4 indexOf与lastIndexOf方法

- **indexOf()方法可以检索一个字符串中是否含有指定的字符**,找到返回该字符在字符串中的索引位置,没有找到则返回-1

  **该方法可以指定第二个参数,第二个参数是指定开始查找的位置(0是第一个位置)**

  ```js
var str="hello"
  var result=str.indexOf("h");
  console.log(result);
  result=str.indexOf("h",1);
  console.log(result);
  ```
  
- lastIndexOf()方法用法和IndexOf()类似,不同的是检索字符时是**从右到左**检索,也可以指定开始查找的位置

  **注意:**虽然是从右往左检索,但是在设置第二个参数的时候指定开始查找的位置还是从左往右数的,只是查找的时候是从右往左


 

### 8.5 strartsWith,endsWith与includes方法

- **includes()：**返回布尔值，表示是否找到了参数字符串，支持第二个参数，表示开始搜索的位置

- **startsWith()：**返回布尔值,表示参数字符串是否在查找字符串的头部,支持第二个参数，表示开始搜索的位置

- **endsWith()：**返回布尔值，表示参数字符串是否在查找字符串的尾部,一般用做后缀名判断,支持第二个参数，

  表示前n个字符,与前两个的表现形式不同

  ```js
  var str = 'Hello world!';
  str.startsWith('Hello') // true
  str.endsWith('!') // true
  str.includes('o') // true
  
  str.startsWith('world', 6) // true
  str.endsWith('Hello', 5) // true
  str	.includes('Hello', 6) // false
  ```

  

### 8.6 slice方法

**slice()方法可以从字符串中截取指定的内容**,截取完成后会将截取到的字符串作为返回值返回

该方法有**两个参数,**第一个参数是开始位置,第二个参数是结束位置(但查找时不包括结束位置),可以不写第二个参数,这样会将从开始到后面所有的字符截取

```js
var str="hello";
var result=str.slice(0,3);
console.log(result);//返回hel(不包含索引为3的l)
```

**注意:**

- 该方法只能从左往右边截取,如果第一个数比第二个数大,则会返回空串

-  该方法接收负值,负值意味着从右往左数

  

### 8.7 subString与substr方法

- **subString()方法可以用来截取一个字符串，和slice()类似**，也有两个参数,不同的是这个方法不能接收负值作

  为参数，如果传递了一个负值，则默认变成0,而且它会自动调整参数的位置，如果第二个参数小于第二个，则

  自动交换参数位置 

- **substr()方法也是用来截取字符串**，该方法有两个参数,与slice()和subString()都不同,第一个参数是开始位置的

  索引，**第二个参数是要截取的长度**

 

### 8.8 split方法

**split()方法可以将一个字符串拆分为一个数组,**需要一个**字符串**作为参数，该方法将会根据字符串去拆分数组

```js
var str="abc,bcd,efg,hij"

var result=str.split(",");//将str用,作为间隔去拆分字符串

console.log(result);//result就变成了一个数组，第一个值是abc,第二个值是bcd
```

**注意:**

- 如果传递一个空串作为参数，则会将每个字符都拆分为数组中的一个值
- 如果不传入参数会返回一个只有一个整个字符串的值的数组

```js
//split方法的妙用-->字符倒序 
var str="abc";
var result=str.spilt("").reserve().join("");
console.log(result);//值为cba
```



### 8.9 toLowerCase与toUpperCase方法

- **toLowerCase()方法将字符串转换为小写并返回**

- **toUpperCase()方法将字符串转换为大写并返回**

```js
var str="aBc";
var result1=str.toLowerCase();
console.lof(result1);//值为abc

var result2=str.toUpperCase();
console.lof(result2);//值为ABC

//这两种方法通常用做在进行无视大小写验证的时候使用
```



### 8.10 valueOf方法

**valueOf()方法返回字符串的原始值(就是字符串本身)**

```js
var str="abc";
var result=str.valueOf();
console.log(str===result);//true
```



### 8.11 trim方法

**trim()方法用作删除字符串前后的空格(字符中间的空格不会删除),通常在用户输入验证的时候使用**

- 直接使用,调用浏览器中的字符串自带的trim()方法(不兼容低版本IE)

  ```js
  var str="   12  3   "
  var result=str.trim();
  console.log(result);//打印出12  3
  ```

- 间接使用,使用正则表达式

  ```js
  
  function myTrim(x) {
      return x.replace(/^\s+|\s+$/gm,'');
  }
  
  var str = myTrim("        Hello World!        ");
  alert(str);
  ```

  

### 8.12 repeat方法

**repeat()方法通过字符串调用,会返回一个新的字符串,该方法内传入的数值参数表示将该字符串复制几次,不会改变原字符串**

```js
var str = "123";
var str2 = str.repeat(2);
var str3=str.repeat(0);
console.log(str);//"123"
console.log(str2);//"123123"
console.log(str3);//""空字符串
```

**注意:**

- 参数如果是浮点数,会向下取整
- 参数是负值或是Infinity会报错
- 参数为0到-1之间的小数等同于0，这是因为会先进行取整运算,0到-1之间的小数,取整以后等于-0,视同为0
- 参数为NaN等同于0
- 如果参数为字符串则会先转换为数值类型



### 8.13 padStart与padEnd方法

**ES8(ES2017)中引入了字符串的补全功能,如果某个字符串不够指定长度,会在头部会尾部补全该字符串,不会改变原字符串**

- **padStart()方法用于在头部补全**
- **padEnd()方法用于在尾部补全**

```js
var str="123";
var str2=str.padStart(5,"ab");
var str3=str.padEnd(5);
console.log(str);//"123"
console.log(str2);//"ab123"
console.log(str3);//"123  "
```

**注意:**

- 如果原字符串的长度等于或大于指定的最小长度,则返回原字符串
- 如果用来补全的字符串与原字符串,两者的长度之和超过了指定的最小长度,则会截去超出位数的补全字符串
- 如果省略第二个参数,默认使用空格补全长度



## 9.函数对象

**函数也是也是对象，函数中可以封装一些功能(代码)，在需要的时候执行,并且函数对象具有所有普通对象所具备的**

**功能** 

```js
//创建和使用函数
function test(){
    console.log(1);
}
//封装到函数中的代码不会立即执行，函数中的代码会在函数调用的时候执行

//调用函数
test();

console.log(typeof test);
//使用typeof检查一个函数对象时，会返回function,因为函数在js中占了很大的比重,所以把函数单独拿出来当做一个种类,实际上还是一个对象
```



### 9.1 创建函数对象

- 构造函数方法创建函数对象，将要封装的代码以字符串的形式传递给构造函数(不推荐使用这种方法)

  ```js
  var fun=new Function("console.log('HELLO');");
  //这种构成函数的方法Function里面必须是用""括起来的字符串
  ```

- 函数声明方法创建函数对象

  ```js
  /*
  语法:
  function 函数名([形参1，形参2]){
  	功能片段
  }
  形参的[]可以省略，在()里写上对应的变量，不用写var
  */
  function fun(){
      console.log("HELLO");
  }
  ```

- 3.函数表达式创建函数对象

  ```js
  /*
  语法:
  var 函数名=function([形参]){
  	功能片段
  }
  */
  Var fun3=function(){
  	console.log("HELLO");
  };
  fun3();
  //上方相当于把一个函数赋值给了fun3
  ```

  

**注意:**

- 加括号是调用函数，相当于函数的返回值，不加括号是函数对象，相当于直接使用函数对象.把函数赋值给一

  个变量时，只需要将函数名赋值过去,不要加后面的(),如果是没有返回值的函数就会把undefined赋值给变量,如

  果有返回值就把返回值赋值给它,同时会运行函数

- 调用函数时解析器不会检查实参的类型,注意是否有可能会接收到非法的参数，如果有可能则需要对参数类型

  进行检查

- 调用函数时也不会检查实参的数量，多余的实参不会被赋值，如果实参的数量少于形参的数量，则没有对应的

  形参将会是undefined

 

### 9.2 函数的返回值

**return返回一个返回值，在函数return后面的函数都不会执行,直接结束该函数**

return后面可以跟任意数据类型,可以是一个对象,也可以是一个函数,如果return语句后不跟任何值就相当于返回

undefined，如果不写return也会返回undefined

- **返回值为普通数据类型时**

```js
//如:定义一个函数，判断一个数字是否是偶数，如果是返回true，否则返回false
function isou(nume){
return num%2==0;
}
console.log("result="+isou(15));
```

- **返回值为函数时**

```js
//在一个函数内部可以无限次的嵌套函数，同时可以嵌套调用函数
function fun1(){
    
function fun2(){
console.log("123");
}
    
fun2();
    
}
fun1();
//注意:因为fun2是在fun1里面创建的，所以只能在fun1里面用


function fun3(){

function fun4(){
	alert("hello");
}

return fun4;

}
a=fun3();
console.log(a)//这时a是一个函数对象,可以通过a();fun4函数
			  //同时还可以fun3()(); 这种写法和上面一样，只不过没有又创建一个对象
```



### 9.3 函数的参数

**函数的参数分为实参和形参,形参是创建函数的时候写入,形参则是在调用函数的时候写入**

```js
//如果参数过多容易被弄混乱，可以将参数封装到一个对象中,在里面就可以用对象的值,只要属性名正确即可
function fun(obj){
	alert(obj.name)
}
var obj={
name:"孙悟空",
age:18
}

fun(obj);
//实参可以是一个对象，也可以是一个函数 
function fun2(a){
	a(obj);
}
fun2(fun);
//在这个函数中a就是一整个fun函数,也就是函数赋值，把地址赋值给a

//另一种写法:直接将函数传入
fun2(function(){alert("hello")}); 
```



#### 9.3.1 有初始值的形参

**在定义形参的时候为形参设置初始值(ES6新增语法)，在没有传入实参的情况下形参会默认地使用初始值**

```js
function fun(a=123){//因为是ES6的语法,所以这里的a是通过let方法声明的,与后边的作用域有很大联系
    console.log(a);	
}
fun();//打印出123
```



#### 9.3.2 封装实参的对象arguments

**在调用函数时,浏览器每次会传递两个隐含的参数(箭头函数没有这两个参数)**

- 函数上下文对象this
- 封装实参的对象arguments

**arguments是一个类数组对象，很数组很像但不是数组，它可以通过索引来操作数据，也可以用length获取长**

**度，在调用函数时，我们所传递的实参都会在arguments中按照传入的顺序保存**

```js
function fun(){
    console.log(arguments.length)
}
//可以用console.log(arguments.length)来查看函数的实参个数，如果没有实参长度就是0
```

**注意:**即使不在创建函数的时候写形参，如果依然传入了实参,实参也会保留在arguments中，并且可以调用，

```js
//argument中有一个属性callee，这个属性对应当前正在执行的函数对象
Function fun(){
	Console.log(arguments.callee===fun)//返回true
}
```



#### 9.3.3 this

this解析器在调用函数时传递给函数的一个参数,this指向的是一个对象,这个对象我们称为函数指向的上下文对象，**根据函数的调用方式不同(跟创建方式无关)，this会指向不同的对象**

**this指向**

- 以函数形式调用，this永远是window(非严格模式,严格模式是undefined,ES6中的class构造函数就是开启严格

  模式)

  ```js
  "use strict";//在最开头写上这段代码解析器自动开启严格模式解析
  function fun(){
      console.log(this);//undefined
  }
  fun();
  ```

- 以方法的形式调用，this被调用的那个对象

- 以构造函数的形式调用时，this就是创建的那个对象

- 使用call和apply调用时，this就是指定的那个对象

  

#### 9.3.4 rest参数

**rest参数写在函数创建形参时的最后一位,用作收集剩余传入的实参,写法为...数组名**

```js
function show(a, b, ...args){
    console.log(a);
    console.log(b);
    console.log(args);
}

show(1, 2, 3, 4, 5);//此时args为一个有着3,4,5值的数组
```

```js
//rest参数还能用作展开数组,效果和将数组直接拆分开一样,这个用法很有用
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let arr3 = [...arr1, ...arr2];//相当于直接将arr1和arr2拆分开
console.log(arr3);
```



#### 9.3.5 其他属性参数

在函数创建时会默认传入很多的属性参数,这些参数通常情况下很少用

- name属性,函数的名字,name属性是只读的,不能够修改
- arguments属性,这个属性同上面的arguments属性是一样的,只是用作了函数的属性来调用
- length属性,函数定义时形参的个数,但是不包括有默认值的形参和rest形参
- caller属性,说明调用者的属性(在下方实例进行说明)

```js

fun1.name="fun3";//因为name属性时只读的,这行代码没有作用
fun1(1,2,3,4);//fun1.name为"fun1",fun1.length为1,fun1.arguments.length为4
console.dir(fun1);//console.dir可以显示一个对象的所有属性和方法

fun2();//fun1.caller为"fun2"

function fun1(x=0,y){
    console.log(fun1.name);
    console.log(fun1.arguments.length);//和arguments.length作用相同
    console.log(fun1.length);
    console.log(fun1.caller);
}

function fun2(){
    fun1(1,2);//这里就是在fun2中调用fun1,fun1.caller就是"fun2"
}
```



###  9.4 立即执行函数

**立即执行的函数都是通过函数表达式的方法声明的,所以叫作立即执行函数表达式(IIFE)**

**函数定义完成后，立即被调用，这种函数叫立即执行函数，立即执行函数往往只会执行一次**

```js
//第一种方式，函数用小括号包起来,然后后面加小括号
(function(a,b){
    console.log(a+b)
})(10,1000);
//必须要加一个()在前面将function(){}包起来.表示是一个整体，同时说明被括起来的部分是一个函数对象
//后面跟()立即调用它，同时也可以带入参数进去

//第二种方式，函数后面加用小括号,然后再用小括号包起来
(function(a,b){
    console.log(a+b)
}(10,1000));

//第三种方式，函数后面加小括号,然后在函数前面加+ - ~ !其中一个符号
+function(a,b){
    console.log(a+b)
}(10,1000)

-function(a,b){
    console.log(a+b)
}(10,1000)

~function(a,b){
    console.log(a+b)
}(10,1000)

!function(a,b){
    console.log(a+b)
}(10,1000)
```

**立即执行函数的用处:**将在一个script标签中写的代码通过一个立即执行函数包裹起来,可以避免污染全局环境



### 9.5 方法

对象的属性值可以是任何数据类型，也可以是一个函数，如果一个函数作为一个对象的属性保存，那么我们称这个

函数是这个对象的方法，调用用这个函数就是用这个对象的方法，但是这只是名称上的区别,并没有其他的区别

**比如console.log()就是调用console对象的log方法**

 

### 9.6 作用域

**作用域指一个变量的作用的范围,在JS中一共有三种作用域(ES5为两种,ES6有了块作用域,需要用let或const申明)**

#### 9.6.1 全局作用域

**直接编写在script标签的js代码，都在全局作用域中,全局作用域在页面打开时创建，在页面关闭时销毁**

**其他作用域可以访问到全局作用域中的变量.全局作用域不能访问其他作用域的变量**

- 在全局作用域中有一个全局对象window,它代表一个浏览器的窗口，由浏览器创建，我们可以直接使用

```js
//在全局作用域中，创建的变量都会作为window对象属性保存
var a=10;
console.log(window.a);//输出的结果依旧为10
```

**注意:window对象实际可以说是比全局作用域还要高一个层次,因为一个页面可以引入很多script标签,每个script标签中的全局环境是相互独立的,但是所有script标签都只有一个window**

- 一个变量没有声明就使用会报错,但是如果通过作为window对象的属性来调用就不会报错,这个值会是

  undefined

```js
console.log(a);//a没有被声明,会报错
console.log(window.a)//返回undefined
```

- 无论什么作用域下,只要没有声明就使用的变量,会成为类似全局作用域的变量,因为没有声明就赋值的变量会泄

  露出来,成为全局对象window的属性

  ```html
  <!--
  	要特别注意全局变量泄露污染了全局变量window的环境
  -->
  <script>
  	a=10;//a泄露给window成为window的属性
  </script>
  
  <script>
  console.log(a);//通过window访问到了另外一个全局作用域的a,这里不会报错
  </script>
  ```

- 用var声明的全局变量也会成为window对象下的属性，而用ES6中的let和const则不会成为window的属性,但

  是这三者全都可以被其他script标签访问到

- 使用函数声明方法创建的函数都会作为window的方法保存

  ```js
  function fun(){
      alert(1);
  }
  window.fun();//用这种方法也能调用
  
  window.alert(123);
  /*
  alert()也是调用函数，所以alert也是一个函数对象，可以window.alert()，也可以alert()，这是浏览器自己创建的函数	
  */
  ```



#### 9.6.2 函数作用域

**函数作用域是在function字样里的作用域,调用函数时创建函数作用域，函数执行完毕以后，函数作用域销毁**

**每调用一次函数就会创建一个新的函数作用域，它们之间互相独立的**

**注意:**

- 当在函数作用域操作一个变量时，它会先在自身作用域中寻找，如果没有，再在离得最近的一级作用域，直到

  找到全局变量，如果全局作用域中没有找到，就会报错

- 如果想直接**在函数作用域中用全局的变量,使用window.a来调用**(前提是用var或者直接写来声明的变量)

- 在函数作用域中也有声明提前的特性，使用变量声明关键字声明的变量，会在函数中所有的代码执行之前被声

  明，这个变量是在函数作用域中被声明的，同理在函数中声明的函数也是一样

- **有参的函数在调用的时候不传入实参,形参的值是undefined**,因为在定义形参的时候相当于在函数内部用var声

  明了一个变量，没有赋值的变量一律为undefined,即使全局变量中有一样的变量也会用内部没有赋值的变量



#### 9.6.3 块作用域

**块作用域是在{}之内的作用域,块作用域的变量只能由ES6语法中的let和const创建**

```js
{
    let a=123;
}
console.log(a);//报错,只有var声明或者直接写的时候才能访问到
```

**注意:**块级作用域中的变量使用条件和函数作用域基本相同,但是在使用时的情况会有所不同

**块作用域的用处体现**

```js
for(let i=0;i<10;i++){
    div.onclick=function(){
		console.log(i);
    }
}
//打印出的是0到9

for(var i=0;i<10;i++){
    div.onclick=function(){
		console.log(i);
    }
}
//打印出的是10个10
```



#### 9.6.4 声明提前

##### 9.6.4.1 变量的声明提前

**使用var关键词声明的变量,会在所有的代码执行之前被声明,而let和const不会**

```js
/*只写a变量不写var,则a相当于window.a，但是两者区别在于用var声明的变量能够声明提前*/
console.log(a);//在这里会报错
a=123;

console.log(b);//这里不会报错而是会输出undefined
var b=123;

//相当于
var b;
console.log(b);
b=123；
```

```js
console.log(c);
let c=123;
/*
在c被用let声明赋值之前的一段区域叫作c的暂时性死区(TDZ)(这是ES6独有的),在这一个区域里面c是不允许被调用
的,否则就会报错
*/

//再如:
let a=123;
function(a=a){//报错
    console.log(a);
}
/*
在设置形参的默认值时,会在()内临时形参一个作用域,由于找一个变量的时候会先在自身作用域中寻找,由于js的机制
是先编译后执行的,所以先找到自身作用域中的a,但是a并没有被赋值就被使用,陷入了暂时性死区,所以会报错
*/
```



##### 9.6.4.2 函数的声明提前

**使用函数声明形式创建的函数会在所有代码执行之前就被创建(在var声明的变量之后执行,所以在编译期用var声**

**明的变量与函数变量的变量名矛盾的时候函数的变量会将var声明的变量覆盖)**，所以可以在函数声明前调用函数，

这个形式声明的函数会默认放在当前作用域代码最上面

```js
fun();//能正常打印123

function fun(){
    console.log(123);
}
```

**使用函数的表达式创建的函数本身不会声明提前,**只会先将用var创建的变量提前

```js
fun2();//报错

var fun2=function(){
    console.log(123);
}
/*
	使用函数表达式创建的函数只会先创建出变量,这个变量声明提前,但是此时的值是undefined,而undefined不	  是一个函数,所以在调用的时候会报错
*/
```

 

### 9.7 箭头函数

箭头函数为ES6中函数的简写,省略了写function的形式,而改用=>来代替

```js
// 普通函数
function name() {
}
// 箭头函数,去掉 function,加上 =>
() => {
}
```

```js
let show1 = function () {
    console.log('abc');
}
let show2 = () => {
    console.log('abc');
}
show1();
show2();
let show4 = function (a) {
    return a*2;
}
/*
1.如果只有一个参数,()可以省
2.如果函数只有reutrn这一个句子,{}可以省
*/
let show5 = a => a * 2
console.log(show4(10))
console.log(show5(10))

```

**注意:**

- 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象

- 不可以当作构造函数，也就是说，不可以使用new命令，否则会报错

- 不可以使用arguments对象，该对象在函数体内不存在,如果要用，用rest参数代替

- 不可以使用yield命令，因此箭头函数不能用作Generator函数

  

### 9.8 工厂方法创建对象

**使用工厂方法创建对象可以大批量的创建属性相同的对象**

```js
function create(name,age,gender){
Var obj=new Object();
obj.name=name;
obj.age=age;
obj.gender=gender
return obj;
}

var obj1=create(name,age,gender);
var obj2=create(name,age,gender);
```

**注意:**使用工厂方法创建的对象，使用的构造函数都是Object，所以创建的对象都是Object这个类型，导致我们无

法区分出多种不同类型的对象，所以这个方法很少用,经常使用构造函数的方法



### 9.9 构造函数

使用构造函数创建函数,创建的函数和普通函数没有本质区别,只是构造函数习惯上首字母大写

**构造函数和普通函数的本质区别是调用方式不同,普通函数是直接调用，而构造函数需要使用new关键字来使用,一般通过构造函数来实现面向对象编程**

**面向对象**

JS通过原型链构成的模拟了类,通过这个模拟类我们可以实现面向对象编程(在ES6中被推出了class关键字,但是依然是通过原型链模拟出的类),但是并不是其它面对对象编程语言中的真正的类,所以一般我们说JS是一门基于对象的变成语言

**面向对象的三大特征**

- **封装**，对象与对象之间相互独立,不会影响对方
- **继承**，能传递给子对象
- **多态**，传入不同的参数表现不同的效果

```js
function Person(name){
	this.name=name
}		

  var per= new Person("孙悟空");
  console.log(per);//打印的是一个Person对象,如果不加new则per的值为undefined
```

**注意:**new的构造函数后面加括号与不加括号的有区别,但无本质区别,这里具体<<http://www.cnblogs.com/xwant/p/7752658.html>> 



**构造函数的执行流程**

**1.立刻创建一个新的对象**

**2.将新建的对象设置为函数中的this，在构造函数中可以使用this来引用新建的对象**

**3.执行函数中的代码**

**4.将新建的对象作为返回值返回**

**使用同一个构造函数创建的对象，我们称为一类对象，也将一个构造函数称为一个类，我们将通过一个构造函数的对象，称为是该类的实例**

```js
//可以使用instanceof可以检验一个对象是否是一个类的实例

function Person(name){
	this.name=name
}		
  var per= new Person("孙悟空");
  console.log(per instanceof Person);//如果per是Person类的实例返回true，否则返回false
  
  console.log(per instanceof Object);
//所有的对象都是Object的后代，所有任何对象和Object与instanceof检验都会返回true
```



**在构造函数中定义方法**

```js
function Person(name,age,gender){
	this.name=name;
	this.age=age;
	this.gender=gender;
	this.sayName=function(){
		alert("hello");
	};
}
var per=new Person("孙悟空",18,"男");
//但是上面当调用方法的时候没调用一次就要重新创建一次方法，会影响性能

//使用下面的方法可以只用创建一次函数,节省了性能,但是将函数定义在全局作用域，污染了全局作用域的命名空间
this.sayNaem=fun;

funciton fun(){
alert("hello");
};

//我们一般要为对象添加方法时都是通过给原型添加方法从而做到给这个对象添加方法
```



### 9.10 函数原型对象prototype

**只有函数对象才有原型这个属性,每次创建函数时,解析器都会向函数中添加一个属性prototype,这个属性对应一个**

**对象，这个对象就是我们所谓的原型对象**

如果函数**作为普通函数**调用prototype没有任何作用,当函数**以构造函数的形式调用**时,它所创建的对象都会有一个隐含属性__ proto __，指向改构造函数的原型对象prototype

**注意:prototype只有函数才有，而一个对象是没有这个属性的,但是两者都有__ proto __,**一个函数的prototype也

是通过构造函数创造的，用的是new Object()构造函数

```js
function Person(name,age,gender){
	this.name=name;
	this.age=age;
	this.gender=gender;
}
var per=new Person("孙悟空",18,"男");
console.log(Person.prototype===per.__proto__);//返回true
//证明构造函数的prototype属性和per的__proto__属性的指向是相同的
```

**原型对象就相当于一个公共的区域，所有同一个类的实例都可以访问这个原型对象，我们可以将对象中共有的**

**内容，统一到原型对象中,但我们访问对象的一个属性或方法时，它会现在对象自身中寻找，如果有则直接使**

**用，如果没有则会去原型对象中寻找，如果找到则直接使用**

```js
function Person(name,age,gender){
	this.name=name;
	this.age=age;
	this.gender=gender;
}
//向Person的原型中添加属性a
Person.prototype.a=123;
//向Person的原型中添加方法sayName
Person.prototype.sayName=function(){
	alert("hello"+this.name);
}
var per=new Person("孙悟空",18,"男");
console.log(per.a);
per.sayName();

/*
以后创建构造函数时，可以将这些对象共有的属性和方法，统一添加到构造函数的原型对象中，这样不用分别为每一个对象添加，也不会影响到全局作用域，就可以使每一个对象都具有这些属性和方法
*/
```

**注意:**

- **通过原型对象添加的属性和方法函数本身是没有的**,这就是静态方法和实例方法的区别

- 在前面我们知道可以用 "属性名" in 对象 来检验对象中是否含有某个属性,但是如果对象自身没有该属性中但是

  原型对象中有该属性，也会返回true,所以为了区分是否是自己本身的属性,可以使用**hasOwnProperty()**

  方法来检查对象自身中是否含有该属性

  ```js
  function Person(name,age,gender){
  	this.name=name;
  	this.age=age;
  	this.gender=gender;
  }
  Person.prototype.a=123;
  var per=new Person("孙悟空",18,"男");
  console.log("a" in per);//true
  console.log(per.hasOwnProperty("a"));//false
  ```

  **注意:**

  **测试的对象自身并没有hasOwnProperty方法,并且该方法也不在它的原型中,而是在Object的原型中**

  ```js
  function Person(name,age,gender){
  	this.name=name;
  	this.age=age;
  	this.gender=gender;
  }
  Person.prototype.a=123;
  var per=new Person("孙悟空",18,"男");
  console.log(per.hasOwnProperty("per.hasOwnPropert"));//false
  ```

  **原因:**

  原型对象也是一个对象，说明也有一个原型__ proto __，当我们使用一个对象的属性或者方法的时候，会先在

  自身中寻找，如果有则直接使用，如果没有则去原型对象中寻找，如果原型对象中有，则使用，如果没有则去

  原型的原型中寻找，直到找到Object对象的原型，Object对象的原型没有原型，如果在Object中依然没有找

  到，则返回undefined



### 9.11 继承

#### 9.11.1 组合继承

原型可以只通过构造函数再次获得继承,能够继承父级别的方法,但是这样有缺陷，每一次在设置属性的时候都需要再次设置相同的属性,而且如果通过父类的原型设置属性容易造成值重复的情况(这种方式还会造成数据共享)

```js
function Person(name, age, gender) {
    	this.name = name;
       	this.age = age;
        this.gender = gender;
}
Person.prototype.sayHello=function(){
    alert("hello");
}
Person.prototype.name="孙悟空";
/*
下面这种写法更加简便
Person.prototype={
	sayHello:function(){
		alert("hello");
	},
	name="孙悟空"
}
*/
function Student(name,age,gender,grade){//在这里因为是设置了name属性,如果没有设置所有通过
  		this.name = name;				//Student类创建的对象的name都是孙悟空
   	 	this.age = age;
    	this.gender = gender;
    	this.grade.grade
}
Student.prototype=new Person();//虽然这样Student类能继承Person类的sayHello方法,但是需要再设置								 //属性
```

**解决方案:**

- 继承属性使用父类.call(参数)

- 通过函数的原型相同实现继承方法

```js
function Person(name, age, gender) {
    	this.name = name;
       	this.age = age;
        this.gender = gender;
}
Person.prototype.sayHello=function(){
    alert("hello");
}
function Student(name,age,gender,grade){
  		Person.call(this,name,age,gender);//解决了属性继承，并且值不重复的问题
    	this.grade=grade;
}
Student.prototype=Person.prototype;
//虽然能继承到父类的方法,但是却将两者的内存统一了,如果修改子类的prototype父类的protptype也会变化
```

**解决方案:**

- 继承属性使用父类.call(参数)
- 对原型使用构造函数

```js
function Person(name, age, gender) {
    	this.name = name;
       	this.age = age;
        this.gender = gender;
}
Person.prototype.sayHello=function(){
    alert("hello");
}
function Student(name,age,gender,grade){
  		Person.call(this,name,age,gender);
    	this.grade=grade;
}
Student.prototype=new Person();
/*
	虽然这样基本不会影响效果,但是在观察一个由子类创建的对象可以发现虽然子类上继承的属性有正确的属性值,
	但是其父类上依旧显示有该属性,并且该属性的属性值为undefined,会造成属性结构的杂乱
*/
```

**最终解决方案:**

- 继承属性使用父类.call(参数)
- 借助一个无用的构造函数
- 对原型使用无用的构造函数

```js
function Person(name, age, gender) {
    	this.name = name;
       	this.age = age;
        this.gender = gender;
}
Person.prototype.sayHello=function(){
    alert("hello");
}
function Student(name,age,gender,grade){
  		Person.call(this,name,age,gender);
    	this.grade=grade;
}
function Exchange(){
    
}

Exchange.prototype=Person.prototype;//用无用的构造函数的prototype来指向Person的prototype
Student.prototype=new Exchange();
```

**也可以用Object.create()方法来继承原型,同时也不会造成结构杂乱**

```js
function Person(name, age, gender) {
    	this.name = name;
       	this.age = age;
        this.gender = gender;
}
Person.prototype.sayHello=function(){
    alert("hello");
}
function Student(name,age,gender,grade){
  		Person.call(this,name,age,gender);
    	this.grade=grade;
}
Student.prototype = Object.create(Person.prototype);
```

**注意:在继承的时候需要重新设置一下constructor,使用A.prototype.constructor=A来设置**



**为什么需要做A.prototype.constructor=A这样的修正?**

- 在构造函数时new 函数名() 本质上是通过函数原型中的constructor()构造函数方法(该方法是创建函数时就有的属性),所以new 函数名()和new 函数名.prototype.construstor()是一样的效果

  ```js
  function Person(name, age, gender) {
          this.name = name;
          this.age = age;
          this.gender = gender;
  }
  var per = new Person.prototype.constructor("孙悟空", 18, "男");
  console.log(per);
  ```

- 在原型继承的时候,我们将上级函数的实例赋值给了下级函数的原型,所以就会造成下级函数原有的原型发生彻

  底的改变。因为constructor属性是原型默认的两个属性之一。该属性在正常情况下是等于其对应的函数本身

  的。但是在实现原型继承之后,下级函数的constructor属性就会发生改变,变为了等于上级函数本身。但是如果

  我们不进行修正的话也可以正常使用,不会发生什么问题。那么我们修正这个constructor属性就只是为了防止

  一种情况下出错:在不知道某个对象是哪个函数实例化的时候想要克隆一个对象

  来自 <<https://www.aliyun.com/jiaocheng/984663.html>> 

  ```js
  //例如,我不知道person对象是在哪个函数实例化出来的,但是我想克隆一个,这时候就可以这样
  function Person(name, age, gender) {
          this.name = name;
          this.age = age;
          this.gender = gender;
  }
  
  function Student(name,age,gender,grade){
      Person.call(this,name,age,gender);
      this.grade=grade;
  }
  function Exchange(){
      
  }
  
  Exchange.prototype=Person.prototype;
  Student.prototype=new Exchange();
  
  Student.prototype.constructor=Student;
  var stu	= new Student("孙悟空", 18, "男",100);
  var stu1 = new stu.constructor();//而如果在继承时没有通过重新设置construct,那么克隆后的对象
  								 //就相当于是通过new Person()创建的,没有grade属性
  
  //可以写一个继承方法
  function extend(child,parent){
  child.prototype=new parent();
  child.prototype.constructor=child;
  }
  ```



#### 9.11.2 拷贝继承

**通过拷贝将函数原型中的属性和方法复制,达到继承的目的**

```js
function Person(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
}
Person.prototype.sayHello=function(){
    alert("hello");
}
Person.prototype.a=123;

function Student(name,age,gender,grade){
    Person.call(this,name,age,gender);
    this.grade=grade;
}
//子类拷贝父类原型继承父类原型方法
for(var key in Person.prototype){
    Student.prototype[key]=Person.prototype[key];
}


var obj={}; 
for(var key in Person.prototype){
    obj[key]=Person.prototype[key];
}
console.log(obj);
console.log(obj.a);//123
obj.sayHello();
```

**上面的方式是浅拷贝,虽然能够获得相同的值,该对象中对象或数组的值的地址并没有发生改变,还是被拷贝者的地址**



**在ES6中Object.assign()的效果和浅拷贝的效果相同,但该函数可以继承多个类**

Object.assign()方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象并返回目标对象,可以放入多

个参数,第一个是要被放入的对象,可以直接一个空对象,后面的参数是要拷贝的对象，可以传入多个对象把所有对象

的属性或者方法都传入到前面的空对象里，最后将结果返回出来

```js
function MyClass() {
     SuperClass.call(this);
     OtherSuperClass.call(this);
}
// 继承一个类
MyClass.prototype = Object.create(SuperClass.prototype);
// 混合多个类的继承类
Object.assign(MyClass.prototype, OtherSuperClass.prototype);
// 重新指定constructor
MyClass.prototype.constructor = MyClass;

MyClass.prototype.myMethod = function() {
     
};
```

```js
const object1 = {
  a: 1,
  b: 2,
  c: 3
};
const object2 = Object.assign({c: 4, d: 5}, object1);//实际上这种叫做根元素深复制,其他元素浅
console.log(object2.c,object2.d);//3,5				 //复制
```

**注:**

- 如果只有一个参数,该函数会直接返回该参数
- 如果传入null和undefined作为参数会报错,因为null和undefined无法被转换为对象
- 该函数可以用来处理数组,但是会把数组视为对象
- 可以用扩展运算符(...)来对一个对象进行拷贝



**如果想要地址也不同就要进行深拷贝**,深拷贝是将重新开辟空间进行复制

```js
//这里是别人直接写的方法,拿过来引用
function deepClone(obj){
    let objClone = Array.isArray(obj)?[]:{};//Array.isArray()函数可以低版本IE不兼容,可以用
    if(obj && typeof obj==="object"){		//obj instanceof Array来判断是否为数组
        for(key in obj){
            if(obj.hasOwnProperty(key)){
                //判断ojb子元素是否为对象，如果是，递归复制
                if(obj[key]&&typeof obj[key] ==="object"){
                    objClone[key] = deepClone(obj[key]);
                }else{
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}    
```

 **用JSON进行深拷贝(这种拷贝不支持函数)**

```js
var obj={a:1,b:[0,1]};
var json=JSON.parse(JSON.stringify(obj));
json.b.push(2);
console.log(obj);//{a:1,b:[0,1]};
console.log(json);//{a:1,b:[0,1,2]};
```



**兼容深浅拷贝**

```js
//obj为拷贝对象,deep是一个布尔值,默认是flase代表浅拷贝,true代表深拷贝
function extend(obj,deep){//因为不传是undefined转布尔值为false
    var objClone=(obj instanceof Array)?[]:{};
    if(obj && typeof obj==="object"){
        for(key in obj){
            if(obj.hasOwnProperty(key)){
             	if(!!deep&&obj[key]&&typeof obj[key] ==="object"){
               		 objClone[key] = extend(obj[key]);
             	 }else{
              		  objClone[key] = obj[key];
          		  }   
            }
        }
    }
   return objClone;
}
```



#### 9.11.3 获取子类的父类

使用`Object.getPrototypeOf()`可以获取子类的继承类

```js
function Person(name, age, gender) {
    	this.name = name;
       	this.age = age;
        this.gender = gender;
}
Person.prototype.sayHello=function(){
    alert("hello");
}
Person.prototype.name="孙悟空";

function Student(name,age,gender,grade){//在这里因为是设置了name属性,如果没有设置所有通过
  		this.name = name;				//Student类创建的对象的name都是孙悟空
   	 	this.age = age;
    	this.gender = gender;
    	this.grade.grade
}
Student.prototype=new Person();//虽然这样Student类能继承Person类的sayHello方法,但是需要再设置			
console.log(Object.getPrototypeOf(Student) == Person)// true
```



### 9.12 call,apply与bind方法

**这三个方法都是函数对象的方法，需要通过函数对象来使用**

- call()与apply()方法

  当对函数调用call()和apply()方法时都会调用函数执行，在调用call()和apply()可以将一个对象指定为第一个参

  数，此时函数执行时的this将会指向该对象

  ```js
  function fun(){
      console.log(this);
  }
  var obj={name:"孙悟空"}; 
  var obj2={name:"猪八戒"};
  
  fun.call(obj);//this指向obj
  fun.apply(obj2);//this指向obj2
  fun();//this永远指向window
  
  /*
  如果是在通过一个对象运用该对象的方法时在call()或者apply()中指定了其他的对象,this就会变成其他的对象而不会是原来的对象了
  */
  ```

  

  **call()与apply()方法的区别**

  **除了第一个参数,两个方法还可以传入其它的参数,这些参数是要传入指定函数的实参**

  - call()方法传递实参只需要在第一个参数后面依次用,(逗号)隔开

    ```js
    var obj={num:1};
    function fun(number1,number2){
        console.log(this,num+number1+number2);
    }
    fun.call(obj,10,20);//31
    ```

  - apply()方法需要将实参封装到一个数组中统一传递

    ```js
    var obj={num:1};
    function fun(number1,number2){
        console.log(this,num+number1+number2);
    }
    fun.apply(obj,[10,20]);//31
    ```

  **小应用:**使用Object.prototype.toString.call(obj)能够判断一个对象是否为某一类型对象

  ```js
  function getFunc(type){
      return function(obj){
          return Object.prototype.toString.call(obj)===type;
      }
  }
  
  var fun=getFunc("[object Array]");
  var result=fun([10,20,30]);
  console.log(result);//true
  
  var fun=getFunc("[object Objcet]");
  var dt=new Date();
  var result1=fun(dt);
  console.log(result1);//false
  ```

  

- bind()方法

  bind()方法能够复制一个函数或方法,并将赋值后的函数或方法返回,在赋值时可用传入了对象作为复制的函数

  中的this指向(该方法不兼容IE8及以下浏览器)

  **注意:**

  - **传入实参可以在复制的时候传入.也可以在调用的时候传入**
  - 与call()和apply()方法不同,使用了bind方法只会将复制后的函数作为返回值传递回来,并不会调用函数

```js
function Person(age){
    this.age=age;
}
Person.prototype.play=function(){
    console.log(this);
    console.log(this.age);
}

function Student(age){
	this.age=age;
}
var per=new Person(10);
var stu=new Student(20)；

var fun=per.play.bind(stu);
fun();//this指向为stu,this.age的值为20
```

```js
function f1(x,y){
console.log(x+y)；
}
var ff=f1.bind(null,10,20)//在复制的时候传入实参
ff();
Var ff=f1.bind(null);
ff(10,20);//在调用的时候传入参数
```



**bind()的应用**

```js
function showRandom(){
    //产生1-10的随机数
    this.number=parseInt(Math.random()*10+1);
}
//添加原型方法
showRandom.prototype.show1=function(){
    //改变了定时器中的this执行,本来是window,现在为实例对象
    window.setInterval(this,show2.bind(this),1000);
}
//显示随机数
showRandom.prototype.show2=function(){
    console.log(this.number);
}

var num=new showRandom();
num.show1();

//定时器只有在调用函数里面的时候才会把里面的this改为window，而传了一个关于this的函数其实是先用了这个属性再开的定时器
```

 

**兼容代码**

```js
if(!Function.prototype.bind){
	Function.prototype.bind=function(){
	    var bindThis=arguments[0];
 	   var arg=[].slice.call(arguments,1);
 	   var that=this;
 	   return function(){
  	      that.apply(bindThis,arg);
  	  }
	}
}
```



### 9.13 闭包

- **闭包的概念:**函数A中,有一个函数B,函数B中可以访问函数A中定义的变量或者数据,此时形成了闭包(这句话暂时不严谨)

- **闭包的模式:**分为函数模式的闭包和对象模式的闭包(函数中有一个对象)

- **闭包的作用:**缓存数据,延迟作用域链,闭包使用的父函数的变量或参数会被永久保存,直到页面关闭

- **闭包的优点和缺点:**缓存数据(优点也是缺点,没有及时的释放数据)

  局部变量在函数中被函数使用后会被自动的释放,而闭包后,里面的局部变量的使用作用域链就会被延长

- **闭包的形成条件:**

  如果满足以下两点,那么可以把内部函数称为闭包

  - 函数嵌套函数
  - 内部函数使用父函数的变量或参数

```js
//函数模式的闭包
//例一
function f1(num){
    function f2(){
        console.log(++num);//11,因为f2在f1的函数作用域里面,所以f2能用f1传入的参数
    }
    f2()
}
f1(10);//10
//例二2
document.onclick=(function(){
    var a=0;
    return function(){
        console.log("第”+ ++a +“次点击页面");
    }
})()

//对象模式的闭包
function f3(){
    var num=10;
    var obj={
     age:num;   
    }
    console.log(obj.age);
}
f3();//10
```

```js
//案例:输出三个相同的随机数
function fun(){
    var num=parseInt(Math.random()*10+1)；
    return function(){
        console.log(num);
    }
}
var fun2=fun();
//下面输出的结果全部相同
fun2();
fun2();
fun2();
```

```js
//案例:输出阶乘
function fun(){
    var obj={};
    return function f(n){
        var num=n+"!";
        if(obj[num]){
            return obj[num];
        }else if(n=1){
            obj[num]=1;
            return 1;
        }else{
            obj[num]=n*f(n-1)
            return obj[num];
        }
    }
}
var fun1=fun();
console.log(fun1(10));
console.log(fun1(9));
console.log(fun1(11));
/*
在计算10的阶乘时会用一定时间,然后会在obj内部储存着1到10的所有阶乘,这时就相当于数据缓存,在计算9和11的阶乘的时候运行速度更快
*/
```

**总结:**如果想要缓存数据,就把这个数据放在外层函数和里层函数的中间位置



### 9.14 垃圾回收(GC)

**垃圾积攒过多会导致程序运行的速度过慢，所以需要一个垃圾回收机制**

当一个对象没有任何的变量或属性对它进行引用，此时我们将永远无法操作该对象，此时这种对象就像一个垃圾，

会占用大量内存空间，导致程序运行变慢，在JS中拥有自动的垃圾回收机制，会自动回收,但是如果有一个变量或

属性还占用着这个对象.这个对象就不会被自动回收,所有我们需要**将不再使用的对象设置为null来解放该对象**



以下内容来自[js闭包详解](https://www.cnblogs.com/zhangweidongx/p/4330222.html)

**JS的内存回收机制**

一个函数在执行开始的时候,会给其中定义的变量划分内存空间保存,以备后面的语句所用,等到函数执行完毕返回了,这些变量就被认为是无用的了，对应的空间也就被回收了。下次再执行此函数的时候，所有的变量又回到最初的状态，重新赋值使用。

但是如果这个函数内部又嵌套了另一个函数(这就是闭包了)，而这个函数是有可能在外部被调用到的。并且这个内部函数又使用了外部函数的某些变量的话.这种内存回收机制就会出现问题。如果在外部函数返回后，又直接调用了内部函数，那么内部函数就无法读取到他所需要的外部函数中变量的值了。

所以js解释器在遇到函数定义的时候会自动把函数和他可能使用的变量(包括本地变量和父级和祖先级函数的变量(自由变量))一起保存起来。也就是构建一个闭包，这些变量将不会被内存回收器所回收，只有当内部的函数不可能被调用以后(例如被删除了,或者没有了指针)，才会销毁这个闭包,而没有任何一个闭包引用的变量才会被下一次内存回收启动时所回收。

## 10.Date对象

**在js中使用Date对象来表示一个时间对象**,Date可以作为构造函数,也可以作为一个普通对象使用

### 10.1 创建一个Date对象

- 如果直接使用构造函数创建一个Date对象，则会封装为当前代码执行的时间

  ```js
  var d=new Date();
  console,log(d)//打印出当前执行到该时刻时的时间(以地区时间为准)
  ```

- 创建一个指定的时间对象

  需要在构造函数中传递一个表示时间的字符串作为参数

  ```js
  //格式：月/日/年 时间(比如12:31:30表示12点31分30秒)
  var dt=new Date("2/15/2019 12:31:30");//所有设置年月日前面都可以加0或者不加如02/15/2019
  console.log(dt);//Fri Feb 15 2019 12:31:30
  ```


**注意:**

- new Date()获取的时间对象通过toString()方法可以完全转换为一个字符串
- 通过Date.parse()函数可以将传入的字符串参数转换为一个日期对象



### 10.2 Date对象的方法

#### 10.2.1 getDate方法

**getDate()方法获取当前日期对象是该月中的第几日**

```js
var date=new Date("2/15/2019 12:31:30");
var day=date.getDate();
console.log(day);//15
```



#### 10.2.2 getDay,getMonth与getFullYear方法

- **getDay()方法获取当前日期是周几**,该方法的返回值为0-6**(0为周日)**,1-6依次为周一到周六

  ```js
  var date=new Date("2/15/2019 12:31:30");
  var week=date.getDay();
  console.log(week);//5
  ```

  

- **getMonth()方法获取当前日期月份,**该方法的返回值为0-11**(0为一月)**,依次向后排序

  ```js
  var date=new Date("2/15/2019 12:31:30");
  var month=date.getMonth();
  console.log(month);//1
  ```



- **getFullYear()方法获取当前日期年份**

  ```js
  var date=new Date("2/15/2019 12:31:30");
  var year=date.getFullYear();
  console.log(year);//2019
  ```



#### 10.2.3 getHours,getMinutes,getSeconds与getMilliseconds方法

- getHours()方法获取当前日期的小时数
- getMinutes()方法获取当前日期的分钟数
- getSeconds()方法获取当前日期的秒数
- getMilliseconds()获取当前日期的毫秒数

```js
var date=new Date("2/15/2019 12:31:30");
var hour=date.getHours();
var minute=date.getMinutes();
var second=date.getSeconds();
var millscecond=date.getMilliseconds();
console.log(hour);//12
console.log(minute);//31
console.log(second);//30
console.log(millsecond);//0,因为没有设置,在这里默认是0
```



#### 10.2.4 getTime方法

**getTime()方法获取当前日期对象的时间差,**就是自格林威治标准时间的1970年1月1日0时0分0秒到现在的是

所经历的毫秒数(因为计算机底层报错时间就是用的毫秒)

```js
var date=new Date("2/15/2019 12:31:30");
var time=date.getTime();
console.log(time);//1550205090123
```

**注:**如果想要具体到秒,分或者时,只需要将返回的值除以各自相对于毫秒的倍数即可

**注意:**如果我们(在中国)用new Date()创建对象时传入的日期是1970年1月1日0时0分0秒,用getTime()方法也会返回

一个负数(-28800000),因为我们输入的时间默认是北京时间,相对于标准时间有8个小时的提前时差

```js
var date=new Date("1/1/1970 0:0:0");
var time=date.getTime();
console.log(time);//-28800000
```



#### 10.2.5 getTimezoneOffset方法

**getTimezoneOffset()方法获取当前地区相对于世界时的时间差,**该方法会返回一个数字,单位为分钟

```js
var date=new Date();
var time=date.getTimezoneOffset();//世界时-本地时间=-8*60 分钟
console.log(time);//-480,因为本地时间比世界时快
```



### 10.3 Date.now函数

**Date.now()函数能够获取当前时刻距离格林威治标准时间的1970年1月1日0时0分0秒的时间差,**是一个静态方法,

通常用作测试代码的执行性能(在代码前后分别设置事件作差值)

```js
var time=Date.now();
console.log(time);
```



## 11.Math对象

Math不是一个构造函数,只是一个普通对象,Math对象中包含了许多数学方法

**所有方法通过Math.方法名()来使用;**

**在这里只列举了一些常用数学方法**

- abs()取绝对值

- sqrt()计算开方,得到算术平方根

- ceil()可以对一个数进行向上整数，小数位是要有值就会自动进一位

- floor()可以对一个数进行向下取整，小数部分会被舍掉

- round()可以对一个数进行四舍五入

- max(x,y,z)可以获取多个数中的最大值

- min(x,y,z)可以获取多个数中的最小值

- pow(x,y)返回x的y次幂

- trunc()方法用于去除一个数的小数部分,返回整数部分

  **注意:**

  - 该方法对于大于0的数时向下取整,对于小于0的数是向上取整
  - 对于非数值会先进行强制类型转换,对于空值或无法取整的值会返回NaN

- sign()方法用来判断一个数是正数,负数,还是0,如果是非数值会先进行强制类型转换

  **五种返回值**

  - 参数为正数,返回+1
  - 参数为负数,返回-1
  - 参数为0,返回0
  - 参数为-0,返回-0
  - 参数为其他值,返回NaN

- cbrt()方法用于计算一个数的**立方根**,非数值会进行强制类型转换

- hypot()返回**所有参数的平方和的平方根**,用法相当于用勾股定理已知两直角边求斜边

  ```js
  console.log(Math.hypot(3,4));//5
  console.log(Math.hypot(3,4,5));//7.0710678118654755
  console.log(Math.hypot());//0,不传参数返回值为0
  console.log(Math.hypot(NaN));//NaN
  console.log(Math.hypot(3,4,"num"));//NaN
  console.log(Math.hypot("3","4"));//5
  console.log(Math.hypot(-3,-4));//5
  ```

- random()可以生成一个0-1之间[0,1)的随机数

  - **生成一个0-X之间的随机数Math.random()*X**
  - **生成一个X-Y之间的随机数Math.random()*(Y-X)+X**

  ```js
  //生成一个0-10的随机数
  var num1=Math.random()*10
  console.log(num1);
  //如果要取整可以四舍五入 
  console.log(Math.round(num1));
  ```

- 保留几位小数用toFixed()方法,这个方法不是Math对象调用,而是原来的数值对象调用,参数为具体数值,并将会进行四舍五入

  **注意:**

  - 该方法转换后的值为字符串,不是数值,所以能显示末尾的0

    ```js
    var num=1.200;
    console.log(num.toFixed(2));//1.20
    console.log(parseFloat(num.toFixed(2));//1.2
    ```

  - 该方法内部是用的银行的四舍五入算法,和普通的四舍五入不同(具体的银行四舍五入算法请私下了解)

    ```js
    var num=0.95;
    var num2=1.35;
    console.log(num.toFixed(1));//0.9,代表保留一位小数
    console.log(num2.toFixed(1));//1.4
    ```



## 12.包装类

**js中为我们提供了三个包装类，通过这三个包装类可以将基本的数据类型转换为对象**

**这三个包装类分别为String(),Number()和Boolean()**



**对于包装类的举例**

```js
var num=new Number(3)
//这样就能把基本数据类型number转换为一个对象,3依然是num的值，这样做能为num添加属性

/*
注意:
1.如果为一个基本数据类型添加属性虽然不会报错但是不会添加成功
2.如果在创建number类型的时候用了包装类,那么两个相同的值比较也就不再相等,因为开辟了两片空间,地址不同
*/
```

**注:在实际开发中我们不会使用包装类创建对象，如果使用这种基本数据类型的对象，在做一些比较时可能会带来一些不可预期的结果**



**包装类的用途**

**浏览器会自己调用包装类的方法，**当我们对一些基本数据类型的值调用属性和方法时，浏览器会临时使用包装类将其转换为对象，然后调用对象的属性和方法，调用完以后又转换回来



**对基本数据类型添加属性或方法的解释**

给一个基本数据类型添加属性或方法就是**将该类型的值转换为对象**，其实属性添加进去了，但是因为临时转换，又转换为基本数据类型，然后销毁了属性或方法

```js
var num=1;
num.say="hello";//这时确实进行了类型转换但是又马上转换回来销毁了say属性

console.log(num.say);//undefined
/*
第二次使用该属性时其实是又临时转换了类型,但是由于每次转换都是新建一个对象,所以第二次转换的对象和第一次的
对象不同,这个对象没有添加say属性,所以返回的值为undefined
*/
```



## 13.正则表达式

**正则表达式用于定义一些字符串的规则,计算机可以根据正则表达式，来检查一个字符串是否符合规则,获取字符串中符合规则的内容**

**基本概念**

- 正则表达式是由一个字符序列形成的搜索模式
- 当在文本中搜索数据时，可以用搜索模式来描述要查询的内容
- 正则表达式可以是一个简单的字符，或一个更复杂的模式
- 正则表达式可用于所有文本搜索和文本替换的操作



### 13.1 创建正则表达式对象

**正则表达式实质上也是一个对象**

- 构造函数创建正则表达式对象

  **语法:var 变量=new RegExp("正则表达式","匹配模式")**

  ```js
  var reg=new RegExp("a");//可以不传入匹配模式
  ```

  **正则表达式对象可以使用test()方法可以用来检查一个字符串是否符合正则表达式的规则,如果符合返回true，否则返回false**

  ```js
  var reg=new RegExp("a");
  var str="a";
  var str1="A"；
  var result=reg.test(str);
  var result=reg.test(str1);
  console.log(result);//true
  console.log(result);//false
  //这种正则表达式可以检查一个字符串中是否含有a，严格区分大小写，如果不区分大小写可以传入第二个参数
  ```

  

  **匹配模式**

  正则表达式中有三种匹配模式

  - i 忽略大小写匹配

  - g 全局匹配(查找所有匹配而非在找到第一个匹配后停止)

  - m 多行匹配,作用是修改^和$在正则表达式中的作用,让它们分别表示行首和行尾,在默认情况下,一个字符串无论是否换行只有一个开始^和结尾$,如果采用多行匹配,那么每一行都有一个^和$

    **注:三种匹配模式可以同时使用,写法与顺序无关**

    ```js
    var reg=new RegExp("a","i")//不区分大小写a
    var str="a";
    var str1="A"；
    var result=reg.test(str);
    var result=reg.test(str1);
    console.log(result);//true
    console.log(result);//true
    ```

- 使用字面量创建正则表达式

  **语法:var 变量=/正则表达式/匹配模式**

  ```js
  var reg=/a/i;
  //多个匹配模式
  var reg2=/a/ig;
  ```



**构造函数与创建字面量函数的区别**

- 使用字面量的方式创建更加简单

- 使用构造函数创建更加灵活



### 13.2 正则符号

以下参考自w3cschool

- 方括号,**用于查找某个范围内的字符**

  | 表达式             | 描述                                                         |
  | ------------------ | ------------------------------------------------------------ |
  | [abc]              | 查找方括号之间的任何字符。                                   |
  | [^abc]             | 查找任何不在方括号之间的字符。,只要一个字符串中有除了abc以外的字母就会返回true |
  | [0-9]              | 查找任何从 0 至 9 的数字。                                   |
  | [a-z]              | 查找任何从小写 a 到小写 z 的字符。                           |
  | [A-Z]              | 查找任何从大写 A 到大写 Z 的字符。                           |
  | [A-z]              | 查找任何从大写 A 到小写 z 的字符。                           |
  | [adgk]             | 查找给定集合内的任何字符。                                   |
  | [^adgk]            | 查找给定集合外的任何字符。                                   |
  | (red\|blue\|green) | 查找任何指定的选项。                                         |

  ```js
  var reg1=/ab/i; //检验一个字符串中是否有ab(整体)
  var reg2=/a|b/; //检查一个字符串中是否有a或b
  var reg3=/a|b|c/; //检查一个字符串中是否有a或b或c
  var reg4=/[ab]/;  //[]里面也是或的关系[ab]===a|b
  var reg5=/a[bde]c/; //查一个字符串中是否还有abc或adc或aec(整体)
  ```

  

- 元字符，是拥有特殊含义的字符

  | 元字符 | 描述                                                         |
  | ------ | ------------------------------------------------------------ |
  | .      | 查找单个字符，除了换行和行结束符。                           |
  | \w     | 查找单词字符。                                               |
  | \W     | 查找非单词字符。                                             |
  | \d     | 查找数字。                                                   |
  | \D     | 查找非数字字符。                                             |
  | \s     | 查找空白字符。                                               |
  | \S     | 查找非空白字符。                                             |
  | \b     | 匹配单词边界。一个单词旁边必须是有空白字符作为边界。比如判断是否含有child单词,用/\bchild\b/ 判断是否有一个独立的单词 |
  | \B     | 匹配非单词边界。单词左右两边没有空白字符                     |
  | \0     | 查找 NULL 字符。                                             |
  | \n     | 查找换行符。                                                 |
  | \f     | 查找换页符。                                                 |
  | \r     | 查找回车符。                                                 |
  | \t     | 查找制表符。                                                 |
  | \v     | 查找垂直制表符。                                             |
  | \xxx   | 查找以八进制数 xxx 规定的字符。                              |
  | \xdd   | 查找以十六进制数 dd 规定的字符。                             |
  | \uxxxx | 查找以十六进制数 xxxx 规定的 Unicode 字符。                  |

  **注意:**

  - **在正则表达式中\表示转义字符，但是js中任何地方\都是转义字符，所以要验证\也必须输入两个\\(字面量中)**

    **如:**正则表达式.表示任意字符/./只会表示是否有任意字符，如果要判断有没有.(点这个符号),需要用/\ ./来表示

  - **如果用构造函数的方法，由于它的参数是一个字符串，而\是字符串中的转义字符，如果要使用\则需要使用\ \代替(相当于用了三个\ )，这样相当于把他们先化为字面量，再自变量转换，而因为\ .其实就是.，所以没有作用**

  

- 量词,通过量词可以设置一个内容出现的次数用{}里面写需要的数量,量词只对它前边的一个内容起作用，

  | 量词   | 描述                                                         |
  | ------ | ------------------------------------------------------------ |
  | n+     | 匹配任何包含至少一个 n 的字符串。例如，/a+/ 匹配 "candy" 中的 "a"，"caaaaaaandy" 中所有的 "a"。 |
  | n*     | 匹配任何包含零个或多个 n 的字符串。例如，/bo*/ 匹配 "A ghost booooed" 中的 "boooo"，"A bird warbled" 中的 "b"，但是不匹配 "A goat grunted"。 |
  | n?     | 匹配任何包含零个或一个 n 的字符串。例如，/e?le?/ 匹配 "angel" 中的 "el"，"angle" 中的 "le"。 |
  | n{X}   | 匹配包含 X 个 n 的序列的字符串。例如，/a{2}/ 不匹配 "candy," 中的 "a"，但是匹配 "caandy," 中的两个 "a"，且匹配 "caaandy." 中的前两个 "a"。 |
  | n{X,}  | X 是一个正整数。前面的模式 n 连续出现至少 X 次时匹配。例如，/a{2,}/ 不匹配 "candy" 中的 "a"，但是匹配 "caandy" 和 "caaaaaaandy." 中所有的 "a"。 |
  | n{X,Y} | X 和 Y 为正整数。前面的模式 n 连续出现至少 X 次，至多 Y 次时匹配。例如，/a{1,3}/ 不匹配 "cndy"，匹配 "candy," 中的 "a"，"caandy," 中的两个 "a"，匹配 "caaaaaaandy" 中的前面三个 "a"。注意，当匹配 "caaaaaaandy" 时，即使原始字符串拥有更多的 "a"，匹配项也是 "aaa"。 |
  | n$     | 匹配任何结尾为 n 的字符串。                                  |
  | ^n     | 匹配任何开头为 n 的字符串。注意与[^]作对比                   |
  | ?=n    | 匹配任何其后紧接指定字符串 n 的字符串。                      |
  | ?!n    | 匹配任何其后没有紧接指定字符串 n 的字符串。                  |

  ```js
  var reg=/a{3}/; //表示aaa,但ab{3}只表示abbb,如果要ab一起出现3次，需要/(ab){3}/
  
  
  /*注意：这里面只要就会true正确执行，比如b{3}但是bbbb依然是正确的，因为包含了3个b*/
  ```

  

  **注:在正则表达式中同时使用^ $则要求字符串必须完全符合正则表达式**

  ```js
  var reg1=/^a$/; //表示既要有a开头同时这个开头的a还必须是结尾
  /*上式说明只能有一个a，aaa这种是不行的，因为结尾的a不是开头的a*/
  
  var reg2=/^a|a$/; //表示以a开头或者以a结尾
  
  var reg3=/^b([0-9A-z]){0,}b$/; //表示以b开头同时以b结尾同时中间可以跟任意数字或者字母
  ```

  

- 子集,用圆括号()包起来的属于一个整体,叫做一个子集

  ```js
  //通过子集可以很轻松的实现顺序互换
  var str="abcd";//要改成"cdab"
  var reg=/(ab)(cd)/g；
  var result=str.replace(reg,"$2$1")；
  //此时的$1和$2有特别的含有,$1代表第一个子集,$2代表第二个子集
  ```

  ```js
  var str="111223333";
  var reg=/(/d)\1+/g;//所有相同数字分为一组
  console.log(str.match(reg))//["111","22","3333"]
  ```

  

### 13.3 正则断言

**所谓断言，就是指明某个字符串前边或者后边，将会出现满足某种规律的字符串**

在将断言前.先将子集的捕获与捕获,一般的子集通过()括住都会被捕获,这时可以通过$1等来使用被捕获的子集,而如果不想要子集被捕获,只是用做一个匹配模式,就可以让子集不被捕获到,一般是通过(?:)来括住一个子集

| 模式   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| (?:X)  | 不捕获匹配,不能通过$符号进行捕获到该子集,包括下面的所有都是捕获匹配。 |
| (?=X ) | 零宽度正先行断言。仅当子表达式 X 在 此位置的右侧匹配时才继续匹配。例如，\w+(?=\d) 与后跟数字的单词匹配，而不与该数字匹配。此构造不会回溯。 |
| (?!X)  | 零宽度负先行断言。仅当子表达式 X 不在 此位置的右侧匹配时才继续匹配。例如，例如，\w+(?!\d) 与后不跟数字的单词匹配，而不与该数字匹配 。 |
| (?<=X) | 零宽度正后发断言。仅当子表达式 X 在 此位置的左侧匹配时才继续匹配。例如，(?<=19)99 与跟在 19 后面的 99 的实例匹配。此构造不会回溯。 |
| (?<!X) | 零宽度负后发断言。仅当子表达式 X 不在此位置的左侧匹配时才继续匹配。例如，(?<!19)99 与不跟在 19 后面的 99 的实例匹配 |



**例子:是否为浮点数**

```js
function isFloat(number){
    number=number+"";
    var reg=/^-?\d+\.\d+$/;//\.连在一起表示就是一个(.),没有其他意思
    return reg.test(number);
}
console.log(isFloat(1));//false
console.log(isFloat(1.1));//true
console.log(isFloat(1.0));//false，false是由于JS内部的储存机制,当浮点数后全是0时就被看成一个整数
console.log(isFloat("1.0"));//true,这时因为传入的就是一个1.0的字符串,所以为true
```

**例子:手机号规则**

- 以1开头
- 第二位3-9任意数字
- 三位以后任意数字9位

```js
var reg=/^1[3-9][0-9]{9}$/;
```

**例子:邮箱格式**

```js
var reg=/^\w{3,}(.\w+)*@[A-z0-9]+(.[A-z]{2,5}{1,2}$/;
```



### 13.4 正则表达式和字符串相关的方法

**正则表达式有两种用法，一种是调用自身的方法(如上面的test方法)，一种是作为参数进行传递**

#### 13.4.1 split方法

**split()方法中可以传递一个正则表达式作为参数来把一个字符串分割成字符串数组**

**注意:该方法即使不指定全局匹配的匹配模式，也会在全局进行拆分**

```js
var arr="1a2b3c4d5e6f7g";
var result=arr.split(/[A-z]/);//根据任意字母将字符串拆分
console.log(result);//[1,2,3,4,5,6,7]
```



#### 13.4.2 search方法

**search()可以搜索字符串中是否含有指定的内容**,这个方法和indexOf()方法类似，如果搜索到了指定内容则会返回第一次出现的索引，如果没有搜索到则会返回-1,不同的是**indexOf()方法并不支持正则表达式**

**注意:该方法即使设置全局匹配模式也只会查找出现的第一个结果**

```js
//搜索字符串中是否有abc或aec或afc
var str="abcdef"
var result=str.search(/a[bef]c/);
console.log(result);//0
```



#### 13.4.3 match方法

**match()方法用做从一个字符串中将符合条件的内容提取出来，**可以用正则表达作为参数，默认情况下match只会找到第一个符合要求的内容，找到以后就会停止检索，可以设置正则表达式为g(全局匹配模式)来匹配所有的内容

```js
var str="1a2b3c4d5e6f7A8B9C";
var result=str.match(/[a-z]/gi);//这样就能把所有的字母都提取出来
console.log(result);//[a,b,c,d,e,f,A,B,C]

/*
match()会将匹配到的内容封装到一个数组中返回，即使只查询到一个结果，所以用result[0]等索引能查询到他们
如果没有匹配到就会返回null，适合用作if等判断条件
*/
```

 

#### 13.4.4 replace方法

**replace()方法可以将字符串中指定内容替换为新的内容**

**该方法有两个参数,第一个是被替换的内容,第二个参数是要替换的新的内容**,默认只会替换查找到的第一个结果，所以可以加g进行全局匹配

```js
var str="1a2b3c4d5e";
var result1=str.replace(/[a-z]/gi,"@-@");//把所有字母都替换为@-@
var result2=str.replace(/[a-z]/gi,"");//去除所有字母(把所有字母都替换为空串)

var result3=str.replace(/^\s*|\s*$/gm,"");//去除开头或结尾的空格
```



**不用replace方法实现替换字符串的内容**

```js
//运用数组的join方法和字符串的spilt方法
var arr="1a2a3a4a5a";
var result=arr.spilt("a").join("b");
console.log(result);//"1b2b3b4b5b"
```



## 14.定时器

### 14.1 定时调用

- **当希望一段程序可以每隔一段时间执行一次时，可以使用定时调用函数setInterval()**,该函数有多个参数

  **参数**

  - 要做的事(通常是回调函数)，该函数会每隔一段时间被调用一次，如果不是函数就传入一个字符串,这个字符串中的东西代表要做的事如"alert(123)"(不推荐这样写,最好还是写函数)
  - 每次调用间隔的时间，单位是毫秒ms
  - 往后的参数参数是要传入到回调函数中的实参,没有就不填

  **该函数的返回值是一个Number类型的数据,这个数字用来作为定时器的唯一标识(相当于ID值)**,可以通过变量赋值来接收这个返回值

  ```js
  var timer=setInterval(function(){
      console.log(123);
  },1000);
  //每1S打印一次123
  ```

- **通过clearInerval()函数可以关闭一个通过setInterval()函数开启定时器,**该函数中需要一个定时器的标识作为参数，这样将关闭对应标识对应的定时器

  clearInterval()函数可以接收任意参数，如果参数是一个有效的定时器的标识，则停止对应的定时器，如果参数不是一个有效的标识，则什么也不发生，也可以直接传入定时器传入的数值ID值cleatInterval(1)代表关闭第一个开启的定时器(但是不推荐这样用),最好还是通过变量赋值的标识来关闭定时器

  ```js
  var timer=setInterval(function(){
      console.log(123);
  },1000);
  clearInterval(timer);//因为一运行就直接关闭了.所以不能打印出来
  ```

  ```js
  var num=1;
  var timer=setInterval(function(){
  console.log(123);
  if(num==11){
  clearInterval(timer)
  }
  },1000);
  //当num=11的时候关闭定时器不会再打印了
  ```

 

### 14.2 延时调用

- **希望一段程序不立刻执行，而是隔一段时间以后再执行，可以通过延时调用函数setTimeout()，并且延时调用只执行一次**,该函数的参数和用法和setInterval()一样

  ```js
  var timer=setTimeout(function(){
      console.log(123);
  },1000);
  //1S后打印一次123,并且只会打印一次
  ```

- **通过clearTimeout()函数可以关闭一个通过setTimeout()函数开启定时器,**该函数中需要一个定时器的标识作为参数，这样将关闭对应标识对应的定时器,关闭方法同clearInterval()

  ```js
  var timer=setTimeout(function(){
      console.log(123);
  },1000);
  clearTimeout(timer);//直接关闭定时器
  ```

  

**注意:**所有定时器中函数的异步操作都是放在同步操作的最后进行执行



**延时调用和定时调用的异同**

- 定时调用会执行多次，而延时调用只会执行一次
- 延时调用和定时调用的语法是相同的，并且在时间上是可以相互代替的，在开发中可根据自己需要去选择



### 14.3 运动动画

在JS中可以通过定时器的效果来实现动画效果,动画效果实质上时一帧一帧的图片效果拼接而来,当每秒钟的动画帧数超过60帧时,人眼就分辨不出来图片和动画,以此来实现动画效果

**关于动画的函数**

- requestAnimationFrame()函数(该函数不支持低版本IE浏览器),该函数的内部可以接收一个回调函数,在每一帧的动画结束后该函数就是自动执行内部的回调函数,以此达到动画效果式的移动,该函数的返回值也是一个代表数值类型的ID值

  ```js
  //兼容代码
  window.requestAnimationFrame =window.requestAnimationFrame ||
      function(cb) {
        return setTimeout(cb, 1000 / 60);
      };
  ```

  **注:**可以说该方法也是递归调用函数,但是事实上该方法对调用函数的次数做了限制,每秒只会调用60次,而且不会无限占据系统资源

- cancelAnimationFrame()函数(该函数不支持低版本IE浏览器),该方法内部需要一个ID值作为参数,可以提前停止requestAnimationFrame()继续调用内部回调函数

  ```js
  //兼容代码
  window.cancelAnimationFrame = window.cancelAnimationFrame || clearTimeout;
  ```

  

**动画效果函数**

- **普通匀速动画效果(不通过数值的加减)**

  ```js
  /*
  参数:
  对象obj
  要改变的属性对象json(该对象内部是要改变的属性和要到达目标的属性值,不带单位,透明度100代表CSS的1)	
  time--毫秒(ms)为单位,默认是1000ms
  callback回调函数(可选)
  */
  function animation(obj, json, time = 1000, callback = function() {}) {
    //兼容代码
    window.requestAnimationFrame =
      window.requestAnimationFrame ||
      function(cb) {
        return setTimeout(cb, 1000 / 60);
      };
  
    var startValue = {}, //开始属性值
      changeValue = {}, //改变属性值
      startTime = new Date(); //起始时间
  
    for (var key in json) {
      var value;
      if (key === "opacity") {
        value = Math.round(parseFloat(getStyle(obj, key)) * 100);
      } else {
        value = parseFloat(getStyle(obj, key));
      }
      startValue[key] = isNaN(value) ? 0 : value;//如果原来没有写值原来的值默认是0
      changeValue[key] = parseFloat(json[key]) - startValue[key]; //改变值
    }
  
    run(); //执行动画的函数
    function run() {
      var nowTime = new Date() - startTime;
      var timescale = nowTime / time; //现在所用时间在总时间的比例
  
      if (timescale >= 1) {
        timescale = 1;
      } else {
        requestAnimationFrame(run);
      }
        
      for (key in changeValue) {
        var value = timescale * changeValue[key] + startValue[key]; //每个时刻的目标值
        if (key === "opacity") {
          obj.style.filter = "alpha(opacity:" + value + ")";
          obj.style.opacity = value / 100;
        } else {
          obj.style[key] = value + "px";
        }
      }
      if (timescale === 1) {
        callback();
      }
    }
  }
  
  //获取对象属性
  function getStyle(obj, name) {
    if (window.getComputedStyle) {
      return getComputedStyle(obj, null)[name];
    } else {
      return obj.currentStyle[name];
    }
  }
  
  
  //例子
  var div=document.getElementById("box");//width:100px  height:100px  backgournd:red
  animation(div,{width:200,height:200},2000,function(){
      div.style.display="none";
  });
  ```

- **通过Tween.js实现的变速动画效果**

  ```js
  /*
  参数:
  对象obj
  
  要改变的属性对象json,该属性装有两个对象,data对象和option对象,data对象装的是内部要改变的属性和要到达目标的属性值,不带单位,透明度100代表CSS的1),option对象装的是运动方式easing属性和运动状态speed属性
  
  time--毫秒(ms)为单位,默认是1000ms
  callback回调函数
  */
  function animation(obj, json, time = 1000, callback = function() {}) {
    //兼容代码
    window.requestAnimationFrame =
      window.requestAnimationFrame ||
      function(cb) {
        return setTimeout(cb, 1000 / 60);
      };
   
    //控制速度的Tween.js
    var Tween = {
      Linear: {
        easeIn: function(t, b, c, d) {
          return (c * t) / d + b;
        }
      },
      Quad: {
        easeIn: function(t, b, c, d) {
          return c * (t /= d) * t + b;
        },
        easeOut: function(t, b, c, d) {
          return -c * (t /= d) * (t - 2) + b;
        },
        easeInOut: function(t, b, c, d) {
          if ((t /= d / 2) < 1) return (c / 2) * t * t + b;
          return (-c / 2) * (--t * (t - 2) - 1) + b;
        }
      },
      Cubic: {
        easeIn: function(t, b, c, d) {
          return c * (t /= d) * t * t + b;
        },
        easeOut: function(t, b, c, d) {
          return c * ((t = t / d - 1) * t * t + 1) + b;
        },
        easeInOut: function(t, b, c, d) {
          if ((t /= d / 2) < 1) return (c / 2) * t * t * t + b;
          return (c / 2) * ((t -= 2) * t * t + 2) + b;
        }
      },
      Quart: {
        easeIn: function(t, b, c, d) {
          return c * (t /= d) * t * t * t + b;
        },
        easeOut: function(t, b, c, d) {
          return -c * ((t = t / d - 1) * t * t * t - 1) + b;
        },
        easeInOut: function(t, b, c, d) {
          if ((t /= d / 2) < 1) return (c / 2) * t * t * t * t + b;
          return (-c / 2) * ((t -= 2) * t * t * t - 2) + b;
        }
      },
      Quint: {
        easeIn: function(t, b, c, d) {
          return c * (t /= d) * t * t * t * t + b;
        },
        easeOut: function(t, b, c, d) {
          return c * ((t = t / d - 1) * t * t * t * t + 1) + b;
        },
        easeInOut: function(t, b, c, d) {
          if ((t /= d / 2) < 1) return (c / 2) * t * t * t * t * t + b;
          return (c / 2) * ((t -= 2) * t * t * t * t + 2) + b;
        }
      },
      Sine: {
        easeIn: function(t, b, c, d) {
          return -c * Math.cos((t / d) * (Math.PI / 2)) + c + b;
        },
        easeOut: function(t, b, c, d) {
          return c * Math.sin((t / d) * (Math.PI / 2)) + b;
        },
        easeInOut: function(t, b, c, d) {
          return (-c / 2) * (Math.cos((Math.PI * t) / d) - 1) + b;
        }
      },
      Expo: {
        easeIn: function(t, b, c, d) {
          return t == 0 ? b : c * Math.pow(2, 10 * (t / d - 1)) + b;
        },
        easeOut: function(t, b, c, d) {
          return t == d ? b + c : c * (-Math.pow(2, (-10 * t) / d) + 1) + b;
        },
        easeInOut: function(t, b, c, d) {
          if (t == 0) return b;
          if (t == d) return b + c;
          if ((t /= d / 2) < 1) return (c / 2) * Math.pow(2, 10 * (t - 1)) + b;
          return (c / 2) * (-Math.pow(2, -10 * --t) + 2) + b;
        }
      },
      Circ: {
        easeIn: function(t, b, c, d) {
          return -c * (Math.sqrt(1 - (t /= d) * t) - 1) + b;
        },
        easeOut: function(t, b, c, d) {
          return c * Math.sqrt(1 - (t = t / d - 1) * t) + b;
        },
        easeInOut: function(t, b, c, d) {
          if ((t /= d / 2) < 1) return (-c / 2) * (Math.sqrt(1 - t * t) - 1) + b;
          return (c / 2) * (Math.sqrt(1 - (t -= 2) * t) + 1) + b;
        }
      },
      Elastic: {
        easeIn: function(t, b, c, d, a, p) {
          var s;
          if (t == 0) return b;
          if ((t /= d) == 1) return b + c;
          if (typeof p == "undefined") p = d * 0.3;
          if (!a || a < Math.abs(c)) {
            s = p / 4;
            a = c;
          } else {
            s = (p / (2 * Math.PI)) * Math.asin(c / a);
          }
          return (
            -(
              a *
              Math.pow(2, 10 * (t -= 1)) *
              Math.sin(((t * d - s) * (2 * Math.PI)) / p)
            ) + b
          );
        },
        easeOut: function(t, b, c, d, a, p) {
          var s;
          if (t == 0) return b;
          if ((t /= d) == 1) return b + c;
          if (typeof p == "undefined") p = d * 0.3;
          if (!a || a < Math.abs(c)) {
            a = c;
            s = p / 4;
          } else {
            s = (p / (2 * Math.PI)) * Math.asin(c / a);
          }
          return (
            a *
              Math.pow(2, -10 * t) *
              Math.sin(((t * d - s) * (2 * Math.PI)) / p) +
            c +
            b
          );
        },
        easeInOut: function(t, b, c, d, a, p) {
          var s;
          if (t == 0) return b;
          if ((t /= d / 2) == 2) return b + c;
          if (typeof p == "undefined") p = d * (0.3 * 1.5);
          if (!a || a < Math.abs(c)) {
            a = c;
            s = p / 4;
          } else {
            s = (p / (2 * Math.PI)) * Math.asin(c / a);
          }
          if (t < 1)
            return (
              -0.5 *
                (a *
                  Math.pow(2, 10 * (t -= 1)) *
                  Math.sin(((t * d - s) * (2 * Math.PI)) / p)) +
              b
            );
          return (
            a *
              Math.pow(2, -10 * (t -= 1)) *
              Math.sin(((t * d - s) * (2 * Math.PI)) / p) *
              0.5 +
            c +
            b
          );
        }
      },
      Back: {
        easeIn: function(t, b, c, d, s) {
          if (typeof s == "undefined") s = 1.70158;
          return c * (t /= d) * t * ((s + 1) * t - s) + b;
        },
        easeOut: function(t, b, c, d, s) {
          if (typeof s == "undefined") s = 1.70158;
          return c * ((t = t / d - 1) * t * ((s + 1) * t + s) + 1) + b;
        },
        easeInOut: function(t, b, c, d, s) {
          if (typeof s == "undefined") s = 1.70158;
          if ((t /= d / 2) < 1)
            return (c / 2) * (t * t * (((s *= 1.525) + 1) * t - s)) + b;
          return (c / 2) * ((t -= 2) * t * (((s *= 1.525) + 1) * t + s) + 2) + b;
        }
      },
      Bounce: {
        easeIn: function(t, b, c, d) {
          return c - Tween.Bounce.easeOut(d - t, 0, c, d) + b;
        },
        easeOut: function(t, b, c, d) {
          if ((t /= d) < 1 / 2.75) {
            return c * (7.5625 * t * t) + b;
          } else if (t < 2 / 2.75) {
            return c * (7.5625 * (t -= 1.5 / 2.75) * t + 0.75) + b;
          } else if (t < 2.5 / 2.75) {
            return c * (7.5625 * (t -= 2.25 / 2.75) * t + 0.9375) + b;
          } else {
            return c * (7.5625 * (t -= 2.625 / 2.75) * t + 0.984375) + b;
          }
        },
        easeInOut: function(t, b, c, d) {
          if (t < d / 2) {
            return Tween.Bounce.easeIn(t * 2, 0, c, d) * 0.5 + b;
          } else {
            return Tween.Bounce.easeOut(t * 2 - d, 0, c, d) * 0.5 + c * 0.5 + b;
          }
        }
      }
    };
      
      
    var startValue = {}, //开始属性值
      changeValue = {}, //改变属性值
      startTime = new Date(); //起始时间
  
    var option = json.option;//获取option对象的值
    var dataValue = json.data;//获取data对象的值
  
    for (var key in dataValue) {
      var value;
      if (key === "opacity") {
        value = Math.round(parseFloat(getStyle(obj, key)) * 100);
      } else {
        value = parseFloat(getStyle(obj, key));
      }
      startValue[key] = isNaN(value) ? 0 : value;
      changeValue[key] = parseFloat(dataValue[key]) - startValue[key]; //改变值
    }
  
    var speed = option && option.speed; //控制的是速度的形式,不是具体快慢
    var easing = option && option.easing;
    var speedArray = ["easeIn", "easeOut", "easeInOut"];//装有运动状态的数组
  
    if (typeof option === "object") {//判断option是否有值
      if ("easing" in option) {//option有值时是否改写了easing属性
        speed = speed || 0;//默认是第一种运动状态
        //匀速是强制速度是第一种形式
        if (easing.toLowerCase() === "linear") {
          speed = 0;
          easing = "Linear";
        }
      } else {
        //写了option,但是是空对象
        speed = 0;
        easing = "Linear";
      }
    } else {
      //没有写option,默认情况
      speed = 0;
      easing = "Linear";
    }
  
    run(); //执行动画的函数
    function run() {
      var nowTime = new Date() - startTime; //当前时间
   
      for (key in changeValue) {
          ///下方是Tween函数的自动用法
        var value = Tween[easing][speedArray[speed]](
          nowTime,
          startValue[key],
          changeValue[key],
          time
        );
          //判断是否到达指定时间
        if (time - nowTime <= 0) {
          value = Math.min(value, dataValue[key]);
          value = Math.max(value, dataValue[key]);
           //上方代码是为了让目标值一直都是指定值,减小误差
        }
  
        if (key === "opacity") {
          obj.style.filter = "alpha(opacity:" + value + ")";
          obj.style.opacity = value / 100;
        } else {
          obj.style[key] = value + "px";
        }
      }
        //到达目标值执行回调函数
      if (time - nowTime <= 0) {
        callback();
      } else {
        requestAnimationFrame(run);
      }
    }
  }
  
  //获取对象属性
  function getStyle(obj, name) {
    if (window.getComputedStyle) {
      return getComputedStyle(obj, null)[name];
    } else {
      return obj.currentStyle[name];
    }
  }
  
  
  //例子
  var div=document.getElementById("box");//width:100px  height:100px  backgournd:red
  animation(div,{data:{width:200,height:200},option:{easing:"Bounce",speed:1}},2000,
            function(){
      			div.style.display="none";
  			});
  ```

  

## 15.Class

**class用法跟let和const一样，不存在变量提升,也不能重复声明类名,JS中的类(class)是在ES6中被推出为关键字,实际上也是通过原型构成的模拟类**

ES5面对对象写法和传统的面向对象语言(如C++和JAVA)差异很大,很容易让新学习这门语言的人感到困惑。所以在ES6中提供了更接近传统语言的写法,引入了class这个概念,作为对象的模板,通过class关键字,可以定义类。

ES6中的class可以看作只是一个语法糖,它的大部分功能ES5都可以做到,新的class写法只是让对象原型的写法更加清晰,更像面向对象编程的语法而已

### 15.1 用法

```js
//ES5写法
function Person(name,age,gender){
	this.name=name;
	this.age=age;
	this.gender=gender;
}
Person.prototype.sayName=function(){
	alert("hello"+this.name);
}


//ES6写法
class Person{
    constructor(name,age,gender){
        this.name=name;
		this.age=age;
		this.gender=gender;
    }
    
    sayName(){
        alert("hello"+this.name);
    }//其实就是ES6中函数的简写,但是必须用这种简写形式创建方法
}
// 表达式
const Person = class{
      constructor(name,age,gender){
        this.name=name;
		this.age=age;
		this.gender=gender;
    }
    
    sayName(){
        alert("hello"+this.name);
    }
}
```

**注意:**

- 我们通常只会在原型中放方法,在ES5的语法中虽然在原型中可以放普通对象,但是其实不推荐这样做,所以在用class声明的类中只允许在constructor()函数中放入对象,而在constructor()函数外只能放方法

- 构造函数的prototype属性在ES6的class中依然存在,事实上,class中定义的所有方法全部都作为prototype属性的方法保存

  ```js
  class Fun{
      construct(){
          
      }
      
      say(){
     		     
      }
      
      add(){
          
      }
  }
  //等同于
  function Fun(){
      construct(){
   		       
      },
      say(){
              
      },
      add(){
                  
      }
  }
  ```

- 如果不传入参数,constructor()函数可以不用写,但是在解析的时候会自动为这个类加上该函数方法




### 15.2 静态方法

类相对于实例的原型,所有在类中定义的方法,都会被实例继承。如果在一个方法前,加上**static**关键字,就表示该方法**不会被实例继承,只能通过类本身来调用,这就是静态方法**

**注意:**ES6中规定class内部只有静态方法,并没有静态属性,也就是不能设置静态属性,如果要设置静态属性只能在外面手动设置,**ES7中已经可以使用设置静态属性了,通过static当作变量声明符使用,同时实例属性也可以直接写在constructor函数外**

```js
class Person{
    constructor(name,age,gender){
        this.name=name;
		this.age=age;
		this.gender=gender;
    }
    //静态属性
    static id=1;
    //实例属性
 	id=2;
    static say(){
        alert("hello");
    }
}
Person.say();//"hello"
Person.age=18;
console.log(Person.age);//18
```



### 15.3 继承

在class中通过**extends**关键字进行类的继承

```js
class Person{
    constructor(name,age,gender){
        this.name=name;
		this.age=age;
		this.gender=gender;
    }
    
    sayName(){
        alert("hello"+this.name);
    }
}

class Studnet extends Person{
    constructor(name,age,gender,score){
        super(name,age,gender)//必须调用super()方法,不然新建对象时会报错
        this.score=score;
    }
}

let student=new Student("孙悟空",18,"男",100);
student.sayName();
```

**注意:**

- **子类必须在constructor()方法中调用super()方法,否则新建实例时会报错。**这是因为子类没有自己的this对象,而是继承父类的this对象,然后对其进行加工。如果不调用super()方法,子类就不能得到this对象
- **子类会将父类的静态方法与静态属性也一起继承,通过子类同样也能用父类的静态方法**



### 15.4 super

**super**关键字既可以当作函数使用,又可以直接当作对象调用,这两种情况下的super的用法完全不同

- **super作为函数使用时,代表调用父类的构造函数。**并且在子类的构造函数中必须执行一次super()方法

  **注意:**

  - **super当作函数使用时只能用在子类构造函数中,如果用在其它地方就会报错**
  - 父类可以不写constructor()方法,因为在调用的时候会自动加上该函数,但是子类必须写该方法,因为super()方法必须写在constructor()方法中,而super()方法在继承中又是必写的,不写子类就不具备this,从而无法返回对象出来

  ```js
  class A{
      constructor(){
          
      }
  }
  
  class B extends A{
      constructor(){
          super();
      }
  }
  
  /*
  子类B的构造函数之中的super()虽然代表的是父类A的构造函数,但是返回的确实子类B的实例,即super()内部的
  this是指向B,所以super()相当于A.prototype.constructor.call(this)
  */
  ```

- **super当作对象使用时,在普通实例方法中指向父类的原型对象,在静态方法中指向父类**

  ```js
  class A{
      constructor(){
          say(){
              return "hello";
          }
      }
  }
  
  class B extends A{
      constructor(){
          super();
          console.log(super.say());//"hello"
      }
  }
  
  /*
  在创建实例对象时就会在控制台打印"hello",子类B中的super.say()就是将super当作一个对象进行使用,此时的super在普通实例方法中指向的是A.prototype,super.say()就相当于A.prototype.say()
  */
  ```

  **由于this是指向子类的,所以如果通过super对某个属性进行赋值,这时的super就是this,赋值的属性就会变成子类实例的属性**

  ```js
  class A{
      constructor(){
          this.x=1;
      }
  }
  
  class B extends A{
      constructor(){
          super();
      	console.log(this.x);//1
          this.x=2;
          console.log(this.x);//2
          super.x=3;
          console.log(super.x)//undefined，由于此时的super的指向是A.prototype
          console.log(this.x)//3
      }
  }
  
  /*
  super.x=3等同于this.x=3,而读取super.x的时候此时super的指向是A.prototype,也就是
  A.prototype.x,因为没有设置实例属性,所以此时的值为undefined
  */
  ```




### 15.5 new

`new`关键字除了能够作为创建实例对象的关键字外，**在ES6中还可以在类的内部使用`new.target`来获取到当前实例到的类**

```js
class Parent {
    constructor () {
        // 实例的时候打印出Parent类本身
        console.log(new.target)
    }
}
```

如果是子类继承的父类，在实例的时候不会打印父类而是子类，正如概念，会获取到当前实例的类

```js
class Parent {
    constructor () {
        // 打印Child类
        console.log(new.target)
    }
}
class Child extends Parent{
}
const c = new Child()
```



## 16.Set

**ES6中提供了数据结构Set,该对象类似于数组,但是所有的值都是唯一的,在其中不会有重复的值,Set本身就是一个构造函数,用来生成Set类的实例对象**

**注意:**与数组和一般对象不同的是,Set实例没有属性名(键名),或者说属性名(键名)和属性值(键值)都是相同的值

### 16.1 用法

```js
var s=new Set();
[1,2,3,4,1,2].forEach(function(value){
    s.add(value);//Set类的对象只能通过add方法添加值
})
console.log(s);//Set(4){1,2,3,4}
```

```js
//在Set()中可以传入一个数组作为参数来作为Set实例的初始化对象
var s=new Set([1,2,3,4,1,2]);
/*该写法类似于
	[1,2,3,4,1,2].forEach(function(value){
    s.add(value);
})
*/

console.log(s);////Set(4){1,2,3,4}
console.log(s.size);//Set类有一个实例属性size来计算Set实例对象的长度
console.log([...s]);//[1,2,3,4]
/*
	上面的写法可以用作数组的去重
*/
```

**注意:**

- **在向Set实例中添加值的时候不会进行类型转换**,如:123与"123"不同
- **两个对象总是不同**
- **Set内部的判断两个值是否相同的算法结果类似全等运算符(===),不同在于在Set内部认为NaN与自身相等,所以也会去重**



### 16.2 属性与方法

#### 16.2.1 size

**Set类的实例有一个size属性,专门用来记录Set实例中的值的数量,返回Set实例成员的数量**



#### 16.2.2 add,delete,has与clear方法

- add()方法为Set实例添加一个值,返回实例对象本身,可以用作链式操作
- delete()方法删除Set实例的某个值,删除成功返回true,删除失败返回false
- has()方法判断Set实例中是否有某个值.有就返回true,没有返回false
- clear()方法情况Set实例中的所有值,没有返回值

```js
var s=new Set([1,2,3,4,5]);
s.add(6);
console.log(s);//Set(6){1,2,3,4,5,6}

s.delete(1);
console.log(s);//Set(5){2,3,4,5,6}

console.log(s.has(2));//true

s.clear();
console.log(s);//Set(0)
```



#### 16.2.3 keys,values与entries方法

keys()方法返回装有Set实例的键名的数组，values()方法返回装有Set实例的键名值的数组，entries()方法返回装有Set实例的键名和键值的数组,但是由于在Set实例中只有键值,所有三个方法返回的数组都是相同的,都只装有键值

```js
var s=new Set([1,2,3,4,5]);
console.log(s.keys());//[1,2,3,4,5]
console.log(s.values());//[1,2,3,4,5]
console.log(s.entries());//[1,2,3,4,5]
```



#### 16.2.4 forEach方法

**Set实例的forEach()方法的用法与表现结果与数组的forEach()方法的表现结果一致**,第一个参数是回调函数,第二个参数是回调函数中this的指向对象

```js
var s=new Set([1,2,3,4,5]);
s.forEach(function(value,index){
    console.log(index,value);//1,1 2,2 3,3 4,4 5,5 
})
```



### 16.3 WeakSet

WeakSet也是一个类,可以用new WeakSet()来创建一个WeakSet实例对象,该对象的结构和Set实例相似,但是还是有所区别

**区别:**

- **WeakSet的成员只能是对象,而不能是其他类型的值**
- **WeakSet实例中的成员对象都是弱引用,**即垃圾回收机制不会考虐WeakSet实例中对该对象的引用,如果其它对象没有引用该对象,垃圾回收机制会自动回收该对象所占的内存,不会考虐WeakSet实例中是否还保留着对该对象的引用,由于WeakSet实例的弱引用这个特性,所以ES6中有规定**WeakSet创建的实例不可被遍历**,因为WeakSet中的成员对象的引用随时会消失

```js
var w=new WeakSet([[1,2,3],{a:1,b:2,c:3}]);
console.log(w);
```

**方法**

WeakSet实例有add(),delete()和has()方法,用法同Set,但是没有遍历WeakSet实例本身的方法,因为该类实例没有size属性



## 17.Map

**ES6提供Map数据结构,该对象类似于普通对象,也是键值对的集合,但是该对象的键不像普通对象那样只能用字符串作为键,而是可以使用各种类型的值(包括对象)来作为键**

JS中的对象本质上是键值对的集合(Hash结构),但是传统上只能用字符串当作键,这就使得在使用JS对象的时候有很大的限制。也就是说，Object结构提供的是字符串与值相对应的键值对模式.而Map结构提供的是值与值相对应的键值对模式,相对来说更加的完善

### 17.1 用法

```js
var m=new Map();
var o={a:1};
m.set(o,1);
/*
只能通过set()方法来设置值，最好通过变量来储存键,以便于获取,因为如果不通过变量获取就不是对同一个对象的引
用,会返回undefiend
*/
console.log(m.get({a:1}));//undefined
console.log(m.get(o));//1
console.log(m.o);//undefined
console.log(m[o]);//undefined
/*
	只能通过get()方法来获取到对应键的内容,不能通过点或[]来获取,因为该结构不是普通对象的结构,通过这两种	  方式只能拿到普通对象键对应的值
*/
```

```js
//也可以传入普通的数据类型
var m=new Map([
    ["name","孙悟空"],
	[{age:18},18]
])
console.log(m.get("name"));//"孙悟空"
```

**注意:**

- 在直接通过结构赋值创建对象时,传入的参数需要是一个数组,同时数组中的每一个成员需要用一个二维数组括起来作为键值对
- 如果Map实例的键是一个简单类型的值,则只要两个值**严格相等**,Map内部就会将其视为一个值,比如0和-0依然是一个值,而1与"1"就不是一个值。NaN虽然不等于自身,但是Map内部将其视为同一个值



### 17.2 属性与方法

#### 17.2.1 size

**Map类的实例有一个size属性,专门用来记录Set实例中的值的数量,返会Map实例成员的数量**



#### 17.2.2 get,delete,has与clear方法

- get()方法为Map实例添加一个值,传入两个参数,分别代表键和值,返回实例对象本身,可以用作链式操作
- delete()方法删除Map实例的某个值,删除成功返回true,删除失败返回false
- has()方法判断Map实例中是否有某个值.有就返回true,没有返回false
- clear()方法情况Map实例中的所有值,没有返回值

```js
var m=new Map();
m.set("a",1);
m.set("b",2);

console.log(m);//Map(2) {"a" => 1, "b" => 2}
console.log(m.get("a"));//1

m.delete("a");
console.log(m.get("a"));//undefined

console.log(m.has("b"));//true

m.clear();
console.log(m);//Map(0){}
```



#### 17.2.3 keys,values与entries方法

keys()方法返回装有Map实例的键名的数组，values()方法返回装有Map实例的键名值的数组，entries()方法返回装有Map实例的键名和键值的数组,这些方法与Set实例不同,都能得到不同的数组



#### 17.2.4 forEach方法

**Map实例的forEach()方法的用法与表现结果与数组的forEach()方法的表现结果一致**,第一个参数是回调函数,第二个参数是回调函数中this的指向对象

```js
var m=new Map([["a",1],["b",2],["c",3]]);
m.forEach(function(value,key){
    console.log(key,value)//a 1 b 2 c 3
})
```

**注意:**Map遍历的顺序就是插入顺序,遍历行为基本与Set一致



### 17.3 类型转换

#### 17.3.1 与数组转换

- **Map转数组**

  ```js
  var m = new Map([["a", 1], ["b", 2], ["c", 3]]);
  console.log([...m]);//[Array(2), Array(2), Array(2)]
  /*
  	结果为数组中包含三个二维数组,每个二维数组分别存有一个Map对象的成员
  */
  ```

- **数组转Map**

  ```js
  var m=new Map([["a",1],["b",2],["c",3]]);
  console.log(m);//Map(3) {"a" => 1, "b" => 2, "c" => 3}
  ```

  

#### 17.3.2 与对象转换

- **Map转对象**

  **注:**只有Map实例中所有的键都是字符串才能转为对象

  ```js
  var m=new Map([["a",1],["b",2],["c",3]]);
  var obj={};
  for(var [key,value] of m){
      obj[key]=value;
  }
  console.log(obj);//{a: 1, b: 2, c: 3}
  ```

- **对象转Map**

  ```js
  var m=new Map();
  var obj={a:1,b:2,c:3};
  /*
  for(var key in obj){
      m.set(key,obj[k]);
  }
  由于for...in的性能原因,改用for...of
  
  */
  for(var key of Object.keys(obj)){
      m.set(key,obj[key]);
  }
  
  console.log(m);//Map(3) {"a" => 1, "b" => 2, "c" => 3}
  ```

  

#### 17.3.3 与JSON转换

- **Map转JSON**

  Map转为JSON时分两种情况,如果Map的简明全是字符串就转换为JSON对象,如果键名有非字符串,就转换为JSON数组

  - **Map=>JSON对象**

    ```js
    var m=new Map([["a",1],["b",2],["c",3]]);
    var obj={};
    for(var [key,value] of m){
        obj[key]=value;
    }
    var json=JSON.stringify(obj);//实质上时先转为对象再转为JSON
    console.log(json);
    ```

  - **Map=>JSON数组**

    ```js
    var m=new Map([["a",1],["b",2],[{c:3},3]]);
    var json=JSON.stringify([...m]);//还是先转换为数组在转换为JSON
    console.log(json);
    ```

    

- **JSON转Map**

  JSON转换为Map时,正常情况下所有的键名都会转为字符串，无论是不是对象,但是如果JSON整个是一个数组,且每一个数组成员本身都是一个二维数组,每个二维数组里面有两个成员,这样就能一一对应转换为Map。这往往是数组转为JSON的逆操作

  - **一般情况**

    ```js
    var json = '{"a":1,"b":2,"c":3}';
    var obj = JSON.parse(json);
    var m = new Map();
    for (var key of Object.keys(obj)) {
       m.set(key, obj[key]);
    }
    console.log(m);//Map(3) {"a" => 1, "b" => 2, "c" => 3}
    ```

  - **特殊情况**

    ```js
    var json='[[{"a":1},1],["b",2],["c",3]]';
    var m=new Map(JSON.parse(json));
    console.log(m);//Map(3) {{…} => 1, "b" => 2, "c" => 3}
    ```

    

### 17.4 WeakMap

WeakMap和Map的结果类似,也是一个类,也是用于生成键值对的集合,但是与Map依然有一些区别

**区别:**

- WeakMap实例只接收对象作为键名(null除外),不能使用其它类型的值作为键名使用
- WeakMap的键名指向的对象是弱引用,不会计入垃圾回收机制,同WeakSet一致

**注:**当我们想在某个对象上存储一些数据时,会形成对该对象的引用,从而让这个对象无法被回收,WeakMap就是为了解决这个问题而诞生的,它的键名都是弱引用,即垃圾回收机制不会将该对象的引用保留在内

```js
var div = document.getElementsByTagName("div");
var w = new WeakMap();
w.set(div, "这是div");
console.log(w.get(div)); //"这是div"

div = null;//清除引用对象
console.log(w.get(div)); //undefined
```

**方法**

WeakMap实例有get(),set(),has()和delete()方法,用法同Map,也无法被遍历,也无法被清空,所以没有size属性与clear()方法



**应用场景**

```js
var element=document.getElementById("element");
var w=new WeakMap();
w.set(element,{num:1});
element.onclick=function(){
    var data=w.get(element);
    data.num++;
};
/*
	element为一个DOM节点,每当发生点击事件的时候,就更新内部属性的值,但是确是将新值作为键值放在了		WeakMap中,键名为element,当element这个DOM节点被删除后WeakMap中的值会自动消失,没有了内存泄露的		风险
*/
```



## 18.Proxy

**Proxy用于修改某些操作的默认行为,等同于在语言层面作出修改,所以属于一种"元编程",即对编程语言进行编程。**Proxy可以理解为在目标对象之前设置一层拦截,外界对该对象的访问,都必须先通过这层拦截,因此提供了一种机制.可以对外界的访问进行过滤和改写。proxy这个词的愿意是**代理**,用在这里表示它来代理某些操作,可以看作是**代理器**

**Proxy实际上是重载了点运算符,用自己的定义覆盖了原始定义,而Proxy实际上也是一个类,通过构造函数的方式创建一个Proxy实例,该构造函数中有两个参数.一个是要拦截的目标对象,另一个参数也是一个对象,用来定制拦截的行为**

```js
var p=new Proxy(target,handle);
```

```js
var obj = { a: 1, b: 2, c: 3 };
var p = new Proxy(obj, {
    get:function(target, key, receiver) {
        console.log(target);
        console.log(key);
        console.log(receiver);
        return Reflect.get(target, key, receiver);
        //return target[key];下面的代码同上方
    },
    set:function(target,key,value,receiver){
		Reflect.set(target, key, value, receiver);
    	//target[key]=value;下面的代码同上方
}
});
console.log(p.a);//1
/*
{a: 1, b: 2, c: 3}
a
Proxy {a: 1, b: 2, c: 3}
*/
p.a=4;
console.log(p.a);//4
/*
{a: 4, b: 2, c: 3}
a
Proxy {a: 4, b: 2, c: 3}
*/
```

**注意:**

- 改变Proxy实例内对象的值的时候会自动执行内部的set()方法,也能够改变传入参数对象的值,但是如果通过原对象访问的属性只会返回对应对象的值,如果通过Proxy实例访问对象属性,则会执行内部的get()方法

  ```js
  var obj = { a: 1, b: 2, c: 3 };
  var p = new Proxy(obj, {
      get:function(target, key, receiver) {
          return "hello"
      },
      set:function(target,key,value,receiver){
  		Reflect.set(target, key, value, receiver);
  }
  });
  p.a=4;
  console.log(obj.a);//4
  console.log(p.a);//"hello"
  console.log(p.b);//"hello"
  console.log(p.c);//"hello"
  ```

- 如果Proxy构造函数的第二个参数是一个空对象,那么就没有任何拦截效果,访问proxy实例就等同于访问target

  ```js
  var obj = { a: 1, b: 2, c: 3 };
  var p = new Proxy(obj, {});
  p.a=4;
  console.log(obj.a);//4
  conosole.log(p.a);//4
  ```


**注:**Proxy实例还可以设置很多方法,在这里值将set()与get()的用法



## 19.Reflect

**Reflect不是一个构造函数,而是一个直接调用的对象,内部装有一些ES6中操作对象的API,里面的方法与Proxy一一对应**

**Reflect的用处**

- 将Object对象的一些明显语言内部的方法,如`Object.defineProperty`等放在Reflect对象上,现在虽然在Object对象与Reflect对象中调用这些方法都可以实现,但是在以后会慢慢全部转移到Reflect对象上

- 修改某些Object对象中的方法的返回结果,让其变得更加合理,如:

  ```js
  /*
  	比如同时定义属性的方法,在无法定义属性时,两者的表现结果完全不同
  */
  Object.defineProperty(obj,name,value);//报错
  Reflect.defineProperty(obj,name,value);//返回false
  ```

- 使得对象的操作都变成函数的形式,一些对对象的操作都是命令式的,比如in与delete操作符来对对象操作

  ```js
  var obj={a:1,b:2,c:3};
  console.log("a" in obj);
  console.log(Reflect.has(obj,"a"));//代替上式
  delete obj.a;
  Reflect.deleteProperty(obj,"a");//代替上式
  ```

- Reflect对象的方法与Proxy一一对应,只要是Proxy对象的方法,就能在Reflect对象中找到对应的方法这就让Proxy对象可以方便的调用Reflect方法,完成默认行为,作为修改行为的基础

  

## 20.Promise

Promise对象是对于JS中的异步操作的结果方案,可以说Promise是一个容器,在这里面保存了某个未来才会结束的事件(通常为一个异步操作)的结果

**Promise对象代表一个异步操作,有着三种状态:**

- pending,正在进行状态
- fulfilled,已成功状态
- rejected,已失败状态

**注意:**只有异步操作的结果可以决定当前是哪一种状态,任何其它的操作都无法改变该状态

### 20.1 优缺点

- **优点**

  Promise的状态不会受到外界的影响,并且一旦Promise的状态发生改变时,就不会再进行状态变化,任何时刻都可以得到这个状态结果

- **缺点**
  - 无法取消Promise,一旦新建就会立即执行,无法在中途取消
  - 如果不设置回调函数,Promise的内部机制会报错,不会反应到外部
  - 当处于pending正在执行的状态时,不能知道当前在哪一个阶段,不知道是否是开始还是结束



### 20.2 用法

Promise可以作为一个构造函数对象,传入的两个参数都是函数,第一个函数是代表成功,第二个代表失败,每个函数调用后都会在then()中找到自己对于的函数来执行

**then中函数的返回值**

当一个`Promise`完成（fulfilled）或者失败（rejected），返回函数将被异步调用（由当前的线程循环来调度完成）。具体的返回值依据以下规则返回：

- 如果then中的回调函数返回一个值，那么then返回的Promise将会成为接受状态，并且将返回的值作为接受状态的回调函数的参数值。
- 如果then中的回调函数没有返回值，那么then返回的Promise将会成为接受状态，并且该接受状态的回调函数的参数值为 undefined。
- 如果then中的回调函数抛出一个错误，那么then返回的Promise将会成为拒绝状态，并且将抛出的错误作为拒绝状态的回调函数的参数值。
- 如果then中的回调函数返回一个已经是接受状态的Promise，那么then返回的Promise也会成为接受状态，并且将那个Promise的接受状态的回调函数的参数值作为该被返回的Promise的接受状态回调函数的参数值。
- 如果then中的回调函数返回一个已经是拒绝状态的Promise，那么then返回的Promise也会成为拒绝状态，并且将那个Promise的拒绝状态的回调函数的参数值作为该被返回的Promise的拒绝状态回调函数的参数值。
- 如果then中的回调函数返回一个未定状态（pending）的Promise，那么then返回Promise的状态也是未定的，并且它的终态与那个Promise的终态相同；同时，它变为终态时调用的回调函数参数与那个Promise变为终态时的回调函数的参数是相同的。

```js
var p=new Promise(function(success,rejected){
    setTimeout(function(){
        try{
            console.log(123);
            success(456);
        }catch{
            rejected("error");//在一次操作中之后进行success()函数和rejected()函数中的一个,执行
        }					//完毕后直接到下一个步骤
    },1000)
}).then(function(data){//执行success()后进入该函数
    console.log(data);//456
},function(err){//执行rejected()后进入该函数
    console.log(err);//error
})
/*
then()函数中的第一个回调函数中的参数就是在前一个异步操作中通过success()传的值,而第二个回调函数中的参
数是在前一个异步操作中rejected()传的值
*/
```

```js
//Promise正确的用法是实现链式操作来实现异步
var p=new Promise(function(success,rejected){
    console.log(123);
    success(456);
}).then(function(data){//假设现在只有成功才执行函数
    console.log(data);//456
    return 789;
    /*
    如果这里不写返回值,默认会返回undefined给下面then()中成功时的函数,注意的是这样写无论写什么普通的数据类型都是成功的,都是为下面的then()函数传递了成功时的Promise
    */
}).then(function(data){
    console.log(data);//789
})
```

```js
//链式操作中失败的时候可以执行的异步操作
var p=new Promise(function(success,rejected){
    try{
    	console.log(123);
    	success(456);    
    }catch{
        rejected("error");
    }
}).then(
    function(data){
    
    console.log(data);//456
    return new Promise(function(success,rejected){
        	try{
            	success(789)
        	}catch{
        	    rejected("error");
        	}
    	});
},
    function(err){
    console.log(err);//error
	//失败后不写返回值也会默认返回undefined被下面的then的成功函数接住
}).then(
    function(data){
    console.log(data);//789
    return new Promise(function(success,rejected){
         	 rejected("error");//直接显示失败
      	});
	},
    function(err){
    console.log(err);//error
}).catch(
    function(err){
    	console.log(err);//error
	})
/*
可以在最后加上catch()代表着只有在失败时才会进行的方法,该方法可以看作是then()失败时的简写,因为then()
要写失败时的情况必须要传入两个参数，第二个才是失败时的参数函数,用catch()可以值捕获失败时的结果
*/
```

**注意：**

- Promise 对象的错误具有`冒泡`性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个`catch`语句捕获

- `Promise`的rejected必须要有catch或者后面有then的第二个参数来接收，即使什么都不做，但是必须要接收到，不然会报错

- 如果想要在中途断掉promise链，可以返回一个`new Promise(()=>{})`不定义任何状态，这样promise链会一直处于暂停状态

  

### 20.3 静态方法

#### 20.3.1 Promise.all()

**Promise.all()方法用于将多个Promise实例包装成一个新的Promise实例**

```js
var p1=new Promise(function(){});
var p2=new Promise(function(){});
var p3=new Promise(function(){});
var p=Promise.all([p1,p2,p3]).then(function(data){
	console.log(data);
},function(err){
    console.log(err);
});
/*
	p内部的状态由p1,p2,p3决定,有两种情况:
	1.只有p1,p2,p3全部成功时p的状态才能使成功,p才能够执行后面成功的回调函数,此时data的值是由			  p1,p2,p3共同传入的值组成的数组
	2.只要p1,p2,p3中有一个失败,p的状态就会变成失败,p执行失败后的回调函数,此时err的值为首先完成异步并	   返回失败的Promise的返回值
*/
```

**注意:**

- Promise,all()中包含多个Promise实例的数组,只有这多个实例全部成功,或者至少有一个失败时才会调用后面then()中的回调函数
- **如果作为参数的Promise实例自己定义了catch()方法,那么该实例一旦失败不会触发Promise.all()后面定义的catch()方法,如果没有实例定义catch()方法,那么触发失败后会执行Promise.all()后面的catch()方法**	



#### 20.3.2 Promise.race()

**Promise.race()方法也用于将多个Promise实例包装成一个新的Promise实例**

```js
var p1=new Promise(function(){});
var p2=new Promise(function(){});
var p3=new Promise(function(){});
var p=Promise.race([p1,p2,p3]).then(function(data){
	console.log(data);
},function(err){
    console.log(err);
});
/*
	p内部的状态由p1,p2,p3其中一个决定,只有其中有一个先完成异步操作,p的状态就会跟着发生改变,如果先发生	   成功则执行第一个回调函数,如果先发生失败则执行第二个回调函数
*/
```



#### 20.3.3 Promise.resolve()

**Promise.resolve()方法用于将现有的对象转换为Promise对象,并且该对象中的值就是成功时的函数传入的参数**

```js
var p=Promise.resolve(123);
//等价于
var p=new Promise(success,rejected){
    success(123);
}
```

**该方法的参数有多种情况**

- **参数为一个Promise实例**

  不作任何改变,直接返回这个Promise实例

- **参数为一个具有then()方法的thenable对象**

  Prommise.resolve()方法会将这个对象转换为Promise对象,然后立即执行里面的then()方法

  ```js
  var thenable={
      then:function(success,rejected){
          console.log(123);//123
          success(456)
      }
  };
  var p = Promise.resolve(thenable).then(function(data) {
          console.log(data);//456
  });
  /*
  	上面的代码会在控制台上打印出来
  */
  ```

- **参数为一个普通值类型的或者不是thenable对象时**

  会直接将传入的参数作为成功时的参数传入到then()的成功函数的参数中

- **没有参数**

  直接得到一个成功时不传入参数的Promise对象



#### 20.3.4 Promise.reject()

**Promise.reject()方法也用于将现有的对象转换为Promise对象,并且该对象中的值就是失败时的函数传入的参数**

**注意:**Promise.reject()方法中传入的参数会原封不动的作为失败时的错误理由,不会发生参数的改变,和Promise.resolve()方法有区别

```js
var p=Promise.reject("error");
//等同于
var p=new Promise(function(success,rejected){
	rejected("error");
})
```



### 20.4 同步回调与异步回调

**回调函数的分类：**

- 同步回调：立即执行，完全执行完成才会结束，不会进入到回调队列中。如：数组遍历的相关函数、Promise的excutor函数
- 异步回调：不会立即执行，会放入回调队列中将来执行。如：定时器回调、ajax回调、Promise成功或失败的回调



## 21.Iterator

**Iterator(迭代器)是一种接口,或者说是一种机制。它能为各种不同的数据结构提供统一的访问机制,任何数据结构只要部署Iterator接口,就可以完成遍历操作(即依次处理该数据结构的所有成员)**

**作用**

- 为各种数据结构，提供一个统一的、简便的访问接口
- 使得数据结构的成员能够按某种次序排列
- 供for...of语句使用



**Iterator本质上是一个指针对象**

**指针实现过程**

1. **创建一个指针对象，指向当前数据结构的起始位置**
2. **第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员**
3. **第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员**
4. **不断调用指针对象的`next`方法，直到它指向数据结构的结束位置**



**一些数据结构原生就具有Iterator接口,如果没有就必须设置Iterator接口才能使用**

- **普通函数实现Iterator**

  ```js
  function myIter(obj){
    var i = 0;
    return {
      next(){
        var done = (i>=obj.length);
        var value = !done ? obj[i++] : undefined;
        return {
          value,
          done,
        }
      }
    }
  }
  ```

- **具有Iterator接口的原生数据结构**
  - Array
  - Map
  - Set
  - String
  - 函数的 arguments 对象
  - NodeList 对象

  ```js
  //数组的Symbol.iterator方法
  var arr = ['a', 'b', 'c'];
  var iter = arr[Symbol.iterator]();
  //通过next()方法实现每一次的迭代器的遍历
  iter.next() // { value: 'a', done: false }
  iter.next() // { value: 'b', done: false }
  iter.next() // { value: 'c', done: false }
  iter.next() // { value: undefined, done: true }
  /*
  	value是每次遍历到的值,done代表是否将该数组遍历完全
  */
  ```

- **类数组调用数组的Symbol.iterator方法**

  ```js
  var iterable = {
    0: 'a',
    1: 'b',
    2: 'c',
    length: 3,
    [Symbol.iterator]: Array.prototype[Symbol.iterator]
  };
  //for...of语句实际上就是对数组使用next()方法直到将数组遍历完全
  for (let item of iterable) {
    console.log(item); // 'a', 'b', 'c'
  }
  ```

  **注意:**

  - 普通对象部署数组的Symbol.iterator方法没有任何效果,必须是要属性值为数值,迭代器内部实际上是通过属性名的自增来实现迭代的

    ```js
    var iterable = {
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

  - 字符串虽然是一个类数组的对象，也具有Iterator原生接口

    ```js
    var someString = "hi";
    typeof someString[Symbol.iterator]
    // "function"
    
    var iterator = someString[Symbol.iterator]();
    
    iterator.next()  // { value: "h", done: false }
    iterator.next()  // { value: "i", done: false }
    iterator.next()  // { value: undefined, done: true }
    ```



## 22.Generator

**Generator函数是ES6提供的一种异步编程解决方案，语法行为与传统的普通函数完全不同。**执行Generator函数会返回一个遍历器对象。也就是说,Generator函数还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历Generator函数内部的每一个状态

**Generator函数跟普通函数的区别**

- function关键字与函数名之间有一个星号
- 函数体内部使用yield表达式，定义不同的内部状态
- Generator函数不能使用new关键字,否则会报错

### 22.1 用法

```js
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
console.log(hw);
/*
	上面是一个Generator函数调用后该函数并不会运行,也不会返回函数的运行结果,而是返回的是一个遍历器对	 象,内部的yield表达式为一个个阶段,所以该函数一共有三个阶段状态:hello,world,与retrun语句结束执行	 状态
*/
console.log(hw.next());//{value: "hello", done: false}
console.log(hw.next());//{value: "world", done: false}
console.log(hw.next());//{value: "ending", done: true}
console.log(hw.next());//{value: undefined, done: true}
/*
	如果想要运行函数内部的每一个函数,就必须要调用next()方法,使得指向函数的指针移向下一个状态,每次调用	 next()方法时,内部指针就会从函数头部或上一层停下来的地方开始执行,直到运到下一个yiedl表达式或遇到		return语句(注意遇到return语句函数会直接停止),在停止后依然能调用next()方法,但是此时的返回值为		undefined
*/
```

**总之,Generator函数是分段执行的,yield表达式是暂停执行的标记,而next()方法可以恢复执行**



**Genterator函数的写法**

ES6并没有规定function关键字与函数名之间的星号应该写在哪个位置,所以可以用多种方法书写

```js
function * gen(x, y){}
function *gen(x, y){}
function* gen(x, y){}///推荐使用这种形式声明Genterator函数
function*gen(x, y){}
```



**作为对象方法的Generator函数写法**

```js
let obj = {
  * gen() {
  }
};
```



**与 Iterator 接口的关系** 

**由于Generator函数就是遍历器生成函数，因此可以把 Generator 赋值给对象的Symbol.iterator属性，从而使得该对象具有Iterator接口**

```js
Object.prototype[Symbol.iterator] = function* (){
  for(let i in this){
    yield this[i];
  }
}

function* iterEntries(obj) {
  let keys = Object.keys(obj);
  for (let i=0; i < keys.length; i++) {
    let key = keys[i];
    yield [key, obj[key]];
  }
}

let myObj = { gen: 3, bar: 7 };

for (let [key, value] of iterEntries(myObj)) {
  console.log(key, value);
}
```



### 22.2 yield

**由于Generator函数返回的遍历器对象,只有调用next()方法才会遍历下一个内部状态,所以提供了一种可以暂停执行的函数,yield表达式就是暂停标志**

**注意：**

- yield表达式只能用在Generator函数里面，用在其他地方都会报错。

- 将yield表达式用在另一个表达式之中，必须放在圆括号里面。

```js
console.log('Hello' + (yield 123));
//通过此种形式调用yield表达式,但是此时作为值的并不是123,而是下一次next()中传入的参数
```



**调用next()方法的运用逻辑**

1. 遇到yield表达式，就暂停执行后面的操作，并**将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值**
2. 下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式
3. **如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将`return`语句后面的表达式的值，作为返回的对象的value属性值**
4. 如果该函数没有return语句，则返回的对象的value属性值为undefined



**yield与renturn的异同**

- **相同点**

  都能返回紧跟在语句后面的那个表达式的value属性值

- **不同点**

  每次遇到yield，函数暂停执行，下一次再从该位置继续向后执行，而return语句不具备位置记忆的功能。一个函数里面，只能执行一次return语句，但是可以执行多次yield表达式。正常函数只能返回一个值，因为只能执行一次return,而Generator 函数可以返回一系列的值，因为可以有任意多个yield



### 22.3 Generator的方法

#### 22.3.1 next方法的参数

**yield表达式本身没有返回值，或者说总是返回undefined。next()方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值(注意这里的返回值是在外部返回给函数内部的)**

```js
function* f() {
  for(var i = 0; true; i++) {
    var reset = yield i;
    if(reset) { 
        i = -1; 
    }
  }
}

var g = f();

g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next(true) // { value: 0, done: false }
```

**Generator函数从暂停状态到恢复运行，它的上下文状态是不变的。通过next方法的参数，就可以在Generator函数开始运行之后，继续向函数体内部注入值**

```js
function* gen(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = gen(5);
console.log(a.next()); // Object{value:6, done:false}
console.log(a.next()); // Object{value:NaN, done:false}
console.log(a.next()); // Object{value:NaN, done:true}

var b = geno(5);
console.log(b.next()); // { value:6, done:false }
console.log(b.next(12));  // { value:8, done:false }
console.log(b.next(13));  // { value:42, done:true }
```



#### 22.3.2 return方法

**Generator函数返回的遍历器对象，还有一个return方法，可以返回给定的值，并且立刻结束遍历Generator函数**

```js
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

console.log(g.next());// { value: 1, done: false }
console.log(g.return('foo'))// { value: "foo", done: true }
console.log(g.next());// { value: undefined, done: true }
```

## 

### 22.4 for...of循环

**for...of循环可以自动遍历Generator函数时生成的Iterator对象，且此时不再需要调用next方法**

```js
function *gen() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of gen()) {
  console.log(v);
}
// 1 2 3 4 5
```

```js
function* gen() {
  let [prev, curr] = [1, 1];
  while(true){
    [prev, curr] = [curr, prev + curr];
    yield curr;
  }
}

for (let n of gen()) {
  if (n > 10000000) break;
  console.log(n);
}

```



### 22.5 yield* 

**如果在Generator函数内部，调用另一个Generator函数，默认情况下是没有效果的。这个时候需要用到yield*表达式来在一个Generator函数内部执行另外一个Generator函数**

```js
function* gen() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  gen();
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// "x"
// "y"
//中间的gen()函数并没有被执行
```

```js
function* gen() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  yield* gen();
  yield 'y';
}

//等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}

//等同于
function* bar() {
  yield 'x';
  for (let v of gen()) {
    yield v;
  }
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// "x"
// "a"
// "b"
// "y"
```



## 23.async

为了使得异步操作更加的简便,ES8标准引入了async函数,async函数是Generator函数的语法糖,基本是模仿Generator函数并作出了一些改变

**async与Generator的区别**

- 有了内置执行器,Generator函数的执行必须靠执行器,而async函数自带执行器。也就是说,**async函数的执行，与普通函数一样,只要一行就能执行所以过程**
- async的语义比Generator的语义更加的清楚,**命名与使用async函数需要用到asyuc和await关键字,**相对于Generator函数的*和yield更加让人理解,async表示函数内部有异步操作,而await表示在await后面的表达式需要等待结果
- yeild后面可以是任何数据类型,而正常情况下,**await后面是一个Promise对象,如果并没有手动设置一个Promise对象,而后面的表达式会被转成一个立即成功(await后面上面都不写也会传undefined)的Promise对象**
- **async后的所有能返回值的结果都是Promise**

**所以,async函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而await就是内部then()方法的语法糖**



**async与Generator函数的应用对比**

- Generator

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

- async

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



**注意:**

- **如果await后面的异步操作出错，那么等同于async函数返回的Promise对象直接失败,并且会将错误对象作为参数传递给then()方法的第二个函数或者catch()方法回调函数中**

  ```js
  /* 这种方法是只用运行函数后在函数层面进行捕捉错误 */
  async function f() {
    await new Promise(function (success, rejected) {
      throw new Error('error');
    });
  }
  
  f().then(function(value){
      console.log(value)
  }).catch(function(err){
      console.log(err)//Error:error
  });
  ```

  ```js
  //用try...catch来捕捉
    async function f() {
        try {
            await new Promise(function(success, rejected) {
                throw new Error("error");
            });
        } catch (err) {
            console.log(err);
        }
        return await "hello world";
    }
  f().then(function(data) {
      console.log(data);
  });
  ```

  ```js
  //如果有多个await命令，可以统一放在try...catch结构中。
  async function main() {
    try {
      var val1 = await firstStep();
      var val2 = await secondStep(val1);
      var val3 = await thirdStep(val1, val2);
  
      console.log('Final: ', val3);
    }
    catch (err) {
      console.error(err);
    }
  }
  ```

- **如果要在async函数中实现异步操作（不是一直await最后变成了同步操作），需要配合Promise.all()配合使用**

  ```js
  var fn = function (time) {
    return new Promise(function (resolve, reject) {
      setTimeout(function () {
        resolve(time);
      }, time);
    })
  };
  
  var start = async function () {
    console.log('start');
    const [res,res2,res3] = await Promise.all([fn(3000),fn(500),fn(1000)]) //传入一个数组最后依次返回 
    console.log(res,res2,res3) // 3000 500 1000
    console.log('end');
    return [res,res2,res3]
  };
  
  // async函数默认最外层返回值嵌套的是一个Pmomise对象，直接return后的值会传入then后成功的函数中
  start().then(res=>{
      console.log(res) // 3000 500 1000
  }).catch(err=>{
      console.log(err)
  })
  ```

  **如果要使用循环的async的异步操作，可以直接传递一个数组给Promise.all()**

  ```js
  import {getOneInfo} from '../api/getInfo'
  const items = [{id:0},{id:1}.{id:2}]
  async fetchAll(items) {
      try {
          let arr = []
          items.forEach(val => {
              arr.push(getOneInfo(val.id))
          })
          const results = await Promise.all(arr)
          //判断是否全部获取成功
          if (results.every(val => val.status === 200)) {
              // 这步会改变itmes里面元素的值，因为是引用类型
              items.forEach((value, index) => {
                  value.name = results[index].data.name //name在这里假设就是索引值
              })
              return items
          } else {
              throw new Error("error");
          }
      } catch (err) {
          console.log(err)
      }
  }
  fetchAll(items).then(res=>{
      console.log(res) // [{id:0,name:0},{id:1,name:1},{id:2,name:2}]
  }).catch(err){
      console.log(err) // error
  }
  ```

  

## 24.Module

模块功能主要由两个命令构成

- **export命令用于规定模块的对外接口**
- **import命令用于输入其他模块提供的功能**

**注意**:

- ES6的模块自动采用严格模式,不管有没有在模块头部加上"use strict"

- export与import命令可以出现在模块的任何位置,只要处于模块顶层作用域就可以,如果处于块级作用域内等就会报错

### 24.1 export

**一个模块就是一个独立的文件,该文件内部的所有变量外部都无法获取。如果希望外部能够读取模块内部的某个变量,就必须使用export命令输出该变量**

**具体操作:**

- **export实际上时导出一个接口,让外界能通过该接口访问内部的数据**,所以export会将要导出的变量装在一个对象中,通过传递出这个对象来使用导出文件内部的变量,所以在导出的时候有格式要求

  - **导出变量**

  ```js
  //profile.js
  //第一种写法
  export var firstName = 'Michael';
  export var lastName = 'Jackson';
  export var year = 1958;
  
  //第二种写法(推荐)
  var firstName = 'Michael';
  var lastName = 'Jackson';
  var year = 1958;
  
  export {firstName, lastName, year};//因为实际上时导出对象,所以用{}包裹
  
  /*
  	以下是错误方法
  */
  // 报错
  export 1;
  
  // 报错
  var m = 1;
  export m;
  
  /*
  export不能直接导出一个数据或者一个变量,必须要用对象包装起来
  */
  ```

  - **导出函数**

  ```js
  export function multiply(x, y) {
    return x * y;
  };
  //当然也可以通过传入对象一起传递
  function multiply(x,y){
      return x * y;
  }
  export {multiply}
  ```

- **通常情况下,export导出的变量就是本来的名字,如果想要修改导出后变量的名字,可以使用as关键字重命名**

  ```js
  function v1() { ... }
  function v2() { ... }
  
  export {
    v1 as streamV1,
    v2 as streamV2,
    v2 as streamLatestVersion
  };
   //这样在外界使用时就使用as后面被重新修改后的名字
  ```

- **export语句输出的接口,与其对应的值是动态绑定关系,即通过该接口,可以取到模块内部实时的值**

  ```js
  export var gender = 'boy';
  setTimeout(function(){
      gender = 'girl';
  }, 1000);
  //1s后内部的值发生改变,外界引用的也会发生改变
  ```



### 24.2 import

**使用export命令定义了模块的对外接口以后，其他 JS文件就可以通过import命令加载并使用这个模块**

**具体操作:**

- **使用import命令导入文件中需要的变量时也需要通过一个对象导入,并且必须与被导入摸版的对外接口(变量名)的名称一致**

  ```js
  // main.js
  import { firstName } from 'my_module';
  import { lastName } from 'my_module';
  import { year } from 'my_module';
  //等同于
  import {firstName, lastName, year} from './profile';
  
  console.log(firstName,lastName,year);
  ```

  **注:**如果导入的是JS文件,后缀名可省略

- **如果想为输入的变量重新取一个名字,也是用as关键字将输入的变量重命名**

  ```js
  import { lastName as surname } from './profile';
  ```

- **import命令具有提升效果,会让导入的变量提升到整个模块的头部首先执行(但是不推荐这样做)**

```js
multiply(10,20);

import { multiply } from 'my_module';
```

- **由于import是静态执行,所以不能使用表达式和变量这些只有在运行时才能得到结果的语法结构**

```js
// 报错
import { 'mult' + 'iply' } from 'my_module';

// 报错
let module = 'my_module';
import { multiply } from module;

// 报错
if (x === 1) {
  import { multiply } from 'module1';
} else {
  import { multiply } from 'module2';
}
```



**import引入的模块可以进行整体加载**

**用星号*可以指定一个对象**,所有输出值都加载在这个对象上面,使用整体加载时一般都是**与as关键词一起用**来方便操作

**注意:**模块整体加载所在的那个对象,不允许在运行时发生改变,如修改属性值等

```js
import * as person from './profile';

// 不允许以下操作
person.firstName = "import";
pero.son.lastName = "export"; 
```



#### 24.2.1 动态import

在新的ES提案中添加了import函数的动态导入支持，同node的require一样使用`import()`函数就能动态的导出要导出的文件

```js
const status = 1
if(status){
    import('./a')
}else{
    import('./b')
}
```



### 24.3 export default

**使用import命令的时候,需要知道所要加载的变量名或函数名,否则无法加载。为了不用阅读文档就能加载模块,需要用export default命令,为模块指定默认输出**

- **导出**

```js
// export-default.js
//可以直接用于导出一个匿名函数
export default function () {
  console.log('foo');
}

//也可以导出一个非匿名函数
export default function foo() {
  console.log('foo');
}
//或
function foo() {
  console.log('foo');
}

export default foo;
/*
有名函数的函数名在模块外部是无效的,加载的时候,视同匿名函数加载
*/
```

**注：**　直接//export testdata;//err 这种方式是会报错的，因为不能直接导出变量，需要用一个接口来继承变量然后导出，比如 `export var firstName = 'Michael'`，其中`firstname`就是接口

- **导入**

**其他模块加载该模块时,import命令可以为该匿名函数指定任意名字**

```js
// import-default.js
import customName from './export-default';
customName(); // 'foo'
```

**注:**这时import命令后面不用通过对象的包装,而是可以直接写入



**原理解释**

- **因为一个模块只能有一个默认输出,所以export default命令只能使用一次。这正对应了import命令后面才不用加大括号被对象包装,因为只可能唯一对应export default命令**

- **本质上,export default就是输出一个叫做default的变量或函数,然后系统外界为它取任意名字,所以:**

  ```js
  // add.js
  function add(x, y) {
    return x * y;
  }
  export {add as default};
  // 等同于
  export default add;
  
  ```

  ```js
  //main.js
  import { default as add } from 'add';
  // 等同于
  import foo from 'add';
  ```

- **正是因为export default命令其实只是输出一个叫做default的变量,所以它后面不能跟变量声明语句**

  ```js
  // 正确
  export var a = 1;
  
  // 正确
  var a = 1;
  export default a;//实际是变量的二次赋值
  
  // 正确
  export default 1;
  
  // 错误
  export default var a = 1;
  
  ```



### 24.4 复合写法

**如果在一个模块之中，先输入后输出同一个模块，import语句可以与export语句写在一起**

```js
export { foo, bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';
export { foo, bar };
```

**模块的接口改名和整体输出**

```js
// 接口改名
export { foo as myFoo } from 'my_module';

// 整体输出
export * from 'my_module';
```



## 25.Error对象

### 25.1 错误类型

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

  

### 25.2 错误处理

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



### 25.3 错误对象

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






# dart

## 1.安装与使用

### 1.1 安装

**因为学习 dart 大多数是为了写 flutter，所以推荐直接下载 flutter，下载的 flutter 中会带有 dart 的 SDK**。

flutter 推荐去官网进行[下载](https://flutter.dev/docs/development/tools/sdk/releases)。下载完成后解压，dart 的 SDK 就在`解压目录\bin\cache\dart-sdk\`下。



### 1.2 在 vscode 中使用

为了方便使用，我们可以将 dart 的 SDK 设置在环境变量中，将`解压目录\bin\cache\dart-sdk\bin`的完整路径设置好，在cmd 中输入 dart ，有响应就代表设置成功了。

然后就是如何在 vscode 中使用dart。为了使用 dart，我需要下载两个插件`Dart`和`Code Runner`，下载完成后创建一个文件`main.dart`，输入如下代码：

```dart
// dart中的代码需要放入main方法中执行
main(){
    print('Hello World');
}
```

然后右键`Run Code`,如果控制台成功打印出`Hello World`证明我们已经能够在 vscode 中使用 dart 了。



## 2.类型声明

### 2.1 变量声明

在 dart 中有很多声明变量的关键字，可以使用能接受任何类型值的变量申明（类似 JavaScript），也可以使用只能接受固定类型值的变量声明（类似 JAVA）。

#### 2.1.1 var

类似于JavaScript中的`var`，它可以接收任何类型的变量，但最大的不同是 dart 中`var`变量一旦在声明时被赋值（除了被赋值为 null，因为初始化的时候所有的值都为 null），类型便会确定，则不能再改变其类型，如：

```dart
var t = "hi world";
// 下面代码在dart中会报错，因为变量t的类型已经确定为String
// 类型一旦确定后则不能再更改其类型
t = 1000;
```

**注：**对于前端人员来说，其实看作是 TypeScript 的自动推断类型的功能就好。

但是如果一开始没有直接赋值，而是只定义，那么变量的类型默认会是`dynamic`类型，也就说和 JavaScript 中的声明的变量一样的用法了。

```dart
var t;
t = "hi world";
// 下面代码在dart中不会报错
t = 1000;
```



#### 2.1.2 const 和 final

**如果从未打算更改一个变量，那么使用 `final` 或 `const`，不是`var`，也不是一个单独的类型声明**（类型声明也是可以更改值的，当使用类型声明定义变量时，可以直接在前面加上`const`或`final`关键字使其变成常量，但是我们一般建议省略类型声明）。

**使用`const`和`final`声明的变量都只能被设置一次**，两者区别在于：

**`const`常量是一个编译时常量（就是说必须要是一个在程序编译时就完全固定的常量），`final`常量不仅有`const`的编译时常量的特性，最重要的是它是运行时常量，`final`是惰性初始化的，即在第一次使用时才会初始化。**

```dart
// 可以省略String这个类型声明
final str = "hi world";
final String sstr = "hi world"; 
const str1 = "hi world";
const String sstr1 = "hi world";

// 运行时常量在运行时才会被赋值
// 获取当前时间，因为是动态获取的，所以不能通过const声明
final t = new DateTime.now(); // OK
const t1 = new DateTime.now(); // Error
```

**注意：**

- 虽然 final 是运行时常量，第一次被赋值也必须是在定义的时候赋值。

  ```dart
  final a; // Error
  a = 1;
  ```

- 实例变量可以是 final，但不能是 const。（实例变量定义在对象一级，它可以被类中的任何方法或者其他类中的方法访问，但是不能被静态方法访问）

  ```dart
  class A {}
  main() {
      final a = new A(); // OK
      const b = new A(); // Error 
  }
  ```

- const 关键字不只是声明常量变量。还可以使用它来创建常量值，以及声明创建常量值的构造函数。任何变量都可以赋一个常量值。

  ```dart
  var foo = const [];
  final bar = const [];
  
  // 可以从const声明的初始化表达式中省略const
  const baz = []; // Equivalent to `const []` => const bar = const [];
  // 不能改变const变量的值
  baz = [42]; // Error: Constant variables can't be assigned a value.
  // 可以更改一个非final的非const变量的值，即使它曾经有一个const值
  foo = [1, 2, 3]; // Was const []
  ```

- 有些类提供常量构造函数。要使用常量构造函数创建编译时常量，请将 const 关键字放在构造函数名之前：

  ```dart
  class Person{
      const Person();
  }
  var p = const Person();
  ```



#### 2.1.3 dynamic 和 Object

- `Object` 是 dart 所有对象的根基类，也就是说所有类型都是`Object`的子类（包括`Function`和`Null`），所以任何类型的数据都可以赋值给`Object`声明的对象。
- `dynamic`是与`int`这样一样的类型关键词，改类型声明的变量也可以赋值任意对象。

**`dynamic`与`Object`相同之处在于，它们声明的变量可以在后期改变赋值类型（类似使用 JavaScript 或 TypeScript 中的`any`类型）。**

```dart
dynamic t;
Object x;
t = "hi world";
x = 'Hello Object';
// 下面代码没有问题
t = 1000;
x = 1000;
```

`dynamic`与`Object`不同的是，`dynamic`声明的对象编译器会提供所有可能的组合（也就是相当于就是完完全全的 JavaScript变量），而`Object`声明的对象只能使用 Object 类的属性与方法，否则编译器会报错。

```dart
dynamic a;
Object b;
main() {
    a = "";
    b = "";
    printLengths();
}   

printLengths() {
    // no warning
    print(a.length);
    // warning:
    // The getter 'length' is not defined for the class 'Object'
    print(b.length);
}
```



#### 2.1.4 默认值

未初始化的变量的初始值为 null。甚至具有数字类型的变量最初也是 null，因为在 dart 中所有的东西都是对象。

```dart
int lineCount;
assert(lineCount == null);
```

**注意：**在生产环境中，`assert()`调用被忽略。在开发环境中当`assert(condition)`的 condition 条件不为真时抛出一个异常。



### 2.2 内建类型

**我们要清楚的是，dart 中的所有类型的值全都是对象，所以在其他语言中常用的基本类型声明在 dart 中实质也是类的类型声明。**

#### 2.2.1 Number

**Number 总共能使用三种类型：**

- **num**
- **int**
- **double**

**Dart的数字有两种形式：**

- **int**：根据平台的不同，整数值不大于64位。在 Dart VM 上，值可以从`-263`到`263 - 1`。编译成 JavaScript 的 Dart 使用 JavaScript 代码，允许值从`-253`到`253 - 1`。

  整数是没有小数点的数。这里有一些定义整数字面量的例子：

  ```dart
  int x = 1;
  int hex = 0xDEADBEEF;
  ```

  int 类型指定传统的`(<<， >>)和(&)，或(|)`位操作符。例如：

  ```dart
  assert((3 << 1) == 6); // 0011 << 1 == 0110
  assert((3 >> 1) == 1); // 0011 >> 1 == 0001
  assert((3 | 4) == 7); // 0011 | 0100 == 0111
  ```

- **double**：64位(双精度)浮点数，由IEEE 754标准指定。

  如果一个数字包含一个小数，它就是一个双精度数。这里有一些定义双精字面量的例子：

  ```dart
  double y = 1.1;
  double exponents = 1.42e5;
  ```

**注：**int 和 double 都是 num 的子类型。num 类型包括基本的操作符，如`+、-、/和*`，还可以在其中找到`abs()、ceil()和floor()`等方法。(位运算符，如`>>`，在int类中定义)如果 num 及其子类型没有要查找的内容，那么`dart:math library`可能会有。

**以下是如何将字符串转换成数字的方法，反之亦然：**

```dart
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
```



#### 2.2.2 String

dart 字符串是 UTF-16 编码单元的序列。可以使用单引号或双引号创建一个字符串：

```dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```

可以使用`${expression}`将**表达式的值**放入字符串中。如果表达式是**一个标识符**，可以跳过`{}`。为了获得与对象对应的字符串，dart 会自动调用对象的toString()方法。

```dart
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' ==
       'Dart has string interpolation, ' +
       'which is very handy.');
assert('That deserves all caps. ' +
       '${s.toUpperCase()} is very handy!' ==
       'That deserves all caps. ' +
       'STRING INTERPOLATION is very handy!');       
```

**注意：**==检验两个对象是否相等。如果两个字符串包含相同序列的代码单元，那么它们是等价的，这点与 JavaScript 类似。

可以使用**相邻的字符串字面量（也可以看做是用空格）**或 **+** 运算符连接字符串：

```dart
var s1 = 'String ' 'concatenation' " works even over line breaks.";
assert(s1 == 'String concatenation works even over ' 'line breaks.');
var s2 = 'The + operator ' + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
```

**对于创建多行字符串的方法：**

- 使用`\n`用做换行。

- 使用带有单引号或双引号的三重引号。

```dart
var s = 'a \n multi-line string'

var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```

如果不想要转义字符你可以用`r`前缀创建一个**原始字符串**：

```dart
var s = r'In a raw string, not even \n gets special treatment.';
// In a raw string, not even \n gets special treatment.
```

**要注意一点，字符串字面量是编译时常量，只要任何内插表达式都是编译时常量，计算结果为 null 或数值、字符串或布尔值。**

```dart
// These work in a const string.
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// These do NOT work in a const string.
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList'; // Error
/*
前三个错误因为使用var进行申明的，var声明的变量之后可以改变值，不符合常量定义，而后一个是因为不符合常量所需类型，也就是不符合null或数值、字符串或布尔值。即使使用toString()方法也会报错，这不符合编译时常量的定义，除非用final
*/
```



#### 2.2.3 Boolean

**为了表示布尔值，dart 有一个名为 bool 的类型。只有两个对象具有 bool 类型：布尔字面量 true 和 false，它们都是编译时常量。**

dart 的类型安全性意味着不能使用`if(非booleanvalue)`或`assert(非booleanvalue)`之类的代码。相反，显式地检查值，如：

```dart
// Check for an empty string.
var fullName = '';
assert(fullName.isEmpty);

// Check for zero.
var hitPoints = 0;
assert(hitPoints <= 0);

// Check for null.
var unicorn;
assert(unicorn == null);

// Check for NaN.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
```



#### 2.2.4 List

**也许几乎所有编程语言中最常见的集合就是数组或有序对象组。在 dart 中，数组是列表对象，所以大多数人把它们叫做列表。**

dart 列表字面量看起来像 JavaScript 数组字面量。这是一个简单的 dart 列表：

```dart
var list = [1, 2, 3];
```

**注意：**上面的代码分析器推断该列表具有`List<int>`类型。如果试图向此列表添加非整型对象，则分析器或运行时将引发错误。

**可以获取列表的长度，并引用列表元素，就像在JavaScript中那样：**

```dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
```

**要创建一个编译时常量列表（不能再之后改变值），需要在列表字面量之前添加 const（或者直接使用 const 申明变量）：**

```dart
var constantList = const [1, 2, 3];
// OR
const constantList = [1, 2, 3];
// constantList[1] = 1; // Uncommenting this causes an error.
// 这里不会在编译时报错，但是运行时会抛出异常
```

列表类型有许多便于操作列表的方法，在这里不说，在之后会进行详细说明。



#### 2.2.5 Map

通常，map 是一个关联键和值的对象。**键和值都可以是任何类型的对象**。每个键只出现一次，但是您可以多次使用相同的值。dart 对 map 的支持是通过 map 字面量和 map 类型来提供的。(可以看做是混入了 JavaScript 对象字面量写法和 JAVA 的 HashMap 键值的对象)

```dart
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
```

**注意：**

- 在上面的代码中，解析器推断 gifts 的类型为`Map<String, String>`，nobleGases 的类型为`Map<int, String>`。如果您试图向 map 添加错误类型的值，则分析器或运行时将引发错误。

- dart 中的 map 和 JavaScript 中的对象是有区别的，键（key）可以是任意数据类型，并且如果是 String 类型的话不能省略引号，因为在 dart 中这样会将其解析为一个变量。

  ```dart
  const a = '1';
  var map1 = {
      true: '123',
      a: '2', // 不加引号的a会被解析为'1'
      b: '2', // 报错，没有b变量
      'a': '2'
  };
  ```
  
- 同样的，在通过 map 的键获取值得时候，不能使用 JavaScript 中常见的`点（.）`操作符，只能使用`[]`操作符，**`点（.）`操作符只能用在获取 dart 中通过类生成的对象的属性或方法中（因为 map 生成的自变量只是看起来和 JavaScript 中的一样，实际上还是有很大差别的）**。

**同样可以使用Map构造函数创建对象:**

```dart
var gifts = new Map();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = new Map();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```

**添加值和检索值都如同 JavaScript 中那样进行，不同的是：**

- 如果要获取的键不再 map 中，将会返回一个 null：

    ```dart
    var gifts = {'first': 'partridge'};
    assert(gifts['fifth'] == null);
    ```

- 可以使用`.length`获取 map 中元素的个数：

    ```dart
    var gifts = {'first': 'partridge'};
    gifts['fourth'] = 'calling birds';
    assert(gifts.length == 2);
    ```

**要创建一个编译时常量的 map 需要在 map 的字面量前加`const`关键字（或直接使用 const 声明的变量）：**

```dart
var constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
// OR
const constantMap = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

// constantMap[2] = 'Helium'; // Uncommenting this causes an error.
// 这里不会在编译时报错，但是运行时会抛出异常
```



#### 2.2.6 Runes（字符）

**在 dart 中，字符是字符串的UTF-32编码点。**

Unicode 为世界上所有的书写系统中使用的每个字母、数字和符号定义一个唯一的数值。因为 dart 字符串是 UTF-16 代码单元的序列，所以在字符串中表示32位的 Unicode 值需要特殊的语法。

表示 Unicode 码点的常用方法是`\uXXXX`，其中 XXXX 是4位数的十六进制值。例如,心型字符(♥)的编码为`\u2665`。要指定大于或小于4位十六进制数字，请将值放在花括号中。例如笑脸表情(😆)的编码`\u{1f600}`。

**String 类有几个属性可以用来获取 runes信息。codeUnitAt 和 codeUnit 属性返回16位代码单元。使用字符属性获取字符串的字符。**

**下面的示例说明了字符、16位代码单元和32位代码点之间的关系：**

```dart
main() {
  var clapping = '\u{1f600}';
  print(clapping);
  print(clapping.codeUnits);
  print(clapping.runes.toList());

  Runes input = new Runes(
      '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
  print(new String.fromCharCodes(input));
}

//运行效果如下
😀
[55357, 56832]
[128512]
♥  😅  😎  👻  🖖  👍

Process finished with exit code 0
```

**注意：**使用列表操作操作 runes 时要小心。根据特定的语言、字符集和操作，这种方法很容易出错。有关更多信息，请参见[如何在Dart中反转字符串?](https://stackoverflow.com/questions/21521729/how-do-i-reverse-a-string-in-dart)



#### 2.2.7 Symbols（符号）

符号对象表示在 dart 程序中声明的操作符或标识符。一般来说可能永远不需要使用符号，但是对于按名称引用标识符的api 来说，它们是非常重要的，因为缩小改变了标识符名称而不是标识符符号。

要获取标识符的符号，请使用符号文字，符号文字仅为`#`，后面跟着标识符：

```dart
#radix
#bar
```

**注意：**符号常量是编译时常量。



## 3.运算符

dart 定义了下表中显示的操作符。您可以重写其中的许多操作符，如可重写操作符中所述：

| 描述                 | 操作符                                                       |
| :------------------- | :----------------------------------------------------------- |
| 一元后置操作符       | expr++    expr--    ()    []    .    ?.                      |
| 一元前置操作符       | -expr    !expr    ~expr    ++expr    --expr                  |
| 乘除运算             | *    /    %    ~/                                            |
| 加减运算             | +    -                                                       |
| 移位运算             | <<    >>                                                     |
| 按位与               | &                                                            |
| 按位异或             | ^                                                            |
| 按位或               | \|                                                           |
| 关系和类型测试       | >=    >    <=    <    as    is    is!                        |
| 相等                 | ==    ！=                                                    |
| 逻辑与               | &&                                                           |
| 逻辑或               | \|\|                                                         |
| 是否为null           | ??                                                           |
| 天健判断（三元运算） | expr1 ? expr2 : expr3                                        |
| 级联                 | ..                                                           |
| 赋值                 | =    *=    /=    ~/=    %=    +=    -=    <<=    >>=    &=    ^= |

### 3.1 算术运算符

dart 中的算术运算符与其他语言大体是类似的，不过有一点不一致的是，dart 中的除法是符合我们生活中的正常运算的，即使是`int`类型的值也会有小数的结果出现：

```dart
print(3 / 2); // 1.5 not 1
```

如果想要像其他语言一直取整，需要使用`~/`运算符：

```dart
print(3 ~/ 2);// 1
```



### 3.2 相等和关系运算符

**同其它类型语言，略。**



### 3.3 类型测试操作符

**`as`，`is`和`is!`操作符可以方便地在运行时检查类型。**

| 操作符 | 说明                              |
| :----- | :-------------------------------- |
| as     | 形态转换                          |
| is     | 如果对象具有指定的类型，则为True  |
| is!    | 如果对象具有指定的类型，则为False |

- 对于 is 操作符，如果 obj 实现了 T 指定的接口，则`obj is T`为0真。例如，`obj is Object`总是为真。

- 使用 as 操作符将对象转换为特定类型。一般来说，应该将其作为 is 测试的简写形式，以使用该对象的表达式对对象进行测试：

    ```dart
    if (emp is Person) {
        // Type check
        emp.firstName = 'Bob';
    }
    ```
    
    使用as操作符可以使代码更简短:

    ```dart
    (emp as Person).firstName = 'Bob';
    ```

**注意：**上面两个例子的代码不等效。如果 emp 是 null 或不是 Person，那么第一个示例（使用 is）么都不做；第二个（带有 as）抛出异常。



### 3.4 赋值操作符

dart 的赋值操作符和复合赋值操作符也和其他语言的类似，也有一点要注意，若要仅仅为空的变量赋值可以使用`??=`操作符。

```dart
// Assign value to a
a = value;
// Assign value to b if b is null; otherwise, b stays the same
///仅仅在b为空的情况下b被赋值value否则b的值不变
b ??= value;
```



### 3.5 逻辑运算符

**有`!`、`&&`和`||`，同其它类型语言，略。**



### 3.6 位和移位运算

**可以操纵 dart 中的单个数字位。通常，会使用这些位和移位运算符。**

| 运算符 | 说明     |
| :----- | :------- |
| &      | 按位与   |
| \|     | 按位或   |
| ^      | 按位异或 |
| ~expr  | 按位取反 |
| <<     | 左移     |
| >>     | 右移     |

下例是使用位运算符和唯一运算符的示例：

```dart
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value & ~bitmask) == 0x20); // AND NOT
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR
assert((value << 4) == 0x220); // Shift left
assert((value >> 4) == 0x02); // Shift right
```



### 3.7 条件表达式

dart 中有两个运算符，可以精确地计算那些可能需要`if-else`语句的表达式：

- **condition ? expr1 : expr2（三目表达式）：**如果条件为真，则计算 expr1（并返回其值）；否则，计算并返回 expr2 的值。

- **expr1 ?? expr2：**如果 expr1 是非空的，则返回其值；否则，计算并返回 expr2 的值。

当需要基于布尔表达式的结果选择赋值，请考虑使用 ***?:*** 。

```dart
var visibility = isPublic ? 'public' : 'private';
```

如果布尔表达式只想判断值是否为 null，请考虑使用 ***??*** 。

```dart
String playerName(String name) => name ?? 'Guest';
```



### 3.8 级联表示法 （..）

**级联（..）允许在同一个对象上创建一个操作序列。除了函数调用之外，还可以访问同一对象上的字段。这通常可以省去创建临时变量的步骤，能更为流畅的写代码。**

**看下面这段代码：**

```dart
querySelector('#confirm') // Get an object.
  ..text = 'Confirm' // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
```

首先调用`querySelector()`方法返回一个`selector`对象。跟随级联表示法的代码**对这个选择器对象进行操作，忽略可能返回的任何后续值。**

**注意：**在级联的时候，获取的对象的第一个属性或方法开始就需要使用`..`，全程都不能使用单独的`.`。

等价于：

```dart
var button = querySelector('#confirm');
button.text = 'Confirm';
button.classes.add('important');
button.onClick.listen((e) => window.alert('Confirmed!'));
```

**也可以嵌套级联操作。例如：**

```dart
final addressBook = (AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
```

不过，在返回实际对象的函数上构造级联要小心，可能我们需要级联的表达式根本就没有返回给我们正确的对象。例如，以下代码会出错：

```dart
var sb = StringBuffer();
sb.write('foo')
  ..write('bar'); // Error: method 'write' isn't defined for 'void'.
// sb.write()返回结果为void，所以不能再一个void结果上继续构建级联操作。
```

**注意：**严格地说，级联（..）表示法不是运算符。这只是 dart 语法的一部分。



### 3.9 其它运算符（主要是 ?.）

| 运算符 | 名称             | 说明                                                         |
| :----- | :--------------- | :----------------------------------------------------------- |
| ()     | 功能函数         | 表示一个函数调用                                             |
| []     | 访问列表         | 引用列表中指定索引处的值                                     |
| .      | 访问成员         | 表示表达式的属性;例如:foo.bar从表达式foo中选择属性bar        |
| ?.     | 根据条件访问成员 | 和(.)相似，但是左边的操作数可以为空；例如：`foo?.bar`从foo的表达式中选择bar属性，如果foo为空则返回null |

这里说明一下`.`和`?.`：

```dart	
class A {
    var b;
}

void main() {
    /// aobj为A的一个实例
    var aobj = A();
    ///z为空值的变量
    var z=null;
    print(aobj.b);
    print(z?.b); // null
    // 如果是一个方法，则说明都不会发生
    
    print(z.b); // exception
}
```



## 4.函数

**dart 是一种真正的面向对象的语言，所以即使是函数也是对象，并且有一个类型 Function。这意味着函数可以赋值给变量或作为参数传递给其他函数，这是函数式编程的典型特征。**

### 4.1 函数声明

```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

**对于只包含一个表达式的函数，可以使用简写语法（类似 JavaScript 中的箭头函数）：**

```dart
bool isNoble (int atomicNumber)=> _nobleGases [ atomicNumber ] != null ;
```

**注意：**在`箭头(=>)`和`分号(;)`之间只能出现一个表达式(不是语句)。例如，不能在那里放一个`if`语句，但是可以使用一个条件表达式。



### 4.2 函数赋值

- 函数作为变量

  ```dart
  var say = (str){
    print(str);
  };
  say("hi world");
  ```

- 函数作为参数传递

  ```dart
  void execute(var callback) {
      callback();
  }
  execute(() => print("xxx"))
  ```




### 4.3 main 函数

每个应用程序都必须有一个顶级的`main()`函数，它作为应用程序的入口点。`main()`函数返回`void`，并有一个可选的列表参数作为参数。

```dart
void main() {
  querySelector('#sample_text_id')
    ..text = 'Click me!'
    ..onClick.listen(reverseText);
}
```

**下面是一个命令行应用程序的main()函数示例，它接受参数：**

```dart
// Run the app like this: dart args.dart 1 test
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
```

**注：**可以使用`args`库来定义和解析命令行参数。



### 4.4 函数参数

> **不能同时使用可选的位置参数和可选的命名参数**

可选的位置参数和可选的命名参数都能和普通的函数参数一起使用。

#### 4.4.1 可选的位置参数

包装一组函数参数，用`[]`标记为可选的位置参数，并放在参数列表的最后面

```dart
String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```

下面是一个不带可选参数调用这个函数的例子：

```dart
say('Bob', 'Howdy'); //结果是： Bob says Howdy
```

下面是用第三个参数调用这个函数的例子：

```dart
say('Bob', 'Howdy', 'smoke signal'); //结果是：Bob says Howdy with a smoke signal
```



#### 4.4.2 可选的命名参数

定义函数时，使用`{param1, param2, …}`，放在参数列表的最后面，用于指定命名参数。例如：

```dart
//设置[bold]和[hidden]标志
void enableFlags({bool bold, bool hidden}) {
    // ... 
}
```

调用函数时，可以使用`paramName: value`来指定命名参数。例如:

```dart
enableFlags(bold: true, hidden: false);
```

可选命名参数在 flutter 中使用非常多，可以在任何 dart 代码（不仅仅是 flutter）中注释一个已命名的参数，并使用`@required`说明它是一个必传的参数。例如:

```dart
const Scrollbar({Key key, @required Widget child})
```

当构造Scrollbar时，分析器在没有子参数（child）时报错。

**注：**`Required`在元包中被定义。或者直接使用`import package:meta/meta.dart`导入或者导入其他包含 meta 导出的包，例如 flutter 的包:`Flutter /material.dart`。



#### 4.4.3 默然参数值

可以使用 = 来定义命名和位置参数的默认值。**默认值必须是编译时常量**。如果没有提供默认值，则默认值为 null。

**注意：**函数的默认参数值只能写在可选的位置参数（`{}`）和可选的命名参数(`[]`)内。

下面是为命名参数设置默认值的示例：

```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool bold = false, bool hidden = false}) {...}

// bold will be true; hidden will be false.
enableFlags(bold: true);
```

下一个示例展示了如何设置位置参数的默认值：

```dart
String say(String from, String msg,
    [String device = 'carrier pigeon', String mood]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  if (mood != null) {
    result = '$result (in a $mood mood)';
  }
  return result;
}

assert(say('Bob', 'Howdy') ==
    'Bob says Howdy with a carrier pigeon');
    
```

还可以将 list 或 map 集合作为默认值。下面的示例定义一个函数`doStuff()`，该函数指定列表参数的默认列表和礼品参数的默认map集合：

```dart
void doStuff(
    {List<int> list = const [1, 2, 3],
    Map<String, String> gifts = const {
      'first': 'paper',
      'second': 'cotton',
      'third': 'leather'
    }}) {
  print('list:  $list');
  print('gifts: $gifts');
}
```

## 

### 4.5 匿名函数

**大多数函数都被命名，如`main()`或`printElement()`。也可以创建一个没有函数名称的函数，这种函数称为匿名函数，或者 lambda 函数或者闭包函数。** 

不过可以为变量分配一个匿名函数。例如，可以从集合中添加或删除它。

匿名函数看起来类似于命名函数：有0个或者多个参数，在括号之间用逗号和可选类型标注分隔，后面的代码块包含函数的主体。

```dart
([[Type] param1[, …]]) { 
  codeBlock; 
}; 
```

下面的示例定义了一个带有无类型参数 item 的匿名函数。该函数为列表中的每个 item 调用，打印一个字符串，该字符串包含指定索引的值：

```dart
var list = ['apples', 'bananas', 'oranges'];
list.forEach((item) {
  print('${list.indexOf(item)}: $item');
});
```

如果函数只包含一条语句，可以使用箭头函数：

```dart
var list = ['apples', 'bananas', 'oranges'];
list.forEach((item) => print('${list.indexOf(item)}: $item'));
```



### 4.6 函数闭包

dart 中的函数闭包和函数作用域的问题和 JavaScript 中的一致，略。



### 4.7 返回值

所有函数都返回一个值。如果没有指定返回值，则语句返回 null。

```dart
foo() {}
assert(foo() == null);
```

dart 中的函数声明**如果是写在全局没有显式声明**返回值类型时会默认当做`dynamic`处理。

**注意：**

- 写在全局的函数返回值没有类型推断。
- 写在函数内部（包括 main 函数）的函数有类型推断，不写`return`返回值是 Null 类型，如果返回条件有很多种会返回 Object 类型。

```dart
typedef bool CALLBACK();

// 不指定返回类型，此时默认为dynamic，不是bool
isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}

void test(CALLBACK cb) {
  print(cb());
}

main() {
  /*
  // 返回类型能推断出是boll，写在里面下面的代码不会报错
    isNoble(int atomicNumber) {
      return _nobleGases[atomicNumber] != null;
    }
  */
  // 报错，isNoble不是bool类型
  test(isNoble);
}
```



## 5.类

dart 是一种面向对象的语言，具有类和基于 mixin 的继承。每个对象都是一个类的实例，所有的类都是 Object 的子类。基于 mixin 的继承意味着，尽管每个类（除了 Object）都只有一个超类，但类主体可以在多个类层次结构中重用。

### 5.1 类的基本使用

#### 5.1.1 使用类成员

dart 中的类的成员使用与 JavaScript 中的类似，在使用时只有些许的不同。

**使用点号（.）引用实例变量或方法：**

```dart
var p = Point(2, 2);

// Set the value of the instance variable y.
p.y = 3;

// Get the value of y.
assert(p.y == 3);

// Invoke distanceTo() on p.
num distance = p.distanceTo(Point(4, 4));
```

**为避免最左操作数为空时出现异常，使用 `?.`代替`.`来使用：**

```dart
// If p is non-null, set its y value to 4.
p?.y = 4;
```



#### 5.1.2 使用构造函数

可以使用构造函数创建一个对象。构造函数名可以是 ClassName 或 ClassName.identifier（可以看做是类似重载函数的构造函数）。

**注：**在 dart 中 new 关键字为可选关键字。

```dart
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});
```

下面的代码具有相同的效果，但是在构造函数名之前使用可选的 new 关键字：

```dart
var p1 = new Point(2, 2);
var p2 = new Point.fromJson({'x': 1, 'y': 2});
```

**有些类提供常量构造函数。要使用常量构造函数创建编译时常量，请将 const 关键字放在构造函数名之前：**

```dart
class Person{
    const Person();
}
var p = const Person();
```

构造两个相同的编译时常量会生成一个单一的、规范的实例（可以看做就是单例模式）：

```dart
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // They are the same instance!
```

当然，和前面讲 const 一样，在常量上下文中，可以在构造函数或文字之前省略 const：

```dart
// Lots of const keywords here.
const pointAndLine = const {
  'point': const [const ImmutablePoint(0, 0)],
  'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};
```

除了第一次使用const关键字之外其他的const都可以省略（能自动递归效果）：

```dart
// Only one const, which establishes the constant context.
const pointAndLine = {
  'point': [ImmutablePoint(0, 0)],
  'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};
```



#### 5.1.3 获得对象的类型

要在运行时获得对象类型，可以使用对象的`runtimeType`属性，该属性返回一个类型对象。

```dart
print('The type of a is ${a.runtimeType}');
```



#### 5.1.4 实例变量

**所有未初始化的实例变量都具有 null 值。**

下面是如何声明实例变量的方法：

```dart
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}
```

**所有实例变量都生成隐式getter方法。非最终实例变量也生成隐式setter方法。**

```dart
class Point {
  num x;
  num y;
}

void main() {
  var point = Point();
  point.x = 4; // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}
```

如果在声明实例变量的地方（而不是在构造函数或方法中）初始化实例变量，则在创建实例时（在构造函数及其初始化列表执行之前）设置该值。



### 5.2 构造函数

  通过创建一个与类同名的函数来声明构造函数(另外，还可以像[命名构造函数]中描述的一样选择一个附加标识符)。构造函数最常见的应用形式是使用构造函数生成一个类的新实例：

```
class Point {
  num x, y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
```

this关键字是指当前实例。

> 注意:只有在名称冲突时才使用它。否则，Dart的代码风格需要省略this

使用构造函数的参数为实例复制的使用非常常见，Dart具有语法上的优势，使这种使用更容易实现：

```
class Point {
  num x, y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
```

## 默认构造函数

如果不声明构造函数，则为您提供默认构造函数。默认构造函数没有参数，并在超类中调用无参数构造函数。

## 构造函数不是继承

子类不从父类继承构造函数。没有声明构造函数的子类只有默认的构造函数（没有参数，没有名称）而不是从父类继承的构造函数。

## 命名的构造函数

使用命名构造函数可以在一个类中定义多个构造函数，或者让一个类的作用对于开发人员来说更清晰：

```
class Point {
  num x, y;

  Point(this.x, this.y);

  // Named constructor
  Point.origin() {
    x = 0;
    y = 0;
  }
}
```

一定要记住构造函数是不会从父类继承的，这意味着父类的命名构造函数子类也不会继承。如果你希望使用在超类中定义的命名构造函数来创建子类，则必须在子类中实现该构造函数。

## 调用非默认的超类构造函数

默认情况下，子类中的构造函数调用父类的未命名的无参数构造函数。父类的构造函数在构造函数体的开始处被调用。如果类中有使用初始化列表，初始化列表将在调用超类之前执行。综上所述，执行顺序如下:

- 初始化列表
- 超类中的无参数构造函数
- main类中的无参数构造函数

如果超类没有未命名的无参数构造函数，则必须手动调用超类中的一个构造函数。在冒号(:)之后，在构造函数体(如果有的话)之前指定超类构造函数。

在下例中，Employee类的构造函数中调用了他的超类Person中的命名构造函数。

```
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person does not have a default constructor;
  // you must call super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

main() {
  var emp = new Employee.fromJson({});

  // Prints:
  // in Person
  // in Employee
  if (emp is Person) {
    // Type check
    emp.firstName = 'Bob';
  }
  (emp as Person).firstName = 'Bob';
}

///结果输出为
in Person
in Employee
```

因为父类构造函数的参数是在调用构造函数之前执行的，所以参数可以是表达式，比如函数调用:

```
class Employee extends Person {
  Employee() : super.fromJson(getDefaultData());
  // ···
}
```

> 警告：在超类的构造函数的参数中不能使用this关键字。例如，参数可以调用static方法但是不能调用实例方法

## 初始化列表

除了调用超类构造函数之外，还可以在构造函数主体运行之前初始化实例变量。初始值设定项用逗号分开。

```
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
```

> 警告:初始化器的右边部分中无法访问this关键字。

在开发期间，可以通过在初始化列表中使用assert来验证输入。

```
Point.withAssert(this.x, this.y) : assert(x >= 0) {
  print('In Point.withAssert(): ($x, $y)');
}
```

初始化列表在设置final字段时很方便。下面的示例初始化初始化列表中的三个final字段：

```
import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(x, y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

main() {
  var p = new Point(2, 3);
  print(p.distanceFromOrigin);
}

///运行结果
3.605551275463989
```

## 重定向构造函数

有时，构造函数的唯一目的是重定向到同一个类中的另一个构造函数。重定向构造函数的主体为空，构造函数调用出现在冒号(:)之后。

```
class Point {
  num x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}
```

## 常量构造函数

如果您的类生成的对象不会改变，您可以使这些对象成为编译时常量。为此，定义一个const构造函数，并确保所有实例变量都是final的。

```
class ImmutablePoint {
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);

  final num x, y;

  const ImmutablePoint(this.x, this.y);
}
```

## 工厂构造函数

在实现构造函数时使用factory关键字，该构造函数并不总是创建类的新实例。例如，工厂构造函数可以从缓存返回实例，也可以返回子类型的实例。

以下示例演示工厂构造函数从缓存返回对象:

```
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```

> 注意:工厂构造函数不能访问this关键字。

调用工厂构造函数，就像调用其他构造函数一样:

```
var logger = Logger('UI');
logger.log('Button clicked');
```


  

## 1.4.3 异步支持

Dart类库有非常多的返回`Future`或者`Stream`对象的函数。 这些函数被称为**异步函数**：它们只会在设置好一些耗时操作之后返回，比如像 IO操作。而不是等到这个操作完成。

`async`和`await`关键词支持了异步编程，允许您写出和同步代码很像的异步代码。

### Future

`Future`与JavaScript中的`Promise`非常相似，表示一个异步操作的最终完成（或失败）及其结果值的表示。简单来说，它就是用于处理异步操作的，异步处理成功了就执行成功的操作，异步处理失败了就捕获错误或者停止后续操作。一个Future只会对应一个结果，要么成功，要么失败。

由于本身功能较多，这里我们只介绍其常用的API及特性。还有，请记住，`Future` 的所有API的返回值仍然是一个`Future`对象，所以可以很方便的进行链式调用。

#### Future.then

为了方便示例，在本例中我们使用`Future.delayed` 创建了一个延时任务（实际场景会是一个真正的耗时任务，比如一次网络请求），即2秒后返回结果字符串"hi world!"，然后我们在`then`中接收异步结果并打印结果，代码如下：

```dart
Future.delayed(new Duration(seconds: 2),(){
   return "hi world!";
}).then((data){
   print(data);
});
```

#### Future.catchError

如果异步任务发生错误，我们可以在`catchError`中捕获错误，我们将上面示例改为：

```dart
Future.delayed(new Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");  
}).then((data){
   //执行成功会走到这里  
   print("success");
}).catchError((e){
   //执行失败会走到这里  
   print(e);
});
```

在本示例中，我们在异步任务中抛出了一个异常，`then`的回调函数将不会被执行，取而代之的是 `catchError`回调函数将被调用；但是，并不是只有 `catchError`回调才能捕获错误，`then`方法还有一个可选参数`onError`，我们也可以它来捕获异常：

```dart
Future.delayed(new Duration(seconds: 2), () {
    //return "hi world!";
    throw AssertionError("Error");
}).then((data) {
    print("success");
}, onError: (e) {
    print(e);
});
```

#### Future.whenComplete

有些时候，我们会遇到无论异步任务执行成功或失败都需要做一些事的场景，比如在网络请求前弹出加载对话框，在请求结束后关闭对话框。这种场景，有两种方法，第一种是分别在`then`或`catch`中关闭一下对话框，第二种就是使用`Future`的`whenComplete`回调，我们将上面示例改一下：

```dart
Future.delayed(new Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");
}).then((data){
   //执行成功会走到这里 
   print(data);
}).catchError((e){
   //执行失败会走到这里   
   print(e);
}).whenComplete((){
   //无论成功或失败都会走到这里
});
```

#### Future.wait

有些时候，我们需要等待多个异步任务都执行结束后才进行一些操作，比如我们有一个界面，需要先分别从两个网络接口获取数据，获取成功后，我们需要将两个接口数据进行特定的处理后再显示到UI界面上，应该怎么做？答案是`Future.wait`，它接受一个`Future`数组参数，只有数组中所有`Future`都执行成功后，才会触发`then`的成功回调，只要有一个`Future`执行失败，就会触发错误回调。下面，我们通过模拟`Future.delayed` 来模拟两个数据获取的异步任务，等两个异步任务都执行成功时，将两个异步任务的结果拼接打印出来，代码如下：

```dart
Future.wait([
  // 2秒后返回结果  
  Future.delayed(new Duration(seconds: 2), () {
    return "hello";
  }),
  // 4秒后返回结果  
  Future.delayed(new Duration(seconds: 4), () {
    return " world";
  })
]).then((results){
  print(results[0]+results[1]);
}).catchError((e){
  print(e);
});
```

执行上面代码，4秒后你会在控制台中看到“hello world”。

### Async/await

Dart中的`async/await` 和JavaScript中的`async/await`功能和用法是一模一样的，如果你已经了解JavaScript中的`async/await`的用法，可以直接跳过本节。

#### 回调地狱(Callback Hell)

如果代码中有大量异步逻辑，并且出现大量异步任务依赖其它异步任务的结果时，必然会出现`Future.then`回调中套回调情况。举个例子，比如现在有个需求场景是用户先登录，登录成功后会获得用户ID，然后通过用户ID，再去请求用户个人信息，获取到用户个人信息后，为了使用方便，我们需要将其缓存在本地文件系统，代码如下：

```dart
//先分别定义各个异步任务
Future<String> login(String userName, String pwd){
    ...
    //用户登录
};
Future<String> getUserInfo(String id){
    ...
    //获取用户信息 
};
Future saveUserInfo(String userInfo){
    ...
    // 保存用户信息 
};
```

接下来，执行整个任务流：

```dart
login("alice","******").then((id){
 //登录成功后通过，id获取用户信息    
 getUserInfo(id).then((userInfo){
    //获取用户信息后保存 
    saveUserInfo(userInfo).then((){
       //保存用户信息，接下来执行其它操作
        ...
    });
  });
})
```

可以感受一下，如果业务逻辑中有大量异步依赖的情况，将会出现上面这种在回调里面套回调的情况，过多的嵌套会导致的代码可读性下降以及出错率提高，并且非常难维护，这个问题被形象的称为**回调地狱（Callback Hell）**。回调地狱问题在之前JavaScript中非常突出，也是JavaScript被吐槽最多的点，但随着ECMAScript6和ECMAScript7标准发布后，这个问题得到了非常好的解决，而解决回调地狱的两大神器正是ECMAScript6引入了`Promise`，以及ECMAScript7中引入的`async/await`。 而在Dart中几乎是完全平移了JavaScript中的这两者：`Future`相当于`Promise`，而`async/await`连名字都没改。接下来我们看看通过`Future`和`async/await`如何消除上面示例中的嵌套问题。

##### 使用Future消除Callback Hell

```dart
login("alice","******").then((id){
      return getUserInfo(id);
}).then((userInfo){
    return saveUserInfo(userInfo);
}).then((e){
   //执行接下来的操作 
}).catchError((e){
  //错误处理  
  print(e);
});
```

正如上文所述， *“Future 的所有API的返回值仍然是一个Future对象，所以可以很方便的进行链式调用”* ，如果在then中返回的是一个`Future`的话，该`future`会执行，执行结束后会触发后面的`then`回调，这样依次向下，就避免了层层嵌套。

##### 使用async/await消除callback hell

通过`Future`回调中再返回`Future`的方式虽然能避免层层嵌套，但是还是有一层回调，有没有一种方式能够让我们可以像写同步代码那样来执行异步任务而不使用回调的方式？答案是肯定的，这就要使用`async/await`了，下面我们先直接看代码，然后再解释，代码如下：

```dart
task() async {
   try{
    String id = await login("alice","******");
    String userInfo = await getUserInfo(id);
    await saveUserInfo(userInfo);
    //执行接下来的操作   
   } catch(e){
    //错误处理   
    print(e);   
   }  
}
```

- `async`用来表示函数是异步的，定义的函数会返回一个`Future`对象，可以使用then方法添加回调函数。
- `await` 后面是一个`Future`，表示等待该异步任务完成，异步完成后才会往下走；`await`必须出现在 `async` 函数内部。

可以看到，我们通过`async/await`将一个异步流用同步的代码表示出来了。

> 其实，无论是在JavaScript还是Dart中，`async/await`都只是一个语法糖，编译器或解释器最终都会将其转化为一个Promise（Future）的调用链。

## 1.4.4 Stream

`Stream` 也是用于接收异步事件数据，和`Future` 不同的是，它可以接收多个异步操作的结果（成功或失败）。 也就是说，在执行异步任务时，可以通过多次触发成功或失败事件来传递结果数据或错误异常。 `Stream` 常用于会多次读取数据的异步任务场景，如网络内容下载、文件读写等。举个例子：

```dart
Stream.fromFutures([
  // 1秒后返回结果
  Future.delayed(new Duration(seconds: 1), () {
    return "hello 1";
  }),
  // 抛出一个异常
  Future.delayed(new Duration(seconds: 2),(){
    throw AssertionError("Error");
  }),
  // 3秒后返回结果
  Future.delayed(new Duration(seconds: 3), () {
    return "hello 3";
  })
]).listen((data){
   print(data);
}, onError: (e){
   print(e.message);
},onDone: (){

});
```

上面的代码依次会输出：

```
I/flutter (17666): hello 1
I/flutter (17666): Error
I/flutter (17666): hello 3
```

代码很简单，就不赘述了。

> 思考题：既然Stream可以接收多次事件，那能不能用Stream来实现一个订阅者模式的事件总线？

## 1.4.5 Dart和Java及JavaScript对比

通过上面介绍，相信你对Dart应该有了一个初步的印象，由于笔者平时也使用Java和JavaScript，下面笔者根据自己的经验，结合Java和JavaScript，谈一下自己的看法。

> 之所以将Dart与Java和JavaScript对比，是因为，这两者分别是强类型语言和弱类型语言的典型代表，并且Dart 语法中很多地方也都借鉴了Java和JavaScript。

### Dart vs Java

客观的来讲，Dart在语法层面确实比Java更有表现力；在VM层面，Dart VM在内存回收和吞吐量都进行了反复的优化，但具体的性能对比，笔者没有找到相关测试数据，但在笔者看来，只要Dart语言能流行，VM的性能就不用担心，毕竟Google在Go（没用VM但有GC）、JavaScript（v8）、Dalvik（Android上的Java VM）上已经有了很多技术积淀。值得注意的是Dart在Flutter中已经可以将GC做到10ms以内，所以Dart和Java相比，决胜因素并不会是在性能方面。而在语法层面，Dart要比Java更有表现力，最重要的是Dart对函数式编程支持要远强于Java(目前只停留在Lambda表达式)，而Dart目前真正的不足是**生态**，但笔者相信，随着Flutter的逐渐火热，会回过头来反推Dart生态加速发展，对于Dart来说，现在需要的是时间。

### Dart vs JavaScript

JavaScript的弱类型一直被抓短，所以TypeScript、CoffeeScript甚至是Facebook的flow（虽然并不能算JavaScript的一个超集，但也通过标注和打包工具提供了静态类型检查）才有市场。就笔者使用过的脚本语言中（笔者曾使用过Python、PHP），JavaScript无疑是**动态化**支持最好的脚本语言，比如在JavaScript中，可以给任何对象在任何时候动态扩展属性，对于精通JavaScript的高手来说，这无疑是一把利剑。但是，任何事物都有两面性，JavaScript的强大的动态化特性也是把双刃剑，你可经常听到另一个声音，认为JavaScript的这种动态性糟糕透了，太过灵活反而导致代码很难预期，无法限制不被期望的修改。毕竟有些人总是对自己或别人写的代码不放心，他们希望能够让代码变得可控，并期望有一套静态类型检查系统来帮助自己减少错误。正因如此，在Flutter中，Dart几乎放弃了脚本语言动态化的特性，如不支持反射、也不支持动态创建函数等。并且Dart在2.0强制开启了类型检查（Strong Mode），原先的检查模式（checked mode）和可选类型（optional type）将淡出，所以在类型安全这个层面来说，Dart和TypeScript、CoffeeScript是差不多的，所以单从这一点来看，Dart并不具备什么明显优势，但综合起来看，Dart既能进行服务端脚本、APP开发、web开发，这就有优势了！

综上所述，笔者还是很看好Dart语言的将来，之所以表这个态，是因为在新技术发展初期，很多人可能还有所摇摆，有所犹豫，所以有必要给大家打一剂强心针，当然，这是一个见仁见智的问题，大家可以各抒己见。
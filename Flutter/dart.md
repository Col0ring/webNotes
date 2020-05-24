# dart

> 参考自[《Dart2 中文文档》](https://www.kancloud.cn/marswill/dark2_document/709087)

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



### 2.2 数据类型

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

只要 List、Set、Map 等的基本用法见 [dart 常用库的使用](#12.dart 常用库的使用)

> [Lst API 文档](https://api.dartlang.org/stable/dart-core/List-class.html)

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



#### 2.2.5 Set

>  [Set API 文档](https://api.dartlang.org/stable/dart-core/Set-class.html)

dart 中的集合是一组无序的独特物品集合。因为集合是无序的，所以不能通过索引（位置）获得集合的项。

```dart
var ingredients = Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);
assert(ingredients.length == 3);

// Adding a duplicate item has no effect.
ingredients.add('gold');
assert(ingredients.length == 3);

// Remove an item from a set.
ingredients.remove('gold');
assert(ingredients.length == 2);
```

使用`contains()`和`containsAll()`来检查集合中是否有一个或多个对象：

```dart
var ingredients = Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);

// Check whether an item is in the set.
assert(ingredients.contains('titanium'));

// Check whether all the items are in the set.
assert(ingredients.containsAll(['titanium', 'xenon']));
```

交集是一个集合，其项在另外两个集合中：

```dart
var ingredients = Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);

// Create the intersection of two sets.
var nobleGases = Set.from(['xenon', 'argon']);
var intersection = ingredients.intersection(nobleGases);
assert(intersection.length == 1);
assert(intersection.contains('xenon'));
```



#### 2.2.6 Map

> [Map API 文档](https://api.dartlang.org/stable/dart-core/Map-class.html)

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



#### 2.2.7 Runes（字符）

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



#### 2.2.8 Symbols（符号）

符号对象表示在 dart 程序中声明的操作符或标识符。一般来说可能永远不需要使用符号，但是对于按名称引用标识符的api 来说，它们是非常重要的，因为缩小改变了标识符名称而不是标识符符号。

要获取标识符的符号，请使用符号文字，符号文字仅为`#`，后面跟着标识符：

```dart
#radix
#bar
```

**注意：**符号常量是编译时常量。



#### 2.2.9 枚举类型

**枚举类型可以看做是 dart 对类（class）的一种延伸。**

使用`enum`关键字声明一个枚举类型：

```dart
enum Color { red, green, blue }
```

枚举中的每个值都有一个索引 getter，它返回`enum`声明中值的从0开始的位置。例如，第一个值有索引0，第二个值有索引1。

```dart
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```

使用`enum`的`values`常量要获取枚举中所有值的列表。

```dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```

可以在 switch 语句中使用`enum`，如果 switch 的 case 不处理`enum`的所有值，将会报一个警告消息：

```dart
var aColor = Color.blue;

switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: // Without this, you see a WARNING.
    print(aColor); // 'Color.blue'
}
```

**枚举类型有以下限制:**

- 不能子类化、混合或实现枚举。
- 不能显式实例化一个枚举。



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

- 对于 is 操作符，如果 obj 实现了 T 指定的接口，则`obj is T`为0真。例如，`obj is Object`总是为真（也就是说在对任何父类使用 is 操作符都会返回真）。

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



#### 4.4.3 默认参数值

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



### 4.6 箭头函数

在 dart 中的箭头函数的用法和 JavaScript 中箭头函数只写一行直接返回值是类似的效果，不过有所区别的是 dart 中只有简便写法这个功能，无法修改`this`的指向问题。

同时 dart 中的箭头函数只能写一行表达式，没有`{}`函数体，虽然能在最外层包裹一层`{}`，但这不代表函数体，而是一个 Set 类型的数据。 `{}`不仅在定义函数的时候用到，在构建 `Set、Map集合字面量` 的时候也用到了。

**注意：**如果是单独声明的箭头函数，最后都必须要加上分号`;`作为结尾。

```dart
// 下面的test返回值是{null}，因为 if (true) {print("success")} 这个语句的返回值是null
test() => {
    if (true) {print("success")}
};
main() {
  print(test().runtimeType);
}

// 输出
/*
_CompactLinkedHashSet<Set<void>>
*/
```



### 4.7 函数闭包

dart 中的函数闭包和函数作用域的问题和 JavaScript 中的一致，略。



### 4.8 返回值

所有函数都返回一个值。如果没有指定返回值，则语句返回 null。

```dart
foo() {}
assert(foo() == null);
```

dart 中的函数声明**如果是写在全局没有显式声明**返回值类型时会默认当做`dynamic`处理。

**注意：**

- 写在全局的和类中的函数返回值没有类型推断。
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

**通过创建一个与类同名的函数或添加一个附加标识符来声明构造函数。**

```dart
class Point {
  num x, y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
```

**注意：**

- this 关键字是指当前实例。
- 与 Java 中类似，this 在没有命名冲突的时候是可以省略的.

可以直接通过下面这种方法实现构造函数传递参数直接给对应的实例（与 TypeScript 中直接在形参上写上属性修辞符的用法有相似之处）：

```dart
class Point {
  num x, y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
```

我们也可以在构造函数上使用可选的位置参数、命名参数和默认值：

```dart
//下面这样写也是可行的
class Point {
  num x, y;
  Point({this.x = 1, this.y});
}

void main() {
  // 上面的this是语法同，本质还是x和y
  var p = new Point(y: 2);
  print(p.x);
  print(p.y);
}
```



**下面两点与 JAVA 中的构造函数类似：**

- **默认构造函数：**如果不声明构造函数（包括命名构造函数），则提供默认构造函数。默认构造函数没有参数，并在超类中调用无参数构造函数。

- **构造函数不是继承：**对于子类和父类来说，子类不从父类继承构造函数。没有声明构造函数的子类只有默认的构造函数（没有参数，没有名称）而不是从父类继承的构造函数。
  - 子类必须调用父类的构造函数。默认情况下会自动调用父类的无参构造函数，如果父类没有无参构造函数,则必须显式的用`super()`调用一个构造函数。
  - 创建对象时，先调用父类的构造函数对对象进行初始化，然后再调用子类自己的构造函数。
  - 子类只默认调用父类的默认（无参）构造函数，如果父类重写了自己的构造函数，就会导致父类没有无参构造函数，这样子类就不能从父类继承构造函数。



#### 5.2.1 命名的构造函数

使用命名构造函数可以在一个类中定义多个构造函数，或者让一个类的作用对于开发人员来说更清晰：

```dart
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

**注：**因为构造函数是不会从父类继承的，这意味着父类的命名构造函数子类也不会继承。如果希望使用在超类中定义的命名构造函数来创建子类，则必须在子类中实现该构造函数。



#### 5.2.2 非默认的超类构造函数

**默认情况下，子类中的构造函数调用父类的未命名的无参数构造函数。父类的构造函数在构造函数体的开始处被调用。**如果类中有使用初始化列表，初始化列表将在调用超类之前执行。综上所述，执行顺序如下:

- 初始化列表
- 超类中的无参数构造函数
- main 类中的无参数构造函数

如果超类没有未命名的无参数构造函数，则必须手动调用超类中的一个构造函数。在冒号（:）之后，在构造函数体（如果有的话）之前指定超类构造函数。

**注：**dart 中的超类调用和其它的语言有所区别，不能在构造函数内部使用`super()`来调用，而是要在函数体之前就指定好要运行的超类函数。同时，子类的超类与父类的超类是可以不一致的（按照正常的代码风格，我们应该将其设置为同名的，这样才好管理），但是如果父类自定义了构造函数，子类必须要调用父类的其中一个构造函数。

```dart
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person does not have a default constructor;
  // you must call super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) { // 这里子类可以调用任意的父类拥有的构造函数
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

/// 结果输出为
/*
in Person
in Employee
*/
```

**因为父类构造函数的参数是在调用构造函数之前执行的，所以参数可以是表达式**，比如函数调用：

```dart
class Employee extends Person {
  Employee() : super.fromJson(getDefaultData());
  // ···
}
```

**注意：**在超类的构造函数的参数中不能使用`this`关键字。



#### 5.2.3 初始化列表 

**除了调用超类构造函数之外，还可以在构造函数主体运行之前初始化实例变量。初始值设定项用逗号分开。**

```dart
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
```

**注意：**初始化器的右边部分中无法访问`this`关键字（左边是可以的，如果没有命名冲突也可以省略 this）,所以右边的变量都会使用实例时传入的变量。

在开发期间，可以通过在初始化列表中使用 assert 来验证输入。

```dart
Point.withAssert(this.x, this.y) : assert(x >= 0) {
  print('In Point.withAssert(): ($x, $y)');
}
```

初始化列表在设置`final`字段时很方便。下面的示例初始化初始化列表中的三个`final`字段：

```dart
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

/// 运行结果
/*
3.605551275463989
*/
```

**注意：**当初始化列表和超类的构造函数一起使用时，超类的构造函数要写在最后面，前面也是用逗号隔开。

```dart
class Animal {
  int x;
  int y;
  Animal(this.x, this.y);
}

class Dog extends Animal {
  int z;
  Dog(int x, int y, int z)
      : this.z = z,
        super(x, y);
}

void main() {
  var dog = new Dog(1, 2, 3);
  print(dog.x); // 1
  print(dog.y); // 2
  print(dog.z); // 3
}
```



#### 5.2.4 重定向构造函数

有时，构造函数的唯一目的是重定向到同一个类中的另一个构造函数。重定向构造函数的主体为空，构造函数调用出现在冒号（:）之后。

```dart
class Point {
  num x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}
```

**注意：**重定向的构造函数不能初始化列表和调用超类构造函数，冒号（:）之后只能是重定向方法。



#### 5.2.5 常量构造函数

**如果类生成的对象不会改变，可以使这些对象成为编译时常量。**为此，定义一个 const 构造函数，并确保所有实例变量都是 final 的。

**这样的构造函数实例出的对象属性是不能通过`x.xx = xx`来改变的，是一个编译时就创建好的常量。同时传入相同值得实例都是同一个实例**

```dart
class ImmutablePoint {
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);

  final num x, y;

  const ImmutablePoint(this.x, this.y);
}
void main() {
  var a = const ImmutablePoint(1, 1);
  var b = const ImmutablePoint(1, 1);

  print(a == b); // true
}
```

**注意：**常量构造函数不能够有函数体，也就是后面的`{}`。



#### 5.2.6 工厂构造函数

在实现构造函数时使用`factory`关键字，该构造函数并不总是创建类的新实例。例如，工厂构造函数可以从缓存返回实例，也可以返回子类型的实例。

以下示例演示工厂构造函数从缓存返回对象：

```dart
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

调用工厂构造函数，就像调用其他构造函数一样：

```dart
var logger = Logger('UI');
logger.log('Button clicked');
```

**注意：**

- 工厂构造函数不能访问 this 关键字，所以使用变量默认会使用类的静态属性。

- 工厂函数中缓存的实例都会是同一个，类似常量构造函数使用单例模式。

  ```dart
  var logger = Logger('UI');
  var logger1 = Logger('UI');
  print(logger == logger1); // true
  ```



### 5.3 方法

#### 5.3.1 实例方法

对象上的实例方法可以访问实例变量。下面示例中的`distanceTo()`方法是一个实例方法的示例：

```dart
import 'dart:math';

class Point {
  num x, y;

  Point(this.x, this.y);

  num distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }
}
```



#### 5.3.2 getter 和 setter 方法

getter 和 setter 是对对象属性的读写访问的特殊方法。从前面我们可以知道，每个实例变量都有一个隐式的 getter，如果需要的话还可以加上一个 setter。使用`get`和`set`关键字来实现 getter 和 setter 方法可以来读写其他属性：

```dart
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right => left + width;
  void set right(num value) => left = value - width;
  num get bottom => top + height;
  set bottom(num value) => top = value - height;
  
  // 非箭头函数写法，getter函数后面没有()，不能传入参数
  num get area {
    return this.height * this.width;
  }
  void set areaHeight(num value) {
    this.height = value;
  }
}

test() => {
      if (true) {print("success")}
    };
void main() {
  var rect = Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
```

**注意：**

- getter 与 setter 函数在使用时就如同实例的普通属性的赋值与读取一样使用，而不是像函数一样使用。

- getter 函数与其他函数有所不同，因为不能传入参数，所有 dart 在语法中都没有`()`给我们使用。

- setter 的函数类型只能不写或者是 void 类型，同时虽然是 void 类型但是和普通的变量赋值一样默认还是有赋值的变量作为返回值的。

  ```dart
  // 如果是其他返回值是void的函数使用函数返回值是会报错的
  print(rect.left = 3); // 3
  ```

- 诸如`increment(++)`之类的操作符以预期的方式工作，无论 getter 是否被显式定义。为了避免任何意外的副作用，操作符只调用 getter 一次，将其值保存在一个临时变量中。



#### 5.3.3 抽象方法与抽象类

**实例方法、getter 和 setter 方法都可以是抽象方法，只定义一个接口但是将具体实现留给其他类，其他类必须实现该抽象接口。抽象方法只能存在于抽象类中，抽象方法是没有方法体`{}`的。调用抽象方法会导致运行时错误。**

```dart
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
```

**注意：**dart 中实现抽象方法直接在类名前面加上 abstract 关键字使类变成抽象类就可以了，不需要在特定的方法前面加上 abstract 关键字，**没有方法体的方法 dart 会自动将其看做抽象方法**。



### 5.4 继承

与 Java 和 JavaScript 中一致，使用`extend`创建子类，使用`super`引用超类：

```dart
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
```



### 5.5 重写属性和方法

#### 5.5.1 重写类的成员

子类可以覆盖实例方法、getter 和 setter。可以使用`@override`注释来指示重写了某个成员方法：

```dart
class SmartTelevision extends Television {
  @override
  void turnOn() {...}
  // ···
}
```

**注：**要在类型安全的代码中缩小方法参数或实例变量的类型，可以使用`covariant`关键字。



#### 5.5.2 重写操作符

可以重写下表中显示的操作符。例如，如果定义一个`Vector`类，可以定义一个`+`方法来让两个向量相加。

| <    | +    | \|   | []   |
| :--- | :--- | :--- | :--- |
| >    | /    | ^    | []=  |
| <=   | ~/   | &    | ~    |
| >=   | *    | <<   | ==   |
| -    | %    | >>   |      |

下例在类中重写了`+`和`-`操作符：

```dart
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  // Operator == and hashCode not shown. For details, see note below.
  // ···
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}
```

**注意：**如果重写`==`，还应该重写对象的`hashCode getter`。具体不再介绍。



#### 5.5.3 noSuchMethod()

**可以重写`noSuchMethod()`方法来处理程序访问一个不存在的方法或者成员变量：**

```dart
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  @override
  void noSuchMethod(Invocation invocation) {
    print('You tried to use a non-existent member: ' +
        '${invocation.memberName}');
  }
}
```

**不能调用未实现的方法，除非下列任何一个是正确的：**

- 被调用者有**静态方法** `dynamic`
- 被调用者有一个静态类型来定义未实现的方法（也可以是抽象方法），而接收者的动态类型有一个`noSuchMethod()`的实现，它与类对象中的方法不同。



### 5.6 类（静态）变量和方法

**使用`static`关键字实现类范围的变量和方法。**

#### 5.6.1 静态变量

静态变量（类变量）对于类范围内的状态和常量是有用的：

```dart
class Queue {
  static const initialCapacity = 16;
  // ···
}

void main() {
  assert(Queue.initialCapacity == 16);
}
```

**注：**静态变量在使用之前不会初始化。



#### 5.6.2 静态方法

静态方法（类方法）不对实例进行操作，因此无法访问该实例。例如：

```dart
import 'dart:math';

class Point {
  num x, y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

void main() {
  var a = Point(2, 2);
  var b = Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(2.8 < distance && distance < 2.9);
  print(distance);
}
```

可以使用静态方法作为编译时常量。例如，可以将静态方法作为参数传递给常量构造函数。



### 5.7 私有成员

在 dart 中的类定义私有成员不像 Java 那样有公开、保护和私有的关键字。如果标识符以下划线（_）开头，则该标识符对其库是私有的。

**注意：**私有的类成员在同一个库中是无效的，必须要在不同的库中被引入才能够作为私有成员。在同一个库中使用会被当做是一个普通的成员变量。

```dart
import 'B.dart';

void main() {
  var b = new B();
  b.run();
  b._run(); // Error
}
```



### 5.8 隐式接口

dart 中有接口的概念，但是没有专门单独定义接口的关键字`interface`，在 dart 中的接口使用类来进行代替（一般使用抽象类），每个类都隐式地定义一个接口，该接口包含类的所有实例成员及其实现的任何接口。

```dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}
```

**一个类可以实现多个接口：**

```dart
class Point implements Comparable, Location {...}
```



### 5.9 可调用的类

实现`call()`方法可以让 dart 类的实例像函数一样被调用，相当于实例化返回了一个函数对象。

在下面的示例中，`WannabeFunction`类定义了一个`call()`函数，该函数接受三个字符串并将它们连接起来，每个字符串用空格分隔，并在结尾加一个感叹号。

```dart
class WannabeFunction {
  call(String a, String b, String c) => '$a $b $c!';
}

main() {
  var wf = new WannabeFunction();
  var out = wf("Hi", "there,", "gang");
  print('$out');
}

/// 执行结果
/*
Hi there, gang!
*/
```



### 5.10 mixins

**mixins 是在多个类层次结构中重用类代码的一种方式。mixins 的中文意思是混入，就是在类中混入其他功能，在 dart 中可以使用 mixins 实现类似多继承的功能。**

要使用 mixin，需要在 with 关键字后面加上一个或多个 mixin 名称。下面的例子显示了两个使用 mixin 的类：

```dart
class Musician extends Performer with Musical {
  // ···
}

// 当继承和mixins用在一起是extends关键字应该在with之前
class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

**因为 mixins 使用的条件，随着 dart 版本一直在变，这里说明一下 dart 2.x 中的使用条件：**

- 作为 mixins 的类只能继承 Object 类（也就是创建的时候不能使用 extends 关键字），不能继承其他类。
- 作为 mixins 的类不能有构造函数。
- 一个类可以 mixins 多个 mixins 类。
- mixins 绝不是继承，也不是接口，而是一种全新的特性。

```dart
abstract class Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```

**mixins 使用时注意点：**

- 当继承的类和mixins的类有属性或方法名冲突时，后面应用的类的属性或方法会顶替掉前面应用的。

- mixins 后的类实例的成员是运用到的所有类的实例。

  ```dart
  class Maestro extends Person
      with Musical, Aggressive, Demented {
    Maestro(String maestroName) {
      name = maestroName;
      canConduct = true;
    }
  }
  void main(){
      var m = new Maestro();
      print(m is Person); // true
      print(m is Musical); // true
      print(m is Aggressive); // true
      print(m is Demented); // true
  }
  ```



## 6.泛型

**泛型的用法同 Java 和 TypeScript 中的用法类似，这里只着重说明 dart 中的特性。**

### 6.1 使用集合字面量

List 和 map 字面量可以被参数化。参数化字面量和已经认识的所有字面量一样，仅仅是在字面量的开始括号之前添加`<type>`（对于 List 类型来说）或者添加`<keyType, valueType>`（对于 map 类型来说）。

```dat
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
```



### 6.2 构造函数的参数化类型

要在使用构造函数时指定一个或多个类型，请将类型放在类名后面的尖括号`<…>`中。

```dart
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
var nameSet = Set<String>.from(names);
```

下面的代码创建了一个具有整数键和视图类型值的map映射：

```dart
var views = Map<int, View>();
```



### 6.3 泛型集合及其包含的类型

dart 通用类型被具体化，这意味着它们在运行时携带它们的类型信息。例如，可以测试集合的类型：

```dart
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
```

**注意：**相反，Java 中的泛型使用擦除，这意味着泛型类型参数在运行时被删除。在 Java 中，可以测试一个对象是否是一个列表，但不能测试它是否是 String 类型的列表。



### 6.4 限制参数化类型

在实现泛型类型时，可能希望限制其参数的类型，这个时候需要使用 extends 关键字

```dart
class Foo<T extends SomeBaseClass> {
  // Implementation goes here...
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}
```

可以使用 **SomeBaseClass 或它的任何子类**作为泛型参数：

```dart
var someBaseClassFoo = Foo<SomeBaseClass>();
var extenderFoo = Foo<Extender>();
```

**当不知道泛型参数时默认会是 extends 的类本身（否则会是 dynamic）：**

```dart
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
```

传入其他的非 extends 的类或其子类时会报错：

```dart
var foo = Foo<Object>(); // Error
```



### 6.5 泛型方法

最初，dart 仅仅在类中支持泛型。后来一种称为泛型方法的新语法允许在方法和函数中使用类型参数。

```dart
T first<T>(List<T> ts) {
  // Do some initial work or error checking, then...
  T tmp = ts[0];
  // Do some additional checking or processing...
  return tmp;
}
```

用法同 TypeScript 中非常相似，这里不过多介绍。



## 7.库和可见性

import 和 library 指令可以帮助创建模块化和可共享的代码库。库不仅提供api，而且包含隐私部分：以下划线（_）开头的标识符仅在库中可见。

可以使用包来分发库，一般使用用不到，在这里就不做多的讲解。

**注：**每个 dart 应用程序都是一个库，即使它不使用库指令。



### 7.1 使用库

- 使用`import`来指定如何在另一个库的范围中使用来自一个库的命名空间。

  例如，`dart web`应用程序通常使用`dart:html`库，它们可以这样导入：

  ```dart
  import 'dart:html';
  ```

  导入一个库仅仅需要提供库的 URI。对于内置库，URI具有特定的形式（dart:scheme）。对于其他库，可以使用`文件路径`或者`包:scheme`的形式。`包:scheme`形式指定包管理器（如 pub 工具（类似 npm））提供的库。例如：

  ```dart
  import 'package:test/test.dart';
  ```

  **注意：**URI 表示统一资源标识符。url（统一资源定位器）是一种常见的 URI。

- 使用`library`定义这个库的名字，但库的名字并不影响导入，因为import语句用的是字符串Uri

  ```dart
  library person;
  ```




### 7.2 dart 中的三种库

在 dart 中主要有三种库：

- 自定义库

  ```dart
  import 'lib/hello.dart';
  ```

- 系统内置库

  ```dart
  import 'dart:math';
  import 'dart:io';
  import 'dart:convert';
  ```

- pub 包管理器中的第三方库（pub 中的包都可以在 [pub.dev](https://pub.dev) 中找到）

  要下载第三方库，需要在项目根目录创建一个`pubspec.yaml`的文件，然后在其中写入对应的依赖项，保存后 pub 会自动去下载对应的依赖。

#### 7.2.1 YAML 文件的语法

YAML 是一个类似 XML、JSON 的标记性语言。YAML 强调以数据为中心，并不是以标识语言为重点。

**规范：**

- 大小写敏感
- 缩进代表层级，使用空格，默认2个空格（flutter工具做了处理，tab也可以）
- \# 表示注释内容
-  : 表示键值对，注意后面要空格
-  {} 表示键值表
-  \- 表示列表，注意后面要空格
-  [] 表示数组，注意每项之间有空格
-  ? 表示复杂的键

```yaml
# 键值对，表示app的名字为flutter_app
name: flutter_app
description: A new Flutter application.
# 环境，flutter的sdk版本在此之间
environment:
  sdk: ">=2.0.0-dev.68.0 <3.0.0"

# 依赖库
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^0.1.2
  event_bus: ^1.0.1
  shared_preferences: "^0.4.3"
  flutter_refresh: ^0.0.1

#测试环境的依赖库
dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  #使用Material图标
  uses-material-design: true
  #assets文件
  assets:
    - images/head.png
    - images/1.gif
  #字体样式
  fonts:
     #family与fonts是一个整体，列表的一项
     - family: Schyler
       fonts:
         - asset: fonts/Schyler-Regular.ttf
         - asset: fonts/Schyler-Italic.ttf
           style: italic
     - family: Trajan Pro
       fonts:
         - asset: fonts/TrajanPro.ttf
         - asset: fonts/TrajanPro_Bold.ttf
           weight: 700
```

**注：**`-`可转化为`[]`，`:`也可以转化为`{}`，转化后类似 json

```yaml
dependencies: {flutter: {sdk: flutter}, cupertino_icons: ^0.1.2, event_bus: ^1.0.1, shared_preferences: "^0.4.3", flutter_refresh: ^0.0.1}

assets: [images/head.png, images/1.gif]
```

**对于字符串：**

- 字符串

    ```yaml
     str0: '我是字符串'
     #双引号不会对特殊字符转义
     str1: "It's a test"
     #单引号中单引号的字符，需要转义
     str2: 'It''s a test'
     #~是空的意思
     str3: ~ 
     # 两个!表示强制转换，!!str 表示强制转为str
     str4: !!str true
     str5: 也可以不用引号引起来，厉害不
     str6: '有空格 或者 特殊字符* 就必须放在引号中了'
     str7: <p style="color:red">Hello world<p>
    ```

    转换为 json

    ```json
    { 
        str0: '我是字符串',
        str1: 'It\'s a test',
        str2: 'It\'s a test',
        str3: null,
        str4: 'true',
        str5: '也可以不用引号引起来，厉害不',
        str6: '有空格 或者 特殊字符* 就必须放在引号中了',
        str7: '<p style="color:red">Hello world<p>' 
    }
    ```

- 换行

  ```yaml
   str8: #每行用空格代替换行符，每行行尾空格无效
    第一行            
    第二行       
    第三行     
   str9: | #每行都有换行符
    第一行    
    第二行   
    第三行   
   str10: > #末尾有换行符，中间用空格代替换行符
    第一行   
    第二行    
    第三行  
   str11: |- #末尾去除换行符
    第一行 
    第二行   
    第三行      
   str12: |+ #末尾添加换行符
    第一行  
    第二行   
    第三行   
  ```

  转换为 json

  ```json
  { 
      str8: '第一行 第二行 第三行',
      str9: '第一行    \n第二行   \n第三行   \n',
      str10: '第一行    第二行     第三行  \n',
      str11: '第一行 \n第二行   \n第三行       ',
      str12: '第一行  \n第二行   \n第三行   \n' 
  }
  ```
  




### 7.3 指定一个库前缀

如果导入两个具有冲突标识符的库，那么可以为一个或两个库指定一个前缀（命名空间）。

```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();

// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
```



### 7.4 库的可见性

如果只想使用库的一部分，可以有选择地导入库，这也是库的可见性。

有两种关键字来表示需要使用的内容：

- **show**：导入的库只能使用 show 后面的变量。
- **hide**：导入的库不能使用 hide 后面的变量。

```dart
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```



### 7.5 懒加载库

**延迟加载（也称为懒加载）允许应用程序在需要时按需加载库。**

以下是一些可能使用延迟加载的情况：

- 减少应用程序的初始启动时间。
- 例如，要执行A/B测试——尝试算法的其他实现。
- 加载很少使用的功能，如可选屏幕和对话框。

要延迟加载库，必须首先使用`deferred as`进行导入。

```dart
import 'package:greetings/hello.dart' deferred as hello;
```

**当需要库时，使用库的标识符调用`loadLibrary()`方法。**

```dart
Future greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

**注；**可以在库上多次调用loadLibrary()，但该库只加载一次。

**在使用延迟加载时，请记住以下几点：**

- 在导入文件中，递延库的常量不是常量。记住，**这些常量在延迟库加载之前是不存在的**。
- 不能在导入文件中使用来自延迟库的类型。相反，考虑将接口类型移动到由延迟库和导入文件导入的库。
- dart 隐式地将`loadLibrary()`插入到定义使用`deferred`作为名称空间的名称空间中。函数的作用是：返回一个未来（也就是 JavaScript 中的 Promise）。



### 7.6 库的实现

**实现库的时候可以依靠`part`和`part of`关键字：**

- 为了维护一个库，我们可以把各个功能放到各个`dart`文件中
- 但`part of`所在文件不能包括`import、library`等关键字
- 可以包含在`part`关键字所在文件中
- 建议避免使用`part`和`part of`语句，因为那样会使代码很难阅读、修改
- 可以多用`library` 、`part`加字符串类型的 Uri 类似 include，表示包含某个文件
- `part of`加库名表示该文件属于那个库

```dart
// math.dart文件开头
library math;
part 'point.dart';
part 'random.dart';

// point.dart文件开头
part of math;

// random.dart文件开头
part of math;
```

**有关如何实现库包的建议，请参阅[创建库包](https://alanhou.org/docs/dart-documentation/packages/creating-packages/)，包括：**

- 如何组织库的代码。
- 如何使用 export 指令。
- 何时使用 part 指令。



## 8.异步支持

dart 类库有非常多的返回`Future`或者`Stream`对象的函数。 这些函数被称为**异步函数**：它们只会在设置好一些耗时操作之后返回，比如像 IO 操作。而不是等到这个操作完成。

**同时，`async`和`await`关键词支持了异步编程，允许您写出和同步代码很像的异步代码。**

### 8.1 Future

`Future`与 JavaScript 中的`Promise`非常相似，表示一个异步操作的最终完成（或失败）及其结果值的表示。简单来说，它就是用于处理异步操作的，异步处理成功了就执行成功的操作，异步处理失败了就捕获错误或者停止后续操作。**一个Future 只会对应一个结果，要么成功，要么失败。**

**注：**`Future` 的所有API的返回值仍然是一个`Future`对象，所以可以很方便的进行链式调用。

#### 8.1.1 Future.then

为了方便示例，在本例中使用`Future.delayed` 创建了一个延时任务（实际场景会是一个真正的耗时任务，比如一次网络请求），即2秒后返回结果字符串`hi world!`，然后我们在`then`中接收异步结果并打印结果，代码如下：

```dart
Future.delayed(new Duration(seconds: 2),(){
   return "hi world!";
}).then((data){
   print(data);
});
```



#### 8.1.2 Future.catchError

如果异步任务发生错误，可以在`catchError`中捕获错误，将上面示例改为：

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

`then`方法还有一个可选参数`onError`，也可以它来捕获异常：

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



#### 8.1.3 Future.whenComplete

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



#### 8.1.4 Future.wait

使用`Future.wait`可以做到多个`Future`同时出发才会进行后续操作，同 JavaScript 中的`Promise.all()`方法。

`Future.wait`接受一个`Future`数组参数，只有数组中所有`Future`都执行成功后，才会触发`then`的成功回调，只要有一个`Future`执行失败，就会触发错误回调。下面，我们通过模拟`Future.delayed` 来模拟两个数据获取的异步任务，等两个异步任务都执行成功时，将两个异步任务的结果拼接打印出来，代码如下：

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

/*
	最后会在4秒后拿到结果
*/
```

> **更多 Future 的 api 请自行查询文档。**



### 8.2 async 和 await

异步函数是函数体被用`async`修饰符标记的函数。
向函数中添加`async`关键字将使其返回一个 Future。

```dart
String lookUpVersion() => '1.0.0'; // 返回String
```

```dart
Future<String> lookUpVersion() async => '1.0.0'; // 返回Future<String>
```

然后我们可以使用`await`关键字在内部直接接受一个 Future 的`then`的成功回调，就如同 JavaScript 中的那样：

```dart
task() async {
    try{
        String id = await login("alice","******");
        String userInfo = await getUserInfo(id);
        await saveUserInfo(userInfo);
        // 执行接下来的操作   
    } catch(e){
        // 错误处理   
        print(e);   
    }  
}
```

- `async`用来表示函数是异步的，定义的函数会返回一个`Future`对象，可以使用then方法添加回调函数。
- `await` 后面是一个`Future`，表示等待该异步任务完成，异步完成后才会往下走。注意，`await`必须出现在 `async` 函数内部。

**注意：**函数的主体不需要使用 Future 的 API。如果需要，dart将创建Future的对象。如果没有返回一个有用的值，那么将其返回`Future<void>`类型。



### 8.3 处理流(Stream)

`Stream` 也是用于接收异步事件数据，和`Future` 不同的是，它可以接收多个异步操作的结果（成功或失败）。 也就是说，在执行异步任务时，可以通过多次触发成功或失败事件来传递结果数据或错误异常。 `Stream` 常用于会多次读取数据的异步任务场景，如网络内容下载、文件读写等。

**当需要从 Stream 获取值时，有两个选择:**

- 使用`async`和异步的`for`循环（`await for`）

  **注：**在使用`await for`之前，请确保它使代码更清晰，并且确实希望等待流（Stream）的所有结果。例如，通常不应该为 UI 事件侦听器使用`await`，因为 UI 框架会发送无穷无尽的事件流。

  **异步`for`循环有以下形式：**

  ```dart
  await for (varOrType identifier in expression) {
    // Executes each time the stream emits a value.
  }
  ```

  表达式的值必须具有 Stream 类型。执行过程如下:

  1. 等待流发出值。
  2. 执行`for`循环的主体，并将变量设置为发出的值。
  3. 重复1和2，直到流关闭。

  **要停止侦听流，可以使用`break`或`return`语句，该语句将跳出`for`循环，并从流中取消订阅。**

  如果在实现异步`for`循环时出现编译时错误，请确保`await`在异步函数中。例如，要在应用程序的`main()`函数中使用异步`for`循环，`main()`的主体必须标记为`async`：

  ```dart
  Future main() async {
    // ...
    await for (var request in requestServer) {
      handleRequest(request);
    }
    // ...
  }
  ```

- 使用Stream API

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
  }, onDone: (){
  
  });
  /*
  上面的代码依次会输出：
  I/flutter (17666): hello 1
  I/flutter (17666): Error
  I/flutter (17666): hello 3
  */
  ```
  
  具体的 Stream 的 API 还有很多，比如创建一个 Stream 的实例等，在这里就不再赘述，想了解更多可查看相关文档。



## 9.生成器

**当需要延迟地生成一个值序列时，请考虑使用生成器函数。**

dart 内置支持两种生成器函数：

- 同步生成器：返回 Iterable 对象
- 异步生成器：返回 Stream 对象

要实现同步生成器函数，将函数体标记为`sync*`，并使用`yield`语句传递值：

```dart
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}
```

要实现异步生成器函数，将函数体标记为`async*`，并使用`yield`语句传递值：

```dart
Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
```

如果生成器是递归的，可以使用`yield*`来改进它的性能：

```dart
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}
```



## 10.类型定义

在 dart 中，函数是对象，就像字符串和数字是对象一样。`typedef`或`function-type`为函数提供一个类型别名，可以在声明字段和返回类型时使用这个名称。当函数类型被分配给变量时，`typedef`保留类型信息。

以下代码不使用`typedef`：

```dart
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);

  // All we know is that compare is a function,
  // but what type of function?
  assert(coll.compare is Function);
}
```

上面的代码中，当给`compare`分配f时类型信息会丢失。`f`的类型是`(Object, Object)->int(int表示返回值类型)`，当然，`compare`的类型是`Function`。如果我们更改代码以使用显式名称和保留类型信息，开发人员和工具都可以使用这些信息。

```dart
typedef Compare = int Function(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}
```

**注意：**目前，`typedefs`仅限于函数类型，可能在之后会有所改变。

因为`typedef`仅仅是别名，所以它们提供了一种检查任何函数类型的方法。例如：

```dart
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  assert(sort is Compare<int>); // True!
}
```



## 11.元数据

使用元数据提供关于代码的附加信息。元数据注释以字符`@`开头，后跟对编译时常量（如`deprecated`）的引用或对常量构造函数的调用。

所有 dart 代码都可以使用两个注释：`@deprecated`（弃用注释）和`@override`。这里有一个使用`@deprecated`注释的例子：

```dart
class Television {
  /// _Deprecated: Use [turnOn] instead._
  @deprecated
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {...}
}
```

可以定义自己的元数据注释（也就是类似 JavaScript 中的装饰器的效果）。这里有一个定义带有两个参数的@todo注释的示例：

```dart
library todo;

class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}
```

这里有一个使用`@todo`注释的例子:

```dart
import 'todo.dart';

@Todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}
```

元数据可以出现在库、类、类型定义、类型参数、构造函数、工厂、函数、字段、参数或变量声明之前，也可以出现在导入或导出指令之前。可以使用反射在运行时检索元数据。



## 12.dart 常用库的使用                                                                                                                                                                                                                                                                                                                     

[dart 常用库的使用](https://www.kancloud.cn/marswill/dark2_document/728986)


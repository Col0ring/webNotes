# less

## 1.认识less

**less是一种动态样式语言,属于CSS预处理器的范畴,它拓展了CSS语言,**其余的CSS预处理器有sass和stylus等

less增加了变量,MIxin,函数等特性,使CSS更易维护和扩展,less既可以在客户端上运行,也可以借助node.js在服务端

运行

**less的编译工具**

koala官网:www.koala-app.com



## 2.less的引入

less可以通过多种方式引入具体在http://lesscss.cn/中有详细说明

这里主要说明**浏览器端的用法:**

在上面的网址中下载less的js文件,然后放入项目中引入,然后在前面引入已经编写的less文件

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>less引入</title>							
    <link rel="stylesheet/less" type="text/css" href="css/less初见.less" />
    <script src="js/less.min.js"></script>				<!--引入less的地址-->
  </head>
  <body>
  </body>
</html>
<!--
	引入less文件的时候和引入普通外部CSS文件类似,只是需要在rel="stylesheet"后面加上/less表明引入的	 是less样式
-->
```

**注意:less在引入的时候js文件一定要写在引入的less文件的后面,这样才能实现less的编译**



## 3.less的注释

less中的注释和CSS一样有两种单行注释//，多行注释/**/

**注意:通过编译器转换的时候,以//开头的注释不会被编译到CSS文件中,而已/* */包裹的注释才会被编译在CSS文件**

**中**



## 4.less的变量

**less中使用@来申明和使用一个变量**

具体用法为:@变量名:属性值

```less
@pink:pink//在以后的使用中就可以用@pink来代替pink
```

- 变量作为普通的属性值使用,直接用这个变量:@变量名

- 变量作为选择器或者属性名来使用,需要在变量两边加上{}:@{变量名}

- 变量作为url，使用是也需要在两边加上{}:@{url}

- 变量的加载会有延迟效果,当整个文件的所有东西被加载完时才会将变量赋值给对应的属性

- 变量运用的时候会现在自身的作用域里面找到是否定义了该变量,如果定义了就直接用自身作用域中的,如果没

  有才会在外层逐层寻找,如果在最外层都没有找到对应的变量就会报错

  ```less
  @var:0;
  .class{
      @var:1;
      .brass{
          @var:2;
          three:@var;
          @var:3;
      }
    one:@var
  }
  /*
  	最终的结果为one:1;
  			three:3;
  	变量在最后赋完所有值才会赋值给属性,而且这个赋值也是只针对自身的作用域
  */
  ```

  

## 5.嵌套规则

### 5.1 基本嵌套规则

**less中可以在父选择器中包含有子选择器,在转换为CSS文件的时候会自动用空格来隔开**

```less
.father{
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
  .child {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    width: 100px;
    height: 100px;
    background-color: red;   
  }
}
/*
	通过上方的写法可以分别对父元素和子元素进行样式设置
*/
```



### 5.2 &符号的使用

**&符号能取消默认的嵌套关系的中的空格,**从而从父子关系变成兄弟关系,这个兄弟关系是本身拓展出的一些兄弟关

系,如伪类选择器等，可以理解为&符号就是这个选择器本身

```less
.father {
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
  .child {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    width: 100px;
    height: 100px;
    background-color: red;
    &:hover {
      background-color: pink;
    }
  }
}
/*
	如果在:hover前面没有写&符合,hover的效果就不能起作用
*/
```



## 6.less的混合

**混合就是将一系列属性从一个规则集引入到另外一个规则集的方式**

### 6.1 普通混合与不带输出混合

把样式中共同的代码单独提出来在最上方,单独的用一个类名或ID名来定义(因为在开头用了点号和#号,暂且叫作是

类名和ID名),然后在下面输入的时候通过 .类名/ID名;来调用

**注意:混合中写样式代码的方式和写普通CSS或less文件是一样的,混合的名字也是可以用选择器的方式的,只不过所**

**有混合的名字都必须是看起来相识类名或者ID名,下面先统一说成是类名**



**普通混合定义的类名是会被编译到CSS文件中的,如果不想被编译到CSS文件中,需要使用不带输出的混合在名字后**

**面再加上()**

```less
/*这是不混合的写法*/
.father {
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
  .child1 {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    width: 100px;
    height: 100px;
    background-color: red;
  }
  .child2 {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    width: 100px;
    height: 100px;
    background-color: red;
  }
}

```

```less
/*这是混合后的写法*/
.mix(){//不加()是普通混合,会被编译到CSS文件中
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    width: 100px;
    height: 100px;
    background-color: red;
}

.father {
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
  .child1 {
    .mix;//调用混合样式
  }
  .child2 {
   	.mix;
  }
}

```



### 6.2 带参数的混合

混合的参数也是**通过变量使用的形式来定义**的,混合中加参数的用法和函数类似,为了方便记忆可以理解为函数,但是

不是函数,同时这个参数还可以有默认的值,默认值的定义也是和变量的定义相同

```less
/*定义形参*/
.mix(@w:100px,@h:100px,@b:pink) { 
  /*这是定义的默认参数的写法,如果不定义默认的参数就是直接@w,@h,@b*/  		
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  width: @w;
  height: @h;
  background-color: @b;
}

.father {
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
  .child1 {
    .mix(100px, 100px, pink);
  }
  .child2 {
    .mix(200px, 200px, red);
  }
}

```

**注意:定义的外部的类并且后面加了()的时候即使不传入参数在写实参的时候也应该写.mix**



### 6.3 命名参数

在实参传递给形参值的时候可以不用按照顺序进行传递,而是**直接通过参数的指定进行传参**,这样就能够实现当只想

传递某些参数而不是所有参数的时候参数的值不产生混乱

**实参传入命名参数的时候和变量定义的时候是一样的**

```less

.father {
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
  .child1 {
    .mix(100px, 100px, pink);
  }
  .child2 {
    .mix(@b:red);/*直接通过命名参数传递*/
  }
}

```



### 6.4 匹配模式

用一些专门的标识符放在形参的首位用做匹配,注意**这个是标识符不用@符号**,当使用的时候会根据传入的第一个参

数,匹配到那个,就使用哪一个样式

```less
.float(l,@w){
    float:left
    width:@w
}
.float(r,@w){
    float:right
    width:@w
}
.box1{
        height: 200px;
        background: green;   
        .float(l,200px);
    }

.box2{
        width: 300px;
        height: 200px;
        background: green;
        .float(r,200px);
    }
/*
	匹配模式相当于if从句,当匹配到哪个正确的时候就会使用哪一个样式
*/
```



### 6.5 arguments变量

**arguments相当于是类中所有实参的集合,**其作用和js中的arguments类似

这个变量是less已经设置了的,**可以直接调用**它来使用,调用它的时候它包含了所有传入的实参,同时每个实参是通过

空格隔开的

```less
.border(@size,@style,@color){
    border:@arguments;
}
div{
 	width:200px;
 	height:200px;
   	.border(1px,solid,#000);
}
```

**注意:**

和js中不一样的是这个属性虽然保留着所有实参但是在命名类的时候还是需要写入对应的形参,不然会出现报错



## 7.less的运算

在less中可以进行加减乘除的运算

**注意:在less的计算中计算的双方只需要一方带单位,最后再计算的时候单位会保留下来加在新值的后面**

```less
.box{
    widht:(100+100px)
}
```



## 8.less的继承

less的继承性能上高于less的混合,但是灵活性弱于混合,继承不支持参数的形式,同时后面也不加(),继承的类是需要

被编译到CSS文件中的

**less的继承用关键词&:extend()函数来实现**,下面是一种用法

```less
.center {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}

.father {
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
  .child {
    &:extend(.center);
    &:nth-child(1) {
      width: 100px;
      height: 100px;
      background-color: pink;
    }
    &:nth-child(2) {
      width: 200px;
      height: 200px;
      background-color: red;
    }
  }
}
/*
	上面的.child通过继承效果能够得到.center里面的样式
	转换为CSS样式如下
*/
.center,
.father .child {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
.father {
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
}
.father .child:nth-child(1) {
  width: 100px;
  height: 100px;
  background-color: pink;
}
.father .child:nth-child(2) {
  width: 200px;
  height: 200px;
  background-color: red;
}
/*
	实质上继承是让两个选择器之间通过并集选择器把公共的代码合并起来
*/
```

**注意:**

继承如果只是传入了一个需要被继承的参数到extend函数里面,只会接受这一个选择器,与它相关的兄弟选择器如

:hover等是不会继承的,如果想要继承所有,需要在后面加上:空格all

```less
.center {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
.center:hover{
    background:black !important;/*因为权重不够,就这样写了*/	
}

.father {
  position: relative;
  margin: 200px auto;
  width: 200px;
  height: 200px;
  border: 1px solid #000;
  .child {
    &:extend(.center all);
    &:nth-child(1) {
      width: 100px;
      height: 100px;
      background-color: pink;
    }
    &:nth-child(2) {
      width: 200px;
      height: 200px;
      background-color: red;
    }
  }
}
/*
	最终.child能继承到:hover中的样式
*/
```



## 9.less的避免编译

**在less中如果想要一些值不被运算想要原封不动的变成CSS文件**,可以用~"值"的方式直接将值原封传递给CSS文件

```less
*{
    margin:~"calc(100px+100)";//calc()是CSS中本来就有的计算形式的函数
}
```



## 10.less的导入

在less中使用@import关键字能够将需要的文件中的代码导入到当前的less文件中

- 导入less文件可一直接导入

  ```less
  @import "a.less"
  ```

- 导入CSS文件时需要申明这是CSS文件

  ```less
  @import (CSS) "b.css"
  ```

  
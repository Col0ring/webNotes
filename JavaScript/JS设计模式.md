JS设计模式

## 1.什么是设计模式



### 1.1 设计模式

- 设计模式是**前人总结出的，解决开发中某类问题的方法**；

- 我们在过去的代码编写中已经接触过很多的设计模式了，只不过当时咱们不知道这就是一种设计模式而已；

- 设计模式之间并不是互相独立的，往往一个功能需要多个设计模式的配合实现；

- 每个设计模式所解决的问题肯定是不同的，根据这些模式的功能我们可以将他们分成很多几大类：创建型设计模式、结构型设计模式、行为型设计模式。当然在JavaScript里面还可以有其他的一些特殊的设计模式，我们课程中也会介绍到。



### 1.2 学前准备

- 原生JS的基础

- 面向对象相关的基础

- ES6基础



## 2.创建型设计模式

创建型设计模式 -- “创建”说明该类别里面的设计模式就是用来创建对象的，也就是在不同的场景下我们应该选用什么样的方式来创建对象。



### 2.1 单例模式

单例模式（Singleton）：确保某一个类只有一个实例。

JavaScript创建实例对象的方法有很多，所以很多写法我们都可以认为是单例模式：

- 场景1

  ```js
  /*
   我们以完成一个右下角弹窗的情景来理解
   封装一个弹窗功能，先不管样式等，页面中可以多次调用该弹窗
   弹窗出现的这个div是不必要每次调用都新建一个的，所有的弹窗使用统一的div就可以了，这就是很典型的单例
  */
  let Popup = document.createElement("div");
  Popup.show = function (arg) {
      this.innerText = arg;
      document.body.appendChild(this)
  };
  Popup.show("张三");//测试使用1
  Popup.show("李四");//测试使用2
  ```

  ```js
  /*
      上面做法在没有调用时就已经初始化DOM节点了，很显然，当我们使用时再初始化这样更好
      改良一下：
  */
  let Popup = (function () {
      let instance;
      return function () {
          if (!instance) {
              instance = document.createElement("div");
          }
          instance.show = function (arg) {
              this.innerText = arg;
              document.body.appendChild(this);
          };
          return instance;
      };
  })();
  Popup().show("张三");//测试使用1
  Popup().show("李四");//测试使用2
  console.log(Popup() === Popup());//测试单例
  ```

  ```js
  /*
      抽象化一下，写成类的形式：
  */
  let Popup = (function () {
      let instance = null;
      class Pop{
          constructor(){
              if( instance ){
                  return instance;
              }
              instance = this;
              this.ele = document.createElement("div");
              this.show = function (arg) {
                  this.ele.innerText = arg;
                  document.body.appendChild(this.ele);
              };
          }
      }
      return Pop;
  })();
  let a = new Popup();
  a.show("张三");//测试使用1
  let b = new Popup();
  b.show("李四");//测试使用2
  console.log(a === b);//测试单例
  ```

  ```js
  /*
      样式等其他因素稍微完善下：
  */
  let Popup = (function(){
      let instance = null;
      class Pop{
          constructor(){
              if( instance ){
                  return instance;
              }
              instance = this;
              //下面全是元素&样式的处理，理解单例的话就不用管下面了
              this.ele = document.createElement("div");
              this.ele.style.cssText = "position:fixed;bottom:-300px;right:0;z-index:999999999999999;width:260px;height:110px;padding:20px;background-color:#eee;box-shadow:0 0 2px #000;transition:bottom .5s;user-select:none;-moz-user-select:none;";
              this.timer1 = null;
              this.timer2 = null;
              this.show = function (arg) {
                  clearTimeout(this.timer1);
                  clearTimeout(this.timer2);
                  this.ele.innerText = arg;
                  document.body.appendChild(this.ele);
                  this.timer1=setTimeout(()=>{
                      this.ele.style.bottom = "0";
                  });
                  this.timer2=setTimeout(()=>{
                      document.body.removeChild(this.ele);
                      this.ele.style.bottom = "-300px";
                  },2000);
              }
          }
      }
      return Pop;
  })();
  
  let a = new Popup();
  let b = new Popup();
  console.log(a === b);
  document.onclick = function(){
      a.show("张三");
  };
  ```

  ```js
  //核心实现代码
  let Single = (function(){
      let instance = null;
      class S{
          constructor(){
              if(instance){
                  return instance;
              }
              instance = this;
              //code……
          }
      }
  })();
  //实例化测试
  console.log( new Single() === new Single() );
  ```

- 场景2

  JS里面我们可以直接定义一个对象字面量，很显然你定义的对象那肯定就只有一个，所以这样的形式我们也可以理解为单例：

  ```js
  let Single = {
      //code      
  };
  ```

  我们可以将需要用到的属性或方法全部设置到该对象里面，更广泛的运用就是我们见到过的--命名空间

  ```js
  let a = 10; 
  let b = 20;
  let c = true;
  let d = function(){};
  //全局变量非常宝贵，特别是多人协作开发的时候，为了避免和别人命名冲突啊，我们来换个方式定义
  ```

  ```jsx
  //你自己的变量全部放入一个对象里面，这样可以避免很多问题
  let person = {
      a : 10,
      b : 20,
      c : true,
      d(){
          //code
      }
  }
  ```

  ```js
  //当然，有些时候我们可能希望不是全部的变量都暴露出来可以访问，而是只有内部能访问，那我们可以这么写
  let person = (function(){
      let NUM = 10;//这个NUM外界就不能直接访问了，我们习惯于将静态变量全大写
      return {
          addNum(){
              return ++NUM;
          }
      }
  })();
  console.log( person.NUM ); //undefined
  console.log( person.addNum() ); // 11
  ```



**总结:当需求实例唯一、命名空间时，就可以使用单例模式。结合闭包特性，用途广泛。**



### 2.2 工厂模式



工厂模式（Factory）：定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。这里将**简单工厂模式**和**工厂方法模式**一起讲。

还有一种工厂模式叫做**抽象工厂模式**，JS里面其实这个模式的概念及运用也是很抽象的，基础的运用单独列出讲有点难受，放到结构性设计模式的组合模式再提及。

- 场景1

  ```js
  //卖小吃场景
  //牛排
  class Steak{
      constructor(){
          this.price = 30;
          this.time = 20;
      }
  }
  
  //烧烤
  class Grill{
      constructor(){
          this.price = 20;
          this.time = 15;
      }
  }
  
  //面条
  class Noodles{
      constructor(){
          this.price = 15;
          this.time = 10;
      }
  }
  
  let a = new Steak(); //老板来份牛排
  let b = new Grill(); //老板来份烧烤
  let c = new Noodles(); //老板来份面条
  
  //三个店跑来跑去太累了，客人买食物也不方便，开个总店吧：
  ```

  ```js
  //牛排
  class Steak{
      constructor(){
          this.price = 30;
          this.time = 20;
      }
  }
  
  //烧烤
  class Grill{
      constructor(){
          this.price = 20;
          this.time = 15;
      }
  }
  
  //面条
  class Noodles{
      constructor(){
          this.price = 15;
          this.time = 10;
      }
  }
  
  //总店面
  class Shop{
      constructor(name){
          let o = null
          switch(name){
              case "Steak":
                  o = new Steak();
                  break;
              case "Grill":
                  o = new Grill();
                  break;
              case "Noodles":
                  o = new Noodles();
                  break;
          }
          return o;
      }
  }
  
  //统一方法调用
  let a = new Shop("Steak"); //老板来份牛排
  let b = new Shop("Grill");//老板来份烧烤
  let c = new Shop("Noodles");//老板来份面条
  ```

  ```js
  //接口统一了但是还是不方便，首先有四个变量的出现，其次假设要新增菜谱呢？需要新添加一个类，还需要修改Shop的判断，改进一下：
  //总店面
  class Shop{
      constructor(name){
          return this[name].call({});
      }
      Steak(){//牛排
          this.price = 30;
          this.time = 20;
          return this;
      }
      Grill(){//烧烤
          this.price = 20;
          this.time = 15;
          return this;
      }
      Noodles(){//面条
          this.price = 15;
          this.time = 10;
          return this;
      }
  }
  
  //统一方法调用
  let a = new Shop("Steak"); //老板来份牛排
  let b = new Shop("Grill");//老板来份烧烤
  let c = new Shop("Noodles");//老板来份面条
  
  //这样的话，新增菜谱也就只需要在Shop里面新加入一个方法就可以了
  //当然，不是说非得把东西全塞到原型里面才叫工厂模式，我们所举的例子只是统一接口的一种方法，所有我们遇到的，其他统一接口的方式都可看成工厂模式。
  ```



**总结：**工厂模式就是使 同一类别 的 类 综合起来，以使接口统一方便调用，同时在修改以及扩展时更加方便。



### 2.3 建造者模式

建造者模式（Builder）：将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

看名称我们首先想到的就是造房子。建造者模式就像是施工团队，包工头和客户沟通了解了客户的建房需求后，在自己团队内部分发任务，将复杂的建房过程分解成若干小组，各小组分工合作最终得到需求的房子。

- **场景1**

```js
//建造房子场景
//建造者 - 施工团队
let Builder = function(){
    //成员01 -- 决定厅室
    function Rooms(member){
        if( member <= 0 ){
            throw new Error("入住人数错误！");
        }
        this.rooms = member>=3?3:2;
    }

    //成员02 -- 决定面积
    function FloorSpace(budget){
        if( (typeof budget !== "number") || Number.isNaN(budget) || (budget < 60) ){
            throw new Error("预算过低或错误！");
        }
        this.budget = budget/2;
    }

    //成员03 -- 整体风格
    function Style(style){
        this.style = style || "常规风格";
    }

    return class {
        //住几人，预算多少(万)，风格
        constructor(member, budget, style) {
            Rooms.call(this,member);
            FloorSpace.call(this,budget);
            Style.call(this,style);
        }
    };
}();

//包工头获取客户需求，然后建造房子
let house1 = new Builder(1,100,"小清新");//客户1的需求
let house2 = new Builder(4,200,"欧美");//客户2的需求
```

**建造者模式的定义:**将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。通过上面的例子我们对该解释也就有了一定的理解。其实类似于ajax的实现，发送请求返回数据 与 成功的处理函数这种也是分离状态，我们调用封装好的ajax传入不同的各类参数也可以看成建造者模式。



**总结：**当我们构造的对象，内部结构较复杂时，使用建造者模式将内部各模块分开创建就非常合适。



### 2.4 原型模式

原型模式（Prototype）：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

这个概念在JavaScript中和原型继承是同一个意思。

- 场景1

  ```js
  //父类
  class Parent{
      constructor(x){
          this.x = x;
      }
      showX(){
          alert( this.x );
      }
  }
  
  //子类1继承
  class ChildA extends Parent{
      constructor(x,y){
          super();
          this.y = y;
      }
      showY(){
          alert( this.y );
      }
  }
  //子类2继承
  class ChildB extends Parent{
      constructor(x,z){
          super();
          this.z = z;
      }
      showZ(){
          alert( this.z );
      }
  }
  ```

- 场景2

  ```js
  let obj = {
      sayHello(){
          alert( "Hello" );
      }
  };
  
  let objA = Object.create(obj,{
      name :{
          writable:true,
          configurable :true,
          enumerable:true,
          value : "AA"
      }
  });
  
  let objB = Object.create(obj,{
      name :{
          writable:true,
          configurable :true,
          enumerable:true,
          value : "BB"
      }
  });
  
  objA.sayHello()
  ```



**总结：**多个类使用到了相同的属性或方法，那我们就可以通过原型继承的方式来创造出类或者实例对象。



## 3. 结构性设计模式

结构性设计模式 -- 关注于如何将类或对象组合成更大的结构，以便在使用时更简化。

### 3.1 外观模式

外观模式（Facede）：为一组复杂的子接口提供一个更高级的统一接口，以便更方便的去实现子接口的功能。

JavaScrip最常见的外观模式就是对各种API的统一的兼容处理

- 场景1

  ```js
  //以添加事件为例：我们是不推荐直接 on+事件 的赋值形式添加事件的，因为这是DOM0级事件，下次再添加时就直接覆盖上一次的了，所以我们使用DOM2级事件添加方式 addEventListener，而IE是不兼容的，需要使用attachEvent，从而添加一个click的事件写法如下：
  function click(){
      //code……
  }
  
  if( document.addEventListener ){
      oDiv.addEventListener("click" , click, false);
  }else if(document.attachEvent){
      oDiv.attachEvent("onclick" , click);
  }else{
      oDiv.onclick = click;
  }
  //很显然每个事件都要写这么一堆是很麻烦的，我们都会封装一下：
  ```

  ```js
  function addEvent(dom , eName , fn){
      if( document.addEventListener ){
          dom.addaddEventListener(eName,fn,false);
      }else if( document.attachEvent ){
          dom.attachEvent("on"+eName,fn);
      }else{
          dom["on"+eName] = fn;
      }
  }
  //使用
  addEvent(oDiv , "click" ,click);
  function click(){
      //code……
  }
  //其它事件的添加也不同写很多
  addEvent(oDiv,"mouseenter",enter);
  function enter(){
      //code……
  }
  ```

- 场景2

  ```js
  //javascrip的兼容部分确实比较多，所以我们可以将众多兼容操作综合起来，这样就通过外观模式封装了一个小型的库：
  var F = {
      getDOM : function(selector){
          return document.querySelector(selector);
      },
      on: function(selector,eName,fn){
          var dom = this.getDOM(selector);
          if( document.addEventListener ){
              dom.addEventListener(eName,fn,false);
          }else if( document.attachEvent ){
              dom.attachEvent("on"+eName,fn);
          }else{
              dom["on"+eName] = fn;
          }
      },
      getStyle : function(selector,attr){
          var dom = this.getDOM(selector);
          if( window.getComputedStyle ){
              return getComputedStyle(dom)[attr];
          }else{
              return dom.currentStyle[attr];
          }
      }
      //code……
  }
  
  //使用
  F.on("#wrap","click" , function(){
      //code……
  })
  ```



**总结：**对外提供统一的接口，内部实现各种不同的差异处理。或是将各类子操作综合在一起，对外提供统一的使用接口。这就是外观模式。



### 3.2 适配器模式

适配器模式（Adapter）：将一个类的接口转换成另外一个接口，以满足用户需求，解决接口不一样而产生的兼容问题。

适配的概念我们都应该很了解，比如iphone耳机接口不合适需要搞个转化器来接耳机这种，这个转换线，就是一个典型的适配器。

- **场景1**

```js
//外观模式时，我们写过一个小型的库。而当我们的需求越来越高时，这个库可能已经满足不了各种其他的需求了，我们或许会需要jQuery来丰富某个功能。例如，F.getDOM 可能会不兼容IE，我们替换为jQuery的实现：
F.getDOM = function(selector){
    return $(selector)[0];
}
//这个适配器就替换了功能，之前的代码也不受影响
```

- 场景2

  ```js
  //某个项目中我们使用ajax请求，获取到了数据data，data是一个数组格式。而后端更新了，将返回的数据换成了键值对形式，此时修改原来写好的代码成本较大，所以我们可以加一段适配代码：
  $.ajax({
      //url type dataType……
      success : function (msg) {
          //以前的msg是数组格式： ["张三" , "18" , "未婚"]
          //后端数据格式更新为 {name:"张三",age:"18",marry:"未婚}
          //done函数代码已成一个完整的逻辑，修改接口参数类型的话很麻烦
          //所以我们添加适配器：
          msg = [msg.name,msg.age,msg.marry];
  
          //调用done
          done(msg);
      }
  });
  
  function done(msg) {
      for (let i = 0, length = msg.length; i < length; i++) {
          //dosomething……
      }
  }
  ```

- 场景3

  ```js
  function fn(name,age,marry,sex,index){
      //dosomething……
  }
  fn("张三",18,"未婚","男",1);
  //参数很多，传参时必须要保证顺序一致，还不能设置默认值，很麻烦，修改成：
  ```

  ```js
  function fn(options){
      let {name,age,marry,sex,index=1}=options;
      //dosomething……
  }
  //使用时，只需键值对应，无关乎顺序，并且可以在解构阶段给默认值
  fn({
      name:"张三",
      sex : "男",
      age : 18,
      marry : "未婚"
  });
  
  //这种写法我们很早就已经使用过，这其实也是一种参数适配器
  ```

当然我们举的例子都是适配器代码比较简单的，实际过程中可能需要的适配器代码会更多一些，但是原理是一样的。

**总结：**由于各种原因（结构升级，优化代码等），导致接口和之前的不一样，而重构整个代码是很麻烦的，所以我们使用适配器代码将接口转换一下以保证能正确的使用，这就是适配器模式的作用。



### 3.3 代理模式

代理模式（Proxy）：为对象提供一个代理，用来控制对这个对象的访问。

代理模式就是通过代理访问对象而不是直接访问对象。

- 场景1

  ```js
  //通过代理过滤不必要或者不允许的访问
  const WULA = {
      name : "张三",
      age : 18,
      sex : "男"
  };
  
  let Fn = function(info){
      let handler = info.index===222?{
          get(target,key){
              return target[key];
          },
          set(target,key,value){
              return target[key] = value;
          }
      }:{
          get(target,key){
              if( key === "age" ){
                  return "error";
              }
              return target[key];
          },
          set(target,key,value){
              console.warn("不允许修改属性！");
              return false;
          }
      };
      const Wula = new Proxy(WULA,handler);
  
      //code……
      console.log(Wula.age);
      console.log(Wula.name);
      Wula.age = 80;
      console.log(Wula.age);
  };
  
  //Fn通过参数信息，限制Fn内部能对WULA进行的操作
  //Fn({index:222});
  Fn({index:7});
  ```

- **场景2** 
  我们常用的图片延时加载，就是一个很典型的代理模式，使用一张loading图来代替真正的图片，而当条件满足时再去加载真正的src。这个就不上代码了。



**总结：**通过对访问的代理，我们可以用于 *远程代理、虚拟代理、安全代理、智能指引*。



### 3.4 装饰者模式

装饰者模式（Decorator）：在不改变原对象的基础上，对其进行包装拓展，以满足更复杂的需求。

听起来和继承有点像，但是更灵活一些

- 场景1

  ```js
  //Teacher类
  class Teacher{
      constructor(name,sex){
          this.name = name;
          this.sex = sex;
      }
      showName(){
          alert(this.name);
      }
  }
  
  //实例 、 使用……
  let afei = new Teacher("张三","男");//实例1
  let wula = new Teacher("李四","女");//实例2
  let zhuque = new Teacher("王五","女");//实例3
  let xinai = new Teacher("赵六","女");//实例4
  //code…… 这里的代码是和上面的实例相关的使用代码
  
  //现在需要对 person实例(或者个别实例)进行age、marry等属性的扩展
  //很显然此时不能直接扩展到Teacher，不然会影响其他实例
  //如果继承，那么需要重新实例化出新的，不必要且浪费
  //我们目前只需要在已有的实例上稍作装饰就能满足需求
  function Decorator(obj,age,marry){
      obj.age = age;
      obj.marry = marry;
      return obj;
  }
  Decorator(person,18,"未婚");
  //code…… 新属性的使用代码
  ```

更复杂的装饰者模式还可以继续抽象成类，实现对对象的扩展。

**总结：**装饰者模式就是一种更灵活的继承方案，对对象进行所需要的扩展而不用重新继承构造出新的实例。



### 3.5 桥接模式

桥接模式（Bridge）：将抽象部分与它的实现部分分离，使它们都可以独立的变化。

- 场景1

  桥接模式在我们处理事件监听的时候经常用到：

  ```js
  //一个经典的例子：
  //前:假设已经定义好事件绑定函数addEvent和ajax函数
  function addEvent(){/*……*/}
  function ajax(){/*……*/}
  
  addEvent(dom,"click",getBeerById);
  function getBeerById(e){
      var id = this.id;
      ajax("GET","beer.url?id="+id,function(rsp){
          console.log('Requested Beer: ' + rsp.responseText);
      });
  }
  //此时getBeerById是一个纯纯的事件函数，内部涉及到this的指向问题，调用起来局限性较大，只能当做事件函数才能正常的工作
  //如果你要单独的去测试该函数某个id值的返回，你需要写个点击事件或者改变this指向为拥有特定id值的对象，特别麻烦
  //所以作为API开发者来说，这不是一个好用的API，因为局限性太大太，我们可以把它分离一下：
  addEvent(dom,"click",getBeerByIdBridge);
  //这就是桥接器，将修改后的getBeerById和事件函数连接起来
  function getBeerByIdBridge(e){
      getBeerById(this.id,function(msg){
          console.log('Requested Beer: ' + msg);
      });
  }
  //直接传入id，this挂钩，任何场合传入id值就可以发送请求
  //和使用的场景关联更小，适用性就更大，后期扩展/测试时就变得更加的方便快捷
  function getBeerById(id,callback){
      ajax("GET","beer.url?id="+id,function(rsp){
          callback(rsp.responseText);
      });
  }
  ```

- 场景2

  用桥接模式联结多个类：

  ```js
  //基本类
  class A{
      //code……
  }
  class B{
      //code……
  }
  class C{
      //code……
  }
  class D{
      //code……
  }
  
  //桥接类
  class Bridge1{
      constructor(){
          this.w = new A();
          this.x = new B();
      }
      //code……
  }
  class Bridge2{
      constructor(){  
          this.w = new A();
          this.x = new B();
          this.y = new C();
      }
      //code……
  }
  class Bridge3{
      constructor(){
          this.x = new B();
          this.z = new D();
      }
      //code……
  }
  ```



**总结：**什么时候用到桥接？某些逻辑要扩展或者其他不希望和实现结构复杂时，可以单独抽象出来，再利用桥接使用，而抽象部分和实现部分又可以单独进行扩展。



### 3.6 组合模式

组合模式（Composite）：又称部分-整体模式，将对象组合成树形结构以表示“部分-整体”的层次结构，组合模式使得用户对单个对象和组合对象的使用具有一致性。

就是说不管是操作单个东西，还是操作多个东西操作方式都是一样的。例如生活中我们淘宝购物车付款，不管是一个商品还是多个商品都是一次性付款；清理硬盘文件，不管是删除单个文件还是一个包含多个文件的文件夹都是一样的操作。

- 场景1

  一听到树形结构，我们肯定是想到DOM树，是的没错，DOM天生就是属性的结构，所以我们在统一DOM节点操作方式的时候，就是一个非常典型的组合模式案例，比如说JQ：

  ```js
  //用JQ不管是操作一个节点还是多个节点，写法都是一样的
  //这就是一个典型的组合模式的实现
  $("#box").css("color" , 'red');
  $(".box").css("color" , "red");
  
  //我们来用原生js简单的模拟一下
  function $(selector){
      let ele = document.querySelectorAll(selector);
      let eleArr = [];
      if( ele instanceof NodeList){
          eleArr = [...ele];
      }else if( ele instanceof Node){
          eleArr = [ele];
      }
      return {
          css : function(attr,value){
              eleArr.forEach(function (e,i) {
                  e.style[attr] = value;
              });
          }
      }
  }
  ```

- 场景2

  ```js
  /*
    模拟一个简单的输出点餐订单场景：
    总订单包含若干大类（如：主菜、甜品、饮料等），每个大类里面对应有若干商品
  */
  /*
    模拟一个简单的输出点餐订单场景：
    总订单包含若干大类（如：主菜、甜品、饮料等），每个大类里面对应有若干商品
   */
  //抽象类
  //没有实例化的价值，仅仅作为父类继承用，作用是统一API接口，并提醒规避错误，这也就是我们之前说到的抽象工程模式的作用
  class AbsMenu{
      constructor(){}
      add(){
          throw new Error("如需使用请重写！");
      }
      price(){
          throw new Error("如需使用请重写！");
      }
  }
  
  //创建最基础的商品类
  class Item extends AbsMenu{
      constructor(name,spicy,price){
          super();
          this.name = name;
          this.spicy = spicy;
          this.price = price;
      }
      print(){
          console.log(this.name+":"+this.spicy+"-----"+this.price+"元");
      }
  }
  
  //创建大类
  class Category extends AbsMenu{
      constructor(name){
          super();
          this.name = name;
          this.children = [];
      }
      add(child){
          this.children.push(child);
          return this;
      }
      print(){
          console.group(this.name+"类别");
          this.children.forEach((item)=>{
              item.print();
          });
          console.groupEnd();
      }
  }
  
  //创建主订单
  class Menu extends AbsMenu{
      constructor(number){
          super();
          this.number = number;
          this.children = [];
      }
      add(child){
          this.children.push(child);
          return this;
      }
      print(){
          console.group(this.number+"号桌订单");
          this.children.forEach((item)=>{
              item.print();
          });
          console.groupEnd();
      }
  }
  
  //订单实例
  let a = new Menu("8").add(
      new Category("饮料").add(
          new Item("括落","无","5")
      ).add(
          new Item("昏哒","无","5")
      )
  ).add(
      new Category("主菜").add(
          new Item("辣椒炒肉","不要辣椒","20")
      ).add(
          new Item("白菜","猛辣","10")
      )
  );
  
  a.print();
  ```



**总结：**想构建整体-部分的结构时使用，忽略单个对象和多个对象的使用区别，统一调用接口。同时我们也需要注意的时实际面临这种问题时，并没有如此简单，比如我们把上面的console如果改成打印HTML的话，就会复杂很多，不过最基础的构建做好之后，后面就会好写很多。



### 3.7 享元模式



享元模式（Flyweight ）：通过共享大量细粒度的对象，避免拥有相同内容造成额外的开销。



也就是说享元模式是一种代码优化策略，再浅显点解释就是：相同的部分提出来或者采用其他形式优化掉。



- 场景1

  ```js
  //最常见的享元模式01
  //提出相同事件函数，只需要定义一个函数就能满足所有人的需求
  a.onclick = function(){alert(this.name)};
  b.onclick = function(){alert(this.name)};
  c.onclick = function(){alert(this.name)};
  
  //提出后
  let clickEvent = function(){alert(this.name)};
  a.onclick = b.onclick = c.onclick = clickEvent;
  ```

  ```js
  //最常见的享元模式02
  //事件委托
  //code....
  //ps:这个很好理解，代码就不写了
  ```

- 场景2

  ```js
  /*
    现在我们要排出一周100节课的上课信息
    信息包含老师信息，时间信息等
  */
  //课程类
  class ClassInfo{
      constructor(id,name,sex,time){
          this.TID = id;
          this.TName = name;
          this.TSex = sex;
          this.time = time;
      }
      getInfo(){
          return this.time + "，工号："+this.TID+"，"+this.TName+"老师，TA是一位厉害的"+this.TSex+"老师";
      }
  }
  //实例
  let classList = [
      new ClassInfo("02050","张三","男","周一 14:30"),
      new ClassInfo("02051","李四","女","周一 15:30"),
      new ClassInfo("02052","王五","男","周一 16:30"),
      new ClassInfo("02053","赵六","女","周一 17:30"),
      new ClassInfo("02050","张三","男","周一 18:30"),
      new ClassInfo("02052","王五","男","周一 19:30"),
      new ClassInfo("02051","李四","女","周一 20:30"),
      new ClassInfo("02050","张三","男","周一 21:30"),
      //………………
  ];
  //console.log(c1.getInfo());
  
  
  //很显然，每一次实例的时候我们都重新创建了老师的信息，然而这一部分的信息是会有重复的，因为总共就4位老师
  //这时候我们就可以使用享元模式来优化代码：
  ```

  ```js
  //定义基础的老师类
  class Teacher{
      constructor(id,name,sex){
          this.TID=id;
          this.TName=name;
          this.TSex = sex;
      }
      getInfo(){
          return "工号："+this.TID+"，"+this.TName+"老师，TA是一位厉害的"+this.TSex+"老师哦！";
      }
  }
  //单例模式检测确保老师信息只创建一次
  let SingleTeacher=(function(){
      let teacher = {};
      return {
          create : function(id,name,sex){
              if( !teacher[id] ){
                  teacher[id] = new Teacher(id,name,sex);
              }
              return teacher[id];
          }
      };
  })();
  //再定义课程信息类
  class ClassInfo{
      constructor(id,name,sex,time){
          this.teacherInfo = SingleTeacher.create(id,name,sex);
          this.time = time;
      }
      getInfo(){
          return this.time+"，"+this.teacherInfo.getInfo();
      }
  };
  
  //实例
  let classList = [
      new ClassInfo("02050","张三","男","周一 14:30"),
      new ClassInfo("02051","李四","女","周一 15:30"),
      new ClassInfo("02052","王五","男","周一 16:30"),
      new ClassInfo("02053","赵六","女","周一 17:30"),
      new ClassInfo("02050","张三","男","周一 18:30"),
      new ClassInfo("02052","王五","男","周一 19:30"),
      new ClassInfo("02051","李四","女","周一 20:30"),
      new ClassInfo("02050","张三","男","周一 21:30"),
      //………………
  ];
  
  console.log(classList[0].getInfo());
  ```



**总结：**享元模式就是把我们说过的封装的概念运用的更高级点~同时呢，一般都会需要配合单例模式来实现。



## 4.行为型设计模式

行为型设计模式:不单单只涉及到类和对象，更关注于类或对象之间的通信交流。

### 4.1 模板方法模式

模板方法模式（Template Method）：父类中定义一组操作算法骨架，而将一些实现步骤延迟到子类中，使得子类可以不改变父类的算法结构的同时可以重新定义算法中某些实现步骤。

模板方法模式是代码复用的基础技术，在写类库的时候非常重要。

- **场景一**

```js
//定义父类
function Person(name, age, gender) {
      this.name = name;
        this.age = age;
        this.gender = gender;
}
Person.prototype.sayHello=function(){
    alert("hello");
}
//定义子类
function Student(name,age,gender,grade){
      Person.call(this,name,age,gender);
      this.grade=grade;
}
//赋值原型
Student.prototype=new Person();
```



**总结：**优化结构最基础的技术。



### 4.2 观察者模式

观察者模式（Observer）：又叫发布-订阅模式，定义了一种一对多的关系，让多个观察者对象同时监听某一个对象，当该对象发生改变时，多个观察者对象也做出相应的改变。

就比如你关注了一个主播，当然还有很多人也关注了这个主播，当这个主播发布开播信息时，所有关注她的人都会收到消息然后有的人开电脑看，有的打开app看等

**组成该模式的有两个关键部分：多个订阅者 和 消息发布者。**

- **场景1**

``` js
//事件绑定的机制，其实就是一种观察者模式
//事件触发的时候（发布消息）就执行对应的 事件函数（订阅者）
//code...
//ps:代码省略..
```

- 场景2

  我们先来讲观察者模式的基本式，从简单的功能到复杂的实现。

  ```js
  //定义基础观察者对象
  let Observer = (function(){
      let QUEUE = []; //利用闭包定义QUEUE数组，用来存储订阅者，并防止外界修改
      return {
          //订阅接口
          subscribe(){},
          //发布接口
          trigger(){}
      };
  })();
  
  //实际中我们肯定还需要退订、不同订阅类别、筛选等等其他功能，我们先从基础的实现讲起，后面我们再完善功能。
  ```

  ```js
  //基础版完善+订阅发布演示
  //定义观察者对象
  let Observer = (function(){
      let QUEUE = []; //利用闭包定义QUEUE数组，用来存储订阅者，并防止外界修改
      return {
          //订阅接口
          subscribe(fn){
              QUEUE.push(fn);
              console.log("订阅成功！");
          },
          //发布接口
          trigger(msg){
              QUEUE.forEach((fn,index)=>{
                  fn(msg,index);
              });
          },
      };
  })();
  
  //订阅01
  Observer.subscribe(function(data){
      console.log("1111 我已经订阅（关注）这个主播了，所以当发布（开播）的时候就可以看到这句话哦！","同时，我接受到了发布时给我的参数data："+data);
  });
  //订阅02
  Observer.subscribe(function(data){
      console.log("2222 我，已订阅，发布后，执行","接受到，data："+data);
  });
  
  //发布，发布代码的位置是根据你的需求来定的，比如你需要在ajax请求之后发布消息，那就写在ajax的成功函数里面，你要在某个事件触发之后，就写在事件函数里面等等
  Observer.trigger("开播啦！开播啦！--<发布（开播）的同时给的数据>");
  ```



**下面看个案例：ajax请求成功的同时，页面多处内容都需要更新**

```html
<!DOCTYPE HTML>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <meta name="keywords" content="关键词">
        <meta name="description" content="描述">
        <meta name="author" content="作者">
        <style>
            body{font-family: "Microsoft YaHei",serif;}
            body,dl,dd,p,h1,h2,h3,h4,h5,h6{margin:0;}
            ol,ul,li{margin:0;padding:0;list-style:none;}
            img{border:none;}
            i{
                font-weight: bolder;
                color: pink;
            }
        </style>
    </head>
    <body>
        <input type="button" value="按钮">
        <div id="wrap"></div>
        <div id="main"></div>

        <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
        <script>
            (function(){
                //定义观察者对象
                let Observer = (function(){
                    let QUEUE = []; //利用闭包定义QUEUE数组，用来存储订阅者，并防止外界修改
                    return {
                        //订阅接口
                        subscribe(fn){
                            QUEUE.push(fn);
                        },
                        //发布接口
                        trigger(msg){
                            QUEUE.forEach((fn,index)=>{
                                fn(msg,index);
                            });
                        },
                    };
                })();
                //其他变量
                let $button = $("input"),
                    $wrap = $("#wrap"),
                    $main = $("#main");

                //订阅
                Observer.subscribe(function(data){
                    let strUl = "<ul>";
                    data.forEach(dataItem=>{
                        strUl += `<li>姓名：${dataItem.name}，年龄：${dataItem.age}</li>`;
                    });
                    strUl += "</ul>";
                    $wrap.html(strUl);
                });
                Observer.subscribe(function(data){
                    let html = "";
                    data.forEach(dataItem=>{
                        html += `<p><i>姓名</i>：<span>${dataItem.name}</span>，<i>年龄</i>：<span>${dataItem.age}</span></p>`;
                    });
                    $main.html(html);
                });

                //点击触发ajax且发布
                $button.click(function () {
                    $.ajax({
                        type : "GET",
                        url : "test.txt",
                        success : function (msg) {
                            //发布
                            Observer.trigger(JSON.parse(msg));
                        }
                    });
                });
            })();
        </script>
    </body>
</html>
```

基础的实现差不多理解了吧，接下来我们将功能更完善点：

```js
//考虑到代码多个逻辑都需要用到观察者模式，我们肯定是将之定义为类更合适
//只不过此时要让存储队列的变量变成不可访问的话，就有点蛋疼了，不过也不用太担心这种情况，毕竟自己构建的逻辑自己还是能遵守的
class Observer{
    constructor(){
        //对象来存储订阅部分，这样可以分开为多个队列订阅
        this.QUEUE = {};
        this.id = 0;
    }
    //订阅 -- 订阅的参数变成两个，这样不仅仅可以存入执行的函数，还是能存入订阅的类别信息
    //也就是说，现在有多个直播的小姐姐了，你可以选择性的关注
    subscribe(type,fn){
        //如果第一次订阅，初始化类别队列
        if( !this.QUEUE[type] ){
            this.QUEUE[type] = {};
        }
        this.id ++;
        this.QUEUE[type][this.id] = fn;
        return this.id;
    }
    //发布 -- 第一个参数表示哪个主播，第二个参数表示附带的数据
    trigger(type , data){
        let que = this.QUEUE[type];
        if( !que )return;
        for (let [id,item] of Object.entries(que)) {
            item(data,id);
        }
    }
    remove(type,id){
        if(!this.QUEUE[type] || !this.QUEUE[type][id])return;
        Reflect.deleteProperty(this.QUEUE[type],id);
    }
}

//实例
let observer1 = new Observer();
//订阅
observer1.subscribe("wula" , function (data,id) {
    console.log("ID为"+id+"的订阅执行了！")
});
let x = observer1.subscribe("wula" , function (data,id) {
    console.log("ID为"+id+"的订阅执行了！")
});
observer1.subscribe("xinai" , function (data,id) {
    console.log("ID为"+id+"的订阅执行了！")
});
observer1.subscribe("zhuque" , function (data,id) {
    console.log("ID为"+id+"的订阅执行了！")
});

console.log(observer1);
observer1.trigger("wula","数据")
observer1.remove("wula","2");
observer1.trigger("wula","数据")
```



**总结：**怎么样，观察者模式的这种思想这种结构很棒吧，普通的封装写法也能实现上述的功能，但是想想看需要添加订阅的时候，是不是方便很多呢，不用改来改去，只需要一个subscribe接口就搞定了



### 4.3 状态模式

状态模式（State Pattern）：当对象内部状态发生改变时，它的行为也对应的发生改变，使之看起来像是改变了这个对象。

当需求有多种状态，并在某些条件下会从一种状态变成另一种状态时，使用状态模式就很合适。

- 场景1

  ```js
  //开关灯状态的切换
  //普通写法
  //这是一个只有两个状态的模型，多个状态写法也是一样的
  let oBtn = document.getElementById("btn");
  let state = "off";
  let switchFn = function(){
      if( state === "off" ){
          console.log("之前是off状态，现在变成on状态");
          state = "on";
      }else if( state === "on" ){
          console.log("之前是on状态，现在变成off状态");
          state = "off";
      }
  };
  oBtn.onclick = switchFn;
  ```

  假设某个状态对应的代码需要修改，或者需要添加新的状态，就得修改整个switchFn，状态与状态模块之间不够独立，很显然它不是一个稳定的方法，违背了开闭原则。我们将这种状态模型的代码修改的好看点：

  ```js
  let Switch = {
      //状态机
      FSM : {
          on : {
              to : "off",
              action : function(){
                  console.log("从on变到off");
              }
          },
          off : {
              to : "on",
              action : function(){
                  console.log("从off变到on");
              }
          }
      },
  
      //当前状态
      currentState : "off",
  
      //初始化事件
      init(){
          let oBtn = document.getElementById("btn");
          oBtn.onclick = this.transition.bind(this);
      },
  
      //状态切换
      transition(){
          let s = this.FSM[this.currentState];
          this.currentState = s.to;
          s.action();
      }
  };
  ```

- 场景2

  Javascript Finite State Machine

  ,这是一个有限状态机的js库，可以让我们非常方便的创建状态模式。

  外部引入：`<script src="state-machine.min.js"></script>`

  **注:**npm等其他使用方法可以查看Readme

  ```js
  //状态机实例
  let fsm = new StateMachine({
      //初始状态
      init: 'solid',
  
      //状态切换的规律
      transitions: [
          { name: 'melt',     from: 'solid',  to: 'liquid' },
          { name: 'freeze',   from: 'liquid', to: 'solid'  },
          { name: 'vaporize', from: 'liquid', to: 'gas'    },
          { name: 'condense', from: 'gas',    to: 'liquid' }
      ],
  
      //对应的需要执行的方法
      methods: {
          onMelt:     function() { console.log('I melted')    },
          onFreeze:   function() { console.log('I froze')     },
          onVaporize: function() { console.log('I vaporized') },
          onCondense: function() { console.log('I condensed') }
      }
  });
  ```



**总结：**哪些情况适用状态模式 :1.对象的行为取决于它的状态，对应的操作会改变它的状态。2.大量的判断操作，这些分支语句可以视作对象的状态。



### 4.4 策略模式

策略模式（Strategy）：策略模式定义了一系列的算法，并将每一个算法封装起来，而且使他们可以相互替换，且具有一定的独立性，不会随客户端变化而变化。

一系列的算法，可以相互替换，也就是说为了同一个目的，可能采取的算法不一样，同时要体现出*独立性*。

- 场景1

  基础的理解

  ```js
  //Tween.js运动框架就是一套非常典型的策略模式，目的性都是达到终点，但是根据参数选择的不同而采用不同的算法
  //code...
  //ps:代码省略..
  ```

- 场景2

  ```js
  //玩家类
  class Player{
      constructor(){
          this.totalCost = 0;
          this.level = "C";
      }
      consum(price){
          this.totalCost += price;
          let result = Strategy.calc(this.level , price);
          this.getLevel();
          return result;
      }
      getLevel(){
          let totalCost = this.totalCost;
          if( totalCost >= 50000 ){
              this.level = "S";
          }else if( totalCost >= 30000 ){
              this.level = "A";
          }else if( totalCost >= 20000 ){
              this.level = "B";
          }else{
              this.level = "C";
          }
      }
  };
  
  //计价策略类
  let Strategy = (function(){
      //策略
      let s = {
          S(price){
              return price*0.85;
          },
          A(price){
              return price*0.9;
          },
          B(price){
              return price*0.95;
          },
          C(price){
              return price;
          }
      };
      return {
          //添加新策略的接口
          addsty(name , fn){
              s[name] = fn;
          },
          //计算策略对应最终价格的接口
          calc(sty,price){
              if( s[sty] ){
                  return s[sty](price);
              }else{
                  throw new Error("对应的优惠策略不存在！");
              }
          }
      };
  })();
  
  
  //测试：
  let afei = new Player();
  console.log(afei.consum(20));
  console.log(afei.consum(10000));
  console.log(afei.consum(50000));
  console.log(afei.consum(200));
  ```



### 4.5 命令模式

命令模式（Command）：将请求与实现解耦并封装成独立对象，从而使不同的请求对客户端的实现参数化。

- 场景1

  下面的例子来自《javascript设计模式》 -- 张容铭

  ```js
  let CanvasCmd = (function(){
      let canvas = document.getElementById("canvas"),
          ctx = canvas.getContext("2d");
      let cmdList = {
          beginPath(){
              console.log(1);
              ctx.beginPath();
          },
          close(){
              ctx.closePath();
          },
          strokeStyle(color){
              ctx.strokeStyle = color;
          },
          moveTo(x,y){
              console.log(x,y);
              ctx.moveTo(x,y);
          },
          lineTo(x,y){
              console.log(x,y);
              ctx.lineTo(x,y);
          },
          stroke(){
              console.log(1);
              ctx.stroke();
          },
          fillStyle(color){
              ctx.fillStyle = color;
          },
          fillRect(x,y,width,height){
              ctx.fillRect(x,y,width,height);
          },
          strokeRect(x,y,width,height){
              ctx.strokeRect(x,y,width,height);
          },
          arc(x,y,r,begin,end,dir){
              ctx.arc(x,y,r,begin,end,dir);
          }
      };
      return {
          execute(data){
              data.forEach(item=>{
                  let {command,param} = item;
                  cmdList[command] && cmdList[command](...param);
              });
          },
          addCmd(key,value){
              cmdList[key] = value;
          },
          removeCmd(key){
              Reflect.deleteProperty(cmdList,key);
          }
      };
  })();
  
  //命令测试
  CanvasCmd.execute([
      {command:"beginPath",param:[]},
      {command:"strokeStyle",param:["red"]},
      {command:"moveTo",param:[10,10]},
      {command:"lineTo",param:[100,100]},
      {command:"stroke",param:[]}
  ]);
  ```



### 4.6 职责链模式

职责链模式（Chain of responsibility）：是使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递请求，直到有一个对象处理它为止。

我们接触过作用域链、原型链，回想一下概念，都是沿着链找直到找到为止。职责链也就是要构建这样一个结构，一层一层的传递请求直到处理了为止。

- 场景1

  情景：要提交一个申购申请，小于10000，部门负责人处理；大于10000小于50000，院负责人处理；大于50000小于100000群负责人处理；大于100000，董事长处理。

  我们先按找平常的写法实现

  ```js
  function request(value){
      if( value <= 10000 ){
          console.log("移交部门负责人处理。");
          //some code……
      }
      else if( value <= 50000 ){
          console.log("移交院负责人处理。");
          //some code……
      }
      else if( value <= 100000 ){
          console.log("移交群负责人处理。");
          //some code……
      }
      else if( value > 100000 ){
          console.log("移交董事长处理。");
          //some code……
      }
  }
  
  request(20000);
  ```

  需求虽然可以解决，但是if else的结构看起来未免也太麻烦了，并且每个分支要修改的话必须进入request函数，违反了开闭原则，我们使用职责链模式来改写一下代码：

  ```js
  //将每个分支结构分离：
  function director01(value){
      if( value <= 10000 ){
          console.log("移交部门负责人处理。");
          //some code……
      }else{
          //移交给下一个处理人
          director02(value);
      }
  }
  function director02(value){
      if( value <= 50000 ){
          console.log("移交院负责人处理。");
          //some code……
      }else{
          //移交给下一个处理人
          director03(value);
      }
  }
  function director03(value){
      if( value <= 100000 ){
          console.log("移交群负责人处理。");
          //some code……
      }else{
          //移交给下一个处理人
          director04(value);
      }
  }
  function director04(value){
      if( value > 100000 ){
          console.log("移交董事长处理。");
          //some code……
      }
  }
  
  //只需要从第一个处理人开始
  director01(20000);
  ```

  现在这样的结构就把每个分支分开了，耦合程度比上面的写法要好很多。但是每个处理人内部都必须强关联下一个处理人，不然就没法链式调用，这又是一个问题，层层之间的耦合还是很高，假设要添加一个处理人，那要改的地方就有上下两处了。我们将代码继续修改一下：

  ```js
  //构建一个通用的职责链类：
  class Chain{
      constructor(){
          this.successor = [];
          this.length = 0;
      }
      setSuccessor(...rest){
          this.successor = rest;
          this.length = rest.length;
      }
      request(...rest){
  
          (function getResult(index){
              if( index >= this.length ){
                  return "无法处理。";
              }
              let result = this.successor[index](...rest);
              if( result === "next" ){
                  index ++;
                  getResult.call(this,index);
              }else{
                  return result;
              }
          }).call(this,0);
      }
  };
  
  //将每个分支结构分离：
  function director01(value){
      if( value <= 10000 ){
          console.log("移交部门负责人处理。");
          //some code……
      }else{
          //无须再强关联下一个处理函数
          return "next";
      }
  }
  function director02(value){
      if( value <= 50000 ){
          console.log("移交院负责人处理。");
          //some code……
      }else{
          //无须再强关联下一个处理函数
          return "next";
      }
  }
  function director03(value){
      if( value <= 100000 ){
          console.log("移交群负责人处理。");
          //some code……
      }else{
          //无须再强关联下一个处理函数
          return "next";
      }
  }
  function director04(value){
      if( value > 100000 ){
          console.log("移交董事长处理。");
          //some code……
      }
  }
  
  //实现链
  let request = new Chain();
  //设置处理队列
  request.setSuccessor(
      director01,
      director02,
      director03,
      director04
  );
  //调用
  request.request(20000);
  request.request(80000);
  request.request(800000);
  ```



### 4.7 访问者模式

访问者模式（Visitor）：在不改变对象的前提下，定义作用于对象的新操作。

这个模式在其他的强语言里面实现起来还是比较麻烦的，但是再javaScript中，就非常简单了

- 场景1

  ```js
  let person = {
      addAttr(){
          this.zhangsan = "张三";
          this.lisi = "李四";
      }
  };
  person.addAttr();
  //对象person，拥有addAttr方法，作用是给对象添加两个属性
  
  //现在我们需要给对象student，也添加这个两个属性，但是不希望student拥有addAttr的方法
  let student = {};
  person.addAttr.call(student);
  //很简单的我们通过call就可以解决
  //所以说JavaScript里面实现访问者模式是非常简单的，借助于call/apply就可以实现
  ```

- 场景2

  JavaScript里面很多原生的API都是采用的访问者模式，比如数组相关的操作

  ```js
  //数组有push方法，现在我们希望普通的对象 x 也有push方法能添加数字属性和length
  let Visitor = {
      push(obj , value){
          return Array.prototype.push.call(obj,value);
      }
  }
  let x = {};
  Visitor.push(x,5);
  console.log(x);
  ```



### 4.8 中介者模式

中介者模式（Mediator）：通过中介者对象封装一系列对象之间的交互，使对象之间不再相互引用，降低他们之间的耦合。

就像房屋中介一样，有很多人需要买房租房，有很多人需要卖房或者出租，还有的人需要卖房然后买个更大的房。这时候如果买家卖家之间互相沟通，那不仅费时费力，并且还不一定找的到最满意最合适的。房屋中介的作用就体现出来了。中介者模式就是解决多个对象之间相互交互的问题，只需要访问同一个中介，就可以解决所有的问题，把多对多复杂的问题变成一对多相对简单的问题。

- 场景1

  ```js
  var playerDirector= ( function(){
      var players = {}, // 保存所有玩家
          operations = {}; // 中介者可以执行的操作
      /****************新增一个玩家***************************/
      operations.addPlayer = function( player ){
          var teamColor = player.teamColor; // 玩家的队伍颜色
          players[ teamColor ] = players[ teamColor ] || []; // 如果该颜色的玩家还没有成立队伍，则新成立一个队伍
          players[ teamColor ].push( player ); // 添加玩家进队伍
      };
      /****************移除一个玩家***************************/
      operations.removePlayer = function( player ){
          var teamColor = player.teamColor, // 玩家的队伍颜色
          teamPlayers = players[ teamColor ] || []; // 该队伍所有成员
          for ( var i = teamPlayers.length - 1; i >= 0; i-- ){ // 遍历删除
              if ( teamPlayers[ i ] === player ){
                  teamPlayers.splice( i, 1 );
              }
          }
      };
      /****************玩家换队***************************/
      operations.changeTeam = function( player, newTeamColor ){ // 玩家换队
          operations.removePlayer( player ); // 从原队伍中删除
          player.teamColor = newTeamColor; // 改变队伍颜色
          operations.addPlayer( player ); // 增加到新队伍中
      };
  
      operations.playerDead = function( player ){ // 玩家死亡
          var teamColor = player.teamColor,
          teamPlayers = players[ teamColor ]; // 玩家所在队伍
          var all_dead = true;
          for ( var i = 0, player; player = teamPlayers[ i++ ]; ){
              if ( player.state !== 'dead' ){
                  all_dead = false;
                  break;
              }
          }
          if ( all_dead === true ){ // 全部死亡
              for ( var i = 0, player; player = teamPlayers[ i++ ]; ){
                  player.lose(); // 本队所有玩家lose
              }
              for ( var color in players ){
                  if ( color !== teamColor ){
                      var teamPlayers = players[ color ]; // 其他队伍的玩家
                      for ( var i = 0, player; player = teamPlayers[ i++ ]; ){
                          player.win(); // 其他队伍所有玩家win
                      }
                  }
              }
          }
      };
      var reciveMessage = function(){
          var message = Array.prototype.shift.call( arguments ); // arguments 的第一个参数为消息名称
          operations[ message ].apply( this, arguments );
      };
  
      return {
          reciveMessage: reciveMessage
      }
  })();
  ```



### 4.9 备忘录模式

备忘录模式（Memento）：在不破坏对象封装性的前提下，在对象之外捕获并保存该对象内部的状态以便日后使用。

- 场景1

  在写练习题的时候，每一题后面可以点击查看答案然后得到后端返回的结果，再点可以收起。如果收起之后再点查看答案，就再需要继续发送ajax请求去拿结果，这是没必要的，我们可以把已经请求过的结果先存起来，下次请求之前先进行判断看是否已经得到过数据，再决定是否需要ajax。

  很多分页的按钮原理也是一样，点击下一页，发送请求拿到数据，再点上一页就没必要发送请求了，因为数据已经展示过，保存起来再用现成的数据就可以了。减少请求也是优化的关键一步。

  ```js
  let request = function (){
      let cache = {};
      return function(key){
          if(!cache[key]){
              //缓存里面没有数据 -- 发送请求成功后再处理数据，并放入缓存
              $.ajax({
                  //参数省略
                  
                  success(data){
                      //处理data
                      result(data);
                      
                      //缓存data
                      cache[key] = data;
                  }
              });
          }else{
              //缓存里面有数据 -- 直接处理
              result(data);
          }
      }
  }();
  ```



### 4.10 迭代器模式

迭代器模式（Iterator）：在不暴露对象内部结构的同时，可以顺序地访问集合对象内部的各个元素。

看名字我们就可以发现，其实我们已经遇到过很多原生的迭代器了，比如forEach，every，map等等。在ES6里面迭代器的概念就更明显了，很多功能都是基于迭代器来实现的。

- 内部迭代器

  迭代的过程在方法内部完成，也就是说我们不需要管迭代的进度，也没法管，比如forEach：

  ```js
  [...document.getElementsByTagName("p")].forEach(item=>{
      item.onclick = function(){alert("TEST")};
  });
  ```

  因为forEach不兼容低版本IE，我们可以自己来简单实现：

  ```js
  Array.prototype.forEach = function(callBack){
      for(var i=0;item=this[i++];){
          callBack(item,i,this);
      }
  };
  ```

- 外部迭代器

  需要我们操作，才会进行下一次的迭代，ES6里面原生就已经实现了这个功能，通过.next() 操控迭代器对象：

  ```js
  let arr = [1,2,3];
  let arrIterator = arr[Symbol.iterator]();
  
  console.log( arrIterator.next() ); //{value:1,done:false}
  console.log( arrIterator.next() ); //{value:2,done:false}
  console.log( arrIterator.next() ); //{value:3,done:false}
  console.log( arrIterator.next() ); //{value:undefined,done:true}
  ```

  自己实现一个外部迭代器：

  ```js
  var Iterator = function(arr){
      var index = 0,
          length = arr.length;
      return {
          next : function(){
              return {
                  value : arr[index++],
                  done : index >= length
              };
          }
      }
  }
  ```

- JQ里的迭代器

  JQ里整个操作都是基于迭代器完成的，这个功能归功于它的each方法。当然，这是一个内部迭代器：

  ```js
  $.each([1,2,3],function(item,index){
      console.log(item);
      console.log(index);
  });
  ```

- **对象相关的迭代器与迭代器函数** 
  ES6定义了Object.keys等相关的方法，让我们能方便的遍历对象的属性或者值。同时也定义了forof循环，能让外部迭代器直接遍历相当于内部迭代器。ES6同时也定义了Generator函数和async函数，能让我们手动的控制函数执行的流程。



### 4.11 解释器模式

解释器模式（Interpreter）：定义一种文法的表示，并定义一种解释器，通过这个解释器来解析对应文法的内容。

- 场景1

  jQ提供的选择器

  我们知道原生js获取DOM元素的方式是有限的，而JQ给我们提供了更为复杂的选择方式，比如：`$("#wrap p:odd")`，这个参数规则就是JQ定义的一套选择元素的语法，而最终解析这个字符串，使之能获取对应DOM元素的代码就是解释器。

  ```html
  <!DOCTYPE HTML>
  <html>
      <head>
          <meta charset="UTF-8">
          <title>Title</title>
          <meta name="keywords" content="关键词">
          <meta name="description" content="描述">
          <meta name="author" content="作者">
          <style>
              body{font-family: "Microsoft YaHei",serif;}
              body,dl,dd,p,h1,h2,h3,h4,h5,h6{margin:0;}
              ol,ul,li{margin:0;padding:0;list-style:none;}
              img{border:none;}
          
          </style>
      </head>
      <body>
          <div id="wrap">
              <!-- p>i*3 -->
              <!-- <p><i></i><i></i><i></i></p> -->
          </div>
          
          <script>
  
              let Interpreter = (function(){
  
                  let createDOM = function( domARR ){
                      let domF = document.createDocumentFragment();
                      let thisArr = domARR.shift();
                      thisArr.forEach(item=>{
                          let dom = document.createElement(item);
                          if( domARR.length ){
                              dom.appendChild( createDOM([...domARR]) );
                          }
                          domF.appendChild(dom);
                      });
                      return domF;
                  };
  
                  return function(dom,selector){
  
                      //先切割找到对应的结构
                      let selectorArr = selector.split(">");
                      let domArr = [];
  
                      selectorArr.forEach(item=>{
                          let x = item.split("*");
                          if(!x[1]){
                              x[1] = 1;
                          }
                          x[1] = parseInt(x[1]);
                          let y = [];
                          for(let i=0;i<x[1];i++){
                              y.push(x[0]);
                          }
                          domArr.push(y);
                      });
  
                      dom.append(createDOM(domArr));
                  };
              })();
  
              Interpreter(wrap , "p*2>span*3>i>b*2")
  
          </script>
      </body>
  </html>
  ```

- 场景2

  获取某个标签的DOM父级结构

  ```js
  let getPath = (function(){
      function getSiblingsCount(dom){
          let count = 0;
          let nodeName = dom.nodeName;
          while (dom){
              if( dom.nodeName === nodeName ){
                  count++;
              }
              dom = dom.previousElementSibling;
          }
          return count === 1?"":count+"";
      }
  
      return function(dom){
          if( !(dom instanceof HTMLElement))throw new Error("参数不是DOM节点");
          let path = [];
          while (dom !== document){
              path.unshift(dom.nodeName + getSiblingsCount(dom));
              dom = dom.parentNode;
          }
          return path;
      }
  })();
  ```


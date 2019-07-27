# BOM

**BOM全称Browser Object Model(浏览器对象模型),**是用于描述这种对象与对象之间层次关系的模型，浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构，BOM可以使我们通过JS来操作浏览器

**BOM由多个对象组成，其中代表浏览器窗口的Window对象是BOM的顶层对象，其他对象都是该对象的子对象**

- Window-代表的是整个浏览器的窗口，同时window也是网页中的全局对象
- Navigator-代表当前浏览器的信息，通过该对象可以来识别不同的浏览器
- Location-代表当前浏览的地址栏信息，通过Location可以获取地址栏信息，或者操作浏览器跳转页面
- History-代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录，由于隐私原因，该对象不能获取到具体的历史记录，只能操作浏览器向前和向翻页，而且该操作只在当次访问时游戏
- Screen-代表用户的屏幕信息，通过该对象可以获取到用户的显示器的相关信息

**Window下方的BOM对象在浏览器中都是作为window对象的属性保存的，可以通过window对象来使用，也可以直接使用navigator location history screen**

 

## 1.Navigator

**Navigator代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器，由于历史原因，Navigator对象中的大部分属性已经 不能帮助我们识别浏览器了**

一般我们只会使用userAgent属性来判断浏览器的信息,userAgent是一个字符串，这个字符串包含有用来描述浏览器信息的内容，不同的浏览器会有不同的userAgent，usetAgent里面会有与浏览器相关的标识，但在IE11中已经将微软和IE相关的标识都已经取消了，所以我们基本已经不能通过UserAgent识别一个浏览器是否是IE浏览器

```js
//chrome浏览器
console.log(navigator.userAgent);
//Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36
```

```js
if(navigator.userAgent.indexOf("MSIE")!==-1){
    alert("IE");
}else{
    alert("not IE")
}
```

如果通过userAgent属性不能判断，还可以通过一些浏览器中特有的对象，来判断浏览器的信息，比如：通过ActiveXObject属性判断是否为IE浏览器

```js
//通过"ActiveXObject" in window来判断是不是IE浏览器
console.log("ActiveXObject" in window);
```

 

## 2.History

**History对象可以用来操作浏览器向前或向后翻页**

- length属性，可以获取到当次访问的链接数量


- back()方法可以用来退到上一个页面，作用和浏览器的回退按钮一样

  ```js
  history.back();
  ```

- forward()方法可以跳转下一个页面，作用和浏览器的前进按钮一样

  ```js
  history.forward()
  ```

- go()方法可以用来跳转到指定页面，该方需要一个整数作为参数

  **参数用法:**1表示向前跳转一个页面，2表示向前跳转2个页面，-1表示向后跳转一个页面，-2表示向后跳转2个页面

  ```js
  history.go(1);
  history.go(-1);
  ```

  

## 3.Location

**Location对象中封装了浏览器的地址栏信息**

- 直接打印location对象可以获取到地址栏的信息(当前页面的完整路径)


- 如果直接将location属性修改为一个完整的路径，或相对路径，则页面会自动跳转到该路径，并且会生成相应的历史记录

  ```js
  location="https://www.baidu.com";
  ```

**Location对象的属性**

| 属性     | 描述                          |
| -------- | ----------------------------- |
| hash     | 返回一个URL的锚部分           |
| host     | 返回一个URL的主机名和端口     |
| hostname | 返回URL的主机名               |
| href     | 返回完整的URL                 |
| pathname | 返回的URL路径名。             |
| port     | 返回一个URL服务器使用的端口号 |
| protocol | 返回一个URL协议               |
| search   | 返回一个URL的查询部分         |

**Location对象的方法**

- assign()方法用来跳转到其他页面，作用和直接修改location一样

  ```js
  location.assign("https://www.baidu.com");
  ```

- reload()方法会重新加载当前页面，作用和刷新按钮一样，该方法可以接受一个布尔值参数,默认是flase,代表只刷新当前页面,不清除页面的缓存内容(一般的刷新都会有缓存留着input标签里面的文字),如果传入true则会强制清空页面缓存内容

  **注意:**如果要通过刷新按钮清空页面缓存,需要同时按住ctrl和F5

  ```js
  location.reload();//不清除缓存
  location.reload(true);//清除缓存
  ```

- replace()方法可以使用一个新的页面替换当前页面，调用完毕也会跳转页面，不会生成历史记录，所以不能使用回退按钮回退

  ```js
  location.replace("https://www.baidu.com");
  ```




## 4.Window

- **open()方法打开一个新的浏览器窗口或查找一个已命名的窗口,该方法的返回值是新窗口的window对象**

  ```js
  window.open(url,target);//url默认是打开一个新的空白页面,target默认是打开新窗口
  window.open("https://www.baidu.com","_self");//在自身页面打开,域名必须要加上协议
  ```

- **close()方法关闭浏览器窗口**

  ```js
  window.close();
  ```

- **setInterval()方法按照指定的周期(以毫秒计)来调用函数或计算表达式**

- **setTimeout()方法在指定的毫秒数后调用函数偶计算表达式**

- **clearInterval()方法取消由setInterval()设置的定时器**

- **clearTimeout()方法取消由setTimeout()方法设置的定时器**

- **scrollTo()方法把内容滚动到指定坐标**

  ```js
  document.onclick=function(){
      window.scrollTo(0,500);
  }
  ```

- **scrollBy()方法按照指定像素来滚动内容,不带px单位**

  **参数**

  - xnum,把文档向右滚动的像素数
  - ynum,把文档向下滚动的像素数

  ```js
  document.onclick=function(){
      window.scrollBy(0,500);
  }
  ```

- **alert("内容")警告框**
- **confirm("文本")确认框**
- **prompt("文本","默认值")提示框**
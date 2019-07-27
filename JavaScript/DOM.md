# DOM

**DOM全称Document Object Model(文档对象模型)**

- **文档:**整个HTML网页文档对象
- **对象:**网页中的每一个部分都被转换为了一个对象
- **模型:**使用模型表示对象之间的关系，方便我们获取对象

## 1.节点

**Node(节点)是构成HTML5的基本单元**,DOM节点主要分为四类

- **文档节点:**整个HTML文档
- **元素节点:**HTML文档中的HTML标签
- **属性节点:**元素的属性
- **文本节点:**HTML标签中的文本内容

| 节点     | nodeName(节点名) | nodeType(节点类型) | nodeValue(节点值) |
| -------- | ---------------- | ------------------ | ----------------- |
| 文档节点 | #document        | 9                  | null              |
| 元素节点 | 标签名           | 1                  | null              |
| 属性节点 | 属性名           | 2                  | 属性值            |
| 文本节点 | #text            | 3                  | 文本内容          |

**注:**

- 浏览器已经为我们提供了文档节点对象window,该对象可以在页面中直接使用,代表的是整个网页
- 通过nodeType===1可以判断某节点是否是元素节点
- 文本节点包含了在写代码时的回车换行符间产生的空白，但是IE8及以下的浏览器中不会将空白节点当作子节点，而其他浏览器会
- 通过nodeName返回的标签名是全大写的
- 元素节点的名字也可以通过专门的TagName属性获得



## 2.DOM事件

**事件是用户和浏览器之间的交互行为**

### 2.1 鼠标事件

| 属性          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| onclick       | 当用户点击某个对象时调用的事件句柄。单击鼠标左键或者按下回车键时触发,意味着onclick事件处理程序既可以通过键盘也可以通过鼠标执行 |
| oncontextmenu | 在用户点击鼠标右键打开上下文菜单时触发。可以用做自定义菜单   |
| ondblclick    | 当用户双击某个对象时调用的事件句柄。双击鼠标左键时触发       |
| onmousedown   | 鼠标按钮被按下。                                             |
| onmouseenter  | 当鼠标指针移动到元素上时触发。该事件不冒泡，即鼠标移到其后代元素上时不会触发。 |
| onmouseleave  | 当鼠标指针移出元素时触发。该事件不冒泡，即鼠标移出其后代元素时不会触发。 |
| onmousemove   | 鼠标被移动。                                                 |
| onmouseover   | 鼠标移到某元素之上。鼠标移到其后代元素上时会触发,意味着支持冒泡 |
| onmouseout    | 鼠标从某元素移开。支持冒泡                                   |
| onmouseup     | 鼠标按键被松开。                                             |



### 2.2 键盘事件

| 属性       | 描述                       |
| ---------- | -------------------------- |
| onkeydown  | 某个键盘按键被按下。       |
| onkeypress | 某个键盘按键被按下并松开。 |
| onkeyup    | 某个键盘按键被松开。       |



### 2.3 框架/对象事件

| 属性           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| onabort        | 图像的加载被中断。 ( <object\>)                              |
| onbeforeunload | 该事件在即将离开页面（刷新或关闭）时触发                     |
| onerror        | 在加载文档或图像时发生错误。 ( <object\>, <body\>和 <frameset\>) |
| onhashchange   | 该事件在当前 URL 的锚部分发生修改时触发。                    |
| onload         | 一张页面或一幅图像完成加载。                                 |
| onpageshow     | 该事件在用户访问页面时触发                                   |
| onpagehide     | 该事件在用户离开当前网页跳转到另外一个页面时触发             |
| onresize       | 窗口或框架被重新调整大小。由windwo调用                       |
| onscroll       | 当文档被滚动时发生的事件。                                   |
| onunload       | 用户退出页面。 ( <body\> 和 <frameset\>)                     |



### 2.4 表单事件

| 属性       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| onblur     | 元素失去焦点时触发                                           |
| onchange   | 该事件在表单元素的内容改变时触发( <input\>, <keygen\>, <select\>, 和 <textarea\>) |
| onfocus    | 元素获取焦点时触发                                           |
| onfocusin  | 元素即将获取焦点时触发                                       |
| onfocusout | 元素即将失去焦点时触发                                       |
| oninput    | 元素获取用户输入时触发                                       |
| onreset    | 表单重置时触发                                               |
| onsearch   | 用户向搜索域输入文本时触发 ( <input="search">)               |
| onselect   | 用户选取文本时触发 ( <input\> 和 <textarea\>)                |
| onsubmit   | 表单提交时触发                                               |



### 2.5 剪贴板事件

| 属性    | 描述                           |
| ------- | ------------------------------ |
| oncopy  | 该事件在用户拷贝元素内容时触发 |
| oncut   | 该事件在用户剪切元素内容时触发 |
| onpaste | 该事件在用户粘贴元素内容时触发 |



### 2.6 打印事件

| 属性          | 描述                                                 |
| ------------- | ---------------------------------------------------- |
| onafterprint  | 该事件在页面已经开始打印，或者打印窗口已经关闭时触发 |
| onbeforeprint | 该事件在页面即将开始打印时触发                       |



### 2.7 拖动事件

| 事件        | 描述                                 |
| ----------- | ------------------------------------ |
| ondrag      | 该事件在元素正在拖动时触发           |
| ondragend   | 该事件在用户完成元素的拖动时触发     |
| ondragenter | 该事件在拖动的元素进入放置目标时触发 |
| ondragleave | 该事件在拖动元素离开放置目标时触发   |
| ondragover  | 该事件在拖动元素在放置目标上时触发   |
| ondragstart | 该事件在用户开始拖动元素时触发       |
| ondrop      | 该事件在拖动元素放置在目标区域时触发 |



### 2.8 多媒体事件

| 事件             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| onabort          | 事件在视频/音频（audio/video）终止加载时触发。               |
| oncanplay        | 事件在用户可以开始播放视频/音频（audio/video）时触发。       |
| oncanplaythrough | 事件在视频/音频（audio/video）可以正常播放且无需停顿和缓冲时触发。 |
| ondurationchange | 事件在视频/音频（audio/video）的时长发生变化时触发。         |
| onemptied        | 当期播放列表为空时触发                                       |
| onended          | 事件在视频/音频（audio/video）播放结束时触发。               |
| onerror          | 事件在视频/音频（audio/video）数据加载期间发生错误时触发。   |
| onloadeddata     | 事件在浏览器加载视频/音频（audio/video）当前帧时触发触发。   |
| onloadedmetadata | 事件在指定视频/音频（audio/video）的元数据加载后触发。       |
| onloadstart      | 事件在浏览器开始寻找指定视频/音频（audio/video）触发。       |
| onpause          | 事件在视频/音频（audio/video）暂停时触发。                   |
| onplay           | 事件在视频/音频（audio/video）开始播放时触发。               |
| onplaying        | 事件在视频/音频（audio/video）暂停或者在缓冲后准备重新开始播放时触发。 |
| onprogress       | 事件在浏览器下载指定的视频/音频（audio/video）时触发。       |
| onratechange     | 事件在视频/音频（audio/video）的播放速度发送改变时触发。     |
| onseeked         | 事件在用户重新定位视频/音频（audio/video）的播放位置后触发。 |
| onseeking        | 事件在用户开始重新定位视频/音频（audio/video）时触发。       |
| onstalled        | 事件在浏览器获取媒体数据，但媒体数据不可用时触发。           |
| onsuspend        | 事件在浏览器读取媒体数据中止时触发。                         |
| ontimeupdate     | 事件在当前的播放位置发送改变时触发。                         |
| onvolumechange   | 事件在音量发生改变时触发。                                   |
| onwaiting        | 事件在视频由于要播放下一帧而需要缓冲时触发。                 |



### 2.9 动画事件

| 事件               | 描述                            |
| ------------------ | ------------------------------- |
| animationend       | 该事件在 CSS 动画结束播放时触发 |
| animationiteration | 该事件在 CSS 动画重复播放时触发 |
| animationstart     | 该事件在 CSS 动画开始播放时触发 |



### 2.10 过渡事件

| 事件          | 描述                          |
| ------------- | ----------------------------- |
| transitionend | 该事件在 CSS 完成过渡后触发。 |



### 2.11 其他事件

| 事件             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| onmessage        | 该事件通过或者从对象(WebSocket, Web Worker, Event Source 或者子 frame 或父窗口)接收到消息时触发 |
| onmousewheel     | 已废弃。 使用 onwheel事件替代                                |
| ononline         | 该事件在浏览器开始在线工作时触发。                           |
| onoffline        | 该事件在浏览器开始离线工作时触发。                           |
| onpopstate       | 该事件在窗口的浏览历史（history 对象）发生改变时触发。       |
| onshow           | 该事件当 <menu\> 元素在上下文菜单显示时触发                  |
| onstorage        | 该事件在 Web Storage(HTML 5 Web 存储)更新时触发              |
| ontoggle         | 该事件在用户打开或关闭 <details\> 元素时触发                 |
| onwheel          | 该事件在鼠标滚轮在元素上下滚动时触发                         |
| DOMMouseScroll   | 火狐(firefox)支持的绑定鼠标滚轮的事件，并且该事件必须要通过addEventListener()函数来绑定事件 |
| DOMContentLoaded | 监听文档DOM元素的内容是否加载完成，比onload事件要监听的东西更少，DOM元素加载完毕后就会执行该事件,通过addEventListener()函数来绑定事件 |



## 3.DOM查询

### 3.1 获取DOM节点

#### 3.1.1 获取元素节点

- **getElementById()通过ID获取元素节点**,返回一个普通对象

  **注意:**ID其实可以不用获取而直接使用,因为ID是具有唯一性的,但是不推荐不获取就直接使用ID，因为以后会很难区分这个变量是哪里来的

- **getElementsByTagName()方法通过标签名获取元素节点**,返回一个类数组对象

- **getElementsByClassName()方法通过类名获取元素节点(IE8及以下版本不支持)**,返回一个类数组对象

- **getElementsByName()方法通过name属性获取元素节点,**这个方法主要是获取表单项,返回一个类数组对象

- **quertSelector()方法需要一个选择器的字符串作为参数,可以根据一个CSS选择器来查询一个元素节点对象，**语法和CSS语法一样，可以多个选择器一起使用,该方法在IE8也可用

  **注意:**使用该方法总会返回唯一的一个元素，如果满足条件的元素有多个，也只会返回第一个找到的

- **quertSelectorAll()方法用法同quertSelector()方法相同**

  **注意:**使用该方法就返回一个类数组对象，类数组对象里包含的所有符号要求的元素对象,即使符合条件的元素只有一个，也会返回类数组     

**注意:queryslector()和quertSlectorAll()方法尽量少用,这两种对元素节点的查找效率很低**



**静态获取与动态获取元素节点**

- 通过ID查找元素节点和以query开头的查找的元素节点是静态获取元素节点,这时无论元素怎么改变已经获取的元素节点所赋值后的变量永远指向该被查找到的元素节点

  ```js
  var box=document.getElementById("box");
  box.onclick=function(){
      box.id="wrap";
      console.log(box);//还是能获取到
  }
  ```

  ```js
  var box=document.querySelectorAll(".box");//假设有5个box
  document.onclick=function(){
      document.body.innerHTML+="<div class='box'></div>";
      console.log(box);//还是5个box
  }
  ```

- 通过集合进行获取元素节点的方式为动态获取,这时每次使用集合都会获取集合的最新状态,此时通过集合赋值的变量的值也会发生相应变化

  ```js
  var box=document.getElementByClassName("box");//假设有5个box
  document.onclick=function(){
      document.body.innerHTML+="<div class='box'></div>";
      console.log(box);//6个box
  }
  ```

  **注意:**如果选中了集合中的单个元素,这时获取的元素节点是静态获取



#### 3.1.2 获取属性节点

- 读取元素的属性节点,使用 元素.属性名 或 元素[属性名]
- 修改元素的属性节点,使用 元素.属性名=新值 或 元素[属性名]=新值

**注意:读取元素的class属性时必须用className来代替class关键字**



**获取,设立和删除标签属性节点**

如果JS中的自定义属性与标签属性重复时,直接获取会得到自定义属性的值,所以需要特定的值获取标签属性的方法

- 通过getAttribute()方法获取标签属性节点

  ```js
  var box=document.getElementById("box");
  console.log(box.getAttribute("id"));//box
  ```

- 通过setAttribute()方法设立或改变一个标签属性节点

  ```js
  var box=document.getElementById("box");
  box.setAttribute("class","nameBox");
  console.log(box);
  //如果已有该属性会是重新赋值
  ```

- 通过removeAttribute()方法删除一个标签属性节点

  ```js
  var box=document.getElementById("box");
  box.removeAttribute("id");
  console.log(box);
  ```

  

#### 3.1.3 获取文本节点

- **innerHTML属性可以获取双标签元素内部的html代码，包括子标签,这个属性对于单标签元素(如表单标签)没有意义**,返回一个字符串

- **InnerText属性可以获取双标签元素内部的文本内容，它和InnerHTML属性类似，但会自动将HTML标签去除**,返回一个字符串

- **value属性可以获取单标签元素(如表单标签)内部的内容,**同时要向单标签元素写入内容也必须使用value属性,返回一个字符串

- **nodeValue属性通过获取标签内的文本节点的内容来获取元素内部的文本内容(因为文本节点实际是一个标签的子节点,所以需要先找到文本节点)**,返回一个字符串

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
    </head>
    <div>123</div>
    <body>
      <script>
        	let div = document.getElementsByTagName("div")[0];
        	console.log(div.firstChild.nodeValue);//123
          console.log(div.nodeValue);//null
      </script>
    </body>
  </html>
  
  ```

 

#### 3.1.4 获取元素子节点

**元素子节点的有同父节点一样获取子节点的方法或属性**

- **childNodes属性获取元素子节点，**该属性会获取包括文本节点的所有子节点，返回一个类数组对象

- **children属性可以获取当前元素的所有子元素，**推荐用这个属性，返回一个类数组对象,而且是动态获取

- **firstChild属性表示当前节点的第一个子节点，**也包括空白文本，返回一个普通对象

- **firstElementChld属性获取第一个子元素，**返回一个普通对象，但是不兼容IE8，不推荐使用

  **兼容代码**

  ```js
  //获取任意一个父级元素的第一个子级元素
  function getFirstElementChild(element){
      if(element.firstElementChild){
          return element.firstElementChild;
      }else{
          let node=element.firstChild;//第一个节点
          while(node&&node.nodeType!==1){//判断是否为元素节点
              node=node.nextSibling;
  		}
      }
  }
  ```

- **lastChild属性表示当前节点的最后一个子节点，**也包括空白文本，返回一个普通对象

- **lastElementChld属性获取最后一个子元素，**返回一个普通对象，但是不兼容IE8，不推荐使用

- **childElementCount属性元素子节点个数,不兼容IE8**



#### 3.1.5 获取父和兄弟节点

- **parentElement属性获取当前节点的父元素(只在IE中可用)**

- **parentNode属性获取当前节点的父节点,**这是W3C标准的,推荐使用

- **offsetParent属性获取到离当前元素最近的开启了定位(除去默认的static)的祖先元素**，如果所以的祖先元素都没有开启定位，则会返回body

  **IE7以上(如果body和html之间的margin被清除掉则同样适用于IE7以下浏览器)**

  - **本身定位为fixed:**非火狐浏览器的offsetParent为null,火狐浏览器为body

    **注意:**在当前元素定位为fixed时(并且是非火狐浏览器),offsetLeft和offsetTop相对的还是body

  - **本身定位不为fixed:**如果父级有定位offsetParent为定位父级,父级没有定位为offsetParent为body

- **previousSibling属性获取当前节点的前一个兄弟节点**

  **注意:**可能获取空白文本，如果两个元素中间有空白就会获取空白

- **previosElementSibling属性获取前一个元素，**IE8不支持

- **nextSibiling属性表示当前节点的后一个兄弟节点**

- **nextElementSibling获取后一个元素，**IE8不支持

 

#### 3.1.6 获取特殊元素节点

- 在document元素中有一个body属性,它保存的是对body元素的引用

  ```js
  var body=document.body
  ```

- 在document元素中有一个documentElement属性,它保存html标签的引用

  ```js
  var html=document.documentElement
  ```

- 在document元素中有一个all属性,该属性代表页面中所有元素(不建议使用)

  ```js
  var all=document.all;//它的值是undefined,但是却有长度数组的特性
  ```



**表格的简便操作**

- getElementsByTagName("tbody")[0]与tBodies[0]相同
- getElementsByTagName("thead")[0]与tHead相同
- getElementsByTagName("tfoot")[0]与tFoot相同
- getElementsByTagName("tr")[0]与rows[0]相同
- getElementsByTagName("td")[0]与cells[0]相同



### 3.2 创建或添加元素节点

- **createElement()方法可以创建一个元素节点对象**，它需要一个标签名作为参数，将会根据该标签名创建元素节点对象，并将创建好的对象作为返回值返回

  **注意:**该方法由document使用

  ```js
  var oDiv=document.createElement("div");
  ```

- **createTextNote()方法可以创建一个文本节点对象**，需要一个文本内容作为参数，将会根据该内容创建文本节点，并将新节点返回

  **注意:**该方法由document创建

  ```js
  var oTxt=document.createTextNote("123");
  ```

- **createElementFragment()方法创建一个文档片段,**可以向这个文档片段中添加一个个节点,然后直接将该文档片段加到要添加到的父元素中,该方法可以实现同时给页面加多个节点而只用渲染一次页面,所以如果要添加多个同级节点时最好使用这个来添加

  **注意:**放入文档片段的节点一定要是同级的片段

  ```js
  var box=document.createDocumentFragment();
  box.appendChild(p1);//创建的p元素节点
  box.appendChild(p2);
  document.body.appendChild(box);
  ```

- **cloneNode()方法可以传入一个参数，克隆一个DOM节点，如果传入true则是把元素中的所有内容也一起克隆，如果传入的是false则会只克隆这一个DOM节点，不会将里面的内容也克隆**(不传入参数默认为flase)

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
    </head>
    <div>123</div>
    <body>
      <script>
        	let Odiv = document.getElementsByTagName("div")[0];
        	let cloneDiv = Odiv.cloneNode(false);
         	let cloneDiv2= Odiv.cloneNode(true);
        	console.log(cloneDiv);//<div></div>
          console.log(cloneDiv);//<div>123</div>
      </script>
    </body>
  </html>
  
  ```

- **appendChild()向父节点中添加一个新的子节点**，可以逐层添加子节点

  **注意:**

  - 该方法由父节点调用

  - 要添加子节点之前必须要先有这个子节点，没有就要先创建，并且新加的子节点会自动添加到所有子节点的最后面

    ```js
    var parent=document.getElementById("parent");
    var child=document.createElement("div");
    parent.appendChild(child);
    ```

  - 如果子节点不是新创建而是从原有父级节点上调用的,那么在用appenChild()方法时会先将原有父级节点上的该子节点删除

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
    </head>
    <body>
      <table>
        <tbody>
          <tr>
            <td>3</td>
          </tr>
          <tr>
            <td>1</td>
          </tr>
          <tr>
            <td>2</td>
          </tr>
        </tbody>
      </table>
  
      <script>
        var arr = [];
        var oTab = document.getElementsByTagName("table")[0];
  
        oTab.onclick = function() {
          for (var i = 0; i < oTab.tBodies[0].rows.length; i++) {
            arr[i] = oTab.tBodies[0].rows[i];
          }
          //arr = [...oTab.tBodies[0].rows];简写模式
            
          arr.sort(function(tr1, tr2) {
            var n1 = parseInt(tr1.cells[0].innerHTML);
            var n2 = parseInt(tr2.cells[0].innerHTML);
            return n1 - n2;
          });
  
          for (var i = 0; i < arr.length; i++) {
            oTab.tBodies[0].appendChild(arr[i]);//排序之后页面的结果为1 2 3
          }
        };
      </script>
    </body>
  </html>
  
  ```

- **insertBefore()方法在指定的子节点前面插入子节点**

  **注意:**

  - 该方法由父节点调用,传入两个参数(新节点和旧节点),第一个参数必填,第二个参数可选
  - 要添加子节点之前必须要先有这个子节点，没有就要先创建，并且**如果没有传入第二个参数**会自动添加到所有子节点的最后面
  - 如果子节点不是新创建而是从原有父级节点上调用的,那么在用insertBefore()方法时会先将原有父级节点上的该子节点删除

  ```js
  var parent=document.getElementById("parent");
  var child=document.createElement("div");
  parent.insertBefore(child,parent.childNotes[0]);//将新创建的元素插入的父元素内容最前面
  ```

- **使用innerHTML属性也可以完成DOM的增删改等操作**,可以给innerHTML属性赋值带有HTML标签的字符串实现操作,**一般我们会将DOM查询和innerHTML属性两种方法同时使用**

  ```js
  var parent=document.getElementById("parent"); 
  var child=document.createElement("div");
  
  child.innerHTML="123"；
  parent.appendChild(child);
  ```

  

### 3.3 替换或删除元素节点

- **replaceChild()方法可以使用指定的子节点替换已有的子节点**

  **注意:**

  - 该方法由父节点调用
  - 要替换子节点之前必须要先有这个新的子节点，没有就要先创建
  - 如果子节点不是新创建而是从原有父级节点上调用的,那么在用replace()方法时会先将原有父级节点上的该子节点删除

  ```js
  var parent=document.getElementById("parent");
  var child=var child=document.createElement("div");
  parent.replaceChild(child,parent.children[0]);//替换父元素的第一个子元素
  ```

- **removeChild()方法可以用作删除一个子节点**

  **注意:**该方法由父节点调用

  ```js
  var parent=document.getElementById("parent");
  parent.removeChild(parent.children[0]);//删除父元素的第一个子元素
  ```



**注:上述的DOM查询方法可以混合搭配使用**

**如:**在不知道一个子节点的父节点的情况下删除该子节点

通过 **子节点.parentNode.removeChild(子节点)** 的方法就可以删除



### 3.4 遍历DOM元素

```js
/*
	第一个函数:功能函数,用来打印节点名称
	第二个函数:给根节点，找到所有子节点
	第三个函数:给子节点，把每个节点的名字显示出来
*/
function f1(node){
    console.log("节点的名字:"+node.nodeName);
}

function forDOM(root){
    //调用f1,显示节点的名字
    f1(root);
    //获取根节点中所有的子节点
    var children=root.children;
    //调用遍历所有子节点的函数
    forChildren(children);
}

//给该函数一个子节点,显示所有该子节点的子节点
function forChildren(children){
    //遍历所有的子节点
    for(var i=0;i<children.length;i++){
        //每个子节点
        var child=children[i];
        //显示每个子节点的名字
        f1(child);
        //判断child下面是否还有子节点,如果有继续遍历
        child.children&&forDOM(child);
    }
}
```

##  

## 4.元素样式

### 4.1 修改元素CSS样式

#### 4.1.1 通过style属性修改元素样式

通过 **元素.style.样式名="样式值"** 的方式设置修改CSS样式

**注意:**

- 如果CSS的样式名中含有-(也就是减号)，这种名称在JS中是不合法的，比如background-color,需要将这种样式名修改为**驼峰命名法**，去掉-，然后将-后的字母大写，如:background-color写作backgroumdColor，border-top-width写作borderTopWidth
- 因为float样式是JS中的保留字,所有在修改元素浮动属性的时候不能够直接写作 **元素.style.float="样式值"** 的形式,需要**用styleFloat(兼容IE6~8)或cssFloat(IE9以上使用)**来代替使用
- 通过style属性设置的样式都是内联样式，而内联样式有较高的优先级，所以通过JS修改的样式往往会立即重置，但是如果在样式中用了!important则样式会拥有最高优先级，导致JS修改样式失效，所以尽量不要使用important
- 通过JS的style属性设置和读取的都是内联样式，无法读取CSS样式表中的样式(外部样式和嵌套样式)

**简写方式**

通过 **元素.style.cssText="样式值"**  的方式可以将多个CSS属性同时写入,右边的值的写法和内联的CSS样式一样的,并不用遵循驼峰命名法,也不用在意JS的保留字问题



#### 4.1.2 通过类修改元素样式

通过**修改类(class)**来修改CSS样式

- **修改className属性**

  通过修改元素的className属性(在JS中的class是保留字,所以用className来代替)来间接修改样式，这样一来，只需要修改一次，即可同时修改多个样式，浏览器只需要重新渲染页面一次，性能比较好

  **用className属性添加一个类**

  **元素.className+=" 类名"**，记得在类名前面加上空格

- **通过classList属性**

  每个标签元素都有classList属性,这个属性和当中的一些方法可以让我们修改类时更加简便

  - **classList属性返回元素的类名,该属性只读**,只能通过该属性的一些方法来修改元素的类

  - **classList属性有length属性,该属性返回元素类的属性,并且该属性只读**

  - **classList属性的方法**

    | 方法                          | 描述                                                         |
    | ----------------------------- | ------------------------------------------------------------ |
    | add(*class1, class2, ...*)    | 在元素中添加一个或多个类名。  如果指定的类名已存在，则不会添加 |
    | contains(*class*)             | 返回布尔值，判断指定的类名是否存在。可能值：true - 元素包已经包含了该类名false - 元素中不存在该类名 |
    | item(*index*)                 | 返回元素中索引值对应的类名。索引值从 0 开始。  如果索引值在区间范围外则返回 *null* |
    | remove(*class1, class2, ...*) | 移除元素中一个或多个类名。**注意:** 移除不存在的类名，不会报错。 |
    | toggle(*class,* true\|false)  | 在元素中切换类名。  第一个参数为要在元素中移除的类名，并返回 false。  如果该类名不存在则会在元素中添加类名，并返回 true。   第二个是可选参数，是个布尔值用于设置元素是否强制添加或移除类，不管该类名是否存在。也就是说如果写了第二个参数那么该方法就变成了add()或remover()方法 |

    

### 4.2 获取元素样式

#### 4.2.1 通用样式

- **在IE了浏览器中通过currentStyle属性来获取当前元素正在显示的样式**,如果获取的是没有设置的样式,就会返回该样式的默认值,并且如果**默认是以px为单位**来返回的

  **如:**没有设置width就会返回默认值auto

  **语法:**元素.currentStyle.样式名

- **在其他浏览器可以使用getComputedStyle()方法来获取元素样式(IE8以下不支持,)这个方法是window的方法，可以直接进行调用**

  该方法**有两个参数，**第一个参数传入一个元素,第二个参数传入一个伪类,第二个参数可选,当不查询伪类元素的时候可以忽略或者传入null。该方法会**返回一个对象**，对象中封装了当前元素对应的样式，可以通过样式名来读取样式，如果获取的样式没有设置，与currentStyle属性不同,会获取真实的值，而不是默认值，同时值也是默认以**px为单位**来返回的

  **如:**没有设置width,不会返回auto,而是返回一个长度

  ```js
  var div=document.getElementById("box");
  var obj=getComputedStyle(Odiv,null);
  
  console.log(obj.width);
  console.log(getComputedStyle(box1,null).width);//也可以直接输出
  ```

**注意:**通过currentStyle属性和getComputedStyle()方法读取到的样式都是只读的，不能修改，如果要修改必须通过style属性

 

**兼容写法**

```js
/*
参数:obj 要获取样式的元素
	name 要获取的样式名
*/
function getStyle(obj,name){

if(window.getComputedStyle){   //因为getComputedStyle函数实质上是一个对象，所以如果有就返回true,这里必须要用window，因为这是要在全局范围类寻找，但是IE8全局里没有这个变量，肯定会报错，必须加上window，如果没有这个window里的属性就会返回undefined

return getComputedStyle(obj，null)[name]//因为是变量必须使用[]的方法

}else{
return obj.currentStyle[name]
}
//当然if的对象也可以反过来，但是如果判断obj.currentStyle有问题在于IE8以上的IE浏览器两种方法都有，这样它就会优先使用第一个方法，但是我们推荐优先使用getComputedStyle()
}
```



#### 4.2.2 获取特殊样式

**注:以下获取的样式都是只读的,不能够修改**

**属性**

- **clientWidth和clientHeight属性获取元素的可见宽度和高度(内容区和内边距)，这些属性都是返回纯数值**，不带单位,可以直接进行计算

  **注意:**`document.documentElement.clientWidth`和`document.documentElement.clientHeight`并不是根标签的可视区域,就是视口的大小

- **offsetWidth和offsetHeight属性获取元素的整个宽度和高度(内容区，内边距和边框),返回值也是纯数值**

  **注意:**在IE10及以下浏览器根标签的offsetWidth和clientWdith等都被统一指定为视口的大小

- **offsetLeft和offsetTop属性获取当前元素相对于其定位父元素(也就是offsetParent)的水平偏移量和垂直偏移量,不管该元素是否有定位**

  **注意:**

  + 偏移量的原点是父元素的左上角(left top),和背景图片的原点相同
  + 在当前元素定位为fixed时(并且是非火狐浏览器),offsetLeft和offsetTop相对的还是body

- **window.innerWidth和window.innerHeight属性可以获取window窗口的内部宽高**

  **注意:**不包括页面的导航栏以及页面滚动条和控制台

  ```js
  console.log(window.innerHeigth);
  console.log(window.innerWidth);
  ```

- **document.documentElement.clientWidth和document.documentElement.clientHeight属性获取文档可视区域**,该属性的值虽然与window,innerWidth和window.innerHeight的值相同,但是对象的调用者不同,内部的含义也不同

  ```js
  console.log(document.documentElement.clientHeith);
  console.log(document.documentElement.clientWidth);
  ```

- **scrollWidth和scrollHeight属性可以获取元素整个滚动区域的宽度和高度,如果没有隐藏的部分则等于clientWidth和clientHeight**

- **scrollLeft和scrollTop属性可以获取水平和垂直滚动条滚动的距离**

  **浏览器滚动条问题**

  chrome认为浏览器的滚动条是body元素的，可以通过body.scrollTopl来获取，而火狐和IE浏览器认为浏览器的滚动条是html元素的，所以在用到浏览器滚动条的时候就会出现兼容问题(现在的chrome版本已经和火狐与IE统一都是通过html元素获取了,通过body获取反而值为0)

  **兼容代码**

  ```js
  var st=document.body.scrollTop||document.documentElement.scrollTop;
  var sl=document.body.scrollLeft||document.documentElement.scrollLeft;
  //该兼容代码可以在计算页面的滚动条距离的时候使用
  ```

**注:当满足`scrollHeight-ScrollTop===clientHeight` 这个表示式时说明垂直滚动条到底了，同理水平滚动条也一样**



**自制滚动条**

```js
/*
	一个最大的容器box,一个装滚动条的div容器,一个滚动条div,一个文字div
	滚动条的高/装滚动条div的高=box的高/文字的高-->滚动条的高=装滚动条div的高*box的高/文字div的高
*/
//bar为滚动条,scroll为包裹滚动条的层，content为装文字的盒子
var box=document.getElementById("box");
var scroll=document.getElementById("scroll");
var content=document.getElementById("content");
var bar=document.getElementById("bar");

var height=scroll.offsetHeight*box.offsetHeight/content.offsetHeight;
bar.style.height=height+"px";
//移动滚动条
bar.onmousedowm=function(e){
    //鼠标在滚动条上的位置
    var spaceY=e.clientY-bar.offsetTop;
    document.onmousemove=function(e){
        //滚动条纵坐标
        var y=e.clientY-spaceY;
        y=y<0?0:y;//最小值
        y=y>(scroll.offsetHeight-bar.offsetHeight)?(scroll.offsetHeight-bar.offsetHeight):y;
        bar.style.top=y+"px";
        //设置鼠标移动时文字不被选中
        window.getSelection?window.getSelection().removeAllRanges():document.selection.empty();
        
        //文字div的移动距离=滚动条的移动距离*文字div的最大移动距离/滚动条的最大移动距离
        //文字的最大移动距离是content盒子的高度减去最外层盒子的高度
        var moveY=y*(content.offsetHeight-box.offsetHeight)/(scroll.offsetHeight-bar.offsetHeight);
        content.style.marginTop=-moveY+"px";
    }
}

document.onmouseup=function(){
    //鼠标抬起将鼠标移动事件去除
    document.onmousemove=null;
}
```

```html
<!--下面滚动条的方法与上方没有太大联系-->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>滚动条</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      #box {
        position: relative;
        overflow: hidden;
        margin: 50px auto;
        width: 500px;
        height: 600px;
        background-color: rgb(231, 223, 223);
        border-radius: 5px;
      }
      #content {
        position: absolute;
        float: left;
        width: 480px;
      }
      #content > div {
        text-indent: 2em;
      }
      #bar-container {
        position: relative;
        float: right;
        height: 100%;
        width: 20px;
        background-color: #ccc;
        border-radius: 5px;
        cursor: pointer;
      }
      #bar {
        position: absolute;
        top: 0;
        left: 0;
        width: 20px;
        height: 40px;
        background-color: pink;
        border-radius: 5px;
        cursor: pointer;
      }
    </style>
    <script src="js/animation.js"></script><!--装有运动函数-->
  </head>
  <body>
    <div id="box">
      <div id="content">
        code....
      </div>
      <div id="bar-container">
        <div id="bar"></div>
      </div>
    </div>
    <script>
      ///最大的包裹盒子
      let box = document.getElementById("box");
      //包裹文档的盒子
      let content = document.getElementById("content");
      //装滚动条的盒子
      let barContainer = document.getElementById("bar-container");
      //滚动条
      let bar = document.getElementById("bar");
      //计算滚动条的大小
      let barHeight =
        Math.min(box.offsetHeight / content.offsetHeight, 1) *
        barContainer.offsetHeight;
      bar.style.height = barHeight + "px";
      //滚动条最大滚动距离
      let maxScroll = barContainer.offsetHeight - bar.offsetHeight;
      //内容能滑动的最大距离
      let maxContent = content.offsetHeight - box.offsetHeight;
      //滑动时距顶部的距离
      let Top = 0;
      //判断内容长度绑定事件
      if (bar.offsetHeight / content.offsetHeight < 1) {
        mousewheel(box, function(event) {
          event = event || window.event;
          if (event.wheelDetail > 0) {
            //每次滑动的像素
            Top -= 10;
          } else {
            Top += 10;
          }
          Top = Math.max(0, Top);
          Top = Math.min(maxScroll, Top);
          bar.style.top = Top + "px";
          //文字内容滑动比例
          content.style.top = -(Top / maxScroll) * maxContent + "px";
        });
      } else {
        barContainer.style.display = "none";
        content.style.width = "100%";
      }
      barContainer.onclick = function(event) {
        Top = event.pageY - getDistance(this).top - bar.offsetHeight / 2;
        //上面也可以用event.clientY-this.getBoundingClientRect().top-bar.offsetHeight / 2
        Top = Math.max(0, Top);
        Top = Math.min(maxScroll, Top);
        if (event.target === this) {
          animation(//运动函数
            bar,
            {
              data: {
                top: Top
              }
            },
            500
          );
          animation(
            content,
            {
              data: {
                top: -(Top / maxScroll) * maxContent
              }
            },
            500
          );
        }
      };

      bar.onmousedown = function(event) {
        event = event || window.event;
        //开始时的鼠标坐标和滚动条的距离
        let startY = event.clientY;
        let startTop = bar.offsetTop;
        //清除阻止冒泡
        event.stopPropagation();
        document.onmousemove = function(event) {
          event = event || window.event;
          //拖动时的鼠标坐标和滚动距离
          let nowY = event.clientY;
          Top = startTop + nowY - startY;

          Top = Math.max(0, Top);
          Top = Math.min(maxScroll, Top);
          bar.style.top = Top + "px";
          //文字内容滑动比例
          content.style.top = -(Top / maxScroll) * maxContent + "px";
          event.preventDefault();
        };

        document.onmouseup = function() {
          this.onmousemove = null;
          this.onmouseup = null;
        };
      };

      function mousewheel(dom, callback, bool) {
        //bool为传入的一个布尔值,如果是true则阻止默认行为,默认是不阻止
        var type = "mousewheel";

        if (dom.onmousewheel === undefined) {
          //不能通过判断下面的那个,因为在火狐中dom属性也没有这个属性
          type = "DOMMouseScroll";
        }
        //真正的事件函数
        function cb(event) {
          /*
              自定义属性event.wheelDtail控制鼠标滚轮,所以在外部使用时应该用event.wheelDetail
          */
          /*外部可以传入event作为事件对象也可以不传入直接使用,也不需要进行兼容,但是不传入时应该用				event
          */
          event = event || window.event;
          //把滚动事件的方向处理一致
          event.wheelDetail = event.wheelDelta / 123 || event.detail / -3;
          //每次滚动的值为1,并且向上滚动为正值,向下滚动为负值

          //阻止默认行为
          if (!!bool) {
            if (event.preventDefault) {
              event.preventDefault();
            } else {
              event.returnValue = flase;
            }
          }
          callback.call(this, event);
        }

        if (dom.addEventListener) {
          dom.addEventListener(type, cb);
        } else {
          dom, attachEvent("on" + type, cb);
        }
      }

      //获取当前定位元素距离body顶部的距离
      function getDistance(dom) {
        let obj = {
          top: 0,
          left: 0
        };
        while (dom != document.body) {
          obj.top = dom.offsetTop;
          obj.left = dom.offsetLeft;
          dom = dom.offsetParent;
        }
        return obj;
      }
    </script>
  </body>
</html>
```

**方法**

- **getBoundingClientRect()方法获取元素节点对象到窗口的左上角(恒定为刚打开页面时的左上角为原点)的距离值以及自身的长宽等属性**,该方法不用传入参数

  **返回值属性**

  + height和width代表元素的border-box尺寸
  + left和top能代表元素左上角距离视图左上角的相对位置
  + right和bottom能代表元素右下角距离视图左上角的相对位置

  **注:**通过该方法可以拿到一个元素四个角的相对于当前视图位置,通过该方法加上滚动条的滚动距离可以拿到四个角相对于整个网页的绝对位置

- **scrollIntoView()方法让滚动条滚动到调用对象的可视区,一旦执行此方法调用此方法的页面就会滚动到该元素节点的位置**,此方法不用传入参数



## 5.事件对象

当事件的响应函数被触发时，浏览器每次都会将一个事件对象作为默认实参(如果没有传入参数位置为arguments[0])传递进响应函数，在事件对象中封装了当前事件相关的一些信息，比如:鼠标的坐标，键盘哪个按键被按下 ，鼠标滚轮滚动的方向

**注:**

- 一般都会把形参写在事件对象的形参里,虽然不写也不会报错,但是获取方式会相对麻烦
- 在IE中,event事件对象通过window.event来获取，在其他浏览器中是作为参数传入使用 

```js
//写实参调用event
function eventTest(event){
   event=event||window.event;
}

//不写实参调用event
function eventTest(){
    var event = window.event||arguments[0];
}

//传入额外实参
function eventTest(a,b){
    var event = window.event || arguments.callee.caller.arguments[0];
}

//如果传入了参数却如第二种写法的话，则arguments中将会传入已经传入的参数，这时获取的arguments[0]就会是第一个传入的参数
```

```js
//target为该调用对象
target = event.srcElement||event.target//低版本IE用srcElement
```



**以下内容来自W3Cschool**

### 5.1 基本事件对象

- **常量**

| 静态变量        | 描述                                 |
| --------------- | ------------------------------------ |
| CAPTURING-PHASE | 当前事件阶段为捕获阶段(1)            |
| AT-TARGET       | 当前事件是目标阶段,在评估目标事件(1) |
| BUBBLING-PHASE  | 当前的事件为冒泡阶段 (3)             |

- **属性**

| 属性          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| bubbles       | 返回布尔值，指示事件是否是起泡事件类型。                     |
| cancelable    | 返回布尔值，指示事件是否可拥可取消的默认动作。               |
| currentTarget | 返回其事件监听器触发该事件的元素。                           |
| eventPhase    | 返回事件传播的当前阶段。                                     |
| target        | 返回触发此事件的元素（事件的目标节点）。不兼容低版本IE,低版本IE用srcElement |
| timeStamp     | 返回事件生成的日期和时间。                                   |
| type          | 返回当前 Event 对象表示的事件的名称。                        |

- **方法**

| 方法              | 描述                                     |
| ----------------- | ---------------------------------------- |
| initEvent()       | 初始化新创建的 Event 对象的属性。        |
| preventDefault()  | 通知浏览器不要执行与事件关联的默认动作。 |
| stopPropagation() | 不再派发事件。                           |



### 5.2 目标事件对象

**方法**

| 方法                  | 描述                                                    |
| --------------------- | ------------------------------------------------------- |
| addEventListener()    | 允许在目标事件中注册监听事件(IE8 = attachEvent())       |
| dispatchEvent()       | 允许发送事件到监听器上 (IE8 = fireEvent())              |
| removeEventListener() | 运行一次注册在事件目标上的监听事件(IE8 = detachEvent()) |



### 5.3 事件监听对象

**方法**

| 方法          | 描述                         |
| ------------- | ---------------------------- |
| handleEvent() | 把任意对象注册为事件处理程序 |



### 5.4 文档事件对象

**方法**

| 方法          | 描述                  |
| ------------- | --------------------- |
| createEvent() | 返回新创建的event对象 |



### 5.4 鼠标/键盘事件对象

- **属性**

| 属性          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| altKey        | 返回当事件被触发时，"ALT" 是否被按下。                       |
| button        | 返回当事件被触发时，哪个鼠标按钮被点击。                     |
| clientX       | 返回当事件被触发时，鼠标指针的水平坐标。                     |
| clientY       | 返回当事件被触发时，鼠标指针的垂直坐标。                     |
| ctrlKey       | 返回当事件被触发时，"CTRL" 键是否被按下。                    |
| Location      | 返回按键在设备上的位置                                       |
| charCode      | 返回onkeypress事件触发键值的字母代码。                       |
| key           | 在按下按键时返回按键的标识符。                               |
| keyCode       | 返回onkeypress事件触发的键的值的字符代码，或者 onkeydown 或 onkeyup 事件的键的代码。 |
| which         | 返回onkeypress事件触发的键的值的字符代码，或者 onkeydown 或 onkeyup 事件的键的代码。 |
| metaKey       | 返回当事件被触发时，"meta" 键是否被按下。                    |
| relatedTarget | 返回与事件的目标节点相关的节点。                             |
| screenX       | 返回当某个事件被触发时，鼠标指针的水平坐标。                 |
| screenY       | 返回当某个事件被触发时，鼠标指针的垂直坐标。                 |
| shiftKey      | 返回当事件被触发时，"SHIFT" 键是否被按下。                   |

- **方法**

| 方法                | 描述                   |
| ------------------- | ---------------------- |
| initMouseEvent()    | 初始化鼠标事件对象的值 |
| initKeyboardEvent() | 初始化键盘事件对象的值 |



## 6.事件的冒泡(Bubble)

所谓的冒泡指的就是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发，在开发中大部分情况冒泡都是有用的，如果不希望发生事件冒泡，可以通过事件对象来取消冒泡，可以通过设置`event.cancelBubble=true`来取消冒泡，这个需要**设置在子元素的事件中**，并且大多数都是设置的可以冒泡，除了某些函数默认不冒泡

**如果在父元素里面设置了一个事件,而在子元素中也有相同的事件,不想要父元素的事件在子元素的范围内出现时，可以通过多种方式设置取消冒泡**

**1.event.cancelBubble=true(低版本IE都通过该方法)** 

**2.因为这是默认行为，所以也可以直接在事件函数返回flase即可(return flase)**

**注意:**这种方法阻止默认行为只能够阻止通过onclick等绑定的事件,不能阻止通过addEventListener()等绑定的事件

**3.通过event.stopPropagation()来阻止事件冒泡，但是不会阻止默认行为(低版本IE不兼容),低版本IE用event.returnValue=false**

**4.通过event.preventDefault()阻止默认行为来阻止冒泡**

**兼容写法**

```js
if(event.stopPropagation){
    event.stopPropagation();
}else{
    event.cancelBubble=true;//或event.returnValue=false;
}
```



## 7.事件的委派

事件的委派指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素，从而通过祖先元素的响应函数来处理事件,事件委派是利用了冒泡，通过委派可以事件绑定的次数，提高程序的性能



**例子:**

为每一个超链接都绑定一个单击响应函数，这里我们为每一个超链接都绑定了一个单击响应函数，这种中操作比较麻烦，而且这些操作只能为已有的超链接设置事件，而新添加超链接必须重新绑定

```js
var a=document.getElementByTagName("a");
for(var i=0;i<a.length;i++){
    a[i].onclick=function(){
        alert(123);
    }
}
```

我们希望，只绑定一次事件，即可应用到多个元素上，即使元素是后添加的，我们可以尝试将其绑定给元素的共同的祖先元素

```js
//假设所以的a元素都是由一个父元素包裹
var parent=document.getElementById("parent");
parent.onclick=function(){
    alert(123);
}
```

但是这个例子需要判断如果触发的对象是我们期望的元素,就执行该代码,否则不执行,因为如果点击的是a元素之外的其它地方就不触发弹窗效果,这个时候需要使用target 事件对象中的event中的target表示的触发事件的对象

```js
parent.onclick=function(event){
    event=event||window.event;
    if(event.target.nodeName==="A"){//nodeName返回的标签名全大写
    	alert(123);   
    }
}
```



## 8.事件的绑定

### 8.1 绑定事件

通过 **对象.事件=函数** 的形式绑定的响应函数，只能同时为一个元素的一个事件绑定一个响应函数，不能够绑定多个，如果绑定了多个，则后边绑定的响应函数会覆盖掉前边的

- **通过addEventListener()方法(不支持IE8及以下浏览器)可以为元素绑定多个响应函数**,该方法有三个参数

  **参数**

  - 代表事件的字符串,不要加on
  - 回调函数，当事件触发时该函数会被调用
  - 是否在捕获或冒泡阶段触发事件，需要一个布尔值，true表示事件在捕获阶段执行,flase表示在冒泡阶段执行,默认值为false(可选),如果想要这两个事件都可以在同一个对象上发生必须添加两次事件

  ```js
  btn.addEventListener("click",function(){
      alert(123);
  },false)
  ```

  addEventListene0r()方法可以同时为一个元素的相同事件绑定响应函数，这样当事件被触发时，响应函数将**会按照函数的绑定顺序执行**

- **在IE8及以下浏览器中可以通过attachEvent()方法为元素绑定多个响应函数**,该方法有两个参数

  **参数**

  - 事件的字符串，要加on
  - 回调函数

  ```js
  btn.attachEvent("onclick",function(){
      alert(123);
  })
  ```

​	attachEvent()方法也可以同时为一个元素的相同事件绑定响应函数，不同的是该方法是**后绑定的响应函数先执行**

**注:**

- **大部分时候响应函数的执行顺序都不重要，如果需要对顺序要求就写成一个响应函数，要用这种方法添加的响应函数都是顺序不重要的函数**
- **事件的绑定和委派等最好都是通过addEventListener()方法等来绑定,以免和别人的发生冲突**



**注意:**addEventListener()方法中的this是绑定事件的对象，attachEvent()方法中的this是window对象



**兼容写法**

注意:addEventListener()中的this是绑定事件的对象，attachEvent()中的this是window，需要统一两个方法的this

```js
/*
参数:obj 要绑定事件的对象
	eventStr 事件的字符串
	callback 回调函数
*/
function addEvent(obj,eventStr,callback) {
    if(obj.addEventListener){
        obj.addEventListener(eventStr,callback,false)
    }
    else{
        obj.attachEvent("on"+eventStr,function () {
            callback.call(obj);
        })
    }
}
/*
因为this的不统一必须在attchEvent()中加一个回调函数，浏览器自动调用函数，然后我们自己手动调用函数，这样我们就能控制callback方法中的this了
*/
```



### 8.2 移除绑定

- **通过removeEventListener()方法(不支持IE8及以下浏览器)可以为元素移除响应函数**,该方法有三个参数

  **参数**

  - 代表事件的字符串,不要加on
  - 需要被移除的事件函数
  - 是否在捕获或冒泡阶段取消绑定事件，需要一个布尔值，true表示事件在捕获阶段执行,flase表示在冒泡阶段执行,默认值为false(可选)

- **在IE8及以下浏览器中可以通过detachEvent()方法为元素移除响应函数**,该方法有两个参数

  **参数**

  - 事件的字符串，要加on

  - 回调函数


**注意:**

- 如果要使用移除绑定,那么绑定时需要必须通过函数赋值的方式来绑定,因为如果是匿名函数移除的函数并不是同一个对象
- 如果不为removeEventListener()方法传入第三个参数会默认是移除冒泡状态的事件函数,不会移除捕获阶段的,因为这其实是两个不同的事件
- 移除绑定只能对用上述方法绑定的函数器作用,不会对用onclick属性等绑定的函数起作用



**兼容写法**

```js
/*
参数:obj 要绑定事件的对象
	eventStr 事件的字符串
	callback 回调函数
*/
function removeEvent(obj,eventStr,callback) {
    if(obj.removeEventListener){
        obj.removeEventListener(eventStr,callback,false)
    }
    else{
        obj.detachEven("on"+eventStr,callback)
    }
}
```



## 9.事件的传播

**关于事件的传播网景公司和微软公司有不同的看法**

- 微软公司认为事件应该是由内向外传播，也就是当事件触发时，应该先触发当前元素上的事件，然后再向当前元素的祖先元素上传播，也就是说事件应该在冒泡阶段执行


- 网景公司认为事件应该是由外向内传播，也就是当前事件被触发时，应该先触发当前元素的最外层的祖先元素的事件，然后再向内传播给后代元素，这个阶段叫做捕获阶段


**最后，W3C综合了两个公司的方案，将事件传播分成了三个阶段**

**1.捕获阶段**

在捕获阶段时从最外层的祖先元素，向目标元素进行进行事件的捕获，但是默认此时不会触发事件

**2.目标阶段**

事件捕获到目标元素，捕获结束开始在目标元素上触发事件

**3.冒泡阶段**

事件从目标元素向它的祖先元素传递，依次触发祖先元素上的事件

![1550819787607](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\1550819787607.png)

**注意:**

- 如果希望在捕获阶段就触发事件，可以将addEventListener()的第三个参数设置为true,一般情况下我们不会希望在捕获阶段触发事件，所以这个事件一般都是false

- IE8及以下没有捕获阶段




## 10.拖拽事件

当我们拖拽一个网页中的内容时，浏览器会默认去搜索引擎中搜索内容，此时会导致拖拽功能的异常，这个是浏览器提供的默认行为,如果不希望发生这个行为，可以通过return false来取消默认行为，但是对IE8及以下浏览器不起作用(并不是说IE8不支持使用return false来取消默认效果，只是不支持在拖拽时取消默认效果)

**注意:**

- **使用addEventListener()方法绑定的响应函数，取消默认行为时不能使用return false,需要使用event事件对象通过event.preventDefault()方法来取消默认行为**,但是IE8及以下浏览器不能支持该方法(包括addEventListener()方法)，使用该方法会报错，所以在调用的时候需要使用：`event.preventDefault&&:event.preventDefault();`

-  **IE8中有setCapture()这一对鼠标按下相关事件进行捕获的方法，**当一个调用一个元素的setCapture()方法以后，这个元素将会把下一次所有的鼠标按下相关的事件捕获到自己身上，其他鼠标触发都会由这个元素执行(包含在网页外面点击)，所以可以设置需要进行的行为对象在按下鼠标的时候捕获到所有的事件

  当鼠标松开时，取消对事件的捕获，**取消捕获用releaseCapture()方法**

  ```js
  var Odiv=document.getElementById("Odiv");
  Odiv.attachEvent("onmousedown",function(){
      Odiv.setCapture();
      Odiv.attachEvent("onmousemove",function(){
      console.log(123);
  });
  });
  Odiv.attachEvent("onmouseup",function(){
      Odiv.releaseCapture();
      console.log(456);
  });
  ```

  **注:**

  - setCapture()和releaseCapture()必须成对出现

  - 其它浏览器一样不支持该方法,所以在调用的时候用

    `Odiv.setCapture&&Odiv.setCapture()`和`Odiv.releaseCapture()&&Odiv.releaseCapture()`

 

## 11.滚轮事件

- **onscroll事件为元素添加滚动(注意不是滚轮,滚动是滚动条发生变化)事件,该事件是滚动条变化就发生事件,所以一次鼠标滚动可能会发生很多次该事件**
- **onwheel鼠标滚动事件,会在鼠标滚动时触发,推荐使用该事件**
- onmousewheel鼠标滚轮事件，会在滚轮滚动时触发，但是火狐不支持该属性，在火狐中需要使用DOMMouseScroll来绑定滚动事件，注意该事件需要通过**addEventListener()方法**来绑定



**鼠标滚轮滚动方向**

- 通过事件对象event.wheelDelta可以获取鼠标滚轮滚动的方向，**向上滚为正，向下滚为负,值为数值120的倍数(一般为120,滚动过快会为120的倍数) **

  **注:**只看正负，不看大小

- 在火狐中不支持event.wheelDelta属性,使用事件对象event.detail，**向上滚为负，向下滚为正**，与上方属性相反，**并且每次滚动的数值为3的倍数(一般为3,滚动过快会为3的倍数)**


```html
<!--通过滚轮控制盒子大小-->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>鼠标的滚轮事件</title>
    <style>
      #box1 {
        width: 100px;
        height: 100px;
        background-color: red;
        margin: 500px auto;
      }
    </style>
    <script>
      window.onload = function() {
        var box1 = document.getElementById("box1");
        //为box1绑定一个鼠标滚轮的事件
        box1.onmousewheel = function(event) {
          event = window.event || event;
          //火狐对鼠标滚轮方向的获取的属性时detail，其他浏览器是wheelDelta
          //鼠标向上滚时wheelDelta是正数，向下是负数，detail刚好相反。
          if (event.wheelDelta > 0 || event.detail < 0) {
            box1.style.height = parseFloat(box1.style.height) - 10 + "px"; //两种方式都可以，需要注意style.height必须是内联样式
            //所以必须用内联样式，同时这个的返回值是有单位的，必须转换
            box1.style.width = box1.clientWidth - 10 + "px";
          }
          //当鼠标滚轮向下滚动,box1变长
          //当鼠标滚轮向上滚动，box1变短
          else {
            box1.style.height = parseFloat(box1.style.height) + 10 + "px";
            box1.style.width = box1.clientWidth + 10 + "px";
          }
          //这个只是为了再火狐通过addEventListener()方法添加事件的时候用
          event.preventDefault && event.preventDefault();
          //去除默认事件
          return false;
        };

        addEvent(box1, "DOMMouseScroll", box1.onmousewheel);//给火狐添加

        function addEvent(box1, eventStr, callback) {
          if (box1.addEventListener) {
            box1.addEventListener(eventStr, callback, false);
          } else {
            box1.attachEvent("on" + eventStr, function() {
              callback.call(box1);
            });
          }
        }
      };
    </script>
  </head>
  <body>
    <div id="box1" style="height: 100px"></div>
  </body>
</html>

```



**兼容写法(包括控制滚动的方向)**

```js
function mousewheel(dom,callback,bool){
    //bool为传入的一个布尔值,如果是true则阻止默认行为,默认是不阻止
    var type="mousewheel";
  
    if(dom.onmousewheel===undefined){//不能通过判断下面的那个,因为在火狐中dom属性也没有这个属性
        type="DOMMouseScroll";
    }
    //真正的事件函数
    function cb(event){
        /*
        	自定义属性event.wheelDtail控制鼠标滚轮,所以在外部使用时应该用event.wheelDetail
        */
        //外部可以传入event作为事件对象也可以不传入直接使用,也不需要进行兼容,但是不传入时应该用event
        event=event||window.event;
        //把滚动事件的方向处理一致
        event.wheelDetail=event.wheelDelta/123 || event.detail/-3
        //每次滚动的值为1,并且向上滚动为正值,向下滚动为负值
        
        //阻止默认行为
        if(!!bool){
            if(event.preventDefault){
                event.preventDefault();
            }else{
                event.returnValue=flase;
            }
        }
        callback.call(this,event);
    }
    
    if(dom.addEventListener){
        dom.addEventListener(type,cb);
    }else{
        dom,attachEvent("on"+type,cb)
    }
}
```



## 12.键盘事件

### 12.1 事件触发

- **onkeydown事件触发表明键盘按键被按下,如果一直按着某个按键不松手，则事件会一直触发**

  **注意:**当onkeydown事件连续触发时，第一次和第二次之间会间隔稍微长一点，其他的会非常快，这种设计是为了防止误操作事故的发生,如果不想要这种效果请使用定时器

- **onkeyup事件触发表明键盘按键被松开**

**注:**键盘事件有一般都会绑定给一些可以获取到焦点的对象或者是document，有焦点的对象就是可以让光标停留的地方，比如input标签

```js
var input=document.getElementById("input");
input.onkeydowm=function(){

}
```



### 12.2 事件属性

**通过事件对象event的keyCode和key属性可以获取按键的编码和输入键的字符**，可以判断哪个按键被按下

```js
if(event.keyCode===89){
 console.log("y被按下了");
}

if(event.key==="y"){
    console.log("y被按下了");
}
```

**事件对象中还提供了几个辅助属性**

- altKey属性判断alt键是否被按下，如果按下则返回true，否则返回false
- ctrlKey属性判断ctrl键是否被按下，如果按下则返回true，否则返回false
-  shiftKey属性判断shift键是否被按下，如果按下则返回true，否则返回false

```js
//判断y和ctrl是否同时被按下
document.onkeydown=function(event){
    event=event||window.event;
	if(event.keyCode===89&&event.ctrlKey){
			console.log("ctrl和y都被按下了")
		}
}

```



**注意:在文本框中输入内容，属于onkeydown的默认行为，如果在onkeydown中取消了默认行为return false，则输入的内容不会出现在文本框中**

```js
//文本框中不能输入数字
input,onkeydown=function(event){

	event=event||window.event;
	if(event.keyCode>=48&&event.keyCode<=57){
			return false;
		}
}
```
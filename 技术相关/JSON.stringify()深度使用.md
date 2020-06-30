# JSON.stringify()深度使用

## 1.注意点

**在使用JSON.stringify()对JSON数据进行序列化时需要注意以下几点：**

- 如果里面的属性是function,则会被忽略

    ```js
    const data = {
         a: 'a',
         fn: funciton() {
               return true   
          }   
    }

    JSON.stringify(data); // "{"a":"a"}",fn属性被忽略了
    ```

- 如果里面的属性的值是undefined, 也是会被忽略的

    ```js
    const data = {
         a: 'a',
         b: undefined
    }

    JSON.stringify(data); // "{"a":"a"}",b属性被忽略了
    ```

- 但是如果里面的属性值是null, 是不会被忽略的

    ```js
    const data = {
         a: 'a',
         b: null
    }

    JSON.stringify(data); // "{"a":"a","b":null}",b属性没有被忽略
    ```
    
- 如果对象中某个熟悉不可枚举，也会被忽略

    ```js
    const obj = Object.create({},{
      name:{
        value:'zhangsan',
        enumerable:false
      },
      age:{
        value:18,
        enumerable:true
      }
    })
    console.log(obj)  // {age: 18, name: "zhangsan"}
    console.log(JSON.stringify(obj))  // {"age":18}
    ```

    

### 1.1 toJSON 方法

**注意：**自定义对象属性的时候, 尽量不要定义toJSON方法，因为toJSON会把其他的属性都覆盖掉

```js
const data = {
     a: 'a',
     toJSON: function() {
         return true;
    }
}
JSON.stringify(data); //"true"  (toJSON会把其他的属性都覆盖掉)
```

如果`toJSON`没有返回值，结果就为`undefined`

关于`toJSON	的使用，`Date类型内部就对其做了修改：

```js
console.log(JSON.stringify(new Date()))  // "2020-06-21T11:55:43.970Z"
```



## 2.解决方案

**同样，JS为我们提供了自定义的转换规则来适应这种情况**

**JSON.stringify()其实可以接收有三个参数：**

- **value：**转换对象
- **replacer：自定义的函数，**
- **space：**格式化输出，相当于tab键，值的范围是[1(负数的时候默认是１)，10]

**为了属性值为function和undefined的属性在序列化的时候不要被忽略，我们可以对replacer做以下操作**

```js
const replace = function(k, v) {
  if(v === undefined) {　　　　return 'undefined';　　 }
  if(typeof v === 'function') {
    return Function.prototype.toString.call(v);
  }
  return v;
}

JSON.stringify(data, replace);  //"{"a":"a","c":"undefined","b":"function () {\n         return true;\n    }"}"
```

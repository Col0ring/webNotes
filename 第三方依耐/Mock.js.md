# Mock.js

> [Mokc.js指南](https://github.com/nuysoft/Mock/wiki)

## 1.安装

```shell
npm install mockjs
```

```js
// 使用 Mock
var Mock = require('mockjs')
var data = Mock.mock({
    // 属性 list 的值是一个数组，其中含有 1 到 10 个元素
    'list|1-10': [{
        // 属性 id 是一个自增数，起始值为 1，每次增 1
        'id|+1': 1
    }]
})
// 输出结果
console.log(JSON.stringify(data, null, 4))
```



## 2.Mock语法

**Mock.js的语法规范包括两部分：**

1. 数据模板定义规范（Data Template Definition，DTD）
2. 数据占位符定义规范（Data Placeholder Definition，DPD）

### 2.1 数据模板定义规范 DTD

**数据模板中的每个属性由 3 部分构成：属性名、生成规则、属性值**

```js
// 属性名   name
// 生成规则 rule
// 属性值   value
'name|rule': value
```

**注意：**

- 属性名 和生成规则之间用竖线 `|` 分隔
- 生成规则 是可选的
- 生成规则有 7 种格式：

  + `'name|min-max': value`

  + `'name|count': value`
  + `'name|min-max.dmin-dmax': value`
  + `'name|min-max.dcount': value`
  + `'name|count.dmin-dmax': value`
  + `'name|count.dcount': value`
  + `'name|+step': value`
- **生成规则 的 含义 需要依赖 属性值的类型 才能确定**
- 属性值中可以含有 `@占位符`
- 属性值还指定了最终值的初始值和类型

#### 2.1.1 属性值是字符串String

1. `'name|min-max': string`

   通过重复 `string` 生成一个字符串，重复次数大于等于 `min`，小于等于 `max`

2. `'name|count': string`

   通过重复 `string` 生成一个字符串，重复次数等于 `count`

   

#### 2.1.2  属性值是数字Number

1. `'name|+1': number`

   属性值自动加 1，初始值为 `number`。

2. `'name|min-max': number`

   生成一个大于等于 `min`、小于等于 `max` 的整数，属性值 `number` 只是用来确定类型。

3. `'name|min-max.dmin-dmax': number`

   生成一个浮点数，整数部分大于等于 `min`、小于等于 `max`，小数部分保留 `dmin` 到 `dmax` 位。

```js
Mock.mock({
    'number1|1-100.1-10': 1,
    'number2|123.1-10': 1,
    'number3|123.3': 1,
    'number4|123.10': 1.123
})
// =>
{
    "number1": 12.92,
    "number2": 123.51,
    "number3": 123.777,
    "number4": 123.1231091814
}
```



#### 2.1.3. 属性值是布尔型Boolean

1. `'name|1': boolean`

   随机生成一个布尔值，值为 true 的概率是 1/2，值为 false 的概率同样是 1/2

2. `'name|min-max': value`

   随机生成一个布尔值，值为 `value` 的概率是 `min / (min + max)`，值为 `!value` 的概率是 `max / (min + max)`

   

#### 2.1.4. 属性值是对象Object

1. `'name|count': object`

   从属性值 `object` 中随机选取 `count` 个属性

2. `'name|min-max': object`

   从属性值 `object` 中随机选取 `min` 到 `max` 个属性

   

#### 2.1.5 属性值是数组Array

1. `'name|1': array`

   从属性值 `array` 中随机选取 1 个元素，作为最终值。

2. `'name|+1': array`

   从属性值 `array` 中顺序选取 1 个元素，作为最终值。

3. `'name|min-max': array`

   通过重复属性值 `array` 生成一个新数组，重复次数大于等于 `min`，小于等于 `max`。

4. `'name|count': array`

   通过重复属性值 `array` 生成一个新数组，重复次数为 `count`。



#### 2.1.6 属性值是函数Function

1. `'name': function`

   执行函数 `function`，取其返回值作为最终的属性值，函数的上下文为属性 `'name'` 所在的对象。



#### 2.1.7 属性值是正则表达式RegExp

1. `'name': regexp`

   根据正则表达式 `regexp` 反向生成可以匹配它的字符串。用于生成自定义格式的字符串。

   ```js
   Mock.mock({
       'regexp1': /[a-z][A-Z][0-9]/,
       'regexp2': /\w\W\s\S\d\D/,
       'regexp3': /\d{5,10}/
   })
   // =>
   {
       "regexp1": "pJ7",
       "regexp2": "F)\fp1G",
       "regexp3": "561659409"
   }
   ```



### 2.2 数据占位符定义规范 DPD

**占位符**只是在属性值字符串中占个位置，并不出现在最终的属性值中

**占位符**的格式为：

```js
@占位符
@占位符(参数 [, 参数])
```

**注意：**

1. 用 `@` 来标识其后的字符串是占位符
2. 占位符引用的是 `Mock.Random` 中的方法
3. 通过 `Mock.Random.extend()` 来扩展自定义占位符
4. 占位符也可以引用 *数据模板* 中的属性
5. 占位符会优先引用 *数据模板* 中的属性
6. 占位符支持相对路径和绝对路径

```js
Mock.mock({
    name: {
        first: '@FIRST',
        middle: '@FIRST',
        last: '@LAST',
        full: '@first @middle @last'
    }
})
// =>
{
    "name": {
        "first": "Charles",
        "middle": "Brenda",
        "last": "Lopez",
        "full": "Charles Brenda Lopez"
    }
}
```



## 3.Mock.mock()

### 3.1 Mock.mock( rurl?, rtype?, template|function( options ) )

根据数据模板生成模拟数据



### 3.2 Mock.mock( template )

根据数据模板生成模拟数据



### 3.3 Mock.mock( rurl, tmplate )

记录数据模板。当拦截到匹配 `rurl` 的 Ajax 请求时，将根据数据模板 `template` 生成模拟数据，并作为响应数据返回



### 3.4 Mock.mock( rurl, function( options ) )

记录用于生成响应数据的函数。当拦截到匹配 `rurl` 的 Ajax 请求时，函数 `function(options)` 将被执行，并把执行结果作为响应数据返回



### 3.5 Mock.mock( rurl, rtype, template )

记录数据模板。当拦截到匹配 `rurl` 和 `rtype` 的 Ajax 请求时，将根据数据模板 `template` 生成模拟数据，并作为响应数据返回



### 3.6 Mock.mock( rurl, rtype, function( options ) )

记录用于生成响应数据的函数。当拦截到匹配 `rurl` 和 `rtype` 的 Ajax 请求时，函数 `function(options)` 将被执行，并把执行结果作为响应数据返回



###  3.7 参数

- rurl，可选。表示需要拦截的 URL，可以是 URL 字符串或 URL 正则。例如 `/\/domain\/list\.json/`、`'/domian/list.json'`
- rtype可选。表示需要拦截的 Ajax 请求类型。例如 `GET`、`POST`、`PUT`、`DELETE` 等

- template，可选。表示数据模板，可以是对象或字符串。例如 `{ 'data|1-10':[{}] }`、`'@EMAIL'`

- function(options)，可选。表示用于生成响应数据的函数
  - options，指向本次请求的 Ajax 选项集，含有 `url`、`type` 和 `body` 三个属性



## 4.Mock.setup()

Mock.setup( settings )用于配置拦截 Ajax 请求时的行为。支持的配置项有：`timeout`

**参数：**

- settings：必选。配置项集合
  - timeout：可选。指定被拦截的 Ajax 请求的响应时间，单位是毫秒。值可以是正整数，例如 `400`，表示 400 毫秒 后才会返回响应内容；也可以是横杠 `'-'` 风格的字符串，例如 `'200-600'`，表示响应时间介于 200 和 600 毫秒之间。默认值是`'10-100'`

      ```js
      Mock.setup({
          timeout: 400
      })
      Mock.setup({
          timeout: '200-600'
      })
      ```

**注：**目前，接口 `Mock.setup( settings )` 仅用于配置 Ajax 请求，将来可能用于配置 Mock 的其他行为



## 5.Mock.Random()

Mock.Random 是一个工具类，用于生成各种随机数据。**Mock.Random 的方法在数据模板中称为『占位符』，书写格式为 @占位符(参数 [, 参数]) 。**

```js
var Random = Mock.Random
Random.email()
// => "n.clark@miller.io"
Mock.mock('@email')
// => "y.lee@lewis.org"
Mock.mock( { email: '@email' } )
// => { email: "v.lewis@hall.gov" }
```

### 5.1 方法

**Mock.Random 提供的完整方法（占位符）如下：**

| Type          | Method                                                       |
| ------------- | ------------------------------------------------------------ |
| Basic         | boolean, natural, integer, float, character, string, range, date, time, datetime, now |
| Image         | image, dataImage                                             |
| Color         | color                                                        |
| Text          | paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle |
| Name          | first, last, name, cfirst, clast, cname                      |
| Web           | url, domain, email, ip, tld                                  |
| Address       | area, region                                                 |
| Helper        | capitalize, upper, lower, pick, shuffle                      |
| Miscellaneous | guid, id                                                     |

具体的随机方法见：[Mock的随机方法](https://github.com/nuysoft/Mock/wiki/Mock.Random)



### 5.2 扩展

**Mock.Random 中的方法与数据模板的 `@占位符` 一一对应，在需要时还可以为 Mock.Random 扩展方法，然后在数据模板中通过 `@扩展方法` 引用**

```js
Random.extend({
    constellation: function(date) {
        var constellations = ['白羊座', '金牛座', '双子座', '巨蟹座', '狮子座', '处女座', '天秤座', '天蝎座', '射手座', '摩羯座', '水瓶座', '双鱼座']
        return this.pick(constellations)
    }
})
Random.constellation()
// => "水瓶座"
Mock.mock('@CONSTELLATION')
// => "天蝎座"
Mock.mock({
    constellation: '@CONSTELLATION'
})
// => { constellation: "射手座" }
```



## 6.Mock.valid()

**Mock.valid( template, data )**用于校验真实数据 `data` 是否与数据模板 `template` 匹配

**参数：**

- template：必选。表示数据模板，可以是对象或字符串。例如 `{ 'list|1-10':[{}] }`、`'@EMAIL'`

- data：必选。表示真实数据

```js
var template = {
    name: 'value1'
}
var data = {
    name: 'value2'
}
Mock.valid(template, data)
// =>
[
    {
        "path": [
            "data",
            "name"
        ],
        "type": "value",
        "actual": "value2",
        "expected": "value1",
        "action": "equal to",
        "message": "[VALUE] Expect ROOT.name'value is equal to value1, but is value2"
    }
]
```



## 7.Mock.toJSONSchema()

**Mock.toJSONSchema( template )**用于把 Mock.js 风格的数据模板 `template` 转换成 [JSON Schema](http://json-schema.org/

**参数：**

- template：必选。表示数据模板，可以是对象或字符串。例如 `{ 'list|1-10':[{}] }`、`'@EMAIL'`

```js
var template = {
    'key|1-10': '★'
}
Mock.toJSONSchema(template)
// =>
{
    "name": undefined,
    "path": [
        "ROOT"
    ],
    "type": "object",
    "template": {
        "key|1-10": "★"
    },
    "rule": {},
    "properties": [{
        "name": "key",
        "path": [
            "ROOT",
            "key"
        ],
        "type": "string",
        "template": "★",
        "rule": {
            "parameters": ["key|1-10", "key", null, "1-10", null],
            "range": ["1-10", "1", "10"],
            "min": 1,
            "max": 10,
            "count": 3,
            "decimal": undefined,
            "dmin": undefined,
            "dmax": undefined,
            "dcount": undefined
        }
    }]
}
```

```js
var template = {
    'list|1-10': [{}]
}
Mock.toJSONSchema(template)
// =>
{
    "name": undefined,
    "path": ["ROOT"],
    "type": "object",
    "template": {
        "list|1-10": [{}]
    },
    "rule": {},
    "properties": [{
        "name": "list",
        "path": ["ROOT", "list"],
        "type": "array",
        "template": [{}],
        "rule": {
            "parameters": ["list|1-10", "list", null, "1-10", null],
            "range": ["1-10", "1", "10"],
            "min": 1,
            "max": 10,
            "count": 6,
            "decimal": undefined,
            "dmin": undefined,
            "dmax": undefined,
            "dcount": undefined
        },
        "items": [{
            "name": 0,
            "path": ["data", "list", 0],
            "type": "object",
            "template": {},
            "rule": {},
            "properties": []
        }]
    }]
}
```



## 9.模块化

```js
// mock/index.js
// 只要引入了全局的mock就能使用扩展
import './extend'
import './goods'
```

```js
// mock/extend.js
import { Random } from 'mockjs'

Random.extend({
  fruit() {
    const arr = ['苹果', '香蕉', '榴莲', '橘子', '菠萝']
    return this.pick(arr)
  }
})

```

```js
// mock/goods.js
// 导入模拟数据的包
import Mock from 'mockjs'
// 通过Mock.mock()来模拟API接口
/*
    第一个参数是拦截的地址，第二个参数是拦截的请求，第三个参数是要返回的模拟数据
*/

Mock.mock('/api/goodslist', 'get', {
  status: 200,
  message: '获取商品列表成功',
  'data|5-10': [
    //返回的数据,data对象中是一个5到10为数的数组
    {
      id: '@increment(1)', //自增的整数,从1开始
      /*
          'id|+1':1 上面的方法等同于这个，这是mock语法，也是在模拟每次id值+1
          */
      name: '@cword(2,10)', //2到10个中文
      price: '@natural(2,8)', //2到8的自然数
      count: '@natural(100,999)', //100到999的自然数
      img: '@dataImage(100x100)' //100x100的图片
    }
  ]
})

Mock.mock('/api/addgoods', 'post', function(options) {
  // 这里的options是请求的相关参数
  console.log(options)

  return Mock.mock({
    /*
      post请求返回的参数，外层是拦截ajax的，内层是返回的，所以必须再次嵌套Mock.mock()
      */
    status: 200,
    message: '@cword(2,5)'
  })
})

//通过正则进行匹配
Mock.mock(/\/api\/getgoods/, 'get', function(options) {
  //options是通过ajax发起的参数
  console.log(options)
  //通过正则的.exec()函数从字符串中提取需要的数据,exec能将分组的数据变成数组
  const res = /\/api\/getgoods\/(\d+)/.exec(options.url)

  return Mock.mock({
    data: {
      id: res[1] - 0,
      name: '@fruit()', //扩展的自定义函数
      price: '@natural(2,8)',
      count: '@natural(100,999)'
    }
  })
})
```


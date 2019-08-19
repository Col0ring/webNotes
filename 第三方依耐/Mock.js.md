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
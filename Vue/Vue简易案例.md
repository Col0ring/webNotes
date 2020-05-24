# Vue简易案例

## 1.Vue实现城市选择

**当选择不同省的时候,会在后一个城市的选择中出现对应省的城市**

```html
<div id="app">
    <select v-model="province"><!--通过v-modle双向绑定province-->
        <option v-for="item in provinces" :value="item">{{item}}</option>
    </select>
    <select v-model="city">
        <option v-for="item in cities" :value="item">{{item}}</option>
    </select>
</div>

<script>
    new Vue({
        el:"#app",
        data:{
            provinces:[],
            cities:[],
            province:null,
            city:null,
        },
        watch:{
            province(value){//通过监听province的值来实现动态改变cities的值,一般都是通过接口赋值
                if(value==="浙江省"){
                    this.cities=["杭州市","嘉兴市"]
                }else if(value==="四川省"){
                    this.cities=["成都市","绵阳市","乐山市"]
                }
            }  
        },
        created(){
            //真实案例是通过接口实现
            this.provinces=[
                "省",
                "浙江省",
                "四川省"
            ]
        }
    })
</script>
```



## 2.Vue-computed实现登录效果

**通过Vue的计算属性对登录按钮实现控制,只有当所有的内容都填写完毕后才能够使用登录按钮**

```html
<div id="app">
    <form @submit.prevent="login">
        <input type="text" v-model="mobile">
        <input type="password" v-model="password">
        <button type="submit" :disabled="!isValid">
            Login
        </button>
    </form>
</div>

<script>
    new Vue({
        el:"#app",
        data:{
            mobile:"",
            password:""
        },
        computed:{
            isValid(){
                return String(this.mobile).match(/^1\d{10}$/)&&String(this.password.length)>=6
            }
        },
        methods:{
            login(){
                window.location.href="https://www.baidu.com/";
            }
        }
    })
</script>
```



## 3.大数据加载特效

```vue
<template>
  <div class="dv-loading">
    <svg width="50px" height="50px">
      <circle
        cx="25"
        cy="25"
        r="20"
        fill="transparent"
        stroke-width="3"
        stroke-dasharray="31.415 31.415"
        stroke="#02bcfe"
        stroke-linecap="round"
      >
        <animateTransform
          attributeName="transform"
          type="rotate"
          values="0, 25 25;360, 25 25"
          dur="1.5s"
          repeatCount="indefinite"
        />
        <animate
          attributeName="stroke"
          values="#02bcfe;#3be6cb;#02bcfe"
          dur="3s"
          repeatCount="indefinite"
        />
      </circle>

      <circle
        cx="25"
        cy="25"
        r="10"
        fill="transparent"
        stroke-width="3"
        stroke-dasharray="15.7 15.7"
        stroke="#3be6cb"
        stroke-linecap="round"
      >
        <animateTransform
          attributeName="transform"
          type="rotate"
          values="360, 25 25;0, 25 25"
          dur="1.5s"
          repeatCount="indefinite"
        />
        <animate
          attributeName="stroke"
          values="#3be6cb;#02bcfe;#3be6cb"
          dur="3s"
          repeatCount="indefinite"
        />
      </circle>
    </svg>
    <div class="loading-tip">
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  name: 'DvLoading'
}
</script>

<style lang="less" scoped>
.dv-loading {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.dv-loading .loading-tip {
  font-size: 15px;
}
</style>
```



## 4.Vue指令实现拖拽功能

```js
// 这个助手方法下面会用到，用来获取 css 相关属性值
const getAttr = (obj, key) =>
  obj.currentStyle
    ? obj.currentStyle[key]
    : window.getComputedStyle(obj, false)[key]

const vDrag = {
  inserted(el) {
    /**
     * 这里是跟据 dialog 组件的 dom 结构来写的
     * target: dialog 组件的容器元素
     * header：dialog 组件的头部区域，也是就是拖拽的区域
     */
    const target = el.children[0]
    const header = target.children[0]

    // 鼠标手型
    header.style.cursor = 'move'
    header.onmousedown = e => {
      // 记录按下时鼠标的坐标和目标元素的 left、top 值
      const currentX = e.clientX
      const currentY = e.clientY
      const left = parseInt(getAttr(target, 'left'))
      const top = parseInt(getAttr(target, 'top'))

      document.onmousemove = event => {
        // 鼠标移动时计算每次移动的距离，并改变拖拽元素的定位
        const disX = event.clientX - currentX
        const disY = event.clientY - currentY
        target.style.left = `${left + disX}px`
        target.style.top = `${top + disY}px`

        // 阻止事件的默认行为，可以解决选中文本的时候拖不动
        return false
      }

      // 鼠标松开时，拖拽结束
      document.onmouseup = () => {
        document.onmousemove = null
        document.onmouseup = null
      }
      // 分别计算四个方向的边界值
      const minLeft =
        target.offsetLeft + parseInt(getAttr(target, 'width')) - 50
      const maxLeft =
        parseInt(getAttr(document.body, 'width')) - target.offsetLeft - 50
      const minTop = target.offsetTop
      const maxTop =
        parseInt(getAttr(document.body, 'height')) -
        target.offsetTop -
        parseInt(getAttr(header, 'height'))

      document.onmousemove = event => {
        // 鼠标移动时计算每次移动的距离，并改变拖拽元素的定位
        const disX = event.clientX - currentX
        const disY = event.clientY - currentY

        // 判断左、右边界
        if (disX < 0 && disX <= -minLeft) {
          target.style.left = `${left - minLeft}px`
        } else if (disX > 0 && disX >= maxLeft) {
          target.style.left = `${left + maxLeft}px`
        } else {
          target.style.left = `${left + disX}px`
        }

        // 判断上、下边界
        if (disY < 0 && disY <= -minTop) {
          target.style.top = `${top - minTop}px`
        } else if (disY > 0 && disY >= maxTop) {
          target.style.top = `${top + maxTop}px`
        } else {
          target.style.top = `${top + disY}px`
        }
        return false
      }
    }
  },

  // 每次重新打开 dialog 时，要将其还原
  update(el) {
    const target = el.children[0]
    target.style.left = ''
    target.style.top = ''
  },

  // 最后卸载时，清除事件绑定
  unbind(el) {
    const header = el.children[0].children[0]
    header.onmousedown = null
  }
}

export default vDrap
```

```vue
<!-- 使用 -->
<el-dialog v-drag title="对话框" :visible.sync="dialogVisible"></el-dialog>
```



## 5.实现高阶组件

```js
function WithConsole (WrappedComponent) {
    return {
        mounted () {
            console.log('I have already mounted')
        },
        // 高阶组件的props和被接受的props一样
        props: WrappedComponent.props,
        render (h) {
            // this.slots是传入的的插槽
            const slots = Object.keys(this.$slots)
            .reduce((arr, key) => arr.concat(this.$slots[key]), [])
            // 手动更正 context
            .map(vnode => {
              vnode.context = this._self //绑定到高阶组件上(默认子组件找的是父组件)
              return vnode
            })

            return h(WrappedComponent, {
                // this.$listeners是传入的所有事件
                on: this.$listeners,
                // this.$props是设置的的所有props
                props: this.$props,
                // this.$attrs为所有写在高阶组件上的attrs
                attrs: this.$attrs
                // 第三个参数为slots
            }, slots)
        }
    }
}
```


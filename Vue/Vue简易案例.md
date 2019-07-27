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


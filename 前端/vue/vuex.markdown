- [**VueX使用手册**](#vuex使用手册)
  - [**导入VueX**](#导入vuex)
  - [**使用Vuex**](#使用vuex)
    - [**基本使用**](#基本使用)
      - [**js部分**](#js部分)
      - [**在组件中访问 `state`数据**](#在组件中访问-state数据)
      - [**修改state 的数据**](#修改state-的数据)
      - [**Getter**](#getter)
# **VueX使用手册**
## **导入VueX**
> 
## **使用Vuex**
### **基本使用**

#### **js部分**

创建
**`store.js`**文件
```js
/* 导入 Vue 和 VueX */
import Vue from 'vue'
import Vuex from 'vuex'

/* 把 Vuex 安装为 Vue的插件 */
Vue.use(Vuex)

/* 创建 Store  对象*/
const store = new Vuex.Store({
   state:{},
   mutations:{},
   actions:{},
   getter:{},
   modules:{}
   /* Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式；其中有五个属性，state，getters，mutations,actions，modules；
    (1) state：vuex的基本数据，用来存储变量
    (2) getters：从基本数据(state)派生的数据，相当于state的计算属性
    (3) mutations： mutation同步操作，提交更新数据的方法，是通过修改state来直接变更状态
    (4) actions：Action异步操作，和mutation的功能大体相同，不同的是Action提交的mutation，而不是直接变更状态；action先会执行异步操作再去调用mutation，随后才跟新state；
    (5) modules:模块化vuex，能让每个模块拥有自己的state、mutation、action、getters,使得结构非常清晰，方便管理 */
})

export default store
```
在**`main.js`**中
```js
import App from './App'
import Vue from 'vue'
import store from '@/store/store.js'
Vue.config.productionTip = false
App.mpType = 'app'
const app = new Vue({
    ...App,
	store
})
app.$mount()

```
#### **在组件中访问 `state`数据**
方式一
> 在差值表达式`{{}}` 中访问直接使用`{{$store.state.要使用的数据}}`

方式二
> 使用 `import { mapState } from 'vuex'` 导入`mapState()`函数 <br/> 然后在计算属性`computed`中使用结构赋值`...mapState(['数据名'])`
> 然后就可以直接在差值表达式中使用这个`数据名`

#### **修改state 的数据**
**同步方式**

方式一
> 直接修改 `this.$state.state.数据名 = 新数据`  可以这样用但是不推荐

方式二
> 使用 `mutations`在里面声明事件处理函数 
```javascript
{
    state:{ 
        num
    },
    mutations:{
        add(state,参数列表...){//事件处理函数 第一个参数是自己的state 后面是传入的参数
            state.num=a
        },
        del(state,参数列表...){//事件处理函数
            state.num=a
        }
 },
}
 
```
> 然后在组件代码中使用 `this.$State.commit('事件函数名',参数列表...)`

**异步方式**
```js
//在 action中
action{
    异步方法(context,参数列表...){
        setTimeout(()=>{
            context.commit('事件方法名',参数列表...)
        },1000)
    },...
}
//在 组件中 调用
    this.$store.dispatch('异步方法名',参数列表...)
    //或者
    import { mapActions } from 'vuex'
    methods:{
        mapActions(['异步方法名','异步方法名'])
    }
```
#### **Getter**
> 类似于计算属性

使用getter
```js
//第一种
this.$store.getters.名
//第二种
import { mapGetters } from 'vuex'

computed:{
    ...mapGetters(['getter名'])
}
```
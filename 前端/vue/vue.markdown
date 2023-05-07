- [**vue**](#vue)
  - [**基本**](#基本)
    - [*data 数据*](#data-数据)
    - [`vue.set()`函数](#vueset函数)
    - [`vue.delete()`函数](#vuedelete函数)
    - [*computed计算属性*](#computed计算属性)
    - [*watch监听*](#watch监听)
  - [**指令**](#指令)
    - [`v-pre`](#v-pre)
  - [**事件**](#事件)
    - [全局事件总线](#全局事件总线)
  - [**插件**](#插件)
  - [**动画**](#动画)
    - [*vue控制*](#vue控制)
    - [*集成第三方*](#集成第三方)
  - [**组件**](#组件)
  - [**动态组件 isComponent**](#动态组件-iscomponent)
  - [混入 mixin](#混入-mixin)
  - [**插槽**](#插槽)
    - [*默认插槽*](#默认插槽)
    - [*具名插槽*](#具名插槽)
  - [**路由**](#路由)
    - [*基本使用*](#基本使用)
      - [配置](#配置)
      - [`router-view`](#router-view)
      - [`router-link`](#router-link)
    - [*命名路由*](#命名路由)
    - [*路由传参*](#路由传参)
      - [`query`参数](#query参数)
      - [`params`参数](#params参数)
    - [`replace`](#replace)
    - [*编程式路由导航*](#编程式路由导航)
      - [`push`](#push)
      - [`replace`](#replace-1)
      - [`back`](#back)
      - [`forward`](#forward)
      - [`go`](#go)
    - [*路由组件缓存*](#路由组件缓存)
    - [*路由守卫*](#路由守卫)
      - [前置路由守卫](#前置路由守卫)
      - [后置路由守卫](#后置路由守卫)
      - [组件内路由守卫](#组件内路由守卫)
    - [路由模式切换](#路由模式切换)
  - [**生命周期**](#生命周期)
    - [`$nextTick`](#nexttick)
    - [路由组件生命周期](#路由组件生命周期)
      - [`activated` 激活](#activated-激活)
      - [`deactivated` 失活](#deactivated-失活)
  - [使用Less](#使用less)

# **vue**
## **基本**
### *data 数据*
### `vue.set()`函数
> vue.set(对象名,属性名,属性值)解决添加属性 页面不更新的问题  除此之外还有 this.$set()
### `vue.delete()`函数
> vue.delete(对象名,属性名) 解决删除属性不更新的问题
### *computed计算属性*
```js
//在与 data() 同级出添加
 computed: {
    // 计算属性的 名字
    num () {
      // `this` 指向 vm 实例
      return this.xxx
    },
    num:{
      get(){},
      set(val){
      }
    }
```
### *watch监听*
```js
watch : {
    watchData(newValue, oldValue) {
      //行为
    },
    watchData : {
      immediate:true,
      keep : true,
      handler(newValue, oldValue){
        //行为
      }
    }
  }
```
## **指令**
### `v-pre`
> 加上这个指令的 元素可以跳过vue 的编译 原来是什么样子 呈现到 页面上就是什么样子
## **事件**
### 全局事件总线
安装全局事件总线
```js
/* main.js 中使用beforeCreate */
beforeCreate(){
    /* 安装全局时间事件总线 */
    Vue.prototype.$bus=this
  },
```
## **插件**

## **动画**
### *vue控制*
### *集成第三方*
## **组件**
## **动态组件 isComponent**
## 混入 mixin
## **插槽**
### *默认插槽*
### *具名插槽*
## **路由**
### *基本使用*
#### 配置
在src目录下创建router/index.js
```js
import Vue from "vue";
import VueRouter from "vue-router";
Vue.use(VueRouter);

const routes = [
  {
    path: "/",//路径
    name: "index",//命名路由
    redirect: "/index"//重定向
  },
  {
    path: "/index",
    name: "Index",
    component: Index,//路由组件
    mate:{isAuth:true},// 存放一些数据
    children: [//子路由
        {
            path:'pay'//子路由的path路径前面不用加/
        }
    ]
  },
  {
    path: "/me",
    name: "Me",
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited. 懒加载
    component: () =>
      import(/* webpackChunkName: "about" */ "../views/Me.vue"),
  },
];

const router = new VueRouter({
  routes,
});

export default router;
```
#### `router-view`
> 标签`<router-view/>`用于显示路由组件
#### `router-link`
>`<router-link></router-link>` 用于路由导航

标签属性 to 用法
```html
<!-- 用法一 -->
<router-link to="路由器配置中的path多级路由要求写完整"></router-link>
<!-- 用法二 -->
<router-link :to="{ path : '' }"></router-link>
```
### *命名路由*
> 有些时候path 可能太长 可以通过命名路由来缩短代码 简化路由的跳转

配置
```js
//在路由器 配置中
    {
        path:'/index',
        name:'index'
        children:[
            {
                path: 'pay'
                name: 'pay'
            }
        ]
    }
```
使用 
```html

    //传参的时候 吧path 替换为 name
    <router-link :to="{name : '路由名称'}"></router-link>
    
```
### *路由传参*
#### `query`参数
传参数
> 直接在to 中路径后面加上`?参数名=参数值&参数名=参数值`

```html
<router-link to="/路径?参数名=参数值&参数名=参数值"></router-link>
<router-link :to="`/路径?参数名=${参数值}&参数名=${参数值}`"></router-link>
<router-link :to="{
    path:'路径',
    query: {
        参数名:参数值,
        参数名:参数值
    }
}"></router-link>
```
接收参数
```html
{{$route.query.参数名}}
```
#### `params`参数
传参

**`注意：` params参数必须 使用 name作为配置项**
```js
{
    name:'index',
    params:{
        参数名:参数值,
        参数名:参数值
    }
}
```
接收参数
```html
{{$route.params.参数名}}
```
### `replace`
> 在`<router-link replace>` 加上这个属性 路由每次跳转的时候会覆盖当前路由 不会存在历史记录
### *编程式路由导航*
> 用于解决 不需要用户点击的路由跳转 
#### `push`
> 每操作一次就会留下一个 历史记录
```js
this.$router.push({
    {
    name:'index',
    params:{
        参数名:参数值,
        参数名:参数值
    }
}
})
this.$router.push({
    {
    path:'index',
    query:{
        参数名:参数值,
        参数名:参数值
    }
}
})
```
#### `replace`
> 每次操作只会覆盖上一个历史 只留下当前
```js
this.$router.replace({
    {
    name:'index',
    params:{
        参数名:参数值,
        参数名:参数值
    }
}
})
this.$router.replace({
    {
    path:'index',
    query:{
        参数名:参数值,
        参数名:参数值
    }
}
})
```
#### `back`
> 回退
```js
this.$router.back()
```
#### `forward`
> 前进
```js
this.$router.forward()
```
#### `go`
> 前进或者后退 取决于参数的值正负表示前进或后退 大小表示前进或后退多少
```js
this.$router.go(3)//前进3个
this.$router.go(-3)//后退3个
this.$router.go(1)//前进1个
```
### *路由组件缓存*
1. 使用 `<keep-alive></keep-alive>`包裹`<router-view></router-view>`
2. 在`keep-alive`标签上加上 `include`属性 决定缓存哪些组件，使用的是 **`组件名`** ，如果不加的话就缓存所有组件
### *路由守卫*
> 通过在 路由配置中设置的一系列回调函数
#### 前置路由守卫
> 在每次路由切换之前调用
1. 全局前置路由守卫
> 放到暴露前
```javascript
  router.baforeEach((to,from,next)=>{
    // to 去哪
    // from 从哪来
    // next 放行函数
    next()
  })
```
2. 独享路由守卫
> 直接写到配置里 并且 独享路由守卫只有前置没有后置
```js
{
  path:'/index',
  name:'index',
  component: Index,
  children:[],
  beforeEnter: ((to,from,next)=>{
    // to 去哪
    // from 从哪来
    // next 放行函数
    next()
  })
}
```
#### 后置路由守卫
> 在每次切换之后调用
1. 全局后置路由守卫
```javascript
  router.afterEach((to,from)=>{
    // to 去哪
    // from 从哪来
  })
```
#### 组件内路由守卫
> 直接写到与组件生命周期同级的配置上类似于生命周期钩子
1. `beforeRouterEnter(to,from,next){}`
2. `beforeRouterLeave(to,from,next){}`
### 路由模式切换
> 在路由配置中
```js
{
  mode : 'history' //或者 hash  一般情况 使用hash
}
```
## **生命周期**
### `$nextTick`
> 改变了页面数据 并且 vue把数据渲染到页面上了 之后执行的回调函数
### 路由组件生命周期
当组件在 `keep-alive`中 被缓存的时候 的生命周期钩子
#### `activated` 激活
> 当组件展示出来的时候
#### `deactivated` 失活
> 当组件被隐藏的时候
## 使用Less
> npm install -D less less-loader@7.3.0
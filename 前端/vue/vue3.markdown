- [**Vue 3**](#vue-3)
  - [**创建项目**](#创建项目)
    - [*使用vue-cli*](#使用vue-cli)
    - [*使用 vite*](#使用-vite)
  - [**起步**](#起步)
    - [**`setup()`**](#setup)
    - [**`ref()`**](#ref)
    - [**`reactive()`**](#reactive)
    - [**`computed()`**](#computed)
    - [**`watch()`**](#watch)
    - [**`watchEffect()`**](#watcheffect)
  - [**生命周期**](#生命周期)
  - [**hook**](#hook)
  - [**toRef**与**toRefs**](#toref与torefs)
  - [**shallowReactive**与**shallowRef**](#shallowreactive与shallowref)
  - [**readonly**与**shallowReadonly**](#readonly与shallowreadonly)
  - [**toRow**与**markRow**](#torow与markrow)
  - [**teleport**](#teleport)
  - [**vue3**中的其它改变](#vue3中的其它改变)

 
# **Vue 3**
## **创建项目**
### *使用vue-cli*
> vue create
### *使用 vite*
> npm init vite-app projectName
## **起步**
创建app 的方式发生改变
```js
import Vue from 'vue'
import HelloVueApp from '组件地址'
Vue.createApp(HelloVueApp).mount('#hello-vue')
//HelloVueApp 是组件

```

### **`setup()`**
> 组合式API 执行在create之前
```js
setup(porpos,context){

}
```
### **`ref()`**
> 基本数据的 数据劫持
```js
import { ref } from 'vue'
let data = ref('data')
//在组件中 读取的时候直接使用变量名
//在 代码中读取和修改的时候需要在后面加上.value 
console.log(data.value) 
```
如果 劫持对象类型的数据 `ref()` 会调用 `reactive()`
### **`reactive()`**
> 对象数据的 数据劫持 使用前 
```js
import { reactive } from 'vue'

let data = reactive({a:0})
//读取的时候直接读取就行了 不用加.value
console.log(data.a)
```
### **`computed()`**
> 计算属性API 使用前
```js
import { computed } from 'vue'
```
### **`watch()`**
> 监听
1. 监听`ref()`一个数据
   > 使用的时候不需要ref.value 传入的是refimpl对象
   ```js
    watch(需要监听的值,(new,old)=>{
     //行为
    })
   ```
2. 监听`ref()`多个数据
   ```js
    watch([值,值],(new,old)=>{
     //行为
    })
   ```   
3. 监听`对象`
   > 无法正确的获取oldvalue 监听对象的时候不需要开启深度监视 并且关不掉 deep配置无效
   ```js
    watch(对象,(new,old)=>{
     //行为
    })
   ```   
4. 监听对象中某一个值
   > `wacth()`函数的第一个参数要写成用函数返回的形式`()=>需要监听的值`  这种情况下 深度监视就能够起效
   ```js
    watch(()=>object.name,(new,old)=>{
     //行为
    },{deep:true})
   ```  
5. 监听对象中的多个值 
   ```js
    watch([()=>object.name,()=>object.name],(new,old)=>{
     //行为
    })
   ```
### **`watchEffect()`**
> 监听 这个函数里面使用过的数据
## **生命周期**
## **hook**
>  类似于 mixin
## **toRef**与**toRefs**
* **作用：** 创建一个refimpl对象其value的值是另一个对象中某个值
* **语法：** `const name = toRef(person,'name')`
* **应用：** 将响应式数据中的某个数据单独给外部使用
* **扩展：** `toRefs`和`toRef`作用类似 不过 前者可以批量创建
## **shallowReactive**与**shallowRef**
* *shallowReactive* ：只对对象最外层进行响应式
* *shallowRef* ：只对基本数据进行响应式
## **readonly**与**shallowReadonly**
* *readonly*：让响应式数据变为只读的(深只读)
* *shallowReadonly*:让响应式数据变为只读的(浅只读)
## **toRow**与**markRow**
* *toRow*：
  * 作用 ：把一个由`recative`生成的响应式对象变为普通对象
  * 场景 ：读取响应式对象对应的普通对象 数据改变页面不会发生更新
* *markRow*：
  * 作用 ：标记一个对象，这个对象永远不会变为响应式对象
## **teleport**
> 把 组件传送到某一个html 元素里
## **vue3**中的其它改变
* 移除过滤器
* 不会再有生产提示
* `Vue.mixin`,`Vue.use`,`Vue.component`,`Vue.directive`,`Vue.config.xxx`统统移植到`app.mixin`,`app.use`,`app.component`,`app.directive`,`app.config.xxx`
* vue2 中的 `Vue.prototype` 在vue3中使用`app.config.globalProperties`
* data选项应该始终被声明为一个函数
* 移除KeyCode作为v-on的修饰
* 移除v-on的native修饰
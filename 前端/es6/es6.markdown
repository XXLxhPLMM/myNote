- [es6](#es6)
  - [默认参数](#默认参数)
  - [剩余参数](#剩余参数)
  - [展开运算符](#展开运算符)
  - [暴露](#暴露)
    - [分别暴露](#分别暴露)
    - [统一暴露](#统一暴露)
    - [默认暴露](#默认暴露)
  - [引入](#引入)
    - [通用引入](#通用引入)
    - [解构赋值形式](#解构赋值形式)
    - [简便形式针对于默认暴露](#简便形式针对于默认暴露)
# es6
## 默认参数
首先来看下ES5是怎么处理默认参数的
```js
function sum(a, b){
    a = a || 0 //很巧妙的方法，虽然现在有了ES6了不用这样，但思想还是很重要的
    b = b || 0
    return a + b
}
```
而ES6就终于和各大编程语言一样拥有了默认参数

```js
function sum(a = 0, b = 0){
    return a + b
}
```
## 剩余参数
假如一个函数要传入的参数不知道有多少个怎么办呢？看看ES5是怎么解决的
```js
//不传参数，通过arguments对象来解决
//这个函数是用来自定义一个msg，然后输出和
function sum(){
    //由于arguments是伪数组，要用办法将arguments变成数组
    //1.var args = Array.prototype.slice.call(arguments)
    //2.var args = Array.from(arguments)
    //3.var args = [...arguments]
    var args = [...arguments]
    var numbers = args.slice(1)
    var result = numbers.reduce((p, v) => p+v, 0)
    return msg + result
}
sum("结果是", 1, 2, 3, 4, 5, 6)
```
一切都是因为arguments是一个伪数组，数组的API都没有，处理起来很不方便，所以ES6出了剩余参数来代替arguments
```js
function sum2(msg, ...nums){//值得注意的时候nums是一个数组
    let result = numbers.reduce((p, v) => p+v, 0)
    return msg + result
}
sum2("结果2是", 1,2,3,4,5,6)
sum2("结果2是", [1,2,3,4,5,6])//这样是不对的，因为在参数里就通过...把nums展开了，就应该是一堆数字
sum2("结果2是", ...[1,2,3,4,5,6])//这样也可以，...叫做展开运算符，下面会说，这里就是把[1,2,3,4,5,6]展开成了6个数字
```
## 展开运算符
顾名思义，就是把数组展开的，经常用于数组的初始化
```js
let [...a] = [1,2,3]//a = [1,2,3]
let [a,...b] = [1,2,3]//a = 1, b= [2,3]
```
值得注意的是，展开运算符也可以用于对象，一般用于对象的浅拷贝和合并
``` js
{//对象的浅拷贝
    let obj1 = {name: "Tom"}
    let {...obj2} = obj1
}
{//对象合并
    let obj1 = {a: 1,b:2}
    let obj2 = {a: 11,c:4}
    let obj3 = {...obj1, ...obj2}//{a:11,b:2,c:4}
}
```

## 暴露
### 分别暴露
```js
export let school = ‘杭电’
export function play()
{
console.log(“我会打游戏”)
}
```
### 统一暴露
```js
let school = ‘杭电’
function play(){
console.log(“我会打游戏”)
}
export {school,play}
```
### 默认暴露
```js
export default {
school:‘hangdian’,
change:function(){
console.log(‘change myself’)
}
}
```
## 引入
### 通用引入
```js
import * as m from ‘{{路径名}}’ 全部存入对象m中
```
### 解构赋值形式
```js
import {school,play} from 路径名
```
### 简便形式针对于默认暴露
```js
import school from 路径名
```

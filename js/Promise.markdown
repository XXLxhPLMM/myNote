- [Promise](#promise)
  - [异步编程](#异步编程)
  - [Promise 实例对象](#promise-实例对象)
    - [状态```[[PromiseStatus]]```](#状态promisestatus)
    - [值```[[PromiseValue]]```](#值promisevalue)
    - [then()](#then)
    - [catch()](#catch)
  - [Promise 函数对象](#promise-函数对象)
    - [resolve()](#resolve)
    - [reject()](#reject)
    - [all()](#all)
    - [race()](#race)
# Promise

## 异步编程
- fs文件操作
- ```js
- require('fs').readFile('./index.html',(err,data)=>{})
- ```
- 数据库操作
- ajax操作
- $.get('/ser')
- 定时器
## Promise 实例对象
```js
 var p = new Promise((resolve,reject)=>{
    /* 成功 */
    reslove(r);
    /* 失败 */
    reject(e);
/* 调用上面的方法后 实例对象的值将会被改变*/
})
```
### 状态```[[PromiseStatus]]```
- pending      未决定的
- rejected     失败
- resolved  /fulfilled   成功
> 状态只能由 
> <br/> pending-->rejected
> <br/> pending-->resolved / rulfilled
> <br/> `且状态只能改变一次`

### 值```[[PromiseValue]]```
> 存放异步任务`成功/失败`的值
> resolve()方法 和reject() 方法可以对其进行赋值
### then()
> 指定成功和失败时的回调
```js
p.then((r)=>{
/* 成功的回调 */
},
(e)=>{
/* 失败的回调 */
})
```
### catch()
> 只能指定失败时的回调
```js
p.catch((e)=>{
    失败
})
```
## Promise 函数对象

### resolve()
> 用于快速生成`Promise`对象 返回结果由传入值决定

- 传入非 `Promise`对象
  - ```[[PromiseStatus]]``` 的值为成功 `resolved`
  - ```[[PromiseValue]]``` 的值为传入值
- 传入`Promise`对象 返回结果由 传入的`Promise`对象决定
  -  ```[[PromiseStatus]]``` 的值为传入的`Promise`的状态
  - ```[[PromiseValue]]``` 的值为传入`Promise`的值

### reject()
> 用于返回一个失败的`Promise`对象
- ```[[PromiseStatus]]``` 的值为失败 `rejected`
- ```[[PromiseValue]]``` 的值为传入值

### all()
### race()
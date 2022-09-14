- [事件](#事件)
  - [注册事件](#注册事件)
  - [删除事件](#删除事件)
  - [Dom事件流](#dom事件流)
  - [事件对象](#事件对象)
    - [事件对象属性和方法](#事件对象属性和方法)
    - [事件委托](#事件委托)
  - [常用鼠标事件](#常用鼠标事件)
    - [获取鼠标坐标](#获取鼠标坐标)
  - [获得可视区域处于页面的那个位置](#获得可视区域处于页面的那个位置)
  - [键盘事件](#键盘事件)
    - [获取用户按下那个按键](#获取用户按下那个按键)
  - [文本选中事件](#文本选中事件)
    - [获取选中的文本](#获取选中的文本)
    - [自定义 右键菜单](#自定义-右键菜单)
  - [](#)
- [JavaScript 控制浏览器全屏](#javascript-控制浏览器全屏)
  - [开启全屏](#开启全屏)
  - [退出全屏](#退出全屏)
  - [全屏判断](#全屏判断)
  - [全屏模式下控制css](#全屏模式下控制css)
- [clientHeight、offsetHeight、scrollHeight的区别](#clientheightoffsetheightscrollheight的区别)
  - [clientHeight](#clientheight)
  - [offsetHeight](#offsetheight)
  - [scrollHeight](#scrollheight)
  - [offsetLeft](#offsetleft)
  - [注意点](#注意点)
- [鼠标位置](#鼠标位置)
- [Web API](#web-api)
  - [](#-1)
- [arguments 关键字](#arguments-关键字)

# 事件
## 注册事件
> node.事件类型=事件回调函数 同一事件具有唯一性重复使用会进行事件覆盖
```js
node.oncilck=()={

}
```
> 事件监听器 addEventListener  使用此方法事件会被串联
```js
node.addEventListener("事件类型",回调方法(){

})
```
## 删除事件
> 传统方式 node.事件类型=null
```js
node.onclick=null
```
> 事件监听器 node.removeEventListener(type,func) 使用非匿名函数的类型
```js
node.removeEventlistener("click","函数名")
```
## Dom事件流
> 事件传播的过程叫做`Dom事件流`
![a96c8b12bdbee94429af99cc01790aea.png](https://img.gejiba.com/images/a96c8b12bdbee94429af99cc01790aea.png)

> removeEventlistener("click","函数名") 这个函数如果没有第3个参数 或者第3个参数 为false 那就是冒泡阶段
> <br/> 如果 第3个参数为 true 就是捕获事件流
> <br/> 有些事件是不会 冒泡的 比如说 onblur onfocus onmouseenter onmouseleave
## 事件对象
> 可以往事件回调函数中添加参数 用来得到事件对象
```js
/* 这个时候的event 就是事件对象 */
div.onclick=(event)=>{

}
div.removeEventlistener("click",(event)=>{

})
/* 兼容性处理  */
e = e || window.event
```
### 事件对象属性和方法

| 事件对象属性方法   | 说明    |
| ----------------- | ------ |
|  `e.target` | 返回触发事件的对象   标准 |
|  `e.srcElement` | 返回触发事件的对象   非标准标准 ie678 |
|  `e.type`| 返回事件的类型 比如 click 不 |
|  `e.cancelBubble`| 该属性阻止事件冒泡 非标准 ie678 |
|  `e.returnValue`| 阻止默认事件 非标准 IE678 |
|  `e.prenentDefalut()`| 阻止默认事件 标准  |
|  `e.stopPropagation()`| 阻止事件冒泡 标准 |
### 事件委托
> 一般方法使用的是 循环添加事件
```html
<body id="body">
    <div class="div" width="100px" height="100px">
        <p>b</p>
    </div>
    <div class="div" width="100px" height="100px">
        <p>h</p>
    </div>
</body>
<script>
var o= document.querySelectorAll("div")
    for (let k in o) {
        o[k].onclick=function(){
        console.log(this)
    }
    }
</script>
```
> 使用事件委托 e.target 获取触发事件的对象
```html
<body id="body">
    <div class="div" width="100px" height="100px">
        <p>b</p>
    </div>
    <div class="div" width="100px" height="100px">
        <p>h</p>
    </div>
</body>
<script>
var o= document.querySelector("body")
    
    o.onclick=(e)=>{
        console.log(e);
        console.log(e.target);
    }
   
</script>
```
## 常用鼠标事件
| 鼠标事件 | 触发条件 |
|--|:--|
| `onclick` | 鼠标点击左键触发 |
| `onmouseover` | 鼠标经过触发 |
| `onmousout` | 鼠标离开触发 |
| `onfocus` | 获得鼠标焦点触发 |
| `onblur` | 失去鼠标焦点触发 |
| `onmousemove` | 鼠标移动触发 |
| `onmouseup` | 鼠标弹起触发 |
| `onmousedown` | 鼠标按下触发 |
### 获取鼠标坐标
| 鼠标事件对象 | 说明 |
|--|--|
| `e.clentX` | 返回相对于浏览器可视区的x坐标 |
| `e.clentY` | 返回相对于浏览器可视区的y坐标 |
| `e.pageX` | 返回相对于页面的x坐标 |
| `e.pageY` | 返回相对于页面的y坐标 |
| `e.creenX` | 返回相对于电脑屏幕的x坐标 |
| `e.creenY` | 返回相对于电脑屏幕的x坐标 |
## 获得可视区域处于页面的那个位置
| 事件对象 | 说明 |
|--|--|
| `window.pageXOffset` | 返回当前的X坐标 |
| `window.pageYOffset` | 返回当前的Y坐标 |
## 键盘事件
| 事件对象 | 说明 |
|--|--|
| `onkeydown` | 当某个键弹起时触发 |
| `onkeyup` | 当某个键按下时触发 |
| `onkeypress` | 当某个键按下时触发 但是不触发功能键比如Ctrl |
> 三个事件的触发顺序是 keydown -- keypress -- keyup
### 获取用户按下那个按键
event.KeyCode 返回一个该键的`ASCII`值
> keyup 和 keydown 不区分字母大小写<br/>
> keypress 区分大小写
## 文本选中事件
`onselectstart`  当文本被选中的时候触发
### 获取选中的文本
> 获取选中的对象 `window.getSelection()`
> 重点关注 该对象的
```js
/* 以下数字位置 为偏移量 */
anchorNode: text
anchorOffset: 12
baseNode: text
baseOffset: 12  /* 开始的位置 字符串的下标 */
extentNode: text
extentOffset: 14 /* 最后一个字符是第几个字符 */
focusNode: text
focusOffset: 14
isCollapsed: false
rangeCount: 1
type: "Range"
```
> window.getSelection().getRangeAt(0)
```js
collapsed: false
commonAncestorContainer: p /* 总标签 */
endContainer: text   /* 结束的文本节点 */
endOffset: 2          /* 结束文本的字符串 字符位置 */
startContainer: text  /* 开始的文本节点 */
startOffset: 12     /* 开始文本节点的字符串下标 */
```
> 新建选区
```js
/* 创建空选区 */
const newRange = document.createRange()
选中选区
window.getSelection().addRange(newRange)
```
### 自定义 右键菜单
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<style>
    * {
      margin: 0;
      padding: 0;
    }
    .menu {
      width: 100px;
      border: 1px solid #ccc;
      position: absolute;
      box-shadow: 2px 2px 8px 1px rgba(0, 0, 0, .2);
      display: none;
    }
    .menu li {
      list-style: none;
      width: 100%;
    }
    .menu li span {
      display: inline-block;
      text-decoration: none;
      color: #555;
      width: 100%;
      padding: 10px 0;
      text-align: center;
      cursor: pointer;
    }
    .menu li:first-of-type {
      border-radius: 5px 5px 0 0;
    }
    .menu li span:hover{
      background: #eee;
      color: #CC1A1A;
    }
</style>
<body>
    <ul class="menu" id="menu">
        <li><span>复制</span></li>
        <li><span>粘贴</span></li>
        <li><span>刷新</span></li>
        <li><span>其他功能1</span></li>
        <li><span>其他功能2</span></li>
      </ul>
    <script>
         window.onload = function () {
      // 获取节点
      var menu = document.getElementById('menu');

      //获取可视区宽度,高度
      var winWidth = document.documentElement.clientWidth || document.body.clientWidth;
      var winHeight = document.documentElement.clientHeight || document.body.clientHeight;

      // 点击空白区域 隐藏菜单
      document.addEventListener('click', function () {
        menu.style.display = 'none';
        menu.style.left = 0 + 'px';
        menu.style.top = 0 + 'px';
      })

      // 点击菜单
      menu.addEventListener('click', function (e) {
        var e = e || window.event;
        console.log(e.target.innerText)
      })

      //右键菜单
      document.oncontextmenu = function (e) {
        var e = e || window.event;
        menu.style.display = 'block';
        // 获取鼠标坐标
        var mouseX = e.clientX;
        var mouseY = e.clientY;

        // 判断边界值，防止菜单栏溢出可视窗口
        if (mouseX >= (winWidth - menu.offsetWidth)) {
          mouseX = winWidth - menu.offsetWidth;
        } else {
          mouseX = mouseX
        }
        if (mouseY > winHeight - menu.offsetHeight) {
          mouseY = winHeight - menu.offsetHeight;
        } else {
          mouseY = mouseY;
        }
        menu.style.left = mouseX + 'px';
        menu.style.top = mouseY + 'px';
        return false;
      }
    }
    </script>
</body>
</html>
```
## 
# JavaScript 控制浏览器全屏
## 开启全屏
HTML 5中的`fullscreen`，目前可以在除IE和opera外的浏览器中使用 ，有的时候用来做全屏API，游戏呀，等都很有用。先看常见的API
> element.requestFullScreen()   作用：请求某个元素element全屏
## 退出全屏
> document.exitFullScreen()
## 全屏判断
> document.fullscreen  ,如果用户在全屏模式下，则返回true
> <br/>document.fullscreenElement,返回当前处于全屏模式下的元素

**`注意`**:
1. 在HTML5中,W3C制定了关于全屏的API但是只能由用户事件触发，所以不能自动全屏；用户事件触发方可。
2. 新版本，浏览器都支持了全屏api，不需要带前缀，为兼容性可以带着。

## 全屏模式下控制css
```css
    :fullscreen input{
            background: hotpink;
        }
```
**兼容性处理**<br/>
```css
-webkit-full-screen {
 
 /* properties */
}
:-moz-full-screen {
 /* properties */
}
:-ms-fullscreen {
 /* properties */
}
:full-screen { /*pre-spec */
 /* properties */
}
:fullscreen { /* spec */
 /* properties */
}
/* deeper elements */
:-webkit-full-screen video {
 width: %;
 height: %;
}
/* styling the backdrop*/
::backdrop {
 /* properties */
}
::-ms-backdrop {
 /* properties */
}
```
# clientHeight、offsetHeight、scrollHeight的区别 
![5a4279db126b0bd3de896a3328cd78cc.jpg](https://img.gejiba.com/images/5a4279db126b0bd3de896a3328cd78cc.jpg)

## clientHeight
> `含义` : 元素的像素高度，包含元素的高度+内边距，不包含水平滚动条，边框和外边距
![ec5d90fd555b9c513dce3717ddb65e96.jpg](https://img.gejiba.com/images/ec5d90fd555b9c513dce3717ddb65e96.jpg)

## offsetHeight
> `含义` : 元素的像素高度 包含元素的垂直内边距和边框，水平滚动条的高度，且是一个整数<br/>
![18b9a58d5f510e885ac36905a48d23a8.jpg](https://img.gejiba.com/images/18b9a58d5f510e885ac36905a48d23a8.jpg)

## scrollHeight
> `含义` : 元素内容的高度，包括溢出的不可见内容
![45824fc69735b66739cb14ed05a1c0d1.jpg](https://img.gejiba.com/images/45824fc69735b66739cb14ed05a1c0d1.jpg)

## offsetLeft
> `含义` : 返回元素左上角相对于offsetParent的左边界的偏移像素值
## 注意点
> 1. 对块级元素来说，offsetTop、offsetLeft、offsetWidth 及 offsetHeight 描述了元素相对于 offsetParent 的边界框。但是文本框不是如此.文本框的offsetLeft和offsetTop是第一个文本框的左偏移和上偏移.

> 2. offsetParent元素指改元素的定位元素以及最近的table,td,th,body. 可见，offsetParent和position属性的包含块概念类似.大部分场景下可以通用.

> 3. offsetTop和offsetLeft都是相对于其内边距边界的.
# 鼠标位置

一.`PageX`和`clientX`
`PageX`:鼠标在页面上的位置,从页面左上角开始,即是以页面为参考点,不随滑动条移动而变化
`clientX`:鼠标在页面上可视区域的位置,从浏览器可视区域左上角开始,即是以浏览器滑动条此刻的滑动到的位置为参考点,随滑动条移而变化.

二.`screenX`
`screenX`:鼠标在屏幕上的位置,从屏幕左上角开始,这个没有任何争议

三.`offsetX`和`layerX`
`offsetX`:IE特有,鼠标相比较于触发事件的元素的位置,以元素盒子模型的内容区域的左上角为参考点,如果有boder,可能出现负值。

`layerX`:鼠标相比较于当前坐标系的位置,即如果触发元素没有设置绝对定位或相对定位,以页面为参考点,如果有,将改变参考坐标系,从触发元素盒子模型的border区域的左上角为参考点
也就是当触发元素设置了相对或者绝对定位后。
# Web API
## 

# arguments 关键字
用于获取形参列表

在方法中直接使用**arguments[参数]** 可以直接操作形参
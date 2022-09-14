- [CSS3速查](#css3速查)
- [鼠标样式](#鼠标样式)
- [按钮动画](#按钮动画)
# CSS3速查
[css3参考手册跳转](https://www.runoob.com/cssref/css-reference.html)
[百度](https://www.runoob.com/cssref/css-reference.html)
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.min.js"></script>

<style>
    #quickManual{
      color: brown;
    }
    .closeNow{
      display: none;
    }
    .openNow{
      display: block;
      margin: auto;
      width: 80vw;
      height: 50vh;
    }
  </style>
  <a id="quickManual" href="https://www.runoob.com/cssref/css-reference.html" target="iframe1">就此打开</a>
  <iframe name="iframe1" class="closeNow" src="" frameborder="0"></iframe>
<script>
$("#quickManual").click((a)=>{
    if (a.target.innerText=="就此打开") {
      a.target.innerText="关闭"
      $("iframe[name=iframe1]").addClass("openNow")
      $("iframe[name=iframe1]").removeClass("closeNow")
    }else{
      a.target.innerText="就此打开"
      $("iframe[name=iframe1]").addClass("closeNow")
      $("iframe[name=iframe1]").removeClass("openNow")
    }
  })
</script>

# 鼠标样式
```css
 cursor: none;
```
# 按钮动画
<style>
.button {
  display: inline-block;
  border-radius: 4px;
  background-color: #f4511e;
  border: none;
  color: #FFFFFF;
  text-align: center;
  font-size: 28px;
  padding: 20px;
  width: 200px;
  transition: all 0.5s;
  cursor: pointer;
  margin: 5px;
}

.button span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: 0.3s;
}

.button span:after {
  content: '»';
  position: absolute;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.5s;
}

.button:hover span {
  padding-right: 25px;
}

.button:hover span:after,.button:hover {
  opacity: 1;
  right: 0;
}
</style>

<h2>按钮动画效果</h2>
<button class="button" style="vertical-align:middle"><span>Hover </span></button>

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<style>
.button {
  display: inline-block;
  border-radius: 4px;
  background-color: #f4511e;
  border: none;
  color: #FFFFFF;
  text-align: center;
  font-size: 28px;
  padding: 20px;
  width: 200px;
  transition: all 0.5s;
  cursor: pointer;
  margin: 5px;
}

.button span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: 0.3s;
}

.button span:after {
  content: '»';
  position: absolute;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.1s;
}

.button:hover span {
  padding-right: 25px;
}

.button:hover span:after {
  opacity: 1;
  right: 0;
}
</style>
</head>
<body>

<h2>按钮动画</h2>

<button class="button" style="vertical-align:middle"><span>Hover </span></button>

</body>
</html>
```

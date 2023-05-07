# CSS 记录手册
## 滚动条样式
```css
.scroll::-webkit-scrollbar {
  /*滚动条整体部分，其中的属性有width,height,background,border等（就和一个块级元素一样）（位置1）*/
  width: 7px;
  /* height: 10px; */
}
.scroll::-webkit-scrollbar-button {
  /*滚动条两端的按钮，可以用display:none让其不显示，也可以添加背景图片，颜色改变显示效果（位置2）*/
  display: none;
  background: #74d334;
}
.scroll::-webkit-scrollbar-track {
  display: none;
  /*外层轨道，可以用display:none让其不显示，也可以添加背景图片，颜色改变显示效果（位置3）*/
  background: #ff66d5;
}
.scroll::-webkit-scrollbar-track-piece {
  /*内层轨道，滚动条中间部分（位置4）*/
  display: none;
  background: #ff66d5;
}
.scroll::-webkit-scrollbar-thumb {
  /*滚动条里面可以拖动的那部分（位置5）*/
  background: #646464;
  border-radius: 4px;
}
.scroll::-webkit-scrollbar-corner {
  /*边角（位置6）*/
  background: #82afff;
}
.scroll::-webkit-scrollbar-resizer {
  /*定义右下角拖动块的样式（位置7）*/
  background: #ff0bee;
}
.scroll {
  scrollbar-arrow-color: #f4ae21; /**/ /*三角箭头的颜色*/
  scrollbar-face-color: #333; /**/ /*立体滚动条的颜色*/
  scrollbar-3dlight-color: #666; /**/ /*立体滚动条亮边的颜色*/
  scrollbar-highlight-color: #666; /**/ /*滚动条空白部分的颜色*/
  scrollbar-shadow-color: #999; /**/ /*立体滚动条阴影的颜色*/
  scrollbar-darkshadow-color: #666; /**/ /*立体滚动条强阴影的颜色*/
  scrollbar-track-color: #666; /**/ /*立体滚动条背景颜色*/
  scrollbar-base-color: #f8f8f8; /**/ /*滚动条的基本颜色*/
  scrollbar-width: 0px;
}
```
## 单词换行
word-break

## 规定行数以及行超出部分

```css
  display: -webkit-box;//
  overflow: hidden;//
  margin: 10px 0;
  height: 35px;
  text-overflow: ellipsis;//
  word-break: break-all;//
  -webkit-box-orient: vertical;//
  -webkit-line-clamp: 2;//
```
## 元素不换行
white-space: nowrap; 

# :active

:active CSS伪类匹配被用户激活的元素。它让页面能在浏览器监测到激活时给出反馈。当用鼠标交互时，它代表的是用户按下按键和松开按键之间的时间。 :active 伪类通常用来匹配tab键交互。通常用于但并不限于 <a> 和 <button> HTML元素。

这个样式可能会被后声明的其他链接相关的伪类覆盖，这些伪类包括 :link，:hover和 :visited。为了正常加上样式，需要把 :active的样式放在所有链接相关的样式后，这种链接伪类先后顺序被称为LVHA顺序: :link — :visited — :hover — :active。


```
body{
  background-color:#ffffc9;
}
a:link{
  color: blue; /*未访问链接*/
}
a:visited {
  color: purle;/* 已访问链接*/
}
a:hover{
  font-weight: bold /*用户鼠标悬停*/
}
a:active{
  color:lime; /*激活链接*/
}
```
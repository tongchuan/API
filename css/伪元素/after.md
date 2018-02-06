# :after

## 概述

CSS伪元素::after用来创建一个伪元素，做为已选中元素的最后一个子元素。通常会配合content属性来为该元素添加装饰内容。这个虚拟元素默认是行内元素。

Firefox 3.5 note

Firefox 3.5之前版本仅实现了CSS 2.0版本的语法 :after. 且不允许在 position, float, list-style-* 等属性中使用。Firefox 3.5开始没有了这项限制。


## 语法

```
element:after  { style properties }  /* CSS2 语法 */

element::after { style properties }  /* CSS3 语法 */

```

::after表示法是在CSS 3中引入的，::符号是用来区分伪类和伪元素的。支持CSS3的浏览器同时也都支持CSS2中引入的表示法:after

## 例子

让我们创建两个类：一个无趣的和一个有趣的。我们可以在每个段尾添加伪元素来标记他们。

```
<p class="boring-text">这是些无聊的文字</p>
<p>这是不无聊也不有趣的文字</p>
<p class="exciting-text">在MDN上做贡献简单又轻松。
按右上角的编辑按钮添加新示例或改进旧示例！</p>

```

```
.exciting-text::after {
  content: "<- 让人兴兴兴奋!"; 
  color: green;
}

.boring-text::after {
   content:    "<- 无聊!";
   color:      red;
}
```


我们几乎可以用想要的任何方法给 content 属性里的文字和图片的加上样式.

```
<span class="ribbon">Notice where the orange box is.</span>

```

```
.ribbon {
  background-color: #5BC8F7;
}

.ribbon::after {
  content: "Look at this orange box.";
  background-color: #FFBA10;
  border-color: black;
  border-style: dotted;
}

```
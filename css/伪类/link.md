# :link

:link伪类选择器是用来选中元素当中的链接。它将会选中所有尚未访问的链接，包括那些已经给定了其他伪类选择器的链接（例如:hover选择器，:active选择器，:visited选择器）。为了可以正确地渲染链接元素的样式，:link伪类选择器应当放在其他伪类选择器的前面，并且遵循LVHA的先后顺序，即：:link — :visited — :hover — :active。:focus伪类选择器常伴随在:hover伪类选择器左右，需要根据你想要实现的效果确定它们的顺序。

```
a:link {color: slategray;}
.external:link {background-color: lightblue;}
```
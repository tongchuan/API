# :hover

:hover CSS伪类适用于用户使用指示设备虚指一个元素（没有激活它）的情况。这个样式会被任何与链接相关的伪类重写，像:link, :visited, 和 :active等。为了确保生效，:hover规则需要放在:link和:visited规则之后，但是在:active规则之前，按照LVHA的循顺序声明:link－:visited－:hover－:active。


:hover伪类可以任何伪元素上使用。

用户的可视客户端比如Firefox, Internet Explorer, Safari, Opera or Chrome, 会在光标（鼠标指针）悬停在元素上时提供关联的样式。


注意: 在触摸屏上 :hover 有问题，基本不可用。不同的浏览器上:hover 伪类表现不同。 可能从不会触发；或者在触摸某元素后触发了一小会儿；或者总是触发即使用户不在触摸了，直到用户触摸别的元素。 触摸屏非常普遍，所以网页开发人员不要让任何内容只能通过悬停才能展示出来，不然这些内容对于触摸屏使用者来说是很难或者说不可能看到。


```
:link:hover { outline: dotted red; }
.foo:hover { background: gold; }
```

使用:hover 伪类可以创建复杂的层叠机制。一个常见用途，比如，创建一个纯CSS的下拉按钮（不使用JavaScript）。 本质是创建如下的CSS：

```
div.menu-bar ul ul {
  display: none;
}

div.menu-bar li:hover > ul {
  display: block;
}
```

HTML内容如下:
```
<div class="menu-bar">
  <ul>
    <li>
      <a href="example.html">Menu</a>
      <ul>
        <li>
          <a href="example.html">Link</a>
        </li>
        <li>
          <a class="menu-nav" href="example.html">Submenu</a>
          <ul>
            <li>
              <a class="menu-nav" href="example.html">Submenu</a>
              <ul>
                <li><a href="example.html">Link</a></li>
                <li><a href="example.html">Link</a></li>
                <li><a href="example.html">Link</a></li>
                <li><a href="example.html">Link</a></li>
              </ul>
            </li>
            <li><a href="example.html">Link</a></li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</div>
```
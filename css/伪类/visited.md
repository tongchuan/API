# :visited

## 概要 （summary）

The :visited CSS pseudo-class lets you select only links that have been visited. This style may be overridden by any other link-related pseudo-classes, that is :link, hover and :active, appearing in subsequent rules. In order to style appropriately links, you need to put the :visited rule after the :link rule but before the other ones, defined in the LVHA-order: :link — :visited — :hover — :active. 


Note: For privacy reasons, browsers strictly limit the styles you can apply using an element selected by this pseudo-class: only color, background-color, border-color, border-bottom-color, border-left-color, border-right-color, border-top-color, outline-color, column-rule-color, fill and stroke. Note also that the alpha component will be ignored: the alpha component of the not-visited rule is used instead (except when the opacity is 0, in that case the whole color is ignored, and the one of the not-visited rule is used.

Though the color can be changed, the method getComputedStyle will lie and always give back the value of the non-visited color.

For more information on the limitations and the motivation for them, see Privacy and the :visited selector.


```
a:visited { color: #4b2f89; }
a:visited { background-color: white }

```
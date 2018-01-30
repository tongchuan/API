# Set和Map数据结构
## Set
### 基本用法
ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
Set本身是一个构造函数，用来生成Set数据结构。
~~~
var s = new Set();
[2,3,5,4,5,2,2].map(x => s.add(x))
for (i of s) {console.log(i)}
// 2 3 5 4
~~~
上面代码通过add方法向Set结构加入成员，结果表明Set结构不会添加重复的值。
Set函数可以接受一个数组作为参数，用来初始化。
~~~
var items = new Set([1,2,3,4,5,5,5,5]);
items.size // 5
~~~
向Set加入值的时候，不会发生类型转换，所以5和“5”是两个不同的值。Set内部判断两个值是否不同，使用的算法类似于精确相等运算符（===），这意味着，两个对象总是不相等的。唯一的例外是NaN等于自身（精确相等运算符认为NaN不等于自身）。
~~~
let set = new Set();
set.add({})
set.size // 1
set.add({})
set.size // 2
~~~
上面代码表示，由于两个空对象不是精确相等，所以它们被视为两个值。

### Set实例的属性和方法
Set结构的实例有以下属性。
 * Set.prototype.constructor：构造函数，默认就是Set函数。
 * Set.prototype.size：返回Set实例的成员总数。
Set实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。
 * add(value)：添加某个值，返回Set结构本身。
 * delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
 * has(value)：返回一个布尔值，表示该值是否为Set的成员。
 * clear()：清除所有成员，没有返回值。
上面这些属性和方法的实例如下
~~~
s.add(1).add(2).add(2);
// 注意2被加入了两次
s.size // 2
s.has(1) // true
s.has(2) // true
s.has(3) // false
s.delete(2);
s.has(2) // false
~~~
下面是一个对比，看看在判断是否包括一个键上面，Object结构和Set结构的写法不同。
~~~
// 对象的写法
var properties = {
  "width": 1,
  "height": 1
};
if (properties[someName]) {
  // do something
}
// Set的写法
var properties = new Set();
properties.add("width");
properties.add("height");
if (properties.has(someName)) {
  // do something
}
~~~
Array.from方法可以将Set结构转为数组。
~~~
var items = new Set([1, 2, 3, 4, 5]);
var array = Array.from(items);
~~~
这就提供了一种去除数组的重复元素的方法。
~~~
function dedupe(array) {
  return Array.from(new Set(array));
}
dedupe([1,1,2,3]) // [1, 2, 3]
~~~

### 遍历操作
Set结构的实例有四个遍历方法，可以用于遍历成员。
 * keys()：返回一个键名的遍历器
 * values()：返回一个键值的遍历器
 * entries()：返回一个键值对的遍历器
 * forEach()：使用回调函数遍历每个成员
key方法、value方法、entries方法返回的都是遍历器（详见《Iterator对象》一章）。由于Set结构没有键名，只有键值（或者说键名和键值是同一个值），所以key方法和value方法的行为完全一致。
~~~
let set = new Set(['red', 'green', 'blue']);
for ( let item of set.keys() ){
  console.log(item);
}
// red
// green
// blue
for ( let item of set.values() ){
  console.log(item);
}
// red
// green
// blue
for ( let item of set.entries() ){
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
~~~
上面代码中，entries方法返回的遍历器，同时包括键名和键值，所以每次输出一个数组，它的两个成员完全相等。
Set结构的实例默认可遍历，它的默认遍历器就是它的values方法。
~~~
Set.prototype[Symbol.iterator] === Set.prototype.values
// true
~~~
这意味着，可以省略values方法，直接用for...of循环遍历Set。
~~~
let set = new Set(['red', 'green', 'blue']);
for (let x of set) {
  console.log(x);
}
// red
// green
// blue
~~~
由于扩展运算符（...）内部使用for...of循环，所以也可以用于Set结构。
~~~
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
~~~
这就提供了另一种便捷的去除数组重复元素的方法。
~~~
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]
~~~
而且，数组的map和filter方法也可以用于Set了。
~~~
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}
let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
~~~
因此使用Set，可以很容易地实现并集（Union）和交集（Intersect）。
~~~
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);
let union = new Set([...a, ...b]);
// [1, 2, 3, 4]
let intersect = new Set([...a].filter(x => b.has(x)));
// [2, 3]
~~~
Set结构的实例的forEach方法，用于对每个成员执行某种操作，没有返回值。
~~~
let set = new Set([1, 2, 3]);
set.forEach((value, key) => console.log(value * 2) )
// 2
// 4
// 6
~~~
上面代码说明，forEach方法的参数就是一个处理函数。该函数的参数依次为键值、键名、集合本身（上例省略了该参数）。另外，forEach方法还可以有第二个参数，表示绑定的this对象。

如果想在遍历操作中，同步改变原来的Set结构，目前没有直接的方法，但有两种变通方法。一种是利用原Set结构映射出一个新的结构，然后赋值给原来的Set结构；另一种是利用Array.from方法。
~~~
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set的值是2, 4, 6
// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, val => val * 2));
// set的值是2, 4, 6
~~~
上面代码提供了两种方法，直接在遍历操作中改变原来的Set结构。

### WeakSet
WeakSet结构与Set类似，也是不重复的值的集合。但是，它与Set有两个区别。
首先，WeakSet的成员只能是对象，而不能是其他类型的值。
其次，WeakSet中的对象都是弱引用，即垃圾回收机制不考虑WeakSet对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于WeakSet之中。这个特点意味着，无法引用WeakSet的成员，因此WeakSet是不可遍历的。
~~~
var ws = new WeakSet();
ws.add(1)
// TypeError: Invalid value used in weak set
~~~
上面代码试图向WeakSet添加一个数值，结果报错。
WeakSet是一个构造函数，可以使用new命令，创建WeakSet数据结构。
~~~
var ws = new WeakSet();
~~~
作为构造函数，WeakSet可以接受一个数组或类似数组的对象作为参数。（实际上，任何具有iterable接口的对象，都可以作为WeakSet的对象。）该数组的所有成员，都会自动成为WeakSet实例对象的成员。
~~~
var a = [[1,2], [3,4]];
var ws = new WeakSet(a);
~~~
上面代码中，a是一个数组，它有两个成员，也都是数组。将a作为WeakSet构造函数的参数，a的成员会自动成为WeakSet的成员。

WeakSet结构有以下三个方法。
 * WeakSet.prototype.add(value)：向WeakSet实例添加一个新成员。
 * WeakSet.prototype.delete(value)：清除WeakSet实例的指定成员。
 * WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在WeakSet实例之中。

下面是一个例子。
~~~
WeakSet结构有以下三个方法。
WeakSet.prototype.add(value)
WeakSet.prototype.add(value)
：向WeakSet实例添加一个新成员。
WeakSet.prototype.delete(value)
WeakSet.prototype.delete(value)
：清除WeakSet实例的指定成员。
WeakSet.prototype.has(value)
WeakSet.prototype.has(value)
：返回一个布尔值，表示某个值是否在WeakSet实例之中。
下面是一个例子。
var ws = new WeakSet();
var obj = {};
var foo = {};
ws.add(window);
ws.add(obj);
ws.has(window); // true
ws.has(foo);    // false
ws.delete(window);
ws.has(window);    // false
~~~
WeakSet没有size属性，没有办法遍历它的成员。
~~~
ws.size // undefined
ws.forEach // undefined
ws.forEach(function(item){ console.log('WeakSet has ' + item)})
// TypeError: undefined is not a function
~~~

上面代码试图获取size和forEach属性，结果都不能成功.
WeakSet不能遍历，是因为成员都是弱引用，随时可能消失，遍历机制无法保存成员的存在，很可能刚刚遍历结束，成员就取不到了。WeakSet的一个用处，是储存DOM节点，而不用担心这些节点从文档移除时，会引发内存泄漏。

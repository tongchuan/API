# Reflect
## 概述
Reflect对象与Proxy对象一样，也是ES6为了操作对象而提供的新API。Reflect对象的设计目的有这样几个。
 1. 将Object对象的一些明显属于语言层面的方法，放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。
 2. 修改某些Object方法的返回结果，让其变得更合理。比如`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回false。
 3. 让Object操作都变成函数行为。某些Object操作是命令式，比如`name in obj`和`delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为.
 4. Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。
~~~
Proxy(target, {
  set: function(target, name, value, receiver) {
    var success = Reflect.set(target,name, value, receiver);
    if (success) {
      log('property '+name+' on '+target+' set to '+value);
    }
    return success;
  }
});
~~~
上面代码中，Proxy方法拦截target对象的属性赋值行为。它采用Reflect.set方法将值赋值给对象的属性，然后再部署额外的功能。

下面是get方法的例子。
~~~
var loggedObj = new Proxy(obj, {
  get: function(target, name) {
    console.log("get", target, name);
    return Reflect.get(target, name);
  }
});
~~~

## 方法
Reflect对象的方法清单如下。
 1. Reflect.getOwnPropertyDescriptor(target,name)
 2. Reflect.defineProperty(target,name,desc)
 3. Reflect.getOwnPropertyNames(target)
 4. Reflect.getPrototypeOf(target)
 5. Reflect.deleteProperty(target,name)
 6. Reflect.enumerate(target)
 7. Reflect.freeze(target)
 8. Reflect.seal(target)
 9. Reflect.preventExtensions(target)
 10. Reflect.isFrozen(target)
 11. Reflect.isSealed(target)
 12. Reflect.isExtensible(target)
 13. Reflect.has(target,name)
 14. Reflect.hasOwn(target,name)
 15. Reflect.keys(target)
 16. Reflect.get(target,name,receiver)
 17. Reflect.set(target,name,value,receiver)
 18. Reflect.apply(target,thisArg,args)
 19. Reflect.construct(target,args)
上面这些方法的作用，大部分与Object对象的同名方法的作用都是相同的。下面是对其中几个方法的解释。

### （1）Reflect.get(target,name,receiver)
查找并返回target对象的name属性，如果没有该属性，则返回undefined。
如果name属性部署了读取函数，则读取函数的this绑定receiver。
~~~
var obj = {
  get foo() { return this.bar(); },
  bar: function() { ... }
}
// 下面语句会让 this.bar()
// 变成调用 wrapper.bar()
Reflect.get(obj, "foo", wrapper);
~~~
### （2）Reflect.set(target, name, value, receiver)
设置target对象的name属性等于value。如果name属性设置了赋值函数，则赋值函数的this绑定receiver。
### （3）Reflect.has(obj, name)
等同于name in obj
### （4）Reflect.deleteProperty(obj, name)
等同于delete obj[name]。
### （5）Reflect.construct(target, args)
等同于new target(...args)，这提供了一种不使用new，来调用构造函数的方法。
### （6）Reflect.getPrototypeOf(obj)
读取对象的proto属性，等同于Object.getPrototypeOf(obj)
### （7）Reflect.setPrototypeOf(obj, newProto)
设置对象的proto属性。注意，Object对象没有对应这个方法的方法。
### （8）Reflect.apply(fun,thisArg,args)
等同于`Function.prototype.apply.call(fun,thisArg,args)`。一般来说，如果要绑定一个函数的this对象，可以这样写`fn.apply(obj, args)`
，但是如果函数定义了自己的apply方法，就只能写成`Function.prototype.apply.call(fn, obj, args)`，采用Reflect对象可以简化这种操作。
另外，需要注意的是，Reflect.set()、Reflect.defineProperty()、Reflect.freeze()、Reflect.seal()和Reflect.preventExtensions()返回一个布尔值，表示操作是否成功。它们对应的Object方法，失败时都会抛出错误。
~~~
// 失败时抛出错误
Object.defineProperty(obj, name, desc);
// 失败时返回false
Reflect.defineProperty(obj, name, desc)
~~~
上面代码中，Reflect.defineProperty方法的作用与Object.defineProperty是一样的，都是为对象定义一个属性。但是，Reflect.defineProperty方法失败时，不会抛出错误，只会返回false。

### Object.observe()，Object.unobserve()
Object.observe方法用来监听对象（以及数组）的变化。一旦监听对象发生变化，就会触发回调函数。
~~~
var user = {};
Object.observe(user, function(changes){
  changes.forEach(function(change) {
    user.fullName = user.firstName+" "+user.lastName;
  });
});
user.firstName = 'Michael';
user.lastName = 'Jackson';
user.fullName // 'Michael Jackson'
~~~
上面代码中，Object.observer方法监听user对象。一旦该对象发生变化，就自动生成fullName属性。
一般情况下，Object.observe方法接受两个参数，第一个参数是监听的对象，第二个函数是一个回调函数。一旦监听对象发生变化（比如新增或删除一个属性），就会触发这个回调函数。很明显，利用这个方法可以做很多事情，比如自动更新DOM。
~~~
var div = $("#foo");
Object.observe(user, function(changes){
  changes.forEach(function(change) {
    var fullName = user.firstName+" "+user.lastName;
    div.text(fullName);
  });
});
~~~
上面代码中，只要user对象发生变化，就会自动更新DOM。如果配合jQuery的change方法，就可以实现数据对象与DOM对象的双向自动绑定。
回调函数的changes参数是一个数组，代表对象发生的变化。下面是一个更完整的例子。
~~~
var o = {};
function observer(changes){
  changes.forEach(function(change) {
    console.log('发生变动的属性：' + change.name);
    console.log('变动前的值：' + change.oldValue);
    console.log('变动后的值：' + change.object[change.name]);
    console.log('变动类型：' + change.type);
  });
}
Object.observe(o, observer)
~~~
参照上面代码，Object.observe方法指定的回调函数，接受一个数组（changes）作为参数。该数组的成员与对象的变化一一对应，也就是说，对象发生多少个变化，该数组就有多少个成员。每个成员是一个对象（change），它的name属性表示发生变化源对象的属性名，oldValue属性表示发生变化前的值，object属性指向变动后的源对象，type属性表示变化的种类。基本上，change对象是下面的样子。
~~~
var change = {
  object: {...},
  type: 'update',
  name: 'p2',
  oldValue: 'Property 2'
}
~~~
Object.observe方法目前共支持监听六种变化。
 1. add：添加属性
 2. update：属性值的变化
 3. delete：删除属性
 4. setPrototype：设置原型
 5. reconfigure：属性的attributes对象发生变化
 6. preventExtensions：对象被禁止扩展（当一个对象变得不可扩展时，也就不必再监听了）
Object.observe方法还可以接受第三个参数，用来指定监听的事件种类。
~~~
Object.observe(o, observer, ['delete']);
~~~
上面的代码表示，只在发生delete事件时，才会调用回调函数。
Object.unobserve方法用来取消监听。
~~~
Object.unobserve(o, observer);
~~~
注意，Object.observe和Object.unobserve这两个方法不属于ES6，而是属于ES7的一部分。不过，Chrome浏览器从33版起就已经支持。

# Proxy
## 概述
Proxy用于修改某些操作的默认行为，等同于在语言层面作出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。
Proxy可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
~~~
var obj = new Proxy({}, {
  get: function (target, key, receiver) {
    console.log(`getting ${key}!`);
    return Reflect.get(target, key, receiver);
  },
  set: function (target, key, value, receiver) {
    console.log(`setting ${key}!`);
    return Reflect.set(target, key, value, receiver);
  }
});
~~~
上面代码对一个空对象架设了一层拦截，重定义了属性的读取（get）和设置（set）行为。这里暂时不解释具体的语法，只看运行结果。对设置了拦截行为的对象obj，去读写它的属性，就会得到下面的结果。
~~~
obj.count = 1
//  setting count!
++obj.count
//  getting count!
//  setting count!
//  2
~~~
上面代码说明，Proxy实际上重载（overload）了点运算符，即用自己的定义涵盖了语言的原始定义。
ES6原生提供Proxy构造函数，用来生成Proxy实例。
~~~
var proxy = new Proxy(target, handler)
~~~
Proxy对象的所用用法，都是上面这种形式，不同的只是handler参数的写法。其中，`new Proxy()`表示生成一个Proxy实例，target参数表示所要拦截的目标对象，handler参数也是一个对象，用来定制拦截行为。
下面是另一个拦截读取属性行为的例子。
~~~
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});
proxy.time // 35
proxy.name // 35
proxy.title // 35
~~~
上面代码中，作为构造函数，Proxy接受两个参数。第一个参数是所要代理的目标对象（上例是一个空对象），即如果没有Proxy的介入，操作原来要访问的就是这个对象；第二个参数是一个配置对象，对于每一个被代理的操作，需要提供一个对应的处理函数，该函数将拦截对应的操作。比如，上面代码中，配置对象有一个get方法，用来拦截对目标对象属性的访问请求。get方法的两个参数分别是目标对象和所要访问的属性。可以看到，由于拦截函数总是返回35，所以访问任何属性都得到35。

注意，要使得Proxy起作用，必须针对Proxy实例（上例是proxy对象）进行操作，而不是针对目标对象（上例是空对象）进行操作。
一个技巧是将Proxy对象，设置到`object.proxy`属性，从而可以在object对象上调用。
~~~
var object = { proxy: new Proxy(target, handler) }
~~~
Proxy实例也可以作为其他对象的原型对象。
~~~
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});
let obj = Object.create(proxy);
obj.time // 35
~~~
上面代码中，proxy对象是obj对象的原型，obj对象本身并没有time属性，所有根据原型链，会在proxy对象上读取该属性，导致被拦截。
同一个拦截器函数，可以设置拦截多个操作。
~~~
var handler = {
  get: function(target, name) {
    if (name === 'prototype') return Object.prototype;
    return 'Hello, '+ name;
  },
  apply: function(target, thisBinding, args) { return args[0]; },
  construct: function(target, args) { return args[1]; }
};
var fproxy = new Proxy(function(x,y) {
  return x+y;
},  handler);
fproxy(1,2); // 1
new fproxy(1,2); // 2
fproxy.prototype; // Object.prototype
fproxy.foo; // 'Hello, foo'
~~~
下面是Proxy支持的拦截操作一览。
对于可以设置、但没有设置拦截的操作，则直接落在目标对象上，按照原先的方式产生结果。
**（1）get(target, propKey, receiver)**
    拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`，返回类型不限。最后一个参数receiver可选，当target对象设置了propKey属性的get函数时，receiver对象会绑定get函数的this对象.
**（2）set(target, propKey, value, receiver)**
    拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`,返回一个布尔值。
**（3）has(target, propKey)**
    拦截`propKey in proxy`的操作，返回一个布尔值
**（4）deleteProperty(target, propKey)**
    拦截`delete proxy[propKey]`的操作，返回一个布尔值。
**（5）enumerate(target)**
    拦截`for (var x in proxy)`，返回一个遍历器
**（6）hasOwn(target, propKey)**
    拦截`proxy.hasOwnProperty('foo')`，返回一个布尔值。
**（7）ownKeys(target)**
    拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`，返回一个数组。该方法返回对象所有自身的属性，而`Object.keys()`仅返回对象可遍历的属性。
**（8）getOwnPropertyDescriptor(target, propKey)**
    拦截Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。
**（9）defineProperty(target, propKey, propDesc)**
    拦截Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值。
**（10）preventExtensions(target)** 
    拦截Object.preventExtensions(proxy)，返回一个布尔值。
**（11）getPrototypeOf(target)**
    拦截Object.getPrototypeOf(proxy)，返回一个对象。
**（12）isExtensible(target)**
    拦截Object.isExtensible(proxy)，返回一个布尔值。
**（13）setPrototypeOf(target, proto)**
    拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
**（14）apply(target, object, args)**
    拦截Proxy实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
**（15）construct(target, args, proxy)**
    拦截Proxy实例作为构造函数调用的操作，比如new proxy(...args)。下面是其中几个重要拦截方法的详细介绍。

### get()
get方法用于拦截某个属性的读取操作。上文已经有一个例子，下面是另一个拦截读取操作的例子。
~~~
var person = {
  name: "张三"
};
var proxy = new Proxy(person, {
  get: function(target, property) {
    if (property in target) {
      return target[property];
    } else {
      throw new ReferenceError("Property \"" + property + "\" does not exist.");
    }
  }
});
proxy.name // "张三"
proxy.age // 抛出一个错误
~~~
上面代码表示，如果访问目标对象不存在的属性，会抛出一个错误。如果没有这个拦截函数，访问不存在的属性，只会返回undefined。
利用proxy，可以将读取属性的操作（get），转变为执行某个函数。
~~~
var pipe = (function () {
  var pipe;
  return function (value) {
    pipe = [];
    return new Proxy({}, {
      get: function (pipeObject, fnName) {
        if (fnName == "get") {
          return pipe.reduce(function (val, fn) {
            return fn(val);
          }, value);
        }
        pipe.push(window[fnName]);
        return pipeObject;
      }
    });
  }
}());
var double = function (n) { return n*2 };
var pow = function (n) { return n*n };
var reverseInt = function (n) { return n.toString().split('').reverse().join('')|0 };
pipe(3) . double . pow . reverseInt . get
// 63
~~~
上面代码设置Proxy以后，达到了将函数名链式使用的效果。
### set()
set方法用来拦截某个属性的赋值操作。假定Person对象有一个age属性，该属性应该是一个不大于200的整数，那么可以使用Proxy对象保证age的属性值符合要求。
~~~
let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }
    // 对于age以外的属性，直接保存
    obj[prop] = value;
  }
};
let person = new Proxy({}, validator);
person.age = 100;
person.age // 100
person.age = 'young' // 报错
person.age = 300 // 报错
~~~
上面代码中，由于设置了存值函数set，任何不符合要求的age属性赋值，都会抛出一个错误。利用set方法，还可以数据绑定，即每当对象发生变化时，会自动更新DOM。
### apply()
apply方法拦截函数的调用、call和apply操作。
~~~
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};
var p = new Proxy(target, handler);
p() === 'I am the proxy';
// true
~~~
上面代码中，变量p是Proxy的实例，当它作为函数调用时（p()），就会被apply方法拦截，返回一个字符串。 
### ownKeys()
ownKeys方法用来拦截Object.keys()操作。
~~~
let target = {};
let handler = {
  ownKeys(target) {
    return ['hello', 'world'];
  }
};
let proxy = new Proxy(target, handler);
Object.keys(proxy)
// [ 'hello', 'world' ]
~~~
上面代码拦截了对于target对象的Object.keys()操作，返回预先设定的数组。
### Proxy.revocable()
Proxy.revocable方法返回一个可取消的Proxy实例。
~~~
let target = {};
let handler = {};
let {proxy, revoke} = Proxy.revocable(target, handler);
proxy.foo = 123;
proxy.foo // 123
revoke();
proxy.foo // TypeError: Revoked
~~~
Proxy.revocable方法返回一个对象，该对象的proxy属性是Proxy实例，revoke属性是一个函数，可以取消Proxy实例。上面代码中，当执行revoke函数之后，再访问Proxy实例，就会抛出一个错误。

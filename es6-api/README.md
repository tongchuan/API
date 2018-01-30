# let和const命令
 1. let命令
    1. 基本语法
    2. 不存在
    3. 不允许重复声明
    4. 块级作用域
 2. const命令
    1. 跨模块常量
 3. 全局对象的属性
 

# 变量的解构赋值
 1. 数组的解构赋值
 2. 对象的解构赋值
 3. 字符串的解构赋值
 4. 函数参数的解构赋值
 5. 圆括号问题
 6. 不能使用圆括号的情况
 7. 可以使用圆括号的情况
 8. 用途
    1. 交换变量的值
    2. 从函数返回多个值
    3. 函数参数的定义
    4. 提取JSON数据
    5. 函数参数的默认值
    6. 遍历Map结构
    7. 输入模块的指定方法

# 字符串的扩展
 1. codePointAt()
 2. String.fromCodePoint()
 3. 字符串的遍历器接口
 4. at()
 5. normalize()
 6. includes(), startsWith(), endsWith()
 7. repeat()
 8. padStart()，padEnd()
 9. 模板字符串
 10. 实例：模板编译
 11. 标签模板
 12. String.raw()

# 数值的扩展
 1. 二进制和八进制表示法
 2. Number.isFinite(), Number.isNaN()
 3. Number.parseInt(), Number.parseFloat()
 4. Number.isInteger()和安全整数
 5. Math对象的扩展
    1. Math.trunc()
    2. Math.sign()
    3. Math.cbrt()
    4. Math.clz32()
    5. Math.imul()
    6. Math.fround()
    7. Math.hypot()
 6. 对数方法
    1. Math.expm1()
    2. Math.log1p()
    3. Math.log10()
    4. Math.log2()
 7. 三角函数方法

# 数组的扩展
 1. Array.from()    
 2. Array.of()
 3. 数组实例的find()和findIndex()
 4. 数组实例的fill()
 5. 数组实例的entries()，keys()和values()
 6. 数组实例的includes()
 7. 数组推导
 8. Array.observe()，Array.unobserve()

# 对象的扩展
 1. 属性的简洁表示法
 2. 属性名表达式
 3. 方法的name属性
 4. Object.is()
 5. Object.assign()
    1. 为对象添加属性
    2. 为对象添加方法
    3. 克隆对象
    4. 合并多个对象
    5. 为属性指定默认值
 6. proto属性，Object.setPrototypeOf()，Object.getPrototypeOf() 
    1. proto属性
    2. Object.setPrototypeOf()
    3. Object.getPrototypeOf()

# Symbol
 1. 概述
 2. 作为属性名的Symbol
 3. 属性名的遍历
 4. Symbol.for()，Symbol.keyFor()
 5. 内置的Symbol值
    1. Symbol.hasInstance
    2. Symbol.isConcatSpreadable
    3. Symbol.isRegExp
    4. Symbol.match
    5. Symbol.iterator
    6. Symbol.toPrimitive
    7. Symbol.toStringTag
    8. Symbol.unscopables

# Proxy
### 概述
 1. get(target, propKey, receiver)
 2. set(target, propKey, value, receiver)
 3. has(target, propKey)
 4. deleteProperty(target, propKey)
 5. enumerate(target)
 6. hasOwn(target, propKey)
 7. ownKeys(target)
 8. getOwnPropertyDescriptor(target, propKey)
 9. defineProperty(target, propKey, propDesc)
 10. preventExtensions(target)
 11. getPrototypeOf(target)
 12. isExtensible(target)
 13. setPrototypeOf(target, proto)
 14. apply(target, object, args)
 15. construct(target, args, proxy)
### 方法
 * Reflect.getOwnPropertyDescriptor(target,name)
 * Reflect.defineProperty(target,name,desc)
 * Reflect.getOwnPropertyNames(target)
 * Reflect.getPrototypeOf(target)
 * Reflect.deleteProperty(target,name)
 * Reflect.enumerate(target)
 * Reflect.freeze(target)
 * Reflect.seal(target)
 * Reflect.preventExtensions(target)
 * Reflect.isFrozen(target)
 * Reflect.isSealed(target)
 * Reflect.isExtensible(target)
 * Reflect.has(target,name)
 * Reflect.hasOwn(target,name)
 * Reflect.keys(target)
 * Reflect.get(target,name,receiver)
 * Reflect.set(target,name,value,receiver)
 * Reflect.apply(target,thisArg,args)
 * Reflect.construct(target,args)
### Object.observe()，Object.unobserve()
  1. add：添加属性
  2. update：属性值的变化
  3. delete：删除属性
  4. setPrototype：设置原型
  5. reconfigure：属性的attributes对象发生变化
  6. preventExtensions：对象被禁止扩展（当一个对象变得不可扩展时，也就不必再监听了）

# 函数的扩展
### 函数参数的默认值 
### rest参数
### 扩展运算符
### 箭头函数
### 函数绑定

## 尾调用优化
### 什么是尾调用？
### 尾调用优化
### 尾递归
### 递归函数的改写

# Set和Map数据结构
## Set
### 基本用法
### Set实例的属性和方法
 * Set.prototype.constructor：构造函数，默认就是Set函数。
 * Set.prototype.size：返回Set实例的成员总数。
 * add(value)：添加某个值，返回Set结构本身。
 * delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
 * has(value)：返回一个布尔值，表示该值是否为Set的成员。
 * clear()：清除所有成员，没有返回值。

### 遍历操作
 * keys()：返回一个键名的遍历器
 * values()：返回一个键值的遍历器
 * entries()：返回一个键值对的遍历器
 * forEach()：使用回调函数遍历每个成员

## WeakSet
 * WeakSet.prototype.add(value)：向WeakSet实例添加一个新成员。
 * WeakSet.prototype.delete(value)：清除WeakSet实例的指定成员。
 * WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在WeakSet实例之中。

# Map.
## Map结构的目的和基本用法
## 实例的属性和操作方法
 * size：返回成员总数。
 * set(key, value)：设置key所对应的键值，然后返回整个Map结构。如果key已经有值，则键值会被更新，否则就新生成该键。
 * get(key)：读取key对应的键值，如果找不到key，返回undefined。
 * has(key)：返回一个布尔值，表示某个键是否在Map数据结构中。
 * delete(key)：删除某个键，返回true。如果删除失败，返回false。
 * clear()：清除所有成员，没有返回值。

## 遍历方法
 * keys()：返回键名的遍历器。
 * values()：返回键值的遍历器。
 * entries()：返回所有成员的遍历器。

## WeakMap 

# Iterator和for...of循环
## Iterator（遍历器）的概念
 1. 创建一个指针，指向当前数据结构的起始位置。也就是说，遍历器的返回值是一个指针对象。
 2. 第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。
 3. 第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。
 4. 调用指针对象的next方法，直到它指向数据结构的结束位置。

## 数据结构的默认Iterator接口
## 调用默认Iterator接口的场合
 1. 解构赋值
 2. 扩展运算符
 3. 其他场合
    * yield*
    * Array.from()
    * Map(), Set(), WeakMap(), WeakSet()
    * Promise.all(), Promise.race()

## 原生具备Iterator接口的数据结构
## Iterator接口与Generator函数
## 遍历器的return()，throw()
## for...of循环
## 数组
## Set和Map结构
## 计算生成的数据结构
## 类似数组的对象
## 对象
## 与其他遍历语法的比较

#Generator 函数
## 基本概念
## yield语句
## 与Iterator的关系
## next方法的参数
## for...of循环
## throw方法
## yield*语句
## 作为对象属性的Generator函数   
## Generator函数推导
## 含义
## Generator与状态机
## Generator与协程
 1. 协程与子例程的差异
 2. 协程与普通线程的差异
## 应用 
 1. 异步操作的同步化表达
 2. 控制流管理
 3. 部署iterator接口
 4. 作为数据结构

# Promise对象
## Promise的含义
## 基本用法
## Promise.prototype.then()
## Promise.prototype.catch()
## Promise.all()
## Promise.race()
## Promise.resolve()
## Promise.reject()
## Generator函数与Promise的结合
## async函数

# 异步操作 
## 基本概念
## 异步
### 回调函数
### Promise
## Generator函数
### 协程
### Generator函数的概念
### Generator函数的数据交换和错误处理
### 异步任务的封装
##Thunk函数
### 参数的求值策略
### Thunk函数的含义

## JavaScript语言的Thunk函数
## Thunkify模块
## Generator 函数的流程管理
## Thunk函数的自动流程管理
## co模块
### 基本用法
### co模块的原理
##基于Promise对象的自动执行
## co模块的源码

##处理并发的异步操作
## async函数
### 含义
### async函数的实现
### async 函数的用法
### 注意点
### 与Promise、Generator的比较

# Class
## Class基本语法
### 概述
### constructor方法
### 实例对象
### name属性
### Class表达式
### 不存在变量提升
### 严格模式

## Class的继承
### 基本用法
### 类的prototype属性和proto属性
### Object.getPrototypeOf()
## 实例的proto属性
## 原生构造函数的继承
## class的取值函数（getter）和存值函数（setter）
## Class的Generator方法
## Class的静态方法
## new.target属性
## 修饰器
### 类的修饰
### 方法的修饰
### core-decorators.js
 1. @autobind
 2. @readonly
 3. @override
 4. @deprecate (别名@deprecated)
 5. @suppressWarnings
### Mixin
### Trait	
### Babel转码器的支持	


# Module
## export命令
## import命令
## 模块的整体输入
## module命令
## export default命令
## 模块的继承
## ES6模块的转码
### ES6 module transpiler
### SystemJS

# 编程风格
## 块级作用域
### let取代var
### 全局常量和线程安全
### 严格模式
## 字符串
## 解构赋值
## 对象
## 数组
## 函数
## Map结构
## Class
## 模块

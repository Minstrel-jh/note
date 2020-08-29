# 面向对象

## 对象是什么  

- 是单个实物的抽象
- 是一个容器，封装了属性(property)和方法(method)

## 构造函数

JavaScript的对象体系，不是基于“类”，而是基于`构造函数`和`原型链`

构造函数特点:

- 函数内部使用`this`关键字，代表所要生成的对象实例
- 生成对象时，必须使用`new`命令

## new命令

- 一些细节：
  - `new`命令后面的构造函数可以带括号或不带括号
  - 构造函数内部return非对象，new命令会忽略return，返回this对象
  - 构造函数内部return一个对象，new命令将返回return指定的对象
- 如果忘记使用`new`命令，构造函数就变成普通函数，不会生成对象，`this`这时代表全局对象。保证构造函数的解决方法:
  - use strict
  - this instanceof XXX
  - new.target
- 原理
  - 创建一个空对象，作为返回的对象实例
  - 将对象的原型，指向构造函数的`prototype`属性
  - 将空对象赋值给内部的`this`关键字
  - 执行构造函数内部的代码
- Object.create()可以将现有对象作为模板，生成新的对象

## this关键字

JavaScript 语言之中，一切皆对象，运行环境也是对象，所以函数都是在某个对象之中运行。  
JavaScript 支持运行环境动态切换，也就是说，this的指向是 `动态` 的。  

注意点：

- 避免多层this
- 避免数组处理方法中的this （map和foreach）
- 避免回调函数中的this

绑定this的方法：

- Function.prototype.call()

  - 参数是`null`或`undefined`，默认传入全局对象
  - 可以接受多个参数。第一个参数就是this所要指向的那个对象，后面的参数则是函数调用时所需的参数

  ```javascript
  func.call(thisValue, arg1, arg2, ...)
  ```

- Function.prototype.apply()

  - 作用与`call`方法类似，区别在于接收一个数组作为函数执行时的参数

  ```javascript
  func.apply(thisValue, [arg1, arg2, ...])
  ```

- Function.prototype.bind()

  - 每一次返回一个新函数
  - 结合回调函数使用
  - 结合`call()`方法使用

## 对象的继承

### 原型对象

- 问题
  - 同一个构造函数的多个实例无法共享属性
- 解决
  - javascript规定，每个函数都有一个`prototype`属性，指向一个对象
  - 对于构造函数，生成实例的时候，该属性会成为实例对象的`原型对象`。只要修改原型对象，变动就立刻会体现在`所有`实例对象上。
- 原型链
- constructor属性
  - `prototype`对象有个`constructor`属性，指向构造函数。
  - 作用
    - 确定构造函数
    - 从一个实例新建另一个实例

### instanceof 运算符



---
title: this
date: 2020-07-03 21:18:47
tags:
- 面试
---

## 为什么要用 this
> this提供了一种跟优雅方式来‘隐式传递’一个对象应用，因此可以将API设计的更加简洁并且易于复用。 
 随的使用模式模式越来越复杂，显式传递上下文对象会让代码变得越来越混乱。

```javaScript
function identify () {
    return this.name.toUpperCase()
}
function speak () {
    var greeting = "Hello, i'm" + identify.call(this)
    console.log(greeting)
}
var me = {
    name: 'Kyle'
}
var you = {
    name: 'Reader'
}
identify.call(me) //KYLE
identify.call(you) //READER
speak.call(me) //Hello, i'm KYLE
speak.call(you) //Hello, i'm YOU
```

## this 是什么以及调用位置
> 当一个函数被调用的时候，会创建一个活动记录（执行上下文）。这个记录会包含函数在哪里调用（调用栈）、函数调用方法、传入的参数等信息。this 就是这个记录的一个属性，会在函数执行过程中用到。

> this 不是在编写时候绑定的，而是在代码运行时候被绑定的。this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。

## this 绑定规则

### 默认绑定

> 独立函数调用时候，this一般绑定在全局对象，在严格模式下，就绑定到undefined，可以把这条规则看作是无法应用其他规则的默认规则。

### 隐式绑定

> 当函数引用有上下文对象时，this 会绑定在这个上下问对象中。

> 对象属性引用链中只有最后一层在调用位置中起作用。

```javaScript
function foo(){
    console.log(this.a)
}
var obj = {
    a: 42,
    foo: foo
}
var obj1 = {
    a: 2,
    obj2: obj2
}
obj1.obj2.foo() //42
```
### 显式绑定

> 使用call、apply和bind绑定上下文对象。

> 其中call、apply和bing第一个参数都是一个对象，其中call和apply会被立即调用，并的不会立即执行。call第二个参数开始依次传入，apply第二个参数是个数组也可以是argument。

```javaScript
function foo (content){
    return this.a + this.b + this.c + content
}

let obj = {
    a: 1,
    b: 2,
    c: 3
}

console.log(foo.call(obj,4))  //10
console.log(foo.apply(obj,[4])) //10
console.log(foo.bind(obj,4)) //[Function: bound foo]
```
> 手写call
``` javaScript
Function.prototype.myCall = function (content,...arg) {
   content = typeof content === 'object'? content : window
   let key = Symbol()
   //这里的this指的是上层函数
   content[key] = this
   let result = content[key](...arg)
   delete content[key]
   return result
}

function foo (a1,a2) {
   console.log(a1 + a2)
   console.log(this.a)
}

let obj = {
   a: 2
}
foo.myCall(obj,1,2)   // 3 2
```
> 手写apply
```javaScript
function foo (a1,a2) {
   console.log(a1 + a2)
   console.log(this.a)
}
let obj = {
   a: 2
}
Function.prototype.myApply = function (content,arg) {
   content = typeof content === 'object'? content : window
   let key = Symbol()
   content[key] = this
   let result = content[key](...arg)
   delete content[key]
   return result
}
foo.myApply(obj,[1,2]) // 3 2
```

> 手写bind 
``` javaScript
function foo (a1,a2) {
   console.log(a1 + a2)
   console.log(this.a)
}
let obj = {
   a: 2
}
Function.prototype.myBind = function () {
   if(typeof this !== 'function') return
   let self = this
   let fnArg =typeof arguments[0] == 'object' arguments[0] : window 
   let args = Array.prototype.slice.call(arguments,1)
   //用于寄生继承
   let fnNop = function(){}
    function fnBound() {
      //判断是否new操作，this是否指向构造函数的this
      let _this = this instanceof self? this: fnArg
      return self.apply(fnArg,args)
   }

   if(this.prototype){
      fnNop.prototype = this.prototype
   }
    fnBound.prototype = new fnNop()
   return fnBound
}

let fn = foo.myBind(obj,1,2)

let inster = new fn() //3 undefind
fu()  //3 2
```

### new 绑定

> 使用new来调用函数，或者说发生构造函数调用时候，会自动执行下面操作。

1. 创建一个全新的对象,在新对象执行prototype链接。
2. 这个新对象会绑定到函数调用的 this。
3. 执行内部代码，设置对象属性和方法
4. 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。

```javaScript
 function setName(name,age,sex) {
    this.name = name
    this.age = age
    this.sex = sex
 }

 let zhang = new setName("zhang",27,'男')
```

### 绑定优先级

> new绑定 > 显示绑定 > 隐式绑定 > 默认绑定

9
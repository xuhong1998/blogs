---
title: Javascript继承
date: 2019-11-04 10:03:01
tags:
- 继承
- JavaScript高阶程序指南
---
## 继承
> 许多面向对象语言都支持两种继承方法：接口继承和实现继承。接口继承只继承方法签名，而实现继承则继承实际的方法。由于ECMAScript函数没有签名，无法实现接口继承，只支持实现继承，而且其实现继承主要是依靠原型链来实现
### 原型链
> 利用原型让一个引用类型继承另外一个引用类型的属性和方法。实现的本质是重写原型对象
```javaScript
     function SuperType() {
        this.property = true
    }
    SuperType.prototype.getSuperValue = function() {
      return this.property
    }
    function SubType() {
      this.subproperty = false
    }
    SubType.prototype = new SuperType()  //将SuperType实例赋值给SubTy.prototy
    SubType.prototype.getSubValue = function() {
      return this.subproperty
    }
    var instance = new SubType()
    alert(instance.getSuperValue()) //true
```

#### 1.别忘了默认的原型

>所有的引用类型默认都继承了Object

#### 2.确定原型和实例的关系

```javaScript
//instanceof
console.log(instance instanceof Object)         //ture
console.log(instance instanceof SuperType)      //ture
console.log(instance instanceof SubType)        //ture

//isPrototypeOf
console.log(Object.prototype.isPrototypeOf(instance))       //ture
console.log(SuperType.prototype.isPrototypeOf(instance))    //ture
console.log(SubType.prototype.isPrototypeOf(instance))      //ture
```
#### 3.谨慎定义方法

>子类型有时候要覆盖超类型中的某个方法，或者需要添加超类型中不存在方法，给原型添加方法一定要放在替换原型的语句之后。

```javaScript
//继承SuperType
SubType.prototype = new SuperType()
//使用字面量添加新方法，会导致上一行代码无效
SubType.prototype = {
    //**：function（）{}
}
var instance = new SubType();
alert(instance.getSuperValue())      // instance.getSuperValue is not a function
```
> 通过原型继承的，不能使用字面量创建原型方法。因为这样做就会重写原型链

#### 4.原型链问题

```javaScript
    function SuperType() {
        this.colors = ['red','blue']
    }
    function SubType() {
      this.subproperty = false
    }
    SubType.prototype = new SuperType()  
    var instance1 = new SubType()
    instance.colors.push('black')
    console.log(instance.colors)   //'red,blue,black'
    var instance2 = new subType()
    console.log(instance.colors)   //'red,blue,black'
```
 
 > 1.引用类型值的原型属性会被所有的实例共享
 > 2.在创建子类型实例时，不能向超类型的构造函数中传递参数
 > 3.在实践中很少会单独使用原型链实现继承

### 借用构造函数

> 在子类型构造函数内部调用超类型构造函数。

```javaScript
    function SuperType() {
        this.colors = ['red','blue']
    }
    function SubType() {
      //继承了SuperType
      SuperType.call(this)
    }
    var instance1 = new SubType()
    instance1.colors.push('black')
    console.log(instance1.colors)  //red,blue,black

    var instance2 = new SubType()
    console.log(instance2.colors)  //red,blue

```
#### 1.传递参数
```javaScript
    function SuperType(name) {
        this.name = name
    }
    function SubType() {
      //继承了SuperType
      SuperType.call(this,'xh')

      //实例属性
      this.age = 21
    }
    var instance = new SubType()
    console.log(instance.name)   //xh
    console.log(instance.age)    //21
```
#### 2.借用构造函数的问题
> 1.方法都在构造函数中定义，函数复用无从谈起。
2.超类型的原型定义方法，对于子类型是不可见的。
3.借用构造函数的技术很少单独使用

### 组合继承
> 使用原型链实现对原型属性和方法继承

```javaScript
    function SuperType(name) {
        this.name = name
        this.colors = ['red','blue']
    }
    SuperType.prototype.sayName = function() {
      return this.name
    }
    function SubType(name,age) {
      //继承了SuperType
      SuperType.call(this,name)

      //实例属性
      this.age = age
    }
    SubType.prototype = new SuperType()
    SubType.prototype.constructor = SubType
    SubType.prototype.sayAge = function() {
      return this.age
    }
    var instance1 = new SubType('xh',21)
    instance1.colors.push('black')
    console.log(instance1.colors)       //red,blue,black
    console.log(instance1.sayName())    //xh
    console.log(instance1.sayAge())     //21

    var instance2 = new SubType('xm',26)
    console.log(instance2.colors)       //red,blue
    console.log(instance2.sayName())    //xm
    console.log(instance2.sayAge())     //26
```

> 1.组合继承避免了原型链和借用构造函数的缺陷，融合了他们的优点。
2.是JavaScript最常用继承模式。
3.instanceof和inpototypeof()也能识别基于组合继承创建的对象。

## 原型式继承

> 借助原型可以基于已有的对象创建新对象，同时还不必创建自定义类型

```javaScript
function object(o){
    function F(){}
    F.prototype = o
    return new F()
  }

  var person = {
    name:'Nicholas',
    friends: ['Shelby','Court','Van']
  }

  var anotherPerson = object(person)
  //var anotherPerson = Object.create(person)
  anotherPerson.name = 'Greg'
  anotherPerson.friends.push('Rob')

  var yetAnotherPerson = object(person)
  //var yetAnotherPerson = Object.create(person)
  yetAnotherPerson.name = 'Linda'
  yetAnotherPerson.friends.push('Barbie')

  console.log(person.friends)    //Shelby,Court,Van,Rob,Barbie
```
> 1.先创建一个临时性的构造函数
2.然后将传入的对象作为构造函数的原型
3.最后返回这个临时类型的一个实例
## 寄生式继承

> 创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增加对象，最后再像真地是它做了所有工作一样返回对象
```javaScript
  function object(o){
    function F(){}    
    F.prototype = o
    return new F()
  }
  function createAnother(original) {
    var clone = object(original)   //通过调用函数创建一个新对象
    clone.sayHi = function() {     //以某种方式来增强这个对象
      alert('hi')
    }
    return clone                   //返回这个对象
  }
  var person = {
    name:'Nicholas',
    friends: ['Shelby','Court','Van']
  }
  var anotherPerson = createAnother(person)
  anotherPerson.sayHi()   //'hi'
```
> 1。createAnother() 函数接收一个新对象的基础对象
2。通过调用Objec.create() 创建一个新对象
3。以某种方式来增强这个对象
4。返回这个对象
## 寄生组合式继承

> 通过借用构造函数来继承属性，通过原型链的混成形式来继承方法
> 基本思路：不必为指定子类型的原型而调用超类型的构造函数，我们所需的无非就是超类型原型的一个副本而已。本质上就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型

```javaScript
  function object(o){
    function F(){}
    F.prototype = o
    return new F()
  }
  function inheritPrototype(subType, superType) {
    var prototype = Object(superType.prototype)   //创建对象
    prototype.constructor = subType               //增强对象
    subType.prototype = prototype                 //指定对象
  }
  function SuperType(name) {
    this.name = name
    this.colors = ['red','blue']
  }
  SuperType.prototype.sayName = function() {
    console.log(this.name)
  }
  function SubType(name,age){
    SuperType.call(this,name)
    this.age = age
  }
  inheritPrototype(SubType,SuperType)
  SubType.prototype.sayAge = function() {
    console.log(this.age)
  }
  var instance = new SubType('xh',21)
  instance.sayName();    //xh
  instance.sayAge();     //21
```
>1。创建超类型的原型的一个副本
2。为创建的副本添加一个constructor，从而弥补因重写原型而失去的默认的constructor属性
3。将新创建的对象赋值给子类型的原型


> 最理想的继承方式是寄生组合式继承。
组合继承（构造函数和原型的组合）会调用两次父类构造函数的代码，





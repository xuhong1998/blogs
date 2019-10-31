---
title: ES6面试
date: 2019-10-31 14:46:40
tags: 算法
---


## ES6 模块化如何使用，开发环境如何打包

#### 模块化的基本语法

```javaScript
    //util1.js
    export default { //默认
        a: 100
    }

    //util2.js
    export function fn1(){
        alert('Fn1')
    }
    export function fn2(){
        alert('Fn2')
    }

    //index.js
    import util1 from './util1.js'
    import { fn1 , fn2 } from './util2.js'
    console.log(util1)
    fn1();
    fn2();
```

#### 开发环境配置

+ 开发环境 - babel - 语法层面解析
+ 开发环境 - webpack - 模块化，相互引用，多层次引用
+ 开发环境 - rollup - 更高级打包工具 - 优化打包代码，功能单一 

#### 众多模块化标准

+ 没有模块化
+ AMD 成为标准，require.js(没有 CMD)
+ 前端打包工具，node模块化可以被使用
+ ES6 想统一现在所有模块化标准

## Class 和普通构造函数有何区别 

#### JS构造函数

```javaScript 
    function MathHandle (x, y){
        this.x = x;
        this.y = y;
    }

    MathHandle.prototype.add = function () {
        return this.x + this.y;
    }

    var m = new MathHandle(1, 2);
    console.log(m.add())
```
#### Clss基本语法
```javaScript
    class MathHandle {
        constructor(x, y) {
            this.x = x;
            this.y = y;
        }
        add() {
            return this.x + this.y
        }
    }

    const m = new MathHandle(1, 2);
    console.log(matchMedia.add())
    typeof MathHandle  // 'function'
    MathHandle.prototype.constructor === MathHandle //true
```
语法糖 - 形式上强行模仿 Java C# 语法

#### 继承
+ js继承

```javaScript
    //动物
    function Animal() {
        this.eat = function() {
            console.log('Animal eat')
        }
    }
    //狗
    function Dog() {
        this.bark = function() {
            console.log('dog bark')
        }
    }
    Dog.prototype = new Animal();
    //哈士奇
    var hashiqi = new Dog()
```
+  Class继承

```javaScript
    class Animal {
        constructor(name) {
            this.name = name
        }
        eat() {
            console.log('${this.name} eat')
        }
    }
    class Dog extends Animal {
        constructor(name){
            super(name) //父类的构造函数
            this.name = name
        }
        say() {
            console.log('${this.name} say')
        }
    }
    const dog = new Dog('哈士奇');
    dog.eat();
    dog.say();
```
> 总结
+ Class 在语法上更加贴合面向对象的写法
+ Class 实现继承更加易读、易理解
+ 本质还是语法糖，使用 prototype

## ES6 其他常用功能

+ let/const
+ 多行字符串/模板变量
+ 解构赋值
+ 块级作用域
+ 函数默认参数
+ 箭头函数
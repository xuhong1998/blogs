---
title: for in与for of的区别
date: 2021-06-19 16:37:08
tags:
---


## for in

- 1.以任意顺序遍历一个对象的除Symbol以外的可枚举属性。
- 2.索引为字符串，不能直接进行几何运算

  因此for...in不应该用于迭代一个关注索引顺序的 Array。for...in最常用于调试，可以更方便的去检查对象的属性。

```javaScript
const arr = [1, 2, 3, 4, 5, 6]
arr.name = '小明'
Array.prototype.method = function() {
  console.log(this.length)
}

for(const key in arr) {
  console.log(key)  // 0 1 2 3 4 5 name method
}
```

```javaScript
function funObj() {
  this.name = '小米'
  this.age = 18
}
funObj.prototype.getName = function() {
  console.log(this.name)
}
const obj = new funObj()
for(const key in obj) {
  if (obj.hasOwnProperty(key)) {
    console.log(obj[key])  // 小米 18
  }
}
```

## for of 

- 1.for...of语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句
- 2.对于for...of的循环，可以由break, throw  continue  或return终止。在这些情况下，迭代器关闭。

```javaScript
const arr = [1, 2, 3, 4, 5, 6]
arr.name = '小明'
Array.prototype.method = function() {
  console.log(this.length)
}

for (const key in arr) {
  console.log(key)  // 1 2 3 4 5 6
}
```

## 可枚举属性
可枚举属性是指那些内部 “可枚举” 标志设置为 true 的属性，对于通过直接的赋值和属性初始化的属性，该标识值默认为即为 true，对于通过 Object.defineProperty 等定义的属性，该标识值默认为 false。可枚举的属性可以通过 for...in 循环进行遍历（除非该属性名是一个 Symbol）。属性的所有权是通过判断该属性是否直接属于某个对象决定的，而不是通过原型链继承的

#### 关于判断、迭代/枚举以及获取对象的一个或一组属性的内置方法
- propertyIsEnumerable // 方法返回一个布尔值，表示指定的属性是否可枚举。
- hasOwnProperty  // 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。
- getOwnPropertySymbols  // 方法返回一个给定对象自身的所有 Symbol 属性的数组。
```javaScript
const a = Symbol('1')
const b = Symbol('2')
const obj = {}
obj[a] = 1
obj[b] = 2
const objectSysbols = Object.getOwnPropertySymbols(obj)
console.log(objectSysbols) // [Symbol(1), Symbol(2)]
console.log(obj[objectSysbols[0]]) // 1
```
##### [关于通过可枚举性和所有权获取对象的属性统计表](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)

## 可迭代对象
---
title: Object构造函数的属性
date: 2019-11-16 20:42:12
tags: 面试 es6
---
## assign() 通过复制一个或多个对象来创建一个新的对象

> Object.assign(target,...sources) 方法用于将所有可枚举属性的值从一个或多个资源对象复制到目标对象。他将返回目标对象

#### 示例

- 深拷贝--针对深拷贝需要使用其他的方法，因为assign拷贝的是属性值
- 合并具有相同属性的对象
- 继承属性和不可枚举的属性是不能拷贝的
- 原始类型会被包装为对象

## create() 
> 使用现有对象来提供新创建的对象的__proto__

## defineProperties() 
> 直接在一个对象上定义新的属性或修改现有的属性
## defineProperty() 
> 直接在一个对象上定义一个新属性，或者修改一个对象的现有属性
## entries() 
> 返回一个给定对象自身可枚举属性的键值对数组
```javaScript
for(let [key,value] of Object.entries({a:5,b:7})){
    console.log(`${key}:${value}`)
}
## fromEntries() 方法把键值对列表转换成一个对象
```
## freeze 
> 冻结一个对象，被冻结的对象都不能以任何方式被修改
```javaScript
//深冻结
function deepFreeze(obj){
    var propName = Object.getOwnPropertyNames(obj)
    propName.forEach((name)=>{
        let prop = obj[name]
        if(typeof prop === 'object' && prop != null){
            deepFreeze(prop)
        }
    })
    return Object.freeze(obj)
}
```
---
title: 异步 Promise
date: 2019-11-02 16:19:11
tags: 
- 面试
---


## 什么是单线程，和异步有什么关系
+ 单线程-同时间只能做一件事
+ 原因-避免 DOM 渲染冲突 
+ 解决方案-异步

## 什么是event-loop
+ 事件轮询，js实现异步的具体解决方案
+ 同步代码，直接执行
+ 异步函数先放在异步队列中
+ 待同步函数执行完毕，轮询执行异步队列函数

#### 实例分析
```javaScript
//ajax请求完成时放进异步队列
$ajax({
    url:'****',
    success: function(result) {
        console.log('a')    
    }
})
//100mm后放入异步队列
setTimeout(function() {
    console.log('b')
}, 100)
//立即放入异步队列
setTimeout(function() {
    console.log('c')
})
console.log('d')
```
## 是否用过JQuery的Deferred
```javaScript
  <script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
  <script>
    function waitHandle() {
      var dtd = $.Deferred
      var wait = function(dtd) {
        var task = function() {
          console.log('执行完成')
          //成功
          dtd.resolve()
          //失败
          //dtd.reject()
        }
        setTimeout(task,2000)
        return dtd
      }
      return wait(dtd)
    }
    var w = waitHandle();
    w.then(function(){
      console.log('ok 1')
    },function(){
      console.log('err 1')
    }).then(function() {
      console.log('ok 2')
    },function(){
      console.log('err 2')
    })
  </script>
```
## Promise 的基本使用和原理

> Promise 对象是一个代理对象，被代理的值在Promise对象创建时可能是未知的，它允许你为异步操作的成功和失败分别绑定相应的处理方法。这让异步方法可以像同步方法那样返回值，但并不是立即返回最终的结果，而是一个能代表未来出现结果的promise对象
+ ***pendding*** : 初始状态，既不是成功，也不是失败
+ ***fulfilled*** : 意味操作成功
+ ***rejected*** : 意味的操作失败


![Promise](https://mdn.mozillademos.org/files/8633/promises.png)
### 方法
+ Promise.all(iterable) 
> 方法返回一个实例，此实例在iterable参数内所有的Promise都resolved或者参数中不包含promise时回调完成；如果参数中promsole有一个rejected，此实例回调失败，失败原因的是第一个失败promise的结果
***当且仅当***传入的课迭代对象为空时为同步对象

```JavaScript
Promise.all([1,2,3]).then(value => {console.log(value)})     //[1,2,3]
Promise.all([1,2,3,Promise.resolve(444)]).then(value => {console.log(value)})    //[1,2,3,444]
Promise.all([1,2,3,Promise.reject(22)]).then(value => {console.log(value)})      //rejected 22
```
+ Promise.race(iterable)
> 方法返回一个promise, 一旦迭代器中的某一个promise解决或拒绝，返回的promise就会解决或者拒绝

```javaScript
var p1 = new Promise(function(resolve,reject){
    setTimeout(resolve,500,'one')
})
var p2 = new Promise(function(resolve,reject){
    setTimeout(resolve,100,'two')
})
Promise.race([p1,p2]).then(value=>{
    console.log(value)   //two
})
```

### promise 原型方法

+ Promise.prototype.catch(onRejected)
> 返回一个promise, 并且处理拒绝的情况

+ Promise.prototype.then(onFulfilled,onRejected)
> 返回一个promise，最多需要两个参数：Promise的成功和失败情况的回调函数。
> 当执行 then 方法时，如果前面的 promise 已经是 resolved 状态，则直接将回调放入微任务队列中

+ Promise.prototype.finally(onFinally)
> 由于无法知道promise的最终状态，所以finally的回调函数中不接受任何参数，它仅用于无论最终结果如何都要执行的情况。
 方法返回一个Promise，在promise结束时，无论结果是fulfilled或者是reject的，都会执行指定的回调函数。这是为在Promise是否成功完成后都需要执行的代码提供一种方式。

```javaScript
function promise(data){
    return new Promise((resolve,reject) => {
        if(data){
            resolve(11)
        }else{
            reject(22)
        }
    })
}
promise(false).then(res => {
    console.log(res)
},err=>{
    console.log(err)
}).then(res => {
    console.log(res)
}).finally(()=>{
    console.log(2)
})
//22
//undefined
//2
```
## async/await(优点)
## 当前JS解决异步的方案
 
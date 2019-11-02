---
title: 虚拟dom
date: 2019-11-01 21:41:43
tags: 
- 面试
---

## vdom 是什么？为何会存在vdom？
#### 什么是vdom
+ virtual dom， 虚拟DOM
+ 用js模拟DOM结构
+ DOM变化的对比，放在JS层来做
+ 提高重绘性能
```html
    <ul id="list">
        <li class="item">Item 1</li>
        <li class="item">Item 2</li>
    </ul>
```
```javaScript
{
    tag: 'ul',
    attrs: {
        id: 'list'
    },
    children: [
        {
            tag: 'li',
            attrs: {className: 'item'} //class是js保留字
            children: ['Item 1']
        },{
            tag: 'li',
            attrs: {className: 'item'}
            children: ['Item 2']
        }
    ]
}
```
#### 设计一个需求场景
```javaScript
[
    {
        name: '张三',
        age: '20',
        address: '北京'
    },{
        name: '李四',
        age: '21',
        address: '上海'
    },{
        name: '王五',
        age: '22',
        address: '广州'
    }
]
```
#### 用JQuery实现
```html
  <div class="content"></div>
  <button id="btn-change">change</button>
  <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
  <script type="text/javascript">
    var data = [
        {
            name: '张三',
            age: '20',
            address: '北京'
        },{
            name: '李四',
            age: '21',
            address: '上海'
        },{
            name: '王五',
            age: '22',
            address: '广州'
        }
    ]
    function render(data) {
        var content = $('.content')
        //清空容器
        content.html('')
        //拼接table
        var table = $('<table>')
        table.append($('<tr><td>name</td><td>age</td><td>address</td></tr>'))
        data.forEach(element => {
            table.append($('<tr><td>'+element.name+'</td><td>'+element.age+'</td><td>'+element.address+'</td></tr>'))
        });
        //渲染页面
        content.append(table)
    }
    $("#btn-change").click(function(){
        data[1].age = 30
        data[1].address = '深圳'

        //re-render  再次渲染
        render(data);
    })
    //初次渲染页面
    render(data);
  </script>
```
#### 遇到的问题
+ DOM操作是'昂贵'的，js运行效率高
+ 尽量减少DOM操作
+ 项目越复杂，影响越严重 
## vdom 的如何让应用，核心API是什么
```html
  <div class="content"></div>
  <button id="btn-change">change</button>
  <script type="text/javascript" src="https://cdn.bootcss.com/jquery/3.4.0/jquery.min.js"></script>
  <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom-class.js"></script>
  <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom-props.js"></script>
  <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom-style.js"></script>
  <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom-eventlisteners.js"></script>
  <script src="https://cdn.bootcss.com/snabbdom/0.7.1/h.js"></script>
  <script src="https://cdn.bootcss.com/snabbdom/0.7.1/snabbdom.js"></script>
  <script type="text/javascript">
    var snabbdom = window.snabbdom

    //定义 patch
    var patch = snabbdom.init([
        snabbdom_class,
        snabbdom_props,
        snabbdom_style,
        snabbdom_eventlisteners
    ])

    //定义 h
    var h = snabbdom.h

    //生成 vnode
    var vnode = h('ul#list',{},[
        h('li.item', {}, 'item 1'),
        h('li.item', {}, 'item 2')
    ])
    
    var content = document.getElementsByClassName('content')
    patch(content,vnode)
  </script>
```
+ h函数--将真实dom隐射成虚拟节点
+ patch函数--对比新旧虚拟节点（diff），找到差异，跟新到真实dom
## 介绍一下 Diff 算法
#### 什么是diff算法
+ 比较两个文件差异
+ git diff
#### vdom为何使用diff算法
#### diff算法实现流程


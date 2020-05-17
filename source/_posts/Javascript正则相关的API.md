---
title: JavaScript正则相关的API
date: 2020-05-17 15:49:57
tags:
- 正则
---

## RegExp对象

```javaScript
let regexl = new RegExp(/\w+/)
let regexl = /\w+/
```

### 常用方法

#### exec()

方法在一个指定字符串中执行一个搜索匹配。返回一个结果数组或 null。

- groups: 一个捕获组数组 或 undefined（如果没有定义命名捕获组）。
- index: 匹配的结果的开始位置
- input: 搜索的字符串.

```JavaScript
console.log(/\d+/g.exec('aczsxaa11saa15xs6c'))  
// [ '11', index: 7, input: 'aczsxaa11saa15xs6c', groups: undefined ]
var myRe = /\d+/g;
var str = 'aczsxaa11saa15xs6c';
var myArray;
while ((myArray =  myRe.exec(str)) !== null) {
  console.log(myArray[0]);
}
// 11 15 6
```

**注意：不要把正则表达式字面量（或者RegExp构造器）放在 while 条件表达式里。由于每次迭代时 lastIndex 的属性都被重置，如果匹配，将会造成一个死循环。并且要确保使用了'g'标记来进行全局的匹配，否则同样会造成死循环。**

#### text()

方法执行一个检索，用来查看正则表达式与指定的字符串是否匹配。返回 true 或 false。

```javaScript
 let str = 'xxhh'
 let regexl = /xxhh/g

 console.log(regexl.test(str))  //true
 console.log(regexl.test(str))  //false
```

**如果正则表达式设置了全局标志，test() 的执行会改变正则表达式   lastIndex属性。连续的执行test()方法，后续的执行将会从 lastIndex 处开始匹配字符串，(exec() 同样改变正则本身的 lastIndex属性值).**

## String对象

### 常用正侧方法

#### search()

方法执行正则表达式和 String 对象之间的一个搜索匹配。如果匹配成功，则 search() 返回正则表达式在字符串中首次匹配项的索引;否则，返回 -1。

```javaScript
 let str = 'aaa1a2z6s'
 let regexl = /\d/

 console.log(str.search(regexl))  //3
```

#### match()

方法检索返回一个字符串匹配正则表达式的的结果。
- 如果使用g标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组。
- 如果未使用g标志，则仅返回第一个完整匹配及其相关的捕获组（Array）。 在这种情况下，返回的项目将具有如下所述的其他属性。

```javaScript
 let str = 'aaa1a2z666s'
 console.log(str.match(/\d+/))   
 //[ '1', index: 3, input: 'aaa1a2z666s', groups: undefined ]
 console.log(str.match(/\d+/g))  //[ '1', '2', '666' ]

```

#### matchAll()

 方法返回一个包含所有匹配正则表达式的结果及分组捕获组的迭代器。
 如果使用matchAll ，就可以不必使用while循环加exec方式（且正则表达式需使用／g标志）。使用matchAll 会得到一个迭代器的返回值，配合 for...of, array spread, or Array.from() 可以更方便实现功能

 ```javaScript
 let str = 'aaa1a2az666as'
 let regexl = /\d+/g
 let matches = str.matchAll(regexl)
 for (const match of matches) {
  console.log(match);
 }
 //[ '1', index: 3, input: 'aaa1a2az666as', groups: undefined ]
 //[ '2', index: 5, input: 'aaa1a2az666as', groups: undefined ]
 //[ '666', index: 8, input: 'aaa1a2az666as', groups: undefined ]
 console.log(str.match(regexl)) //[ '1', '2', '666' ]
 ```

#### split()

方法使用指定的分隔符字符串将一个String对象分割成子字符串数组，以一个指定的分割字串来决定每个拆分的位置。 
- 一个整数，限定返回的分割片段数量。当提供此参数时，split 方法会在指定分隔符的每次出现时分割该字符串，但在限制条目已放入数组时停止。如果在达到指定限制之前达到字符串的末尾，它可能仍然包含少于限制的条目。新数组中不返回剩下的文本。

```javaScript
 let str = 'aaa1a2az666as'
 let regexl = /\d+/g
 
 console.log(str.split(regexl))  //[ 'aaa', 'a', 'az', 'as' ]
 console.log(str.split(regexl,2))  //[ 'aaa', 'a' ]
 console.log(''.split('a'))  //['']
 console.log(''.split(''))   //[]
```

**注意：如果使用空字符串(“)作为分隔符，则字符串不是在每个用户感知的字符(图形素集群)之间，也不是在每个Unicode字符(代码点)之间，而是在每个UTF-16代码单元之间。这会摧毁代理对**

#### replace()

 方法返回一个由替换值（replacement）替换一些或所有匹配的模式（pattern）后的新字符串。模式可以是一个字符串或者一个正则表达式，替换值可以是一个字符串或者一个每次匹配都要调用的回调函数。

原字符串不会改变。
```javaScript
 let str = 'aaa1a2az666as'
 let regexl = /\d+/g
 
 console.log(str.replace(regexl,'f'))  
```

[详细介绍](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

## 工具


[正则检测工具](https://regexr-cn.com/)

[正则语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

[正则练习工具](https://www.codejiaonang.com/#/course/regex_chapter1/0/6)

[正则常用方法](https://github.com/any86/any-rule)

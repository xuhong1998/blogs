---
title: CSS动画
date: 2019-12-02 19:49:14
tags:
- css
- 面试
---
 ## transition 
 > transition 属性可以被指定为一个或多个CSS属性的过度效果，多个属性之间用逗号隔开
 
> transition的优点在于简单易用，那么是不是可以完全的取代js渲染出来的动画吗？事实证明我们现在的transition属性还存在很大的局限性：
(1)transition需要事件触发，所以没有办法在网页加载的时候自动的发生。
(2)transition是一次性的，不能重复大声的，除非是一而再在文山的被触发。
(3)transition只能定义开始状态和最终转台，没有中间状态，也就是说，只能拥有2个状态。
(4)一条transition规则，只能定义一个属性的变化，不能涉及多个属性。
。
 #### transition-property 
 > 规定过渡效果的CSS属性名称
 #### transition-duration 
 > 规定完成过渡效果需要多少秒或者毫秒
 #### transition-timing-function
 > 规定速度效果的速度曲线
 + linear: 匀速模式
 + ease-in: 加速模式
 + ease-out: 减速
 + cubi-bezie函数: 自定义的熟读模式
 #### transition-delay 
 > 定义过度效果何时开启


 ## animation

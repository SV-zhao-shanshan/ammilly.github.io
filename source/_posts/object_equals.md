---
layout: post
title: a == 1 && a == 2 && a == 3 ?
tags: [object,equals,js]
date: 2022-07-15 08:40:00 +800
---

> 如何才能让标题的等式成立呢，这是一个很有意思的问题，也很考验 js 的基础知识，涉及到知识点还真不少，在此学习以记之。

<!--more-->

## 松相等和严相等
js 中的相等分别松相等(==)和严相等(===)，这两个都可以用来判断两个值是否**相等**，那么二者的区别是什么呢？下面分情况分析一下：

1. 对于基础类型之间的比较，==和===是有区别的：  
- 同类型比较，直接进行值比较，两者结果相同；
- 不同类型间比较，==会进行强制类型转换，转化成同一类型后，比较值是否相等，===如果类型不同，则结果直接不相等（这里涉及到如何进行类型转换，看下文）
2. 对于引用类型之间的比较，==和===是没有区别的，都是进行**内存地址**比较，不同的两个引用类型比较的结果都不相同
3. 基础类型和引用类型之间的比较，==和===的区别为：  
- 类型不同，===始终不等；
- 对于==，会将引用类型转化为基础类型，进行值比较（这里涉及到引用类型如何转化为基础类型，看下文）

## ==比较的转换规则
*x == y*的比较流程如下（ES5规范中定义：https://262.ecma-international.org/5.1/#sec-11.9.3）
1. 如果Type(x)等于Type(y)，那么：  
a. 如果 Type(x)是 Undefined，返回 true,  
b. 如果 Type(x)是 Null，返回 true,  
c. 如果 Type(x)是 Number，数值类型，则：  
i. 如果 x 是 NaN，返回 false.  
ii. 如果 y 是 NaN，返回 false.  
iii. 如果 x 和 y 都是 Number 类型，并且值相同，则返回 true.  
iv. 如果 x 是 +0 并且 y 是 -0，则返回 true.  
v. 如果 x 是 -0 并且 y 是 +0，则返回 true.  
vi. 返回 false  
d. 如果 Type(x)是 String 类型，若 x 和 y 是完全相同的字符串，则返回 true，否则返回 false。  
e. 如果 Type(x) 是 Boolean 类型，若 x 和 y 同时为 *true* 或同时为*false*，则返回 true，否则返回 false。

2. 如果 x 和 y 中有一个为*undefined*，则返回 true.
3. 如果 Type(x) 和 Type(y) 中有一个为 String，另一个为 Number，则会先把其中的 String 类型转化为 Number 类型再进行比较值。
4. 如果 x 和 y 中有一个为 Boolean，则返回把 Boolean 转化为 Number 后的值和另一个进行比较。(false为0，true为1)
5. 如果 Type(x)或 Type(y)中有一个是 Object，则会比较 x == toPrimitive(y) 或 toPrimitive(x) == y，即将引入类型转换为基本类型再进行比较，否则返回 false。

## 对象和非对象之间的比较
关于引用类型（对象/函数/数组）和基本类型（字符串/数字/布尔值）之间的比较，从上面的比较规则中我们看到，是将引用类型转化为基本类型再进行比较：
1. 如果 Type(x) 是字符串或数字，Type(y) 是对象，则返回 x == toPrimitive(y) 的结果。
2. 如果 Type(x) 是对象，Type(y) 是字符串或数字，则返回 toPrimitive(x) == y 的结果。

> 什么是 toPrimitive() 函数？  

***应用场景***：在 JS 中，如果想要将引用类型转换成基本类型时，实质就是调用对象的 ***valueOf*** 和 ***toString***方法，也就是拆箱转换。

***函数结构***： toPrimitive(input, preferedType)

***参数解释***：
- input 是输入的值，即要转换的对象，必选。
- preferedType 是期望转换的基本类型，他可以是字符串，也可以是数字。选填，默认为 number。

#### 过程说明：
1. 如果 input 是原始值，则直接返回这个值，
2. 如果 input 是对象，调用 input.valueOf()，如果结果是原始值，返回结果；
3. 否则，调用 input.toString()，如果结果是原始值，返回结果；
4. 否则，抛出错误。如果转换的类型是 string，则步骤2和3交换执行顺序。

valueOf 和 toString 的优先级：
1. 进行对象转换时，优先调用 toString 方法，若没有重写 toString，将调用 valueOf 方法，如果两个方法都没有重写，则按 Object 的 toString 输出；
2. 进行强转字符串类型时将优先调用 toString 方法，强转为数字时优先调用 valueOf 方法。
3. 在有运算操作符的情况下，valueOf 的优先级高于 toString。

### 解决标题问题
由上分析可知，在 a 为对象时，可以通过重写 a 的 valueOf 和 toString 方法来让标题的等式成立。

```JavaScript
const a = {
  value: 0,
  valueOf() {
    a.value ++;
    return a.value;
  }
};

console.log(a == 1 && a == 2 && a == 3) // true

```
或者：
```JavaScript
const a = {
  value: 0,
  toString() {
    a.value ++;
    return a.value;
  }
}

console.log(a == 1 && a == 2 && a == 3) // true

```
给对象 a 设置一个属性 value 初始为0，并修改其 valueOf 或 toString 方法，当执行*a == 1 && a == 2 && a == 3*时，每次等式比较都会触发 valueOf 或 toString 方法，就会执行 value ++，同时把新的 value 值用于等式比较，三次等式判断时，value 的值分别为1、2、3，所以等会会成立。

本篇完

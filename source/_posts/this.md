---
title: this、call 和 apply
tags: JS this指向
date: 2018-09-05
---


> Js中的this总是指向一个对象，而具体指向哪一个对象是在运行时基于函数的执行环境动态绑定的，而非函数被声明时的环境。

this的指向大致可以分为以下4种。
* 作为对象的方法调用（当函数作为对象的方法被调用时，this指向该对象）
```
const obj = {
    a: 1,
    getA: () => {
        console.log(this === obj); // true
        console.log(this.a === 1); // true
    },
};
obj.getA();
```
* 作为普通函数调用
当函数不作为对象的属性被调用时，也就是我们常说的普通函数方式，此时的this总是指向全局对象。在浏览器的Js中，这个全局对象是window对象。
```
window.name = 'globalName';
const getName = () => console.log(this.name);
getName(); // 输出globalName
```
* 构造器调用
Js中没有类，但是可以从构造函数中创造对象，同时也提供了new运算符，使得构造器看起来更像一个类。
除了宿主提供的一些内置函数，大部分Js函数都可以当做构造器使用。构造器的外表跟普通函数一模一样，它们的区别在于被调用的方式。当用new运算符调用函数时，该函数总会返回一个对象，通常情况下，构造器里的this就指向返回的这个对象。
```
const myClass = () => {
    this.name = 'yuehun';
};
const obj = new myClass();
console.log(obj.name); // 输出yuehun
```
但当用new调用构造器时，还要注意一个问题，如果构造器显式地返回了一个object类型的对象，那么此次运算结果最终会返回这个对象，而不是我们之前期待的this。
```
const myClass = {
    this.name = 'john';
    return {
        name: 'yuehun',
    };
};
const obj = new myClass();
console.log(obj.name); // 输出yuehun
```
如果构造器不显式地返回任何数据，或者是返回一个非对象类型的数据，就不会造成上述问题
```
const myClass = () => {
    this.name = 'yuehun';
    return 'john';
};
const obj = new myClass();
console.log(obj.name); // 输出yuehun
```
* Function.prototype.call或Function.prototype.apply调用
跟普通的函数相比，用这两种方式能动态地改变传入函数的this
```
const obj1 = {
    name: 'john',
    getName: () => this.name,
};
const obj2 = {
    name: 'yuehun',
};
console.log(obj1.getName()); // 输出john
console.log(obj1.getName.call(obj2)); // 输出yuehun
```
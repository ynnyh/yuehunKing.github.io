---
title: 盲点
date: 2018-12-11
tags: 查缺补漏
category: 查缺补漏
---

### Object.assign()
该方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
```
const object1 = {
  a: 1,
  b: 2,
  c: 3
};

const object2 = Object.assign({c: 4, d: 5}, object1);

console.log(object2.c, object2.d);
// expected output: 3 5

```
Object.assign(target, sources)
target： 目标对象
source：源对象
如果目标对象中的属性具有相同的键，则属性将被源中的属性覆盖。后来的源的属性将类似地覆盖早先的属性。

### 上述方法可以实现克隆且不影响源对象值，下面展示的是相反的效果
```
let a = [1, 2, 3];
let b = a;
b.push(4);
console.log(a); // 1, 2, 3, 4
console.log(b); // 1, 2, 3, 4
```
从上面能看出来，更新数组b会同时改变数组a，因为对象和数组是引用数据类型，这意味着这样的数据类型实际上并不保存值，而是存储指向存储单元的指针。
将a赋值给b，其实我们只是创建了第二个指向同一存储单元的指针。
---
title: toString用法
tags: 每日一题
category: 每日一题
date: 2018-09-08
---


### 将给定的两个数字相加，将最后的值转为二进制，并且为字符串（可以先转换后相加也可以先相加后转换）
```
function addBinary(a, b) {
return (a + b).toString(2);
}
// 此处使用了Number.prototype.toString(radix)方法，其中radix指定要用于数字到字符串的转换的基数（2-36），不写则默认为10
```
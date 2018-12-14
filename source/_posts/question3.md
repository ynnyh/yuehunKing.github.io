---
title: 解析字符串
tags:
    - 每日一题
    - 正则
category:
    - 每日一题
    - 正则
date: 2018-09-09
---


### 完成一个 extractStr 函数，可以把一个字符串中所有的 : 到 . 的子串解析出来并且存放到一个数组当中，例如：
```aidl
extractStr('My name is:Jerry. My age is:12.') // => ['Jerry', '12']
```
注意，: 和 . 之间不包含 : 和 .。也即是说，如果 ::abc..，则返回 ['abc']。
```aidl
const extractStr = (str) => {
  let reg = /:([^\.:]*)\./g;
  let matchArr = str.match(reg) === null ? [] : str.match(reg) ;
  let result = matchArr.map((el) => {
    return el.slice(1, -1); // slice从start索引值开始截取，不包含end值
  });
  return result;
}
```
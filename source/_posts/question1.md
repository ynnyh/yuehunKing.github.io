---
title: 转换指定格式
tags: 每日一题
date: 2018-09-07
---


### 以指定规则将指定字符串转换为指定格式如下
```
// 将'AAA'转换为'TTT'
// 将'TTT'转换为'AAA'
// 将'ATTGC'转换为'TAACG'

// 解法1
function DNAstrand(dna){
    const compire = {
	    A: 'T',
		T: 'A',
		C: 'G',
		G: 'C',
	};
	return dna.split('').map(i => compire[i]).join('');
}
// 解法2
function DNAstrand(dna){
    return dna.replace(/./g, i => ({A:'T',C:'G',G:'C',T:'A'}[i]));
}
// 解法3
function DNAstrand(dna) {
    return dna.replace(/./g, i => 'ACGT'['TGCA'.indexOf(i)]);
}
```
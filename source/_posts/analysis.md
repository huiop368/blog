title: 算法--设计分析
date: 2015-11-04 23:19:23
tags: algorithm
---

[31,-41,59,26,-53,58,97,-93,-23,84] 从这组向量中算出，向量与向量相加的最大值。

这里可以给出2种算法，并逐一对算法的运行速率进行验证。

首先会想到的是：
*1 + *2  ?  *1 + *2 + *3 ？.......
*2 + *3  ?  *2 + *3 + *4 ? .......
..........

以此类推， 找出里面最大的和。

算法:
 1)
```javascript
var max = 0,sum, ret = [31,-41,59,26,-53,58,97,-93,-23,84],len = ret.length;
for(var i = 0;i<len;i++){
    sum = 0;
    for(var j = i;j<len;j++){
        sum += ret[j];
        max = Math.max(max,sum);
    }
}
console.log(max);  // 187
``` 
此算法的时间数量级是 n的2次方


2) 最快运算法:
```javascript
var ret = [31,-41,59,26,-53,58,97,-93,-23,84],len = ret.length,i=0, 
     w = 0,max = 0,dd;
for(;i<len;i++){
    dd = ret[i];
    w = Math.max(w+dd,0);
    max = Math.max(max,w);
}
```
此算法的时间的数量级是线性的，fastest。
此算法最主要的就是w这个变量，扫描整个向量，i-1个向量和一旦小于0，将重置为0，并从第i个向量又向下扫描。

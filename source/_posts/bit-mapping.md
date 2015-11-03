title: 算法--位图法
date: 2015-11-03 22:21:34
tags: algorithm
---

1-n个整数，里面缺少了（打个比方）3个数字，请找出其中缺失的3个数字.

```javascript
var n = 10,m = 3,i,
      arr = [2,5,1,8,6,9,4],    //缺失的数据源
      ret = new Array(11),
      result = [];
for(i = 0;i<n-m;i++){
      ret[arr[i]] = 1;
}

console.log(ret);        
//[undefined × 1, 1, 1, undefined × 1, 1, 1, 1, undefined × 1, 1, 1, undefined × 1] 

for(i = 0,len = ret.length;i<len;i++){
     !ret[i] && result.push(i);
}
result.shift(0);

console.log(result);       //[3,7,10]
```

这里位图法，适合1-n个整数间无重复出现，然后利用数组索引的无重复序列号进行对比，找出索引中值不等于1的就是缺失的数字。

此方法简洁且运行相当高效。


由此问题其实可以引出一个题目~!!!!!

在1-n个整数间，随机生成K个**不同的**整数。

这里给个算法～！
```javascript
var arr = [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19],n = 20,ret = [],k =5,j,i;
for(j = 0; j<k; j++){
    i = Math.floor(Math.random()*arr.length);   //生成0-19的随机数,这里主要用来对应数组索引
    ret.push(arr[i]);
    arr.splice(i,1);
}

console.log(ret);    //[8, 4, 14, 18, 9]
```

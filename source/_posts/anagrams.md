title: 算法--变位词
date: 2015-11-04 22:54:51
tags: algorithm
---

给一组单词pans,pots,opt,snap,stop,stops.
（变位词：组成单词的各字母，进行位置的调换变成另一字母 例如：pans 和 snap）

找出变位词，使得单词组分为  pans snap，opt，pots stop tops   3组变位词

**原理：组成单词的字母不管位置怎么变，字母的数量都是一样的， 一旦将组成这些单词的字母排序后，会发现这些单词都变成一样的了。**

```javascript
var ret = ['pans','pots','snap','stop','stops'],i = 0,dd,dl,arr = [],o = {};

for(len = ret.length;i<len;i++){
    dd = ret[i];
    dl = dd.split('').sort().join(''); 
    // 将字母重新排序，生成一个key
    o[dl] ? o[dl].push(dd) : (o[dl] = [dd]);
   // 将排序后具有相同key的组成一个数组
}
console.log(o);
/*
o = {
'anps': ['pans', 'snap'],
'opt' : ['opt'],
'opst' : ['pots', 'stop', 'tops']
}
*/

//如果需要对输出的单词组排序的话
for(var k in o){
    arr.push(k);
}

arr.sort();
for(j = 0,len = arr.length;j<len;j++){
    console.log(o[arr[j]]);
}
```

title: YDKJS--Types & Grammar(译)
date: 2015-11-29 21:25:42
tags: YDKJS
---

## 第二章:值

数组，字符串和数字是任何语言最基本的类型，但是Javascript有一些不同的性质也许会使你又爱又恨。

让我们来看几个内置的JS类型，探索一些怎样能够跟好的正确理解他们的行为。


### 数组

与其他强类型语言相比，Javascript的`arrays`可以包含任意类型的值，无论是字符串，数字，对象，甚至是数组本身。

```js
var a = [ 1, "2", [3] ];

a.length;       // 3
a[0] === 1;     // true
a[2][0] === 3;  // true
```

你不需要预先设置数组的size, 你可以仅仅声明他们并且增加值。

```js
var a = [ ];

a.length;   // 0

a[0] = 1;
a[1] = "2";
a[2] = [ 3 ];

a.length;   // 3
```

**Warning** : 用`delete`操作符运算`array`时，会从数组中移除一个位置, 但是即使你将最后一个元素移除了，数组也不会更新`length`属性。

创建“稀”数组的时候一定要小心(创建了空的或丢掉了位置)

```js
var a = [ ];

a[0] = 1;
// no `a[1]` slot set here
a[2] = [ 3 ];

a[1];       // undefined

a.length;   // 3
```

这个时候空位置的值是`undefined`, 但是这个与`a[1] = undefined`还是不一样的。

`array`是数字索引，但是奇怪的事情是它同样也是对象,能够有字符串关键字(但是他们是不计算在数组的`length`里)

```js
var a = [ ];

a[0] = 1;
a["foobar"] = 2;

a.length;       // 1
a["foobar"];    // 2
a.foobar;       // 2
```

尽管如此，要小心字符串值作为属性名，当这个字符串可以转换成`number`时，它会试图用数字索引代替字符串属性名。

```js
var a = [ ];

a["13"] = 42;

a.length; // 14
```

一般来说，用数组来存储字符串属性名是不太建议的。 用`Object`来代替这种做法，`array`就严格的保存数字索引值。


### 类数组

肯定遇到过几次需要将类数组转换成真正的数组，这样你就能够调用数组的方法(indexOf, concat, forEach, etc...)

例如Dom集合就是类数组，而不是真正的数组。另一个例子就是function中的agument对象, 我们可能都会在某种场景下将这两种类数组转换成真实的数组。

一个非常公共和方便都做法就是用`slice(..)`方法。

```js
function foo() {
    var arr = Array.prototype.slice.call( arguments );
    arr.push( "bam" );
    console.log( arr );
}

foo( "bar", "baz" ); // ["bar","baz","bam"]
```

像上面的代码片段提到的，如果`slice`方法不带参数的话，将会重新克隆数组。

在ES6里，有个内置的方法`Array.from(..)`是用来做同样事情的。

```js
...
var arr = Array.from( arguments );
...
```

**Note:** `Array.from(..)`还有一些其他强大的功能，将会在后续章节提到。


### 字符串



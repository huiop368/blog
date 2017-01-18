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

<!--more-->

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

这里有个比较通常的说法，字符串有着数组的特性。意识到Javascript中字符串不是真正的和数组特性一样是非常重要的，他们仅仅是表面上又点相似而已。

例如：让我们考虑下这两个值

```js
var a = "foo";
var b = ["f","o","o"];
```

字符串与数组及类数组有一点点的相似，像上面。 他们都有一个`length`属性，一个`indexOf(..)`方法和`concat(..)`方法。

```js
a.length;                           // 3
b.length;                           // 3

a.indexOf( "o" );                   // 1
b.indexOf( "o" );                   // 1

var c = a.concat( "bar" );          // "foobar"
var d = b.concat( ["b","a","r"] );  // ["f","o","o","b","a","r"]

a === c;                            // false
b === d;                            // false

a;                                  // "foo"
b;                                  // ["f","o","o"]
```

所以，他们本质上来说都是数组性？

```js
a[1] = "O";
b[1] = "O";

a; // "foo"
b; // ["f","O","o"]
```

Javascript中`string`是不可变的，而`array`是易变的。此外，`a[1]`字符位置并不能一直有效。在IE的一些老版本中是不支持这种语法的。作为替代方案可以使用`a.charAt(1)`。

`string`方法都不会改变它本身，而都是return新的`string`对象，但`array`的方法都会改变数组本身。

```js
c = a.toUpperCase();
a === c;    // false
a;          // "foo"
c;          // "FOO"

b.push( "!" );
b;          // ["f","O","o","!"]
```

然而，很多`array`的方法对`string`来说都是非常有用的，不过我们可以把`array`的方法带给`string`.

```js
a.join;         // undefined
a.map;          // undefined

var c = Array.prototype.join.call( a, "-" );
var d = Array.prototype.map.call( a, function(v){
    return v.toUpperCase() + ".";
} ).join( "" );

c;              // "f-o-o"
d;              // "F.O.O."
```

让我们再来看一个例子:

```js
a.reverse;      // undefined

b.reverse();    // ["!","o","O","f"]
b;              // ["!","o","O","f"]
```

不幸的是，上面介绍的方法对`reverse`不适用，因为`string`是不可变的。

```js
Array.prototype.reverse.call( a );
// still returns a String object wrapper (see Chapter 3)
```

让我们来看看`string`与`array`之间的转换。

```js
var c = a
    // split `a` into an array of characters
    .split( "" )
    // reverse the array of characters
    .reverse()
    // join the array of characters back to a string
    .join( "" );

c; // "oof"
```

**Warning:** 这个方法对于复杂的`string`(unicode)是不生效的。


### 数字

#### Placeholder


### 特殊值

这里有几个比较特别的值需要开发者注意的, 并且适当地使用。

#### 空值

对于`undefined`类型来说，这里只有唯一的一个值:`undefined`. 对于`null`类型来说，也只有唯一的一个值:`null`。

`undefined`和`null`经常被认为是空值或不存在的值. 然而一些开发者倾向于细小的差别来区分他们. 例如:

* `null`是一个空值
* `undefined`是一个丢失的值

或者

* `undefined`还没有值
* `null`有值，但这个值什么都不是

Regardless of how you choose to "define" and use these two values, null is a special keyword, not an identifier, and thus you cannot treat it as a variable to assign to (why would you!?). However, undefined is (unfortunately) an identifier. Uh oh.
(怎么翻都觉得不太对)


#### undefined

在non-`strict`的模式下，给global的`undefined`赋值是可能的。

```js
function foo() {
    undefined = 2; // really bad idea!
}

foo();
```

```js
function foo() {
    "use strict";
    undefined = 2; // TypeError!
}

foo();
```

在non-`strict`和`strict`模式下, 尽管如此，你都能够创建一个局部变量`undefined`. 但是这同样是非常糟糕的.

```js
function foo() {
    "use strict";
    var undefined = 2;
    console.log( undefined ); // 2
}

foo();
```

** Friends don't let friends override `undefined` ** . Ever.


### Void 操作符

`void ___`这表达式总是返回`undefined`, 它并不会改变现有的值，仅仅是没有任何值能够从这个表达式返回.

```js
var a = 42;

console.log( void a, a ); // undefined 42
```

按照惯例，为了得到`undefined`值，一般我们会用`void 0`(尽管 `void true`和一些其他的void表达式做的是同样的事情), 他们之间是没有什么特别的区别的.

但是在一些情况下，`void`操作符还是比较有帮助的.

例如:

```js
function doSomething() {
    // note: `APP.ready` is provided by our application
    if (!APP.ready) {
        // try again later
        return void setTimeout( doSomething, 100 );
    }

    var result;

    // do some other stuff
    return result;
}

// were we able to do it right away?
if (doSomething()) {
    // handle next tasks right away
}
```

这里, `setTimeout(..)`函数返回一个数字(不同的timer标识，用来取消它), 但是我们希望`if`语句能够有个否定的值，所以用`void`执行了一下.

很多开发者更喜欢把动作分开，不用`void`而达到同样的效果.

```js
if (!APP.ready) {
    // try again later
    setTimeout( doSomething, 100 );
    return;
}
```

一般来说，如果你发现用`undefined`替代其他值能更有帮助的话，那么用`void`操作符. 也许在你的程序里这样的情况不是非常多的，但在一些极少的情况下你一定会需要它的，它会非常有帮助的.


### 特殊的数字
数字类型包含了几个特别的值，让我们详细的看看。

### 不是数字
但两个操作数都不是`number`并进行数学运算的时候，结果会得到一个无效的`number`---`NaN`值.

`NaN`通常被认为“不是一个数字”, 但是将它认为是“无效的数字”也许会比"不是数字"更加精确。

例如:

```js
var a = 2 / "foo";      // NaN

typeof a === "number";  // true
```



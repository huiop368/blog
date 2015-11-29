title: YDKJS--Types & Grammar(译)
date: 2015-11-16 22:37:15
tags: YDKJS
---

## 第一章:类型

大多数开发者都会说一门动态语言是没有类型的。让我们看看ES5.1里面是怎么描述这个话题的。

如果你是强类型语言的忠实粉丝的话，你也许会反对type这个词的用法, 在这些语言里type更加意味着全部,对JS而言。

有些人说JS没有权利拥有type, 而应该被替代为tags或者subTypes

呸！我们将引用一个粗糙的定义: 类型是一种内在的特性，这个特性是一个值行为的唯一标识, 并且与其他值区分开来。

换句话来说，就是如果解释器和开发者都区别对待42和‘42’, 那么这两个值就拥有不同的类型--number和string。


### 内置类型 

Javascript 定义了7种内置类型:

* `null`
* `undefined`
* `boolean`
* `number`
* `string`
* `object`
* `symbol` -- added in ES6!

**Note:** 除了object类型外，其他类型都称为原始类型。


```js
typeof undefined     === "undefined"; // true
typeof true          === "boolean";   // true
typeof 42            === "number";    // true
typeof "42"          === "string";    // true
typeof { life: 42 }  === "object";    // true

// added in ES6!
typeof Symbol()      === "symbol";    // true
```

这里列出来的6个值都拥有相符的类型，返回相同名字的字符串值。
`Symbol`是一种在ES6里的新类型，将会在Chapter3里覆盖到。

也许你注意到了， 这里将null移除在列表外了，因为null是一个特殊情况。

```js
tyoeof null === "object";  // true
```

如果返回的是`"null"`, 也许会更好。但是这个原始的bug已经在JS里存在了20多年，而且是不会被修复的，因为有太多的现存的网站依赖于这个“BUG”,修复它的话只会带来跟多的bug。

如果你想测试`null`的类型，你需要一个复合条件:

```js
var a = null;

(!a && "object" === typeof a);  // true

```


那么第7个`typeof`返回的字符串是？

```js
typeof function a(){ /* .. */} === "function"; // true
```

这就很容易的让人想到在JS里`function`是第一型的，尤其是通过`typeof`这个行为。
经管如此，如果仔细分析，你会发现其实它是`object`的子类型。具体来说，`function`是一种可执行的对象，内部拥有`[[Call]]`属性，允许被执行。

事实上function作为对象来处理是非常有用的。 最重要的一点是，它可以拥有属性。 例如:

```js
function a( b, c ){
    /* .. */
}
```

这个function对象有一个`length`属性， 它所拥有的形参数。

```js
a.length;  // 2
```

因为你定义了两个形参(`b` and `a`), 所以function的length属性值是`2`.


那么数组又是什么呢？ 他们也是特别的类型吗?

```js
typeof [1,2,3] === "object"; // true
```

仅仅是对象，但是你也可以把它当做是object的子类型(see Chapter 3).


<!--more-->


### 值&类型
在Javascript里，变量(variables)是没有类型的 --- **值有类型**。变量能够持有任何类型的值在任何时候。
在JS中是没有强制类型的，一个变量在一种状态中可以持有`string`, 下一个就可能持有`number`。

值`42`有它本身的类型`number`, 并且它的类型是不能改变。像`"42"`字符串类型，可以通过类型转换(coercion)，由数字类型`42`转换得到。

如果你用`typeof`作用于一个变量，它不会问"这个变量是什么类型?", 因为变量是没有类型的在JS里。 它应该是问"在变量里的这个值是什么类型?"

```js
var a = 42;
typeof a; // "number"

a = true;
typeof a; // "boolean"
```

`typeof`操作符永远都返回一个字符串。

```js
typeof typeof 42; // "string" 
```



### undefined vs "undeclared"

变量可以没有值，但事实上这时候它的值是`undefined`. 对这种变量执行`typeof`时将得到`"undefined"`:

```js
var a;

typeof a; // "undefined"

var b = 42;
var c;


// later
b = c;

typeof b; // "undefined"
typeof c; // "undefined"
```

对于大多数开发者来说"undefined"和"undeclared"是同义词, 尽管如此在JS中，他们是两个完全不同的概念。

```js
var a;

a; // undefined
b; // ReferenceError: b is not defined
```

就像你看到得一样，"b is not defined"会让人非常容易的与"b is undefined"混淆。再重复一遍，"undefined"与"is not defined"是完全不同的概念。如果浏览器可以说"b is not found"或者"b is not declared",也许是可以减少疑惑的。

这里还有一个与`typeof`相关的容易产生困惑的行为。

```js
var a;

typeof a; // "undefined"
typeof b; // "undefined"
```

`typeof`都会返回`"undefined"`, 即使是"undeclared"(为声明)的变量。注意这里是不会报错的，当我们执行`typeof b`的时候。这是`typeof`的一种特别的保护措施。

就像上面提到的null类似，如果`typeof`未声明的变量，返回"undeclared"可能会更有帮助。


### `typeof` Undeclared

尽管如此，对于浏览器端的JS来说这个保护措施是比较有用的，因为JS文件加载进来的变量是共享global这个命名空间的。

举个简单的例子，想象一下在你的环境中又个"debug"模式，它被一个全局变量`DEBUG`控制着。
在一个"debug.js"文件里包含了一个全局变量`var DEBUG = true`, 这个文件只会在开发环境中，不会在线上环境。

然而，你必须关心怎么判断`DEBUG`这个变量是否存在。

```js
// oops, this would throw an error!
if (DEBUG) {
    console.log( "Debugging is starting" );
}

// this is a safe existence check
if (typeof DEBUG !== "undefined") {
    console.log( "Debugging is starting" );
}
```

这种检查不仅仅对这种变量有用，如果你在检查一个内置的API，你也许会发现这很有用。

```js
if ( typeof atob === "undefined" ) {
    atob = function(){ /*..*/ };
}
```

还有一种方法不需要用`typeof`去检查变量，在浏览器里面所有的全局变量也是`global`的属性，所以上面的检验也能像下面这样执行:

```js
if (window.DEBUG) {
    // ..
}

if (!window.atob) {
    // ..
}
```

不像未声明的变量那样会抛出异常，如果你访问一个`window`不存在的属性是不会报错的。

另一方面,手动的设置全局变量为`window`的引用是我们应当尽量避免的，特别是当你的代码需要运行在多种JS环境中(不仅仅是浏览器，也可以是node环境),这个时候global就不一定是`window`了。

我们想象一个公共函数，你想其他人可以黏贴到他们的代码中去，这个时候你会想要检查这个方法是否已经被定义过了。

```js
function doSomethingCool() {
    var helper =
        (typeof FeatureXYZ !== "undefined") ?
        FeatureXYZ :
        function() { /*.. default feature ..*/ };

    var val = helper();
    // ..
}
```

`doSomethingCool()`检查了一个叫`FeatureXYZ`的方法，如果找到了就用找到的，如果没有找到，就用它自己的。现在，如果有人把这个函数引用到他的模块中, 对于他们的模块定义或者没定义`FeatureXYZ`都是安全的。

```js
// an IIFE (see "Immediately Invoked Function Expressions"
// discussion in the *Scope & Closures* title of this series)
(function(){
    function FeatureXYZ() { /*.. my XYZ feature ..*/ }

    // include `doSomethingCool(..)`
    function doSomethingCool() {
        var helper =
            (typeof FeatureXYZ !== "undefined") ?
            FeatureXYZ :
            function() { /*.. default feature ..*/ };

        var val = helper();
        // ..
    }

    doSomethingCool();
})();
```

一些开发者可能会比较喜欢一种设计模式叫"依赖注入", 这里就是将`FeatureXYZ`在外面定义，替换为依赖参数传入。

```js
function doSomethingCool(FeatureXYZ) {
    var helper = FeatureXYZ ||
        function() { /*.. default feature ..*/ };

    var val = helper();
    // ..
}
```

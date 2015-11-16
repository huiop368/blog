title: YDKJS--Types & Grammar(译)
date: 2015-11-16 22:37:15
tags: YDKJS
---

## Chapter1:Types

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



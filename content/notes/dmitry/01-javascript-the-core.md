---
title: JavaScript. The Core.
weight: 10
---

{{< feynman >}}

{{% button href="http://dmitrysoshnikov.com/ecmascript/javascript-the-core/" %}}JavaScript. The Core.{{% /button %}}

## 1. 一个对象

ECMAScript 是一种面向对象的语言，对象是这门语言的基础。那么，什么是对象，它是如何定义的？

一个对象是很多属性的组合，除此之外，它还有一个名为 prototype 的子对象。prototype 要么是一个对象，要么是一个 null 值。

对象的原型由内部的 `[[Prototype]]` 引用。但实际使用时，是另一种形式——`__<internel-property>__` 下划线标记，特别是对于原型来说：`__proto__`。

```js
var foo = {
  x: 10,
  y: 20
}
```

如下图所示，对象 foo 由三部分构成——x，y，`__proto__`。前两个是公开的，最后一个是隐藏的。而 `__proto__` 对应之后的 foo 的原型。

![](/images/javascript-the-core-0.svg)

这些原型的作用是什么？原作者认为「原型链（prototype chain）」能回答这个问题。

## 2. 一个原型链

一个对象有名为 prototype 的子对象，而这个子对象又会有自己的 prototype 子对象，由此形成一条围绕 prototype 的原型链条。

原型链条能够实现继承和分享属性。

什么是继承？继承就是，对象能够从自己的原型获得属性，而该原型又能从它的原型获得属性。

如果我们有两个对象，且这两个对象只有少部分属性不同，那么我们何不新建一个对象，将相同的属性包含进去，再通过构造函数新建不同的两个对象呢？

在一些基于类的系统中，这种代码复用方式被称为「基于类的继承」。

ECMAScript 有自己的代码复用逻辑，跟类并无区别。这种继承方式由原型链实现，称为「delegation based inheritance」（特定于 ECMAScript 的话，可称为「基于原型的继承」）。

```js
var a = {
  x: 10,
  calculate: function (z) {
    return this.x + this.y + z;
  }
};
var b = {
  y: 20,
  __proto__: a
};
var c = {
  y: 30,
  __proto__: a
};

b.calculate(30);
c.calculate(40);
```

如代码所示，对象 b 和 c 从 a 处继承了 calculate() 函数。

很简单的规则：如果对象自身并没有相应的属性或方法，那它就会在原型链中寻找。

参考资料：

1. <https://en.wikipedia.org/wiki/Prototype-based_programming>
2. <https://en.wikipedia.org/wiki/Class-based_programming>

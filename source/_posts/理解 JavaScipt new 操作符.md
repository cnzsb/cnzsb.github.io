title: '理解 JavaScipt new 操作符'
date: 2016-10-19 18:10:53
tags: JavaScript

---

在《JavaScript 高级程序设计》中提到使用 `new` 操作符会经历 4 个步骤：

>(1) 创建一个新对象；
>
>(2) 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）；
>
>(3) 执行构造函数中的代码（为这个新对象添加属性）；
>
>(4) 返回新对象。

<!-- more -->

假设我们有一个构造函数 `Foo` 。这里要理解在创建了一个新函数时，就会根据一组特定的规则为该函数创建一个 `prototype` 属性，这个属性指向函数的原型对象。而在默认情况下，所有原型对象都会自动获得一个 `constructor` 属性，这个属性包含一个指向 `prototype` 属性所在函数的指针。

```javascript
    function Foo(name) {
        this.name = name
    }

    Foo.prototype.getName = function() {
        console.log(this.name)
    }
```

然后通过 `new` 方法创建实例 `bar`。我们知道，当调用构造函数创建一个新实例后，该实例的内部将包含一个指针指向构造函数的原型对象。ECMA-262 第 5 版管这个指针叫 `[[Prototype]]` ，但Firefox、Chrome、Safari在每个对象上都支持一个属性 `__proto__` ，也就相当于这个属性。重要理解的是**这个连接存在于实例与构造函数的原型对象之间，而不是存在于实例与构造函数之间**。

```javascript
    var bar = new Foo('bar')
    console.log(bar)    // Foo {name: "bar"}
    bar.getName()   // bar
```

现在根据以上的规则，我们手动来实现一个 `new` 的过程。

```javascript
    // 1. 首先创建一个空对象
    var baz = new Object()

    // 2. 把这个对象的 [[Prototype]] （__proto__）指向 Foo 的 prototype
    Object.setPrototypeOf(baz, Foo.prototype)
    // 在控制台中也可以这样写：
    // baz.__proto__ = Foo.prototype

    // 3. 传递 FOO 的属性及作用域
    Foo.call(baz, 'baz')

    // 4. 验证
    baz.getName()   // baz
```
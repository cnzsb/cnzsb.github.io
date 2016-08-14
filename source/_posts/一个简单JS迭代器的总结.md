title: '一个简单JS迭代器的总结'
date: 2015-12-18 11:29:34
tags: JavaScript

---

近来一直在[codewars](http://www.codewars.com)上练习JS，遇到了一个关于迭代的问题[Function iteration](http://www.codewars.com/kata/54b679eaac3d54e6ca0008c9/train/javascript)。原题目如下：

<!-- more -->

> The purpose of this kata is to write a higher-order function which is capable of creating a function that iterates on a specified function a given number of times. This new functions takes in an argument as a seed to start the computation from.
> 
> For instance, consider the function `getDouble`. When run twice on value `3`, yields `12` as shown below.
>
> ```
 getDouble(3) => 6
 getDouble(6) => 12
 ```
> 
> Let us name the new function `createIterator` and we should be able to obtain the same result using `createIterator` as shown below:
> 
> var doubleIterator = createIterator(getDouble, 2); // This means, it runs *getDouble* twice
doubleIterator(3) => 12
> 
> For the sake of simplicity, all function inputs to createIterator would be functions returning a small number and number of iterations would always be integers.

开始的时候一直没有理解如何复用函数的返回值，索性在`createIterator(func, n)`中直接`return`函数`func`了。但是后来发现要验证的答案格式如下。

```
var getDouble = function(n) {
    return n + n;
};

// Running the iterator for once
var doubleIterator = createIterator(getDouble, 1);
doubleIterator(3); // => 6
doubleIterator(5); // => 10

// Running the iterator twice
var getQuadruple = createIterator(getDouble, 2);
getQuadruple(2); // => 8
getQuadruple(5); // => 20
```

一步步去思考，这里假设为需要运行2次。则最终`getQuadruple`需要内部调用函数2次，因此简单的就是使用循环来达到目的。索性先去根据理解写出想法。

```
var createIterator = function (func, n) {
  // TODO: Write code here to return a function 
  // that executes *func*, *n* times on a supplied input
    return function(arg){
        for(var i = 0; i < n; i++) {
            func(arg);
        }
    };
};
```

写到这里，怎么都不明白如何去写闭包中的返回值了。苦苦挣扎一下午还是参考了别人的思路，于是得出了正确代码。

```
var createIterator = function (func, n) {
  // TODO: Write code here to return a function 
  // that executes *func*, *n* times on a supplied input
    return function(arg){
        for(var i = 0; i < n; i++) {
            arg = func(arg);
        }
        return arg;
    };
};
```

从这里可以看出，在下一次循环所执行函数中的参数是上一次执行函数后得到的结果，因此便达到了迭代的目的，最终返回这个参数即可。
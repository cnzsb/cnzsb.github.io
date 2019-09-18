title: 'callee、caller、call 和 apply'
date: 2015-10-16 18:27:01
tags: JavaScript

---

## 1. callee

`callee`，返回正在执行的函数本身的引用，是`arguments`的一个属性。只有在函数执行时有效；有一个`length`属性，可用于获得形参的个数，来比较是否和实参个数一致，即比较`arguments.length == arguments.callee.length`；还可用于递归匿名参数；可以用来消除耦合。

<!-- more -->

```javascript
// 不用()()立即执行时，则无效。
(function outer() {
	function center(){
		console.log(arguments.callee);		// > center()
		function inner(){
			console.log(arguments.callee);	// > inner()
		}
		inner();
	}
	center();
})()
```

## 2. caller

`caller`，返回一个对函数的引用，该函数调用了当前的函数。只有在函数执行时有效；如果函数由顶层调用，那么返回`null`。

```javascript
// 不用()()立即执行时，则无效。
(function outer() {
	function center(){
		// 调用当前作用域，此时center.caller指向outer
		// center.caller可用arguments.callee.caller表示
		console.log(center.caller);			// > outer
		console.log(outer.caller);			// > null
		function inner(){
			console.log(inner.caller);		// > center()
			console.log(center.caller);		// > outer()
			console.log(outer.caller);		// > null
		}
		inner();
	}
	center();
})()
```

## 3. call&apply

每个函数都包含两个非继承的方法，`call`和`apply`。他们的用途都是在特定的作用域中调用函数，获取了另一个对象的方法，并继承对象的属性，只是接收的参数格式不同。它们的重要作用在于能够扩充函数的作用域，而且对象不需要与函数有任何的耦合。

- `function.call(obj, arg1, arg2, ...)`
- `function.apply(obj, [param1, param2, ...])`
- `obj`将代替函数中的`this`对象
- `call`中第二个参数是一个参数列表
- `apply`中第二个参数是一个数组，用来传参给函数中的`arguments`

```javascript
function Box(name, size){
	this.name = name;
	this.size = size;
}

function Boxes(name, size, number){
	Box.call(this, name, size);
	// Box.apply(this, arguments);
	this.number = number;
}

var mybox = new Boxes('mybox', '5*5*5', 1001);
// boxes中并未给name和size属性赋值，但是还是显示了预期结果，否则应为undefined
alert('name: ' + mybox.name + '\nsize: ' + mybox.size + '\nnumber: ' + mybox.number);
```

使用时，如果希望输出结果的参数位置改变，如为`Boxes（size, name, number）`，则使用`call`更方便`Box.call(this, size， name)`。而需要按照顺序对应的情况下，使用`apply`的数组参数`arguments`则更方便。

## 4. 其他一些高级用法

对于`apply`和`call`，还有一些特别的用法，鉴于它两并无太大区别，这里以`apply`为例。个人总结该用法如下，非标准表达：

```
Fun.apply(Obj, arguments);
```

解释为，Obj对象调用Fun内部提供的方法，对arguments参数进行操作，此处，Obj对象可为this，null等。此方法颇适用于需要传入列表而非数组表达的参数时，即`param1, parram2, param3, ...`而非`[param1, param2, param3, ...]`时。

以下三个示例，分别展示了表达式中Obj，Fun和arguments的含义：

```javascript
var arr = [1, 5, 3];
var maxNum1 = Math.max.apply(null, arr);
var maxNum2 = Math.max.apply(maxNum, arr);
console.log(maxNum1);
console.log(maxNum2);
// 结果都为5，这里更方便理解apply接收的第一个参数的含义
```

```javascript
function Arr(){
	var vals = new Array();
	
	//此处表达vals对象调用了apply前面所示数组的push方法，等价于下面注释掉的表达方法
	vals.push.apply(vals, arguments);
	// Array.prototype.push.apply(vals, arguments);

	return vals;
}
var arr = new Arr(1,2,3);
console.log(arr);  // [1, 2, 3]
```

```javascript
function Box(name, size){
    this.name = name;
    this.size = size;
}

function Boxes(){
    Box.apply(this, arguments);
}

var aB = new Boxes('aBox', '5*5*5');
var bB = new Boxes('bBox', 'sth else', 'blah blah');

console.log(aB);  // Boxes {name: "aBox", size: "5*5*5"}
console.log(bB);  // Boxes {name: "bBox", size: "sth else"}

// 这里可以看最后的参数调用Box的方法时，如果没有提供方法，则不会进行操作
```

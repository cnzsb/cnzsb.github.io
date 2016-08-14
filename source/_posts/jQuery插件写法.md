title: 'jQuery插件写法'
date: 2016-7-17 20:45:39 
tags: [jQuery]

---

在当前的前端时代，react，vue等框架配合ES6满足了日常的多数开发需求，但是了解jQuery的插件开发在我们开发一些小型项目中还是很有用处。我们可以打造自己的插件达到复用的效果。

<!-- more -->

## extend方法

我们首先知道在jQuery中`$.fn === $.prototype`。如果只是简单的需要一个jQuery的扩展方法，那么我们直接使用`$.extend`即可达到目的。

```javascript
$.extend({
	min: function (a, b) {return a < b ? a : b;}
});

$.min(1, 2); // 1
```

这种方法往往不是我们想要的，我们多数时候需要操作一个特定的dom从而使用一些自定义方法。这个时候就需要在jQuery的原型进行拓展，也就是这篇文章所要讨论的插件写法，将会使用到`$.fn.extend()`方法。


## 外部容器

首先需要用一个立即执行的函数给我们的插件一个独立的作用域，防止冲突。

```javascript
;(function ($, window, document, undefined) {
	// do sth.
})(jQuery, window, document)
```

这里把jQuery和系统全局变量传递给插件内部，系统变量在这里实现局部引用可以提高访问速度，`undefined`是为了得到一个没有修改的`undefined`，这里没有传第4个参数即是在那个位置得到了真实的`undefined`。最后我们在最前面加入`;`，是防止其他人的代码造成干扰从而报错无法运行。

## 基本插件写法

有了容器，我们现在就需要定义我们自己的插件了。

```javascript
;(function ($, window, document, undefined) {
	// 创建内部对象
	var Func = function (el, opts) {
		this.$el = $(el);
		this.opts = $.extend({}, Func.DEFAULTS, opts);

		// do sth.
	};

	// 内部对象的默认参数
	Func.DEFAULTS = {
		key: 'val'
	};

	// 内部对象的方法
	Func.prototype = {
		init: function() {}
	};

	$.fn.extend({
		pluginName: function (opts) {
			// 实现循环调用
			return this.each(function () {
				return new Func(this, opts);
			});
		}
	});
})(jQuery, window, document)
```

通过上面的代码，我们首先可以自定义自己的插件名字`pluginName`，以及插件内部需要实现的一些方法。用一个工厂模式来构造我们的方法`Func`，并且定义一些默认的参数。使用插件时传入对象参数时则更新内部的默认参数。这里暂时没有考虑内部其他方法的参数。

此时我们已经可以使用自定义好的插件了。

```javascript
	$('el').pluginName();
	// $('el').pluginName({key: newVal}); // 传入参数
```

## 向内部方法传参的插件写法

为了达到插件内部方法还能传参，我们可以在拓展插件时进行一些定义，在pluginName的方法返回值之前做一些判断。

```javascript
var Func = function () {};

Func.DEFAULTS = {};

var methods = {
	init: function(opts) {
		return this.each(function() {
			var $this = $(this);
			opts = $.extend({}, Func.DEFAULTS, opts);

			// do sth.
		});
	}
};

$.fn.pluginName = function() {
	var method = arguments[0];

	if(methods[method]) {
		method = methods[method];
		args = Array.prototype.slice.call(arguments, 1);
	} else if( typeof(method) == 'object' || !method ) {
		// 使用init初始化数据，或者使用new Func()来实例化，则需要在Func内部做一些处理
		method = methods.init;
	} else {
		$.error( '方法不存在' );
		return this;
	}

	return method.apply(this, arguments);
}
```

现在则可以通过第一个参数获取方法，之后的参数为该方法的参数来使用插件内部方法了。多数情况下，插件的具体实现以及关于可拓展性，我们会在上面2种方法中结合使用。

## 单例模式的优化

为了插件拥有更好的性能以及减少额外的开销，我们可以根据需要使用单例模式。同样在返回一个实例化对象时我们可以提前判断是否已经存在这么一个实例化对象了。这里简单的拿上述第一个结构为例子，在拓展插件时做一下判断。

```javascript
$.fn.pluginName = function(opts) {
	return this.each(function() {
    	// 单例模式
        var self = $(this);
        var instance = self.data('Func');
        if (!instance) {
            instance = new Func(self, opts);
            self.data('Func', instance);
        }
        // 调用插件上的方法，本文中第二种写法可以根据此自由拓展
        if($.type(opts) === 'string') return instance[opts]();
    });
};
```

在判断实例对象时，我们利用了`data()`来存放插件对象的实例，同样利用该方法我们可以在`Func`的内部方法上提供一些数据存取。在不需要的时候使用`removeData()`来进行删除。

## 总结

插件的写法在本篇文章做了总结，但是依然需要在实际开发中按需进行不同的搭配。并且在实际的开发中，一定要辅以一些设计模式，这样才能更好的组织代码，并提供更好的实践。

## 参考资料

1. [深入理解jQuery插件开发](http://blog.jobbole.com/30550/)
2. [jQuery插件开发精品教程，让你的jQuery提升一个台阶](http://www.cnblogs.com/Wayou/p/jquery_plugin_tutorial.html#!comments)
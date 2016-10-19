title: 'JavaScript 的复制操作'
date: 2016-8-17 23:47:46 
tags: JavaScript

---

对于点击按钮就能复制一个链接或其他内容的操作，在很多网站都会有用到。使用原生的js方法 `document.execCommand` 即可实现，它能够对**可以编辑的文档对象（设置contentEditable等）**进行操作。

## 语法

它的基本语法如下：

```javascipt
bool = document.execCommand(aCommandName, aShowDefaultUI, aValueArgument)
```

<!-- more -->

接受三个参数，并返回一个 `Boolean`，如果是 `false` 则表示操作不被支持或未被启用。

第一个参数是一个 `DOMString`，即为用到的命令名称，如 `copy`、`cut` 等，具体的实现因浏览器而异，目前多数的浏览器都能支持，可以[**点此预览**](http://codepen.io/netsi1964/full/QbLLGW/)这些指令并检测当前浏览器是否支持。

第二个参数是一个 `Boolean` 是否展示用户界面，一般为 `false`。Mozilla 没有实现。实际使用中并未用到。

第三个参数是某些命令需要的一些额外参数值（如 insertimage 需要提供这个 image 的 url）。默认为 `null。`实际使用中也为用到。



## 实现复制功能

对于一个页面中已经存在的 `input` 元素来讲，我们只需要直接对调用即可实现。

```javascript
// 假设页面存在以下元素
// <input id="copy" type="text" value="copy command">

document.getElementById('copy').select()
document.execCommand('copy')
```

以上便把 “copy command” 这个内容复制到了剪切板内。对于这样的功能实现并不复杂，实际业务场景中若遇到点击一个叫做“复制链接”的按钮就直接复制到剪切板上，这就让人头疼了。我们很容易想到利用一下代码实现。

```javascript
const input = document.createElement('input')
input.type = 'text'
input.value = someData	// 一些数据
input.select()
document.execCommand('copy')
input.blur()
```

然而现实运行后却发现并没有成功。后来反复试验发现对于任何一个不可见的元素： `display: none`、`type="hidden"`、`width: 0； height: 0` 等，该指令均无效。回头仔细研究基本语法，发现**可编辑的文档对象**似乎有什么猫腻，假设一个元素无法看见，或者无法点击，那么确实好像没办法**直接编辑**，所以这就是没有成功的原因了。但是非要实现这样一个功能怎么办呢，思前想后不妨试试 `opacity: 0`，这下竟然成功了。因此为了不影响UI的前提下可以这样实现功能。

```HTML
<button id="copy-btn" type="button">复制链接</button>

<script type="text/babel">
document.getElementById('button')
    .addEventListener('click', (e) => {
        const _target = e.target
        let input = document.createElement('input')
        input.type = 'text'
        input.className = 'copy-text'	// 利用 class 设置样式
        input.value = someData		// 一些数据
        
        const _i = _target.appendChild('input')	 // 暂时添加进 button 节点中了，也可以放在其他地方

        _i.select()
        document.execCommand('copy')

        _target.removeElement('_i')
        input = null
    }, false)
</script>

<style lang="scss" rel="stylesheet/scss">
#copy-btn {
    position: relative;

    .copy-text {
        position: absolute;	// 脱离文档流，防止对 UI 影响
        width: 1px;         // 没啥大用，强迫症：）
        height: 1px;        // 没啥大用，强迫症：）
        opacity: 0;         // 重点
    }
}
</style>
```

以上便完成了直接点击按钮即复制一条需要的内容的需求。其实多数情况下防止代码复制无效，都会要显示出来要复制的内容的，只是最近恰好遇到了这样一个需求而已。

## 总结

对于其他功能，都是类似实现，并不复杂，但是要注意只对**可以编辑的文档对象**可以使用。另外它的兼容性还可以了，对于我们不需要兼容低版本浏览器的 PC 端来讲不会有啥大问题，不过对于移动端可能会出现一些问题，比如 Safari 等的不支持，暂时没有用到就不讲了。

## 参考资料

* [document.execCommand - Web API 接口 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand#例子)
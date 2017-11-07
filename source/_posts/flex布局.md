title: 'Flex布局'
date: 2016-05-04 08:06:01
tags: [CSS3]

---

# Flexible Box Layout

> CSS 弹性框布局是 CSS 的模块之一，定义了一种针对用户界面设计而优化的 CSS 框模型。在弹性布局模型中，弹性容器的子元素可以在任何方向上排布，也可以“弹性伸缩”其尺寸，既可以增加尺寸以填满未使用的空间，也可以收缩尺寸以避免父元素溢出。子元素的水平对齐和垂直对齐都能很方便的进行操控。通过嵌套这些框（水平框在垂直框内，或垂直框在水平框内）可以在两个维度上构建布局。

<!-- more -->

弹性布局在定义方面是指调整其内项目宽高从而在任何显示设备上实现对可用显示空间最佳填充的能力。弹性容器扩展其内项目来填充可用空间，或将其收缩来避免溢出。

## 1. 基本概念

![flexbox](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/flexbox.png)

容器默认存在两根轴：`main axis`（从`main-start`向`main-end`）或者`cross axis`（从`cross-start`向`cross-end`）。

项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的侧轴空间叫做`cross size`。

* **main axis**：Flex容器的主轴主要用来配置Flex项目。它的方向取决于`flex-direction`属性。
* **main-start** | **main-end**：Flex项目的配置从容器的主轴起点边开始，往主轴终点边结束。
* **main size**：Flex项目的在主轴方向的宽度或高度就是项目的主轴长度，Flex项目的主轴长度属性是`width`或`height`属性，由哪一个对着主轴方向决定。
* **cross axis**：与主轴垂直的轴称作侧轴，是侧轴方向的延伸。
* **cross-start** | **cross-end**：伸缩行的配置从容器的侧轴起点边开始，往侧轴终点边结束。
* **cross size**：Flex项目的在侧轴方向的宽度或高度就是项目的侧轴长度，Flex项目的侧轴长度属性是`width`或`height`属性，由哪一个对着侧轴方向决定。

## 2. 外层父容器属性

定义一个Flex容器，根据其取的值来决定是内联还是块。Flex容器会为其内容建立新的伸缩格式化上下文。

```javascript
.container {
	display: flex | inline-flex;
}
```

注意，Flex容器不是块容器，因此有些设计用来控制块布局的属性在Flexbox布局中不适用。特别是：多列组中所有`column-*`属性、`float`、`clear`属性和`vertical-align`属性在Flex容器上没有作用。

以下6个属性设置在父容器上。

* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content

### 2.1 flex-direction

弹性容器的各个边描述了弹性条目流的起点与终点。它们决定了弹性容器的主轴方向（从左到右、从右到左，等等）。

![flex-direction](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/flex-direction.png)

```javascript
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

* row（默认值）：主轴为水平方向，起点在左端。
* row-reverse：主轴为水平方向，起点在右端。
* column：主轴为垂直方向，起点在上沿。
* column-reverse：主轴为垂直方向，起点在下沿。

### 2.2 flex-wrap

默认情况之下，Flex项目都尽可能在一行显示。你可以根据flex-wrap的属性值来改变，让Flex项目多行显示。

![flex-wrap](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/flex-wrap.png)

```javascript
.container {
   flex-wrap: nowrap | wrap | wrap-reverse;
}
```

* nowrap(默认值)：不换行。
* wrap：换行，第一行在**下方**。
* wrap-reverse：换行，第一行在**上方**。

### 2.3 flex-flow

这是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

```javascript
.container {
   flex-flow: <flex-direction> || <flex-wrap>;
}
```

### 2.4 justify-content

![justify-content](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/justify-content.png)

```javascript
.container {
   justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

* flex-start（默认值）：左对齐。
* flex-end：右对齐。
* center：居中。
* space-between：两端对齐，项目之间的间隔都相等。
* space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

### 2.5 align-items

定义项目在侧轴上如何对齐。

![align-items](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/align-items.png)

```javascript
.container {
   align-items: flex-start | flex-end | center | baseline | stretch;
}
```

* flex-start：侧轴的起点对齐。
* flex-end：侧轴的终点对齐。
* center：侧轴的中点对齐。
* baseline：项目的第一行文字的基线对齐。
* stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

### 2.6 align-content

定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

![align-content](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/align-content.png)

```javascript
.container {
   align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

* flex-start：与侧轴的起点对齐。
* flex-end：与侧轴的终点对齐。
* center：与侧轴的中点对齐。
* space-between：与侧轴两端对齐，轴线之间的间隔平均分布。
* space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
* stretch（默认值）：轴线占满整个侧轴。


## 3. 子元素项目属性

以下6个属性设置在子项目上。

* order
* flex-grow
* flex-shrink
* flex-basis
* flex
* align-self

### 3.1 order

order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0，有最小（负值最大）order的伸缩项目排在第一个。

![order](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/order.png)

```javascript
.item {
	order: <integer>;
}
```

### 3.2 flex-grow

定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

![flex-grow](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/flex-grow.png)

```javascript
.item {
	flex-grow: <number>; /* default 0 */
}
```

### 3.3 flex-shrink

定义了项目的缩小比例，默认为1(负值无效)，即如果空间不足，该项目将缩小。

如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

![flex-shrink](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/flex-shrink.jpg)

```javascript
.item {
	flex-shrink: <number>; /* default 1 */
}
```

### 3.4 flex-basis

定义了Flex项目在分配Flex容器剩余空间之前的一个默认尺寸。`main-size`值使它具有匹配的宽度或高度，不过都需要取决于`flex-direction`的值。

```javascript
.item {
  flex-basis: <length> | auto; /* default auto */
}
```
### 3.5 flex

`flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。    

```javascript
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

建议使用简写属性，而不是设置单独属性。

* auto (1 1 auto)
* none (0 0 auto)。

### 3.6 align-self

允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

![align-self](http://7xlivs.com1.z0.glb.clouddn.com/2016/05/flex布局/align-self.png)

```javascript
.item {
	align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

* flex-start：侧轴的起点对齐。
* flex-end：侧轴的终点对齐。
* center：侧轴的中点对齐。
* baseline：项目的第一行文字的基线对齐。
* stretch：如果项目未设置高度或设为auto，将占满整个容器的高度。

## 4. 兼容性

<iframe width="100%" height="380" src="//caniuse.com/flexbox/embed"></iframe>

* iOS和Android4.4以上可以使用最新的flex布局
* Android4.4以下兼容旧版flexbox布局

```
display: -webkit-box;
display: -webkit-flex;
display: -ms-flexbox;
display: flex;

-webkit-box-flex: 1;
	-webkit-flex: 1;
	    -ms-flex: 1;
	        flex: 1;

-webkit-box-pack: center;
	-webkit-justify-content: center;
	    -ms-flex-pack: center;
	        justify-content: center;

-webkit-box-align: center;
	-webkit-align-items: center;
	    -ms-flex-align: center;
	        align-items: center;

```

## 5 参考资料

1. [Flex 布局教程：语法篇](//www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
2. [一个完整的Flexbox指南](//www.w3cplus.com/css3/a-guide-to-flexbox-new.html)
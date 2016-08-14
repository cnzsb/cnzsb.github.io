title: 'CSS3 text-shadow在IE下的解决'
date: 2016-1-12 18:09:07 
tags: [Tips, CSS3]

---

`text-shadow`兼容IE10及FF3.5以上，在IE8、9下可以使用滤镜`filter：glow`使元素发光来替代。

```css
filter： grow(color: #000, strength: 1);
filter： grow(color: #000, direction: 2);
```

`color`设置发光颜色，在这里`strength`和`direction`均设置阴影强度，但它们2个的表现也不太一样。

在IE7以下这个属性暂时无效。
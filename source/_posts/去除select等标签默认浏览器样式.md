title: '去除select等标签默认浏览器样式'
date: 2016-7-17 16:11:47  
tags: [Tips, CSS]

---

去除select等标签默认浏览器样式的方式：

```css
select {
	-webkit-appearance: none;  /* chrome */
	   -moz-appearance: none;  /* firefox */
	        appearance: none;
}

/* IE */
select::-ms-expand {
	display: none;
}
```
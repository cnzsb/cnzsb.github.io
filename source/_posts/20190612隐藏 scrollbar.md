title: 隐藏 scrollbar
date: 2019-06-12 11:44:31
tags: [CSS, Tips]

---

```css
  // Chrome, Safari
  &::-webkit-scrollbar {
    display: none;
  }

  // IE 10+
  -ms-overflow-style: none;
  // Firefox
  scrollbar-width: none;
```

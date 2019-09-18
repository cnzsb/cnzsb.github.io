title: '点击 QQ 号码弹出聊天窗口'
date: 2015-9-7 14:10:23
tags: [Tips, HTML]

---

2种旧方法。

```html
<a href="tencent://message/?exe=qq&menu=yes&uin=QQ号码" rel="nofollow"></a>
```

```html
<a href="//wpa.qq.com/msgrd?v=3&uin=QQ号码&site=qq&menu=yes" target="_blank" rel="nofollow">
	<img border="0" src="//wpa.qq.com/pa?p=2:QQ号码:41" alt="点击这里给我发消息" title="点击这里给我发消息"/>
</a>
```

1种新的方法，不会提示因“对方按钮版本过低”而无法打开。其中的`aty`和`a`值的参数影响着营销QQ中的分组及对应人员，具体设置时候还是需要重新生成。

```html
//crm2.qq.com/page/portalpage/wpa.php?uin=QQ号码&aty=0&a=0&curl=&ty=1
```

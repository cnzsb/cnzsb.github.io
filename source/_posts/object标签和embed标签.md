title: 'object标签和embed标签'
date: 2015-9-25 15:21:51
tags: [Tips, HTML]

---

最近用到.swf的文件，一时不知如何调用，搜索后知道可以使用`object`标签或者`embed`标签来嵌入，参照着其他代码改完后，马马虎虎可以使用了。今天特意整理了其中的一些说明。原网页使用的代码如下：

<!-- more -->

```HTML
<object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="<a href='https://www.adobe.com/go/getflash'><img src='images/TB21HgpbFXXXXX2XpXXXXXXXXXX_!!163498746.gif' alt='获得 Adobe Flash Player' /></a>" width="100%" height="352">
      <param name="movie" value="1.swf" id="movie">
      <param name="wmode" value="transparent">
      <param name="quality" value="high">
      <param name="bgcolor" value="#e1f7ff">
      <param name="play" value="true">
      <param name="loop" value="true">
      <param name="scale" value="showall">
      <param name="menu" value="true">
      <param name="devicefont" value="false">
      <param name="salign" value="">
      <param name="allowScriptAccess" value="sameDomain">
      <embed src="1.swf" wmode="transparent" quality="high" bgcolor="#e1f7ff" loop="true" scale="showall" menu="true" devicefont="false" allowScriptAccess="sameDomain" width="100%" height="352" type="application/x-shockwave-flash" pluginspage="<a href='https://www.adobe.com/go/getflash'><img src='images/TB21HgpbFXXXXX2XpXXXXXXXXXX_!!163498746.gif' alt='获得 Adobe Flash Player' /></a>" />
</object>
```

这里同时使用了`object`和`embed`标签来嵌入，目的是为了浏览器的兼容性。因此这两种写法对应的属性和参数要一致。

下面记录一些属性的意义。

### 必需属性 ###

- `classid`，设置浏览器的AcriveX控件，与上述使用代码必须一致，仅用于`object`。
- `codebase`，设置flash插件的下载地址，若浏览器未安装，可自动下载安装，仅用于`object`。
- `width`和`height`，设置文件的宽度和高度，以百分比或像素表示。
- `movie`，设置文件地址，仅用于`object`。
- `pluginspage`，设置flash插件的下载地址，若浏览器未安装，可自动下载安装，仅用于`embed`。
- `src`，设置文件地址，仅用于`embed`。

### 可选属性 ###

- `wmode`，设置文件的window mode属性，指定文件在浏览器中的透明，层叠及位置。
	+ `window`，文件在浏览器中自己的矩形窗口内播放。
	+ `opaque`，文件隐藏了所有在它后面的内容。
	+ `transparent`，文件透明，显示文件后面的网页内容，将会降低动画性能，且并不适用于所有浏览器。
- `quality`
	+ `low`，速度优于美观，但不应用反锯齿。
	+ `autolow`，刚开始着重于速度，但当需要时随时提升美观。
	+ `autohigh`，同时着重播放速度和美观，但需要时则牺牲美观来保证播放速度。
	+ `medium`，应用一些反锯齿而不平滑位图。它质量高于`low`设置而低于`high`设置。
	+ `high`，美观优于播放速度，而且一直应用反锯齿。如果影片不包含动画，位图会被平滑化；而如果影片包含动画，位图将不变平滑。
	+ `best`，提供最好的显示质量而不考虑播放速度。所有输出都应用反锯齿及所有位图都被平滑化。
- `bgcolor`，设置文件背景颜色。使用这个属性覆盖文件中设定的背景颜色。
- `play`，设置文件加载完成后是否自动播放，默认为`true`。
- `loop`，设置文件播放完后是否循环，默认为`true`。
- `scale`
	+ `showall`，默认设置，文件在指定区域显示，但保持原始比例，文件两侧会出现边框。
	+ `noborder`，收缩文件以适应指定区域，但保持原始比例，文件不失真，但部分文件可能被裁切。
	+ `exactfit`，使文件自适应指定区域，文件可能失真变形或改变显示比例。
- `menu`
	+ `true`，显示全部的菜单，允许用户放大，缩小等控制文件播放的操作。
	+ `false`，只显示包含设置选项和关于文件的菜单。
- `allowScriptAccess`
	+ `always`，默认设置，代表任何文件都被允许调用脚本。
	+ `sameDomain`，设置文件的脚本调用权限范围为同一域名。
	+ `never`，代表不允许调用脚本。
- `devicefont`，是否设置为设备字体样式。
- `align`，默认为居中，当浏览器窗口小于文件时，边缘会被裁切。可设置为`left`，`right`，`top`，`bottom`，设置后如有需要，另外三边将被裁切。
- `standby`，设置文件正在加载时显示的文本。

### 遇到的问题 ###

1. 在`embed`中设置`play=“false”`即文件加载后不自动播放时如果出问题，可再设置`flashvars="autoplay=false&play=false"`。
2. 视频全屏按钮点击无效时，可设置`allowFullScreen="true"`。

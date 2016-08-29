title: 'HTML5 图片预览'
date: 2016-8-29 22:43:12
tags: HTML5

---

在没有 HTML5 之前，前端的图片预览都是在用户选择图片后就发 Ajax 到服务端，然后由服务端再把图片的 URL 返还给前端来完成的。有了 HTML5 后，在 IE10 以上及代浏览器的版本中我们便无须发请求获得 URL 来完成预览了。这些将依靠 `FileReader` 来实现。

<!-- more -->

我们直接来实现一个 demo，可以点击尝试一下。


<input id="file" type="file" accept="image/jpg, image/jpeg, image/png, image/gif">
<div id="preview"></div>

<script>
var file = document.getElementById('file'),
    preview = document.getElementById('preview');
file.addEventListener('change', function(e) {
    var file = e.target.files[0],
        fr = new FileReader();
    fr.onload = function(e) {
        var img = e.target.result;
        preview.innerHTML = '<img src="' + img + '" width="200" height="200">';
    };
    fr.readAsDataURL(file);
}, false);
</script>


```HTML
<input id="file" type="file" accept="image/jpg, image/jpeg, image/png, image/gif">
<img id="preview" src="" width="200" height="200" style="display: none">

<script>
var file = document.getElementById('file'),
    preview = document.getElementById('preview');

file.addEventListener('change', function(e) {
    var file = e.target.files[0],
        fr = new FileReader();

    fr.onload = function(e) {
        preview.src = e.target.result;
        preview.style.display = 'block';
    };

    fr.readAsDataURL(file);
}, false);
</script>
```

我们用 `FileReader` 的一个实例利用方法 `readAsDataURL` 读取了上传的文件，并且我们事先设置了在读取即 `onload` 时，触发了视图更新。

`FileReader` 的这个特性，主要是把上传的图片给转化为了 `base64` 的格式，因此实现了预览。其实它还可以读取 `Blob` 和 `File` 等文件，具体的方法可以看[这里](https://developer.mozilla.org/en-US/docs/Web/API/FileReader)。
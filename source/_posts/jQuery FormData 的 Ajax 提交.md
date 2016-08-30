title: 'jQuery FormData 的 Ajax 提交'
date: 2016-8-30 22:36:48 
tags: [Tips, jQuery]

---

前些天恰好遇到用 jQuery 提交一个上传的文件一直出错，这里记录一下原因。

我们的项目本身是使用了 vue 的，但是历史原因同时也保留了 jQuery ，然后项目中用 Promise 重新封装了 jQuery 的 Ajax 方法。之前有一个共用的 upload 的上传组件，里面使用了原生 XHR ，没有遇到上传文件的问题，但是我在使用 jQuery 的 Ajax 时却怎么都办法上传，检查发现是 `content-type` 导致的问题。

<!-- more -->

仔细查看 jQuery 的 API 文档才发现原来它默认 `contentType` 为 `application/x-www-form-urlencoded` ，这显然不是 `FormData` 的类型。同时 `processData` 为了配合默认的 `contentType` 类型，也会把需要发送的 `data` 对象转为一个查询字符串。因此在发送一个 `FormData` 类型的数据时，应该这么写：

```javascript
$.ajax({
    type: 'post',
    url: '/api/upload',
    contentType: false,
    processData: false,
    data: FormData,
    success: handleResponse
})
```
title: React Router 阻止路由切换
date: 2019-10-16 15:41:05
tags: [React, Tips]

---

在某些表单页面，监测当前页面路由变动从而提醒用户确认是否需要舍弃当前所编辑内容是一个常见的场景，React Router v4 之后的版本提供了 `Prompt` 和 `getUserConfirmation` 或者利用 history 中的 `history.block` 来实现这个功能。主要的使用方法参见 [React Router 中的讨论](https://github.com/ReactTraining/react-router/issues/4635)。以下是最简单的实现方法：


```javascript
const unblock = history.block((location) => {
    if (input.value !== '' || !location.pathname.includes('login')) {
        // 在此实现自定义功能
        openDialog({
            title: 'Confirm',
            content: 'Are you sure you want to leave this page?'
            onOk: () => history.push(location)
        })
        return false;
    }
    return true;
})

// stop blocking transitions
unblock();
```

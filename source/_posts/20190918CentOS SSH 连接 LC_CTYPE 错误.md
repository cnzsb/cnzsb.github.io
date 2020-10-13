title: CentOS SSH 连接 LC_CTYPE 错误
date: 2019-09-18 10:38:21
tags: [DevOps]

---

`warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory`

```bash
vi /etc/environment
# add these lines...
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
```

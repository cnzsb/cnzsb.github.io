title: '添加多个 SSH 秘钥'
date: 2017-02-28 23:40:30
tags: [Git]

---

本月堆积了好几篇文章没写了，趁着最后几十分钟，为了完成年初计划马虎更新一篇记录性的文章吧。由于本月刚换了新的公司，新公司的代码托管在 GitLab 上，因此有了管理 2 个 SSH 秘钥的需求，查阅资料后发现并不难，记录与分享一下。

首先生成 SSH 的指令不陌生：

```bash
ssh-keygen -t rsa -C "邮箱地址"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa): 
```

默认会存放在个人文档根目录下的 `.ssh` 下，并以 `id_rsa` 的文件名生成秘钥对。<!-- more -->方便起见我没有修改已经存在的这两个配置文件。接下来的操作自然是再生成一份配置文件，但是注意选择保存的时候要改为其他名字防止覆盖已有文件，比如以 `id_rsa_other` 新命名。

接下来需要利用 `ssh-add` 相关命令添加对应的标识。

```bash
ssh-add ~/.ssh/id_rsa_other
ssh-add -L  # 查看已生成的列表，用来确认是否添加成功
```

最后还需要在 `.ssh` 文件夹下创建一份 `config` 配置文件，文件内容如下：

```bash
# github.com
Host github.com
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa
# gitlab.other.com
Host gitlab.other.com
        HostName gitlab.other.com
        User git
        IdentityFile ~/.ssh/id_rsa_other
```

最最后，如果需要测试的话可以执行 `ssh -T "git@HOST"` 进行检验。但是一般公司内的服务器是不会给个人开通权限也就没办法利用这个指令测试了，不过可以尝试 `clone` 一个仓库来检测是否成功。

具体在开发时还需要记得在相应的仓库中重新配置 `git config` 对应的用户名及邮箱哦。

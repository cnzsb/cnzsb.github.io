title: 'git rebase 批量合并 commits'
date: 2017-05-04 15:48:27
tags: [Git]

---

`git rebase` 是一个经常用以美化分支线的指令，近来听说了一个 `squash` 的指令可以用来合并多个 `commits`，研究后更是觉得 `rebase` 的神奇了，本文做一个记录。

<!-- more -->

现在有以下 `commits`：

```bash
* fc24432 - (HEAD -> master) third commit (2 seconds ago) <cnzsb>
* 508a1a0 - second commit (81 seconds ago) <cnzsb>
* 559f659 - first commit (2 minutes ago) <cnzsb>
* 40a59bc - initial commit (5 minutes ago) <cnzsb>
```

现在我们把最新提交的 3 次 `commit` 合并为一次修改则可以使用 `git rebase -i HEAD~3` 指令（更多参数含义参见 [git rebase 文档](https://git-scm.com/docs/git-rebase)）。

其中 `pick` 是挑选出的基准 `commit`，对需要合并的 `commit` 使用 `squash` 或 `s` 则会保存该 `commit` 的 `commit message` 并合并在其前一个 `commit` 上，最终自己可以更新 `commit message` 信息；也可以使用 `fixup` 或 `f` 丢弃 `commit message` 并合并，这样最终的 `commit message` 会是 `pick` 的那条信息。

```bash
pick 559f659 first commit
s 508a1a0 second commit
s fc24432 third commit

# Rebase 40a59bc..fc24432 onto 40a59bc (3 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

这里使用 `squash`，得到下面的信息。

```bash
# This is a combination of 3 commits.
# This is the 1st commit message:
first commit

# This is the commit message #2:

second commit

# This is the commit message #3:

third commit

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Thu May 4 14:36:40 2017 +0800
#
# interactive rebase in progress; onto 40a59bc
# Last commands done (3 commands done):
#    s 508a1a0 second commit
#    s fc24432 third commit
# No commands remaining.
# You are currently editing a commit while rebasing branch 'master' on '40a59bc'.
#
# Changes to be committed:
#       new file:   commits.txt
#
```

可以看到每个对应的 `commit message`，我们这里删掉所有不需要的 `commit message`，并且保存为 “*create commits.txt and update cotents*”，最后可以看到成功信息。

```bash
[detached HEAD b073a14] create commits.txt and update cotents
 Date: Thu May 4 14:36:40 2017 +0800
 1 file changed, 3 insertions(+)
 create mode 100644 commits.txt
Successfully rebased and updated refs/heads/master.
```

新的 `commits`：

```bash
* b073a14 - (HEAD -> master) create commits.txt and update cotents (11 minutes ago) <cnzsb>
* 40a59bc - initial commit (65 minutes ago) <cnzsb>
```

# Git - detached HEAD

detached HEAD，指 HEAD 指向了未知的分支，即不在所有已知的分支范围内。

当你进入 detached HEAD 的状态的时候，git 会给出如下这样的提示：

```bash
Note: checking out 'xxxxx'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>
```

注意：你应该永远在分支上开发 —— 而绝不在分离的 HEAD 上。这样确保你一直可以引用到你的新提交。不过，如果你只是想查看旧的提交，那么是否处于分离 HEAD 状态并不重要。

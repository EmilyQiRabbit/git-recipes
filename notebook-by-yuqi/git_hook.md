# Git 钩子

钩子存在于每个 Git 仓库的 `.git/hooks` 目录中。当你初始化仓库时，Git 自动生成这个目录和一些示例脚本。当你观察 `.git/hooks` 时，你会看到下面这些文件：

```
applypatch-msg.sample
pre-push.sample
commit-msg.sample
pre-rebase.sample
post-update.sample
prepare-commit-msg.sample
pre-applypatch.sample
update.sample
pre-commit.sample
```

这里已经包含了大部分可用的钩子了，但是 `.sample` 拓展名防止它们默认被执行。为了安装一个钩子，你只需要去掉 `.sample` 拓展名。或者你要写一个新的脚本，你只需添加一个文件名和上述匹配的新文件，去掉 `.sample` 拓展名。

比如说，试试安装一个 `prepare-commit-msg` 钩子。去掉脚本的 `.sample` 拓展名，在文件中加上下面这两行：

```sh
#!/bin/sh

echo "# Please include a useful commit message!" > $1
```

钩子需要能被执行，所以如果你创建了一个新的脚本文件，你需要修改它的文件权限。比如说，为了确保 `prepare-commit-msg` 可执行，运行下面这个命令：

```sh
chmod +x prepare-commit-msg
```

接下来你每次运行 `git commit` 时，你会看到默认的提交信息都被替换了。我们会在「准备提交信息」一节中细看它是如何工作的。现在我们已经可以定制 Git 的内部功能，你只需要坐和放宽。

内置的样例脚本是非常有用的参考资料，因为每个钩子传入的参数都有非常详细的说明（不同钩子不一样）。

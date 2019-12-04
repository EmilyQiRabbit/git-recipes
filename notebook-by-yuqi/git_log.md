## git log

一些最常用的 `git log` 配置如下所示。

```
git log -n <limit>
```

用 `<limit>` 限制提交的数量。比如 `git log -n 3` 只会显示 3 个提交。

```
git log --oneline
```

将每个提交压缩到一行。

```
git log --stat
```

将会显示出哪些文件被更改了，以及每个文件相对的增删行数。

```
git log -p
```

显示代表每个提交的详细信息。显示每个提交全部的差异（diff），这也是项目历史中最详细的视图。

```
git log --author="<pattern>"
```

搜索特定作者的提交。`<pattern>` 可以是字符串或正则表达式。

```
git log --grep="<pattern>"
```

搜索提交信息匹配特定 `<pattern>` 的提交。`<pattern>` 可以是字符串或正则表达式。

```
git log <since>..<until>
```

只显示发生在 `<since>` 和 `<until>` 之间的提交。两个参数可以是提交 ID、分支名、`HEAD` 或是任何一种引用。

```
git log 3157e..5ab91
```

会显示所有ID在 `3157e` 和 `5ab91` 之间的提交。除了校验总和之外，分支名、HEAD 关键字也是常用的引用提交的方法。`HEAD` 总是指向当前的提交，无论是分支还是特定提交也好。

```
git log <file>
```

只显示包含特定文件的提交。查找特定文件的历史这样做会很方便。

```
git log --graph --decorate --oneline
```

`--graph` 标记会绘制一幅字符组成的图形，左边是提交，右边是提交信息。
`--decorate` 标记会加上提交所在的分支名称和标签。
`--oneline` 标记将提交信息显示在同一行，一目了然。

~字符用于表示提交的父节点的相对引用。比如，`3157e~1` 指向 `3157e` 前一个提交,`HEAD~3` 是当前提交的回溯3个节点的提交。

### 栗子

可以将很多选项用在同一个命令中：

```sh
git log --author="John Smith" -p hello.py
```

这个命令会显示 `John Smith` 作者对 `hello.py` 文件所做的所有更改的差异比较（diff）。

**..句法是比较分支很有用的工具。**下面的栗子显示了在 `some-feature` 分支而不在 `master` 分支的所有提交的概览。

```sh
git log --oneline master..some-feature
```

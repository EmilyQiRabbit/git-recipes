# 保持同步

Git 给予每个开发者一份自己的仓库拷贝，拥有自己完整的本地历史和分支结构。用户通常共享一系列的提交而不是单个变更集合。Git 允许你在仓库间共享整个分支，而不是从工作副本提交一个差异集合到中央仓库。

## git remote

`git remote` 命令允许你创建、查看和删除和其它仓库之间的连接。远程连接更像是书签，而不是直接跳转到其他仓库的链接。它用方便记住的别名引用不那么方便记住的 URL，而不是提供其他仓库的实时连接。

### 用法

```
git remote
```

列出你和其他仓库之间的远程连接。

```
git remote -v
```

和上个命令相同，但同时显示每个连接的 URL。

```
git remote add <name> <url>
```

创建一个新的远程仓库连接。在添加之后，你可以将 `<name>` 作为 `<url>` 便捷的别名在其他 Git 命令中使用。

```
git remote rm <name>
```

移除名为 `<name>` 的远程仓库的连接。

```
git remote rename <old-name> <new-name>
```

将远程连接从 `<old-name>` 重命名为 `<new-name>`。

#### 名为 origin 的远程连接

当你用 `git clone` 克隆仓库时，**它自动创建了一个名为 origin 的远程连接，指向被克隆的仓库**。当开发者创建中央仓库的本地副本时非常有用，因为它提供了拉取上游更改和发布本地提交的快捷方式。这也是为什么大多数基于 Git 的项目将它们的中央仓库取名为 origin。

#### 仓库的 URL

Git 支持多种方式来引用一个远程仓库。其中两种最简单的方式便是 HTTP 和 SSH 协议。HTTP 是允许匿名、只读访问仓库的简易方式。比如：

```
http://host/path/to/repo.git
```

但是，直接将提交推送到一个 HTTP 地址一般是不可行的（你不太可能希望匿名用户也能随意推送）。如果希望对仓库进行读写，你需要使用 SSH 协议：

```
ssh://user@host/path/to/repo.git
```

你需要在托管的服务器上有一个有效的 SSH 账户，但不用麻烦了，Git 支持开箱即用的 SSH 认证连接。

### 栗子

除了 origin 之外，添加你同事的仓库连接通常会带来一些便利。比如，如果你的同事 John 在 `dev.example.com/john.git` 上维护了一个公开的仓库，你可以这样添加连接：

```
git remote add john http://dev.example.com/john.git
```

通过这种方式访问每个开发者的仓库，中央仓库之外的协作变得可能。这给维护大项目的小团队带来了极大的便利。

## git fetch

`git fetch` 命令将提交从远程仓库导入到你的本地仓库。**拉取下来的提交储存为远程分支，而不是我们一直使用的普通的本地分支**。你因此可以在整合进你的项目副本之前查看更改。

### 用法

```
git fetch <remote>
```

拉取仓库中所有的分支。同时会从另一个仓库中下载所有需要的提交和文件。

```
git fetch <remote> <branch>
```

和上一个命令相同，但只拉取指定的分支。

### 讨论

当你希望查看其他人的工作进展时，你需要 fetch。fetch 下来的内容表示为一个远程分支，因此不会影响你的本地开发。这是一个安全的方式，在整合进你的本地仓库之前，检查那些提交。即，你可以看到中央仓库的历史进展如何，但它不会强制你将这些进展合并入你的仓库。

#### 远程分支

远程分支和本地分支一样，只不过它们代表这些提交来自于其他人的仓库。你可以像查看本地分支一样查看远程分支，但你会处于分离 HEAD 状态（就像查看旧的提交时一样）。你可以把它们视作只读的分支。如果想要查看远程分支，只需要向 `git branch` 命令传入 `-r` 参数。远程分支拥有 remote 的前缀，所以你不会将它们和本地分支混起来。比如，下面的代码片段显示了从 origin 拉取之后，你可能想要查看的分支：

```
git branch -r
# origin/master
# origin/develop
# origin/some-feature
```

同样，你可以用寻常的 `git checkout` 和 `git log` 命令来查看这些分支。如果你接受远程分支包含的更改，你可以使用 `git merge` 将它并入本地分支。所以，不像 SVN，同步你的本地仓库和远程仓库事实上是一个分两步的操作：先 fetch，然后 merge。`git pull` 命令是这个过程的快捷方式。

### 栗子

这个例子回顾了同步本地和远程仓库 `master` 分支的常见工作流：

```
git fetch origin
```

它会显示会被下载的分支：

```
a1e8fb5..45e66a4 master -> origin/master
a1e8fb5..9e8ab1c develop -> origin/develop
* [new branch] some-feature -> origin/some-feature
```

在下图中，远程分支中的提交显示为方块，而不是圆圈。正如你所见，`git fetch` 让你看到了另一个仓库完整的分支结构。

若想查看添加到上游 master 上的提交，你可以运行 `git log`，用 `origin/master` 过滤：

```
git log --oneline master..origin/master
```

用下面这些命令接受更改并并入你的本地 `master` 分支：

```
git checkout master
git log origin/master
```

我们可以使用 `git merge origin/master`：

```
git merge origin/master
```

origin/master 和 master 分支现在指向了同一个提交，你已经和上游的更新保持了同步。

## git pull

在基于 Git 的协作工作流中，将上游更改合并到你的本地仓库是一个常见的工作。我们已经知道应该使用 `git fetch`，然后是 `git merge`，但是 `git pull` 将这两个命令合二为一。

### 用法

```
git pull <remote>
```

拉取当前分支对应的远程副本中的更改，并立即并入本地副本。**效果和 `git fetch` 后接 `git merge origin/.` 一致**。

```
git pull --rebase <remote>
```

和上一个命令相同，但使用 `git rebase` 合并远程分支和本地分支，而不是使用 `git merge`。

#### 基于 Rebase 的 Pull

`--rebase` 标记可以用来保证线性的项目历史，防止合并提交（merge commits）的产生。很多开发者倾向于使用 rebase 而不是 merge，因为「我想要把我的更改放在其他人完成的工作之后」。

事实上，使用 `--rebase` 的 pull 的工作流是如此普遍，以致于你可以直接在配置项中设置它：

```
git config --global branch.autosetuprebase always # In git < 1.7.9
git config --global pull.rebase true              # In git >= 1.7.9
```

在运行这个命令之后，所有的 `git pull` 命令将使用 `git rebase` 而不是 `git merge`。

### 栗子

下面的栗子演示了如何和一个中央仓库的 `master branch` 同步：

```
git checkout master
git pull --rebase origin
```

简单地将你本地的更改放到其他人已经提交的更改之后。

## git push

Push 是你将本地仓库中的提交转移到远程仓库中时要做的事。它和 `git fetch` 正好相反，fetch 将提交导入到本地分支，而 push 将提交导出到远程分支。它可以覆盖已有的更改，所以你需要小心使用。这些情况请见下面的讨论。

### 用法

```
git push <remote> <branch>
```

将指定的分支推送到 `<remote>` 上，包括所有需要的提交和提交对象。它会在目标仓库中创建一个本地分支。为了防止你覆盖已有的提交，如果会导致目标仓库非快速向前合并时，Git 不允许你 push。

```
git push <remote> --force
```

和上一个命令相同，但即使会导致非快速向前合并也强制推送。除非你确定你所做的事，否则不要使用 `--force` 标记。

```
git push <remote> --all
```

将所有本地分支推送到指定的远程仓库。

```
git push <remote> --tags
```

当你推送一个分支或是使用 `--all` 选项时，标签不会被自动推送上去。`--tags` 将你所有的本地标签推送到远程仓库中去。

#### 强制推送

Git 为了防止你覆盖中央仓库的历史，会拒绝你会导致非快速向前合并的推送请求。所以，如果远程历史和你本地历史已经分叉，你需要将远程分支 pull 下来，在本地合并后再尝试推送。

`--force` 这个标记覆盖了这个行为，让远程仓库的分支符合你的本地分支，删除你上次 pull 之后可能的上游更改。只有当你意识到你刚刚共享的提交不正确，并用 `git commit --amend` 或者交互式 rebase 修复之后，你才需要用到强制推送。但是，你必须绝对确定在你使用 `--force` 标记前你的同事们都没有 pull 这些提交。

#### 只推送到裸仓库

此外，你只应该推送到那些用 `--bare` 标记初始化的仓库。因为推送会弄乱远程分支结构，很重要的一点是，永远不要推送到其他开发者的仓库。但因为裸仓库没有工作目录，不会发生打断别人的开发之类的事情。

### 栗子

下面的栗子描述了将本地提交推送到中央仓库的一些标准做法。首先，确保你本地的 `master` 和中央仓库的副本是一致的，提前 fetch 中央仓库的副本并在上面 rebase。交互式 rebase 同样是共享之前清理提交的好机会。接下来，`git push` 命令将你本地 `master` 分支上的所有提交发送给中央仓库.

```
git checkout master
git fetch origin master
git rebase -i origin/master
# Squash commits, fix up commit messages etc.
git push origin master
```

因为我们已经确信本地的 `master` 分支是最新的，它应该导致快速向前的合并，`git push` 不应该抛出非快速向前之类的问题。

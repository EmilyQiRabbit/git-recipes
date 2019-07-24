# git reset 

## Basic Usages

```
git reset <file>
```

从缓存区移除特定文件，但**不改变工作目录**。它会取消这个文件的缓存，而不覆盖任何更改。

```
git reset
```

重设缓冲区，匹配最近的一次提交，但**工作目录不变**。它会取消 *所有* 文件的缓存，而不会覆盖任何修改，给你了一个重设缓存快照的机会。

```
git reset --hard
```

重设缓冲区和工作目录，匹配最近的一次提交。除了取消缓存之外，`--hard` 标记告诉 Git 还要重写所有工作目录中的更改。换句话说：**它清除了所有未提交的更改，所以在使用前确定你想扔掉你所有本地的开发。**

```
git reset <commit>
```

将当前分支的末端移到 `<commit>`，将缓存区重设到这个提交，但**不改变工作目录**。所有 `<commit>` 之后的更改会保留在工作目录中，这允许你用更干净、原子性的快照重新提交项目历史。

```
git reset --hard <commit>
```

将当前分支的末端移到 `<commit>`，将缓存区和工作目录都重设到这个提交。它不仅清除了未提交的更改，同时还清除了 `<commit>` 之后的所有提交。

## 注意 ⚠️

当有 `<commit>` 之后的提交被推送到公共仓库后，你绝不应该使用 `git reset`。发布一个提交之后，你必须假设其他开发者会依赖于它。

**重点是，确保你只对本地的修改使用 `git reset`，而不是公共更改。如果你需要修复一个公共提交，`git revert` 命令正是被设计来做这个的。**

## Examples

#### 取消文件缓存

`git reset` 命令在准备缓存快照时经常被用到。下面的例子假设你有两个文件，`hello.py` 和 `main.py`它们已经被加入了仓库中。

```
# 编辑了 hello.py 和 main.py

# 缓存了目录下所有文件
git add .

# 意识到 hello.py 和 main.py 中的修改应该在不同的快照中提交

# 取消 main.py 缓存
git reset main.py

# 只提交 hello.py
git commit -m "Make some changes to hello.py"

# 在另一份快照中提交 main.py
git add main.py
git commit -m "Edit main.py"
```

如你所见，`git reset` 帮助你取消和这次提交无关的修改，让提交能够专注于某一特定的范围。

#### 移除本地修改

下面的这个栗子显示了一个更高端的用法。它展示了你作了大死之后应该如何扔掉那几个更新。

```
# 创建一个叫`foo.py`的新文件，增加代码

# 提交到项目历史
git add foo.py
git commit -m "Start developing a crazy feature"

# 再次编辑`foo.py`，修改其他文件

# 提交另一份快照
git commit -a -m "Continue my crazy feature"

# 决定废弃这个功能，并删除相关的更改
git reset --hard HEAD~2
```

`git reset HEAD~2` 命令将当前分支向前倒退两个提交，相当于在项目历史中移除刚创建的这两个提交。记住，这种重设只能用在 *非公开* 的提交中。绝不要在将提交推送到共享仓库之后执行上面的操作。

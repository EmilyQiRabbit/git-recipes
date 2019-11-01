# 代码回滚：Reset、Checkout、Revert 的选择

## TLDR（Too Long Don't Read）

下面这个表格总结了这些命令最常用的使用场景。

| 命令 | 作用域 | 常用情景 |
| :-: | :-: | :- |
| git reset | 提交层面 | 在私有分支上舍弃一些没有提交的更改 |
| git reset | 文件层面 | 将文件从缓存区中移除 |
| git checkout | 提交层面 | 切换分支或查看旧版本 |
| git checkout | 文件层面 | 舍弃工作目录中的更改 |
| git revert | 提交层面 | 在公共分支上回滚更改 |
| git revert | 文件层面 | （然而并没有） |

## 提交层面的操作

### reset

```
git checkout hotfix
git reset HEAD~2
```

![把hotfix分支reset到HEAD~2](https://wac-cdn.atlassian.com/dam/jcr:4c7d368e-6e40-4f82-a315-1ed11316cf8b/02-updated.png)

### checkout

![将 HEAD 从 master 移到 hotfix](https://wac-cdn.atlassian.com/dam/jcr:607f1b83-ee7d-494a-b7e2-338d810059fb/04-updated.png)

![将 HEAD 移动到任意 commit](https://wac-cdn.atlassian.com/dam/jcr:3034be0a-fc7b-4c64-b9cd-3ebc8abf3833/05.svg)

## 文件层面的操作

### reset

当检测到文件路径时，`git reset` 将缓存区同步到你指定的那个提交。比如，下面这个命令会将倒数第二个提交中的 `foo.py` 加入到缓存区中，供下一个提交使用。

### checkout

![将文件从提交历史移动到工作目录中](https://wac-cdn.atlassian.com/dam/jcr:cc252fc0-fc76-4740-8458-9c0d7af94bca/08.svg)

比如，下面这个命令将工作目录中的 `foo.py` 同步到了倒数第二个提交中的 `foo.py`。

```
git checkout HEAD~2 foo.py
```

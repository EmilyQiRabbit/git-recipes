## git init

### 用法

``` shell
git init
```

👆将当前的目录转换成一个 Git 仓库。**它在当前的目录下增加了一个 `.git` 目录**，于是就可以开始记录项目版本了。

``` shell
git init <directory>
```

👆在指定目录创建一个空的 Git 仓库。运行这个命令会创建一个名为 `directory`，只包含 `.git` 子目录的空目录。

``` shell
git init --bare <directory>
```

👆初始化一个裸的 Git 仓库，但是忽略工作目录。**共享的仓库应该总是用 `--bare` 标记创建（见下面的讨论）**。一般来说，用 `—bare` 标记初始化的仓库以 `.git` 结尾。

### git init --bare 和 git init 的区别

摘自 [stackoverflow](https://stackoverflow.com/questions/7861184/what-is-the-difference-between-git-init-and-git-init-bare/7861254#7861254?newreg=d9b17c5ef54743c9aba6f24dfb4264f4)

> **Non-Bare Git Repo**
> This variant creates a repository with a working directory so you can actually work (git clone). After creating it you will see that the directory contains a .git folder where the history and all the git plumbing goes. You work at the level where the .git folder is.

---

> **Bare Git Repo**
> The other variant creates a repository without a working directory (git clone --bare). **You don't get a directory where you can work**. **Everything in the directory is now what was contained in the .git folder in the above case**.

---

> **Why You Would Use One vs. the Other**
> The need for git repos without a working directory is the fact that **you can push branches to it and it doesn't manage what someone is working on**. You still can push to a repository that's not bare, but you will get rejected as you can potentially move a branch that someone is working on in that working directory.

> So in a project with no working folder, you can only see the objects as git stores them. They are compressed and serialized and stored under the SHA1 (a hash) of their contents. In order to get an object in a bare repository, you need to git show and then specify the sha1 of the object you want to see. You won't see a structure like what your project looks like.

> **Bare repositories are usually central repositories where everyone moves their work to**. There is no need to manipulate the actual work. It's a way to synchronize efforts between multiple people. You will not be able to directly see your project files.

> You may not have the need for any bare repositories if you are the only one working on the project or you don't want/need a "logically central" repository. One would prefer git pull from the other repositories in that case. This avoids the objections that git has when pushing to non-bare repositories.

总之就是...

`-—bare` 标记创建了一个没有工作目录的仓库，这样我们在仓库中更改文件并且提交了。中央仓库应该总是创建成裸仓库，因为向非裸仓库推送分支有可能会覆盖已有的代码变动。将-—bare看成是用来将仓库标记为储存设施，而不是一个开发环境。也就是说，对于所有的 Git 工作流，中央仓库是裸仓库，开发者的本地仓库是非裸仓库。


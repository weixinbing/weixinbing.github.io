---
layout: wiki
title: Git
categories: Git
description: Git 常用操作记录。
keywords: Git, 版本控制
---

[目前为止最好的 Git 教程](https://github.com/pcottle/learnGitBranching)

git <命令> [<参数>]

### Git Commit

Git 仓库中的提交记录保存的是你的目录下所有文件的快照，就像是把整个目录复制，然后再粘贴一样，但比复制粘贴优雅许多！

Git 希望提交记录尽可能地轻量，因此在你每次进行提交时，它并不会盲目地复制整个目录。条件允许的情况下，它会将当前版本与仓库中的上一个版本进行对比，并把所有的差异打包到一起作为一个提交记录。

Git 还保存了提交的历史记录。这也是为什么大多数提交记录的上面都有父节点的原因 —— 我们会在图示中用箭头来表示这种关系。对于项目组的成员来说，维护提交历史对大家都有好处。

关于提交记录太深入的东西咱们就不再继续探讨了，现在你可以把提交记录看作是项目的快照。提交记录非常轻量，可以快速地在这些提交记录之间切换！

用 `git commit -m "提交日志"` 来提交记录

```ruby
git commit
    --amend
    -a
    --all
    -am
    -m
```

### Git Branch

Git 的分支也非常轻量。它们只是简单地指向某个提交纪录 —— 仅此而已。所以许多 Git 爱好者传颂：

早建分支！多用分支！
这是因为即使创建再多分的支也不会造成储存或内存上的开销，并且按逻辑分解工作到不同的分支要比维护那些特别臃肿的分支简单多了。

在将分支和提交记录结合起来后，我们会看到两者如何协作。现在只要记住使用分支其实就相当于在说：“我想基于这个提交以及它所有的父提交进行新的工作。”

用 `git branch <分支名>` 来创建分支，用 `git checkout <分支名>` 来切换到分支

对了，有个更简洁的方式：如果你想创建一个新的分支同时切换到新创建的分支的话，可以通过 `git checkout -b <your-branch-name>` 来实现。

```ruby
git branch
    -d
    -D
    -f
    --force
    -a
    -r
    -u
    --contains
```

### Git Merge

太好了! 我们已经知道如何提交以及如何使用分支了。接下来咱们看看如何将两个分支合并到一起。就是说我们新建一个分支，在其上开发某个新功能，开发完成后再合并回主线。

咱们先来看一下第一种方法 —— git merge。在 Git 中合并两个分支时会产生一个特殊的提交记录，它有两个父节点。翻译成自然语言相当于：“我要把这两个父节点本身及它们所有的祖先都包含进来。”

用 `git merge <分支名>` 来合并分支

```ruby
git merge
    --no-ff
```

### Git Rebase

第二种合并分支的方法是 git rebase。Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去。

Rebase 的优势就是可以创造更线性的提交历史，这听上去有些难以理解。如果只允许使用 Rebase 的话，代码库的提交历史将会变得异常清晰。

咱们还是实际操作一下吧……

用 `git rebase <分支名>` 来合并分支

### 分离 HEAD

在提交树上移动

在接触 Git 更高级功能之前，我们有必要先学习在你项目的提交树上前后移动的几种方法。

一旦熟悉了如何在 Git 提交树上移动，你驾驭其它命令的能力也将水涨船高！

#### HEAD

我们首先看一下 “HEAD”。 HEAD 是一个对当前检出记录的符号引用 —— 也就是指向你正在其基础上进行工作的提交记录。

HEAD 总是指向当前分支上最近一次提交记录。大多数修改提交树的 Git 命令都是从改变 HEAD 的指向开始的。

HEAD 通常情况下是指向分支名的（如 bugFix）。在你提交时，改变了 bugFix 的状态，这一变化通过 HEAD 变得可见。

如果想看 HEAD 指向，可以通过 cat .git/HEAD 查看， 如果 HEAD 指向的是一个引用，还可以用 git symbolic-ref HEAD 查看它的指向。

#### 分离的 HEAD

```ruby
git checkout
    -b
    -B
    -
```

### 相对引用

通过指定提交记录哈希值的方式在 Git 中移动不太方便。在实际应用时，并没有像本程序中这么漂亮的可视化提交树供你参考，所以你就不得不用 git log 来查查看提交记录的哈希值。

并且哈希值在真实的 Git 世界中也会更长（译者注：基于 SHA-1，共 40 位）。例如前一关的介绍中的提交记录的哈希值可能是 fed2da64c0efc5293610bdd892f82a58e8cbc5d8。舌头都快打结了吧...

比较令人欣慰的是，Git 对哈希的处理很智能。你只需要提供能够唯一标识提交记录的前几个字符即可。因此我可以仅输入 fed2 而不是上面的一长串字符。

正如我前面所说，通过哈希值指定提交记录很不方便，所以 Git 引入了相对引用。这个就很厉害了!

使用相对引用的话，你就可以从一个易于记忆的地方（比如 bugFix 分支或 HEAD）开始计算。

相对引用非常给力，这里我介绍两个简单的用法：

使用 ^ 向上移动 1 个提交记录

使用 ~<num> 向上移动多个提交记录，如 ~3

`git checkout master^`

“~”操作符

如果你想在提交树中向上移动很多步的话，敲那么多 ^ 貌似也挺烦人的，Git 当然也考虑到了这一点，于是又引入了操作符 ~。

该操作符后面可以跟一个数字（可选，不跟数字时与 ^ 相同，向上移动一次），指定向上移动多少次。咱们还是通过实际操作看一下吧

咱们用 ~<num> 一次后退四步。

`git checkout HEAD~4`

### 强制修改分支位置

你现在是相对引用的专家了，现在用它来做点实际事情。

我使用相对引用最多的就是移动分支。可以直接使用 -f 选项让分支指向另一个提交。例如:

`git branch -f master HEAD~3`

上面的命令会将 master 分支强制指向 HEAD 的第 3 级父提交。

相对引用为我们提供了一种简洁的引用提交记录 C1 的方式， 而 -f 则容许我们将分支强制移动到那个位置。

### 撤销变更

在 Git 里撤销变更的方法很多。和提交一样，撤销变更由底层部分（暂存区的独立文件或者片段）和上层部分（变更到底是通过哪种方式被撤销的）组成。我们这个应用主要关注的是后者。

主要有两种方法用来撤销变更 —— 一是 git reset，还有就是 git revert。接下来咱们逐个进行讲解。

#### Git Reset

it reset 通过把分支记录回退几个提交记录来实现撤销改动。你可以将这想象成“改写历史”。git reset 向上移动分支，原来指向的提交记录就跟从来没有提交过一样。

`git reset HEAD~1`

#### Git Revert

虽然在你的本地分支中使用 git reset 很方便，但是这种“改写历史”的方法对大家一起使用的远程分支是无效的哦！

为了撤销更改并分享给别人，我们需要使用 git revert。来看演示：

`git revert HEAD`

### 整理提交记录

到现在我们已经学习了 Git 的基础知识 —— 提交、分支以及在提交树上移动。 这些概念涵盖了 Git 90% 的功能，同样也足够满足开发者的日常需求

然而, 剩余的 10% 在处理复杂的工作流时(或者当你陷入困惑时）可能就显得尤为重要了。接下来要讨论的这个话题是“整理提交记录” —— 开发人员有时会说“我想要把这个提交放到这里, 那个提交放到刚才那个提交的后面”, 而接下来就讲的就是它的实现方式，非常清晰、灵活，还很生动。

看起来挺复杂, 其实是个很简单的概念。

#### Git Cherry-pick

本系列的第一个命令是 git cherry-pick, 命令形式为:

`git cherry-pick <提交号>...`
如果你想将一些提交复制到当前所在的位置（HEAD）下面的话， Cherry-pick 是最直接的方式了。我个人非常喜欢 cherry-pick，因为它特别简单。


## Git 命令大全

### 创建仓库

`git init` 创建 git 仓库

### 添加提交

`git add <file>` 添加(从工作区到暂存区,可多次使用添加多个文件)
`git add .` 添加所有文件/更改到暂存区
`git commit -m “description”` 提交(从暂存区到本地仓库)
`git commit --amend` 修改上次 commit

### 查看信息

`git remote -v` 查看本地远程仓库配置
`git config --list` 查看配置信息
`git ls-files` 查看文件列表
`git status` 查看仓库当前状态
`git diff` 或 `git diff <file>`  比较工作区和暂存区
`git diff HEAD` 或 `git diff HEAD — <file>` 比较工作区和版本库
`git diff --cached` 比较暂存区和版本库

`git log` 查看提交历史
`git log —pretty=oneline` 查看提交日志(单行显示每一条日志)
`git log —graph` 查看分支的合并情况
`git log —graph —pretty=oneline` 查看分支的合并情况(单行)
`git log —graph —pretty=oneline —abbrev-commit` 查看分支的合并情况(单行短 id)
`git reflog` 查看命令历史, 列出 HEAD 曾指向过的一系列 commit

### 版本回退

`git reset —hard HEAD^` 回滚到上个版本
`git reset —hard HEAD^^` 回滚到上上个版本
`git reset —hard HEAD~n` 回滚到上 n 个版本
`git reset —hard <commit id>` 回滚到指定提交 id 的版本

### 撤销修改

`rm <file>` 删除工作区文件
`git checkout — <file>` 撤销(丢弃)工作区的修改(让工作区和 HEAD 保持一致)
`git reset HEAD <file>` 撤销暂存区的文件(个人理解为撤销 git add 命令)
`git rm <file>` 从版本库中删除文件(会同时删除工作区文件,个人理解为 rm <file>命令 + git add <file>命令 )

### 远程仓库

`git remote add origin <SSH/HTTPS>` 关联远程仓库
`git push -u origin master` 第一次推送 master 分支的所有内容
`git push origin master` 推送 master 分支的所有内容(origin 为远程库)
`git pull --rebase origin master` 拉取 master 分支的内容(--rebase 不会产生一个merge提交记录)
`git pull` 拉取关联的远程分支内容
`git branch —set-upstream branch-name origin/branch-name` 设置本地分支和远程分支的链接关系
`git clone <SSH/HTTPS>` 克隆远程仓库

### 分支管理

`git branch` 查看分支(列出所有分支,当前分支前有\*标记)
`git branch dev` 创建一个 dev 分支
`git checkout dev` 或 `git switch dev` 切换到 dev 分支
`git checkout -b dev` 或 `git switch -c dev` 创建并切换到 dev 分支
`git checkout -b dev origin/dev` 创建远程 origin 的 dev 分支到本地
`git merge dev` 合并指定分支到当前分支
`git merge —no-ff -m “desc” dev` 合并 dev 分支到当前分支并禁用”fast forward"
`git branch -d dev` 删除 dev 分支
`git branch -D dev` 强行删除一个没有被合并过的 dev 分支

### 贮藏管理

`git stash` 贮藏当前工作区的修改(可多次 stash)
`git stash list` 查看贮藏列表
`git stash apply` 恢复 stash 内容(不删除 stash 内容)
`git stash drop` 删除 stash 内容
`git stash pop` 恢复并删除 stash 内容

### 标签管理

`git tag <name>` 给当前分支上最新的的 commitId 打上标签
`git tag` 查看所有标签(按字母排序,不按时间)
`git tag <name> <commitId>` 给对应的 commitId 打上标签
`git show <name>` 查看标签信息
`git tag -a <name> -m “desc” <commitId>` 创建带有说明的标签(-a 指定标签名,-m 指定说明文字)
`git tag -s <name> -m “desc” <commitId>` 用 PGP 签名标签
`git tag -d <name>` 删除标签
`git push origin :refs/tags/<name>` 删除远程标签(需要先删除本地标签)
`git push origin <name>` 推送标签到远程
`git push origin —tags` 推送所有标签到远程

### 忽略文件

`git add -f <name>` 强制添加被.gitignore 忽略不能添加的文件
`git check-ignore -v <name>` 查看文件被忽略的原因

### 别名定义

`git config —global alias.st status` 配置查看仓库状态的别名
`git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`

## Q&A

### 如何解决 gitk 中文乱码，git ls-files 中文文件名乱码问题？

在 `~/.gitconfig` 中添加如下内容

```
[core]
   quotepath = false
[gui]
   encoding = utf-8
[i18n]
   commitencoding = utf-8
[svn]
   pathnameencoding = utf-8
```

参考 <http://zengrong.net/post/1249.htm>

### 如何处理本地有更改需要从服务器合入新代码的情况？

```
git stash
git pull
git stash pop
```

### stash

查看 stash 列表：

```
git stash list
```

查看某一次 stash 的改动文件列表（不传最后一个参数默认显示最近一次）：

```
git stash show stash@{0}
```

以 patch 方式显示改动内容

```
git stash show -p stash@{0}
```

### 如何合并 fork 的仓库的上游更新？

```
git remote add upstream https://upstream-repo-url
git fetch upstream
git merge upstream/master
```

### 如何通过 TortoiseSVN 带的 TortoiseMerge.exe 处理 git 产生的 conflict？

- 将 TortoiseMerge.exe 所在路径添加到 `path` 环境变量。
- 运行命令 `git config --global merge.tool tortoisemerge` 将 TortoiseMerge.exe 设置为默认的 merge tool。
- 在产生 conflict 的目录运行 `git mergetool`，TortoiseMerge.exe 会跳出来供你 resolve conflict。

  > 也可以运行 `git mergetool -t vimdiff` 使用 `-t` 参数临时指定一个想要使用的 merge tool。

### 不想跟踪的文件已经被提交了，如何不再跟踪而保留本地文件？

`git rm --cached /path/to/file`，然后正常 add 和 commit 即可。

### 如何不建立一个没有 parent 的 branch？

```
git checkout --orphan newbranch
```

此时 `git branch` 是不会显示该 branch 的，直到你做完更改首次 commit。比如你可能会想建立一个空的 gh-pages branch，那么：

```
git checkout --orphan gh-pages
git rm -rf .
// add your gh-pages branch files
git add .
git commit -m "init commit"
```

### submodule 的常用命令

**添加 submodule**

```
git submodule add git@github.com:philsquared/Catch.git Catch
```

这会在仓库根目录下生成如下 .gitmodules 文件并 clone 该 submodule 到本地。

```
[submodule "Catch"]
path = Catch
url = git@github.com:philsquared/Catch.git
```

**更新 submodule**

```
git submodule update
```

当 submodule 的 remote 有更新的时候，需要

```
git submodule update --remote
```

当在本地拉取了 submodule 的远程更新，但是想反悔时：

```
git submodule update --init
```

**删除 submodule**

在 .gitmodules 中删除对应 submodule 的信息，然后使用如下命令删除子模块所有文件：

```
git rm --cached Catch
```

**clone 仓库时拉取 submodule**

```
git submodule update --init --recursive
```

### 删除远程 tag

```
git tag -d v0.0.9
git push origin :refs/tags/v0.0.9
```

或

```
git push origin --delete tag [tagname]
```

### 基于某次 commit 创建 tag

```
git tag <tag name> <commit id>
```

```
git tag v1.0.0 ef0120
```

### 清除未跟踪文件

```
git clean
```

可选项：

| 选项                    | 含义                             |
| ----------------------- | -------------------------------- |
| -q, --quiet             | 不显示删除文件名称               |
| -n, --dry-run           | 试运行                           |
| -f, --force             | 强制删除                         |
| -i, --interactive       | 交互式删除                       |
| -d                      | 删除文件夹                       |
| -e, --exclude <pattern> | 忽略符合 <pattern> 的文件        |
| -x                      | 清除包括 .gitignore 里忽略的文件 |
| -X                      | 只清除 .gitignore 里忽略的文件   |

### 忽略文件属性更改

因为临时需求对某个文件 chmod 了一下，结果这个就被记为了更改，有时候这是想要的，有时候这会造成困扰。

```
git config --global core.filemode false
```

参考：[How do I make Git ignore file mode (chmod) changes?](http://stackoverflow.com/questions/1580596/how-do-i-make-git-ignore-file-mode-chmod-changes)

### 忽略除某后缀名以外的所有文件

忽略除了 .c 后缀名以外的所有文件。

```
*
!*.c
!*/
```

gitignore 里，\*、?、[] 可用作通配符。

### patch

将未添加到暂存区的更改生成 patch 文件：

```
git diff > demo.patch
```

将已添加到暂存区的更改生成 patch 文件：

```
git diff --cached > demo.patch
```

合并上面两条命令生成的 patch 文件包含的更改：

```
git apply demo.patch
```

将从 HEAD 之前的 3 次 commit 生成 3 个 patch 文件：

（HEAD 可以换成 sha1 码）

```
git format-patch -3 HEAD
```

生成 af8e2 与 eaf8e 之间的 commits 的 patch 文件：

（注意 af8e2 比 eaf8e 早）

```
git format-patch af8e2..eaf8e
```

合并 format-patch 命令生成的 patch 文件：

```
git am 0001-Update.patch
```

与 `git apply` 不同，这会直接 add 和 commit。

### 只下载最新代码

```
git clone --depth 1 git://xxxxxx
```

这样 clone 出来的仓库会是一个 shallow 的状态，要让它变成一个完整的版本：

```
git fetch --unshallow
```

或

```
git pull --unshallow
```

### 基于某次 commit 创建分支

```sh
git checkout -b test 5234ab
```

表示以 commit hash 为 `5234ab` 的代码为基础创建分支 `test`。

### 恢复单个文件到指定版本

```sh
git reset 5234ab MainActivity.java
```

恢复 MainActivity.java 文件到 commit hash 为 `5234ab` 时的状态。

### 设置全局 hooks

```sh
git config --global core.hooksPath C:/Users/mazhuang/git-hooks
```

然后把对应的 hooks 文件放在最后一个参数指定的目录即可。

比如想要设置在 commit 之前如果检测到没有从服务器同步则不允许 commit，那在以上目录下建立文件 pre-commit，内容如下：

```sh
#!/bin/sh

CURRENT_BRANCH=$(git rev-parse --abbrev-ref HEAD)

git fetch origin $CURRENT_BRANCH

HEAD=$(git rev-parse HEAD)
FETCH_HEAD=$(git rev-parse FETCH_HEAD)

if [ "$FETCH_HEAD" = "$HEAD" ];
then
    echo "Pre-commit check passed"
    exit 0
fi

echo "Error: you need to update from remote first"

exit 1
```

### 查看某次 commit 的修改内容

```sh
git show <commit-hash-id>
```

### 查看某个文件的修改历史

```sh
git log -p <filename>
```

### 查看最近两次的修改内容

```sh
git log -p -2
```

### 应用已存在的某次更改 / merge 某一个 commit

```sh
git cherry-pick <commit-hash-id>
```

cherry-pick 有更多详细的用法，可以参见帮助文档。

### 命令行自动补全

在 shell 里加载 git-completion 系列脚本，详见 <https://github.com/git/git/tree/master/contrib/completion>

### 文件每一行变更明细

```sh
git blame <filename>
```

### 找回曾经的历史

```sh
git reflog
```

列出 HEAD 曾指向过的一系列 commit，它们只存在于本机，不是版本仓库的一部分。

还有：

```sh
git fsck
```

### 记住 http(s) 方式的用户名密码

在有些情况下无法使用 git 协议，比如公司的 git 服务器设置了 IP 白名单，只能在公司内网使用 ssh，那么在外面就只能使用 http(s) 上传下载源码了，但每次都手动输入用户名/密码特别惨，于是乎就记住吧。

设置记住密码（默认 15 分钟）：

```sh
git config --global credential.helper cache
```

自定义记住的时间（如下面是一小时）：

```sh
git config credential.helper 'cache --timeout=3600'
```

长期存储密码：

```sh
git config --global credential.helper store
```

### git commit 使用 vim 编辑 commit message 中文乱码

这个问题在 Windows 下出现了，没找到能完美解决的办法，一种方法是在 vim 打开后输入：

```sh
:set termencoding=GBK
```

这就有点太麻烦了，折衷的方法是改为使用 gVim 或其它你喜欢的编辑器来编辑 commit message：

```sh
git config --global core.editor gvim
```

参考：

- [How do I make Git use the editor of my choice for commits?](https://stackoverflow.com/questions/2596805/how-do-i-make-git-use-the-editor-of-my-choice-for-commits)
- [转：git windows 中文 乱码问题解决汇总](http://www.cnblogs.com/youxin/p/3227961.html)

另外在升级 Vim 到 8.1 之后，由于 PATH 环境变量里加的还是 vim80 文件夹，导致 git commit 时提示：

```
error: cannot spawn gvim: No such file or directory
error: unable to start editor 'gvim'
Please supply the message using either -m or -F option.
```

使用 `which gvim` 查看：

```
$ which gvim
/usr/bin/which: no gvim in xxxxxxx
```

将 PATH 里添加的 vim80 路径改为 vim81 后解决。

### git log 中文乱码

只在 Windows 下遇到。

```sh
git config --global i18n.logoutputencoding gbk
```

编辑 git 安装目录下 etc/profile 文件，在最后添加如下内容：

```
export LESSCHARSET=utf-8
```

参考：[Git for windows 中文乱码解决方案](https://segmentfault.com/a/1190000000578037)

### git diff 中文乱码

只在 Windows 下遇到，目前尚未找到有效办法。

### 统计代码行数

CMD 下直接执行可能失败，可以在右键，Git Bash here 里执行。

#### 统计某人的代码提交量

```sh
git log --author="$(git config --get user.name)" --pretty=tformat: --numstat | gawk '{ add += $1 ; subs += $2 ; loc += $1 - $2 } END { printf "added lines: %s removed lines : %s total lines: %s\n",add,subs,loc }'
```

#### 仓库提交都排名前 5

如果看全部，去掉 head 管道即可。

```sh
git log --pretty='%aN' | sort | uniq -c | sort -k1 -n -r | head -n 5
```

#### 仓库提交者（邮箱）排名前 5

这个统计可能不太准，可能有同名。

```sh
git log --pretty=format:%ae | gawk -- '{ ++c[$0]; } END { for(cc in c) printf "%5d %s\n",c[cc],cc; }' | sort -u -n -r | head -n 5
```

#### 贡献者排名

```sh
git log --pretty='%aN' | sort -u | wc -l
```

#### 提交数统计

```sh
git log --oneline | wc -l
```

参考：[Git 代码行统计命令集](http://blog.csdn.net/Dwarven/article/details/46550117)

### 修改文件名时的大小写问题

修改文件名大小写时，默认会被忽略（在 Windows 下是这样），让 git 对大小写敏感的方法：

```sh
git config --global core.ignorecase false
```

或者使用 `git mv oldname newname` 也是可以的。

### 修复 gitk 在 macOS 下显示模糊的问题

gitk 很方便，但是在 Mac 系统下默认显示很模糊，影响体验。

根据网上搜索的结果，解决方法有两种，我采用第一种解决，第二种未尝试。

方法一：

1. 重新启动机器，按 command + R 等 Logo 和进度条出现，会进入 Recovery 模式，选择顶部的实用工具——终端，运行以下命令：

   ```sh
   csrutil disable
   ```

2. 重新启动机器。

3. 编辑 Wish 程序的 plist，启动高分辨率屏支持。

   ```
   sudo gvim /System/Library/Frameworks/Tk.framework/Versions/Current/Resources/Wish.app/Contents/Info.plist
   ```

   在最后的 </dict> 前面加上以下代码

   ```sh
   <key>NSHighResolutionCapable</key>
   <true/>
   ```

4. 更新 Wish.app。

   ```sh
   sudo touch Wish.app
   ```

5. 再次用 1 步骤的方法进入 Recovery 模式，执行 `csrutil enable` 启动对系统文件保护，再重启即可。

参考：[Mac 中解决 gitk 模糊问题](http://roshanca.com/2017/make-gitk-retina-in-mac/)

方法二：

```sh
brew cask install retinizer
open /System/Library/Frameworks/Tk.framework/Versions/Current/Resources/
```

打开 retinizer，将 Wish.app 拖到 retinizer 的界面。

参考：[起底 Git-Git 基础](http://yanhaijing.com/git/2017/02/09/deep-git-4/)

### clone 时指定 master 以外的分支

```sh
git clone -b <branch name> --single-branch <repo address>
```

### 获取当前分支名称

```sh
git symbolic-ref --short -q HEAD
```

### 解决 no man viewer handled the request

运行命令 `git stash --help` 报错：

```sh
warning: failed to exec 'man': Invalid argument
fatal: no man viewer handled the request
```

原因是 Windows 下没有 man 命令。

可以修改 git 配置让命令的帮助文档通过浏览器打开。

```
git config --global help.format web
```

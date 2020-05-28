---
layout: wiki
title: Git
categories: Git
description: Git 常用操作记录。
keywords: Git, 版本控制
---

[目前为止最好的 Git 教程游戏](https://github.com/pcottle/learnGitBranching)

## 起步

### 关于版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

> 集中化的版本控制系统（Centralized Version Control Systems，简称 CVCS）
>诸如 CVS、Subversion 以及 Perforce 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

> 分布式版本控制系统（Distributed Version Control System，简称 DVCS）
>像 Git、Mercurial、Bazaar 以及 Darcs 等，客户端并不只提取最新版本的文件快照， 而是把代码仓库完整地镜像下来，包括完整的历史记录。

### 初次运行 Git 前的配置

- 用户信息

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```ruby
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

- 检查配置信息

`git config --list`

- 查看所有的配置以及它们所在的文件：

`git config --list --show-origin`

你可以通过输入 git config <key>： 来检查 Git 的某一项配置

```ruby
$ git config user.name
John Doe
```

- 更改 pull.rebase 的默认配置

`git config --global pull.rebase true`

## Git 命令大全

### 获取 Git 仓库

`git init` 创建本地 git 仓库
`git clone <HTTPS>` 克隆现有的仓库, 默认服务器名为 origin, 会自动设置本地 master 分支跟踪远程的 master 分支

### 记录每次更新到仓库

`git status` 显示工作区及暂存区中不同状态的文件
`git status -s` 或 `git status --short` 状态简览
`git add <file>` 将文件从工作区添加到暂存区（或称为索引（index）区）
`git add .` 添加所有文件/更改到暂存区

`git diff` 或 `git diff <file>` 比较工作区和暂存区的差异
`git diff HEAD` 或 `git diff HEAD — <file>` 比较工作区和版本库的差异
`git diff --staged` 或 `git diff --cached` 比较暂存区和版本库的差异（ --staged 和 --cached 是同义词）

`git commit` 提交更新 (从暂存区到本地仓库) 会启动你选择的文本编辑器来输入提交说明
`git commit -m 'desc'` -m 选项，将提交信息与命令放在同一行
`git commit -a -m 'desc'` -a 选项，跳过 git add 步骤, 自动把所有已经跟踪过的文件暂存起来一并提交

`rm <file>` 删除工作区文件
`git rm <file>` 从版本库中删除文件(会同时删除工作区文件), 个人理解为 `rm <file>`命令 + `git add <file>`命令  
`git rm -f <file>` 使用强制删除选项 -f  
`git rm --cached <file>` 从 Git 仓库中删除，但仍保留在当前工作目录中
`git mv file_from file_to` 重命名文件

### 撤销修改

`git commit --amend` 修改上次 commit
`git checkout [--] <file>...` 撤销工作区的修改(让工作区和 HEAD 保持一致), 中括号为可选
`git reset` 撤销暂存 (撤销了上一次 git add 命令)
`git reset [--] <file>` 撤销暂存区的文件, 其实是 `git reset --mixed HEAD <file>` 的简写

`git reset [--mixed] HEAD~` 默认 --mixed 选项, 撤销了上一次 git commit 和 git add 命令, 变更保留在工作区
`git reset --soft HEAD~` 撤销了上一次 git commit 命令, 变更保留在缓存区
`git reset --hard HEAD~` 回滚到上个版本, 变更不保留(**_危险用法_**)
`git reset --hard HEAD^^` 回滚到上上个版本 (HEAD^在 Mac 上会报错, 可使用 HEAD\^或 HEAD~)
`git reset --hard HEAD~n` 回滚到上 n 个版本
`git reset --hard <commit id>` 回滚到指定提交 id 的版本

### 查看提交历史

`git log` 查看提交历史
`git log -p -2` -p 或 --patch ，它会显示每次提交所引入的差异, -2 选项来只显示最近的两次提交
`git log --stat` 查看每次提交的简略统计信息
`git log —pretty=oneline` 查看提交日志(单行显示每一条日志)
`git log —graph` 查看分支的合并情况
`git log —graph —pretty=oneline` 查看分支的合并情况(单行)
`git log —graph —pretty=oneline —abbrev-commit` 查看分支的合并情况(单行短 id)
`git reflog` 查看命令历史, 列出 HEAD 曾指向过的一系列 commit

### 远程仓库

`git remote -v` 查看远程仓库
`git remote add <remote> <url>` 添加远程仓库, remote 是远程仓库地址的简写, 在后面命令行中代替 url, clone 时默认是 origin
`git remote rename <remote> < new-remote>` 远程仓库的重命名
`git remote remove <remote>` 远程仓库的删除
`git remote show <remote>` 远程仓库的信息
`git ls-remote <remote>` 远程引用的完整列表

`git fetch origin` 从远程仓库中抓取, 只会将数据下载到你的本地仓库, 不合并
`git pull` 拉取关联的远程分支内容并自动合并
`git pull --rebase` --rebase 选项 不会产生一个 merge 提交记录
`git pull origin <branch>` 拉取指定分支的内容

`git push` 推送当前分支
`git push origin <branch>` 推送指定分支
`git push -u origin <branch>` -u 选项 第一次推送指定分支
`git push origin --delete <branch>` 删除远程分支

### 分支管理

`git branch` 查看分支(列出所有分支,当前分支前有\*标记)
`git branch -v` 查看每一个分支的最后一次提交
`git branch -vv` 查看设置的所有跟踪分支
`git branch --merged` 查看所有已合并到当前分支的分支
`git branch --no-merged` 查看所有包含未合并工作的分支

`git branch dev` 创建一个 dev 分
`git branch -d dev` 删除 dev 分支
`git branch -D dev` 强行删除一个没有被合并过的 dev 分支
`git branch -u origin/<branch>` -u 或 --set-upstream-to 选项, 设置当前分支跟踪的远程分支

`git checkout dev` 或 `git switch dev` 切换到 dev 分支
`git checkout -b dev` 或 `git switch -c dev` 创建并切换到 dev 分支
`git checkout -b <branch> origin/dev` 创建一个跟踪 origin/dev 的自命名分支
`git checkout -t origin/dev` -t 或 --track 选项, 创建一个跟踪 origin/dev 的 同名分支

### 分支合并

`git merge <branch>` 合并指定分支到当前分支 (提交历史有分叉)
`git merge —no-ff -m “desc” <branch>` 合并指定分支到当前分支并禁用”fast forward"
`git rebase master` 取出当前分支的补丁, 变基到 master 分支上 (提交历史为直线)
`git rebase --onto master server dev` 取出 dev 分支，找出它从 server 分支分歧之后的补丁，放在 master 分支上
`git rebase master dev` 快进变基, 直接将 dev 分支变基到 master 分支 (能省去你先切换到 dev 分支)

### 贮藏管理

`git stash` 贮藏当前工作区的修改(可多次 stash)
`git stash list` 查看贮藏列表
`git stash apply` 恢复 stash 内容(不删除 stash 内容)
`git stash drop` 删除 stash 内容
`git stash pop` 恢复并删除 stash 内容
`git stash branch <branch>` 创建一个新分支，检出贮藏工作时所在的提交

### 标签管理

`git tag` 查看所有标签(按字母排序,不按时间)
`git tag -l "v1.8.5*"` 只查看 v1.8.5 系列标签
`git show <tagname>` 查看标签信息

`git tag <tagname>` 创建轻量标签
`git tag -a <tagname> -m “desc”` 创建附注标签(-a 指定标签名,-m 指定说明文字, 如果没有指定信息，会启动编辑器要求你输入信息)
`git tag -s <tagname> -m “desc”` 用 PGP 签名标签
`git tag <tagname> <commitId>` 给对应的 commitId 打上标签
`git tag -a <tagname> -m “desc” <commitId>`
`git tag -s <tagname> -m “desc” <commitId>`

`git push origin <tagname>` 推送标签到远程
`git push origin —tags` 一次性推送所有标签到远程
`git tag -d <tagname>` 删除标签
`git push origin :refs/tags/<tagname>` 删除远程标签(需要先删除本地标签)
`git push origin --delete <tagname>` 删除远程标签

`git checkout <tagname>` 检出标签

### 使用子模块

`git submodule add <url>` 添加子模块
`git clone --recurse-submodules <url>` --recurse-submodules 选项，它就会自动初始化并更新仓库中的每一个子模块， 包括可能存在的嵌套子模块
`git submodule init` 用来初始化本地配置文件
`git submodule update` 从该项目中抓取所有数据并检出父项目中列出的合适的提交
`git submodule update --init` 合并子模块初始化和更新 2 条命令
`git submodule update --init --recursive` 始化、抓取并检出任何嵌套的子模块
`git submodule update --remote` Git 默认会尝试更新 所有 子模块， 所以如果有很多子模块的话，你可以传递想要更新的子模块的名字。

### 搜索

`git grep 'key'` 默认情况下查找你工作目录的文件
`git grep -n` -n 或 --line-number 选项数来输出 Git 找到的匹配行的行号
`git grep -c` -c 或 --count 选项来让 git grep 输出概述的信息
`git grep -p` -p 或 --show-function 选项来显示每一个匹配的字符串所在的方法或函数
`git grep --break --heading` --break 和 --heading 选项来使输出更加容易阅读

`git log -S 'key' --oneline` 日志搜索, -S 选项 来显示新增和删除该字符串的提交
`git log -l 'key'` 行日志搜索, 它可以展示代码中一行或者一个函数的历史

### 忽略文件

`git add -f <name>` 强制添加被.gitignore 忽略不能添加的文件
`git check-ignore -v <name>` 查看文件被忽略的原因

### 别名定义

`git config —global alias.st status` 配置查看仓库状态的别名
`git config --global alias.co checkout`
`git config --global alias.br branch`
`git config --global alias.ci commit`
`git config --global alias.st status`
`git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`

## Q&A

### 如何解决 gitk 中文乱码，git ls-files 中文文件名乱码问题？

在 `~/.gitconfig` 中添加如下内容

```ruby
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

```ruby
git stash
git pull
git stash pop
```

### stash

查看 stash 列表：

```ruby
git stash list
```

查看某一次 stash 的改动文件列表（不传最后一个参数默认显示最近一次）：

```ruby
git stash show stash@{0}
```

以 patch 方式显示改动内容

```ruby
git stash show -p stash@{0}
```

### 如何合并 fork 的仓库的上游更新？

```ruby
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

```ruby
git checkout --orphan newbranch
```

此时 `git branch` 是不会显示该 branch 的，直到你做完更改首次 commit。比如你可能会想建立一个空的 gh-pages branch，那么：

```ruby
git checkout --orphan gh-pages
git rm -rf .
// add your gh-pages branch files
git add .
git commit -m "init commit"
```

### submodule 的常用命令

**添加 submodule**

```ruby
git submodule add git@github.com:philsquared/Catch.git Catch
```

这会在仓库根目录下生成如下 .gitmodules 文件并 clone 该 submodule 到本地。

```ruby
[submodule "Catch"]
path = Catch
url = git@github.com:philsquared/Catch.git
```

**更新 submodule**

```ruby
git submodule update
```

当 submodule 的 remote 有更新的时候，需要

```ruby
git submodule update --remote
```

当在本地拉取了 submodule 的远程更新，但是想反悔时：

```ruby
git submodule update --init
```

**删除 submodule**

在 .gitmodules 中删除对应 submodule 的信息，然后使用如下命令删除子模块所有文件：

```ruby
git rm --cached Catch
```

**clone 仓库时拉取 submodule**

```ruby
git submodule update --init --recursive
```

### 删除远程 tag

```ruby
git tag -d v0.0.9
git push origin :refs/tags/v0.0.9
```

或

```ruby
git push origin --delete tag [tagname]
```

### 基于某次 commit 创建 tag

```ruby
git tag <tag name> <commit id>
```

```ruby
git tag v1.0.0 ef0120
```

### 清除未跟踪文件

```ruby
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

```ruby
git config --global core.filemode false
```

参考：[How do I make Git ignore file mode (chmod) changes?](http://stackoverflow.com/questions/1580596/how-do-i-make-git-ignore-file-mode-chmod-changes)

### 忽略除某后缀名以外的所有文件

忽略除了 .c 后缀名以外的所有文件。

```ruby
*
!*.c
!*/
```

gitignore 里，\*、?、[] 可用作通配符。

### patch

将未添加到暂存区的更改生成 patch 文件：

```ruby
git diff > demo.patch
```

将已添加到暂存区的更改生成 patch 文件：

```ruby
git diff --cached > demo.patch
```

合并上面两条命令生成的 patch 文件包含的更改：

```ruby
git apply demo.patch
```

将从 HEAD 之前的 3 次 commit 生成 3 个 patch 文件：

（HEAD 可以换成 sha1 码）

```ruby
git format-patch -3 HEAD
```

生成 af8e2 与 eaf8e 之间的 commits 的 patch 文件：

（注意 af8e2 比 eaf8e 早）

```ruby
git format-patch af8e2..eaf8e
```

合并 format-patch 命令生成的 patch 文件：

```ruby
git am 0001-Update.patch
```

与 `git apply` 不同，这会直接 add 和 commit。

### 只下载最新代码

```ruby
git clone --depth 1 git://xxxxxx
```

这样 clone 出来的仓库会是一个 shallow 的状态，要让它变成一个完整的版本：

```ruby
git fetch --unshallow
```

或

```ruby
git pull --unshallow
```

### 基于某次 commit 创建分支

```ruby
git checkout -b test 5234ab
```

表示以 commit hash 为 `5234ab` 的代码为基础创建分支 `test`。

### 恢复单个文件到指定版本

```ruby
git reset 5234ab MainActivity.java
```

恢复 MainActivity.java 文件到 commit hash 为 `5234ab` 时的状态。

### 设置全局 hooks

```ruby
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

```ruby
git show <commit-hash-id>
```

### 查看某个文件的修改历史

```ruby
git log -p <filename>
```

### 查看最近两次的修改内容

```ruby
git log -p -2
```

### 应用已存在的某次更改 / merge 某一个 commit

```ruby
git cherry-pick <commit-hash-id>
```

cherry-pick 有更多详细的用法，可以参见帮助文档。

### 命令行自动补全

在 shell 里加载 git-completion 系列脚本，详见 <https://github.com/git/git/tree/master/contrib/completion>

### 文件每一行变更明细

```ruby
git blame <filename>
```

### 找回曾经的历史

```ruby
git reflog
```

列出 HEAD 曾指向过的一系列 commit，它们只存在于本机，不是版本仓库的一部分。

还有：

```ruby
git fsck
```

### 记住 http(s) 方式的用户名密码

在有些情况下无法使用 git 协议，比如公司的 git 服务器设置了 IP 白名单，只能在公司内网使用 ssh，那么在外面就只能使用 http(s) 上传下载源码了，但每次都手动输入用户名/密码特别惨，于是乎就记住吧。

设置记住密码（默认 15 分钟）：

```ruby
git config --global credential.helper cache
```

自定义记住的时间（如下面是一小时）：

```ruby
git config credential.helper 'cache --timeout=3600'
```

长期存储密码：

```ruby
git config --global credential.helper store
```

### git commit 使用 vim 编辑 commit message 中文乱码

这个问题在 Windows 下出现了，没找到能完美解决的办法，一种方法是在 vim 打开后输入：

```ruby
:set termencoding=GBK
```

这就有点太麻烦了，折衷的方法是改为使用 gVim 或其它你喜欢的编辑器来编辑 commit message：

```ruby
git config --global core.editor gvim
```

参考：

- [How do I make Git use the editor of my choice for commits?](https://stackoverflow.com/questions/2596805/how-do-i-make-git-use-the-editor-of-my-choice-for-commits)
- [转：git windows 中文 乱码问题解决汇总](http://www.cnblogs.com/youxin/p/3227961.html)

另外在升级 Vim 到 8.1 之后，由于 PATH 环境变量里加的还是 vim80 文件夹，导致 git commit 时提示：

```ruby
error: cannot spawn gvim: No such file or directory
error: unable to start editor 'gvim'
Please supply the message using either -m or -F option.
```

使用 `which gvim` 查看：

```ruby
$ which gvim
/usr/bin/which: no gvim in xxxxxxx
```

将 PATH 里添加的 vim80 路径改为 vim81 后解决。

### git log 中文乱码

只在 Windows 下遇到。

```ruby
git config --global i18n.logoutputencoding gbk
```

编辑 git 安装目录下 etc/profile 文件，在最后添加如下内容：

```ruby
export LESSCHARSET=utf-8
```

参考：[Git for windows 中文乱码解决方案](https://segmentfault.com/a/1190000000578037)

### git diff 中文乱码

只在 Windows 下遇到，目前尚未找到有效办法。

### 统计代码行数

CMD 下直接执行可能失败，可以在右键，Git Bash here 里执行。

#### 统计某人的代码提交量

```ruby
git log --author="$(git config --get user.name)" --pretty=tformat: --numstat | gawk '{ add += $1 ; subs += $2 ; loc += $1 - $2 } END { printf "added lines: %s removed lines : %s total lines: %s\n",add,subs,loc }'
```

#### 仓库提交都排名前 5

如果看全部，去掉 head 管道即可。

```ruby
git log --pretty='%aN' | sort | uniq -c | sort -k1 -n -r | head -n 5
```

#### 仓库提交者（邮箱）排名前 5

这个统计可能不太准，可能有同名。

```ruby
git log --pretty=format:%ae | gawk -- '{ ++c[$0]; } END { for(cc in c) printf "%5d %s\n",c[cc],cc; }' | sort -u -n -r | head -n 5
```

#### 贡献者排名

```ruby
git log --pretty='%aN' | sort -u | wc -l
```

#### 提交数统计

```ruby
git log --oneline | wc -l
```

参考：[Git 代码行统计命令集](http://blog.csdn.net/Dwarven/article/details/46550117)

### 修改文件名时的大小写问题

修改文件名大小写时，默认会被忽略（在 Windows 下是这样），让 git 对大小写敏感的方法：

```ruby
git config --global core.ignorecase false
```

或者使用 `git mv oldname newname` 也是可以的。

### 修复 gitk 在 macOS 下显示模糊的问题

gitk 很方便，但是在 Mac 系统下默认显示很模糊，影响体验。

根据网上搜索的结果，解决方法有两种，我采用第一种解决，第二种未尝试。

方法一：

1. 重新启动机器，按 command + R 等 Logo 和进度条出现，会进入 Recovery 模式，选择顶部的实用工具——终端，运行以下命令：

   ```ruby
   csrutil disable
   ```

2. 重新启动机器。

3. 编辑 Wish 程序的 plist，启动高分辨率屏支持。

   ```ruby
   sudo gvim /System/Library/Frameworks/Tk.framework/Versions/Current/Resources/Wish.app/Contents/Info.plist
   ```

   在最后的 </dict> 前面加上以下代码

   ```ruby
   <key>NSHighResolutionCapable</key>
   <true/>
   ```

4. 更新 Wish.app。

   ```ruby
   sudo touch Wish.app
   ```

5. 再次用 1 步骤的方法进入 Recovery 模式，执行 `csrutil enable` 启动对系统文件保护，再重启即可。

参考：[Mac 中解决 gitk 模糊问题](http://roshanca.com/2017/make-gitk-retina-in-mac/)

方法二：

```ruby
brew cask install retinizer
open /System/Library/Frameworks/Tk.framework/Versions/Current/Resources/
```

打开 retinizer，将 Wish.app 拖到 retinizer 的界面。

参考：[起底 Git-Git 基础](http://yanhaijing.com/git/2017/02/09/deep-git-4/)

### clone 时指定 master 以外的分支

```ruby
git clone -b <branch name> --single-branch <repo address>
```

### 获取当前分支名称

```ruby
git symbolic-ref --short -q HEAD
```

### 解决 no man viewer handled the request

运行命令 `git stash --help` 报错：

```ruby
warning: failed to exec 'man': Invalid argument
fatal: no man viewer handled the request
```

原因是 Windows 下没有 man 命令。

可以修改 git 配置让命令的帮助文档通过浏览器打开。

```ruby
git config --global help.format web
```

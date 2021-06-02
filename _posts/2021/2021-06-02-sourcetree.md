---
layout: wiki
title: Sourcetree
categories: [Sourcetree]
published: true
topmost: false
---

### 常见问题

#### 一直提示输入密码

终端中执行以下命令即可：
`git config --global credential.helper osxkeychain`

#### git pull 时产生'Merge branch 'master' of...问题

方法一:
在执行`git pull`的时候加上`–rebase`参数。这参数的意思就是在合并代码之前，先执行变基操作，成功后在进行真正的 merge 操作。(如果有冲突需要手动解决)

方法二：在你的 git bash 里执行
`git config --global pull.rebase true`
这个配置就是告诉 git 在每次 pull 前先进行 rebase 操作。这种方法和方法 1 原理一样，只不过方法 1 是每次 pull 前都要手动操作。

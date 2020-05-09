---
title: git 管理多个 SSH-KEY
categories: [Git]
published: true
topmost: false
---

项目托管的仓库多了，使用的账号多了，自然用到的 key 就不同了，比如 gitlab，bitbucket, github, 公司的 code 仓库等，所以管理好 key 很重要。

1，生成一个公司用的 SSH-Key

`ssh-keygen -t rsa -C "xxxxx@xxxxx.com" -f ~/.ssh/gitee_rsa`

2，生成一个 github 用的 SSH-Key

`ssh-keygen -t rsa -C "xxxxx@xxxxx.com" -f ~/.ssh/gitlab_rsa`

此时，.ssh 目录下应该有 4 个文件：id-rsa 和 id-rsa.pub，github-rsa 和 github-rsa.pub，分别将他们的公钥文件（id-rsa.pub，github-rsa.pub）内容配置到对应的 code 仓库上

3，添加私钥

`ssh-add ~/.ssh/gitee_rsa`
`ssh-add ~/.ssh/gitlab_rsa`

如果执行 ssh-add 时提示”Could not open a connection to your authentication agent”，可以现执行命令:

`$ ssh-agent bash`
`# 然后再运行ssh-add命令。`

可以通过 ssh-add -l 来确私钥列表
`ssh-add -l`

可以通过 ssh-add -D 来清空私钥列表
`ssh-add -D`

4，修改配置文件

若.ssh 目录下无 config 文件，那么创建
`touch config`

添加以下内容

```ruby
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitee_rsa
# gitlab
Host gitlab.com
HostName gitlab.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitlab_rsa
```

5，测试
ssh -T git@gitee.com

ssh -T git@gitlab.com

# 输出

Welcome to GitLab, your name!

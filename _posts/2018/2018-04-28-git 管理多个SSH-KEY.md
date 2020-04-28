---
title: git 管理多个SSH-KEY
categories: [git]
published: true
topmost: false
---

项目托管的仓库多了，使用的账号多了，自然用到的key就不同了，比如gitlab，bitbucket, github, 公司的code仓库等，所以管理好key很重要。

1，生成一个公司用的SSH-Key

`ssh-keygen -t rsa -C "xxxxx@xxxxx.com" -f ~/.ssh/gitee_rsa`

2，生成一个github用的SSH-Key

`ssh-keygen -t rsa -C "xxxxx@xxxxx.com" -f ~/.ssh/gitlab_rsa`

此时，.ssh目录下应该有4个文件：id-rsa和id-rsa.pub，github-rsa和github-rsa.pub，分别将他们的公钥文件（id-rsa.pub，github-rsa.pub）内容配置到对应的code仓库上

3，添加私钥

`ssh-add ~/.ssh/gitee_rsa`
`ssh-add ~/.ssh/gitlab_rsa`

如果执行ssh-add时提示”Could not open a connection to your authentication agent”，可以现执行命令:

`$ ssh-agent bash`
`# 然后再运行ssh-add命令。`

可以通过 ssh-add -l 来确私钥列表
`ssh-add -l`

可以通过 ssh-add -D 来清空私钥列表
`ssh-add -D`

4，修改配置文件

若.ssh目录下无config文件，那么创建
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
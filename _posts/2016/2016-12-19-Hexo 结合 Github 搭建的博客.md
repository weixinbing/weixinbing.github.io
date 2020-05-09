---
title: 快速搭建个人博客
tag:
categories: 教程
---

Hexo 结合 Github 搭建博客

<!-- more -->

## 配置环境

- 安装 Node.js [官网 https://nodejs.org/en/][1]
- 安装 Git（Xcode 自带有 Git）
- 注册 GitHub 账号（用于博客的远程仓库）
  [1]: https://nodejs.org/en/

## 安装 Hexo

打开终端，执行命令：

    npm install hexo-cli -g

如果报错，加上 sudo。
hexo 常用命令：

```ruby
    hexo g   #完整命令为hexo generate,用于生成静态文件
    hexo s   #完整命令为hexo server,用于启动服务器，主要用来本地预览
    hexo d   #完整命令为hexo deploy,用于将本地文件发布到github上
    hexo n   #完整命令为hexo new,用于新建一篇文章
    hexo clean   #清除生成的文件
```

## 初始化博客文件夹

    hexo init [文件夹名字]
    cd [文件夹名字]
    npm install

成功后执行下面命令，运行服务，就可以在浏览器中访问了，地址为： http://localhost:4000 ：

    hexo s

如果遇到 hexo s 无效或者错误，可能是因为没有安装 hexo server，执行下面命令，然后再试：

    npm install hexo-server --save

## 编辑 hexo 的配置文件

不要被文件内容吓到了，只需要修改以下地方：

```ruby
# Site
title: xxx
subtitle: xxx
description: xxx
author: xxx
language: zh-Hans
timezone: Asia/Shanghai
```

```ruby
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: github仓库的克隆地址
  branch: master
```

## 部署

通常情况下是先生成网站，然后部署。可以将两个步骤放到一起：

    hexo d -g

现在你就可以打开网站看到效果了。
如果提示：

    ERROR Deployer not found:git

需要要安装 hexo-deployer-git：

    npm install hexo-deployer-git --save

完成后执行部署命令 hexo d -g。

## 发布

通过以下命令新建文章：

    hexo new “文章名字"

然后用编辑器打开 source_posts 里的 md 文件编辑。
更新文章：

    hexo d -g

这样就可以把你的新文章传上去啦。

## 主题

[NexT 官方 WiKi][2]
[NexT 官方网站][3]
[2]: https://github.com/iissnan/hexo-theme-next/wiki
[3]: http://theme-next.iissnan.com/

## 绑定域名

现在默认的域名还是 xxx.github.io，是不是很没有牌面？想不想也像我一样弄一个专属域名呢，首先你得购买一个域名，xx 云都能买，看你个人喜好了。

以我的阿里云为例，如下图所示，添加两条解析记录：

| 记录类型 | 主机记录 | 解析线路(isp) | 记录值               | MX 优先级 | TTL     | 状态 |
| -------- | -------- | ------------- | -------------------- | --------- | ------- | ---- |
| CNAME    | @        | 默认          | weixinbing.github.io | --        | 10 分钟 | 正常 |
| CNAME    | www      | 默认          | weixinbing.github.io | --        | 10 分钟 | 正常 |

然后打开你的 github 博客项目，点击 settings，拉到下面 Custom domain 处，填上你自己的域名，保存

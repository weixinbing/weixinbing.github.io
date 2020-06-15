---
layout: wiki
title: MongoDB
categories: [MongoDB, Database]
description: MongoDB Wiki
keywords: MongoDB
---

Mac 环境安装 MongoDB 有两种方式:

- 下载[安装包](https://www.mongodb.com/try/download/community)
- 通过 homebrew

`MongoDB 已经宣布不再开源，从2019年9月2日开始，HomeBrew 也从核心仓库 (#43770) 当中移除了 mongodb 模块，想要继续使用 brew install mongodb 也是可以的，MongoDB 官方提供了一个单独的 HomeBrew 的社区版本安装：https://github.com/mongodb/homebrew-brew`

### 指定下载源

```ruby
brew tap mongodb/brew
```

### 安装 MongoDB

```ruby
# 安装 MongoDB 社区服务器的最新可用生产版本
brew install mongodb-community
# 当然也可以指定安装的版本
brew install mongodb-community@4.2
```

### 文件路径

配置文件：`/usr/local/etc/mongod.conf`
日志目录路径：`/usr/local/var/log/mongodb`
数据目录路径：`/usr/local/var/mongodb`

### 启动&停止

mongod 作为服务运行，它将使用上面列出的默认路径。

```ruby
[sudo] brew services start mongodb-community
[sudo] brew services stop mongodb-community
```

### 手动启动 MongoDB

如果不需要后台 MongoDB 服务，可以运行：

```ruby
[sudo] mongod --config /usr/local/etc/mongod.conf
```

注意：如果不包括–config 选项具有配置文件的路径，则 MongoDB 服务器没有默认配置文件或日志目录路径，并且将使用 /data/db.

### 运行

```ruby
$ sudo mongo
MongoDB shell version v4.2.7
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("fde32d33-c922-410c-80c8-3f60a310a5f6") }
MongoDB server version: 4.2.7
...
> 1 + 1
2
>
```

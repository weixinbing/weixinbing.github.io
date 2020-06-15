---
title: Mac 使用 MongoDB
categories: []
published: true
topmost: false
---

Mac 环境安装 MongoDB 有两种方式:

- 下载安装包
- 通过 homebrew

`MongoDB 已经宣布不再开源，从2019年9月2日开始，HomeBrew 也从核心仓库 (#43770) 当中移除了 mongodb 模块，想要继续使用 brew install mongodb 也是可以的，MongoDB 官方提供了一个单独的 HomeBrew 的社区版本安装：https://github.com/mongodb/homebrew-brew`

### 指定下载源

```ruby
brew tap mongodb/brew
```

### 安装 MongoDB

```ruby
# 安装MongoDB社区服务器的最新可用生产版本
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
brew services start mongodb-community
brew services stop mongodb-community
```

### 手动启动 MongoDB

如果不需要或不需要后台 MongoDB 服务，可以运行：

```ruby
[sudo] mongod --config /usr/local/etc/mongod.conf
```

注意：如果不包括–config 选项具有配置文件的路径，则 MongoDB 服务器没有默认配置文件或日志目录路径，并且将使用 /data/db.

### 运行

1、首先我们创建一个数据库存储目录 /data/db：

```ruby
sudo mkdir -p /data/db
```

如果你的数据库目录不是/data/db，可以通过 --dbpath 来指定。

```ruby
sudo mongod --dbpath=/data/db
```

2、启动 mongodb，默认数据库目录即为 /data/db：

```ruby
# 相当于上文中 brew services start mongodb-community
sudo mongod
```

3、再打开一个终端进入执行以下命令：

```ruby
$ mongo
MongoDB shell version v4.0.9
connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("3c12bf4f-695c-48b2-b160-8420110ccdcf") }
MongoDB server version: 4.0.9
...
> 1 + 1
2
>
```

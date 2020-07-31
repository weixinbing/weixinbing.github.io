---
title: Cocoapods 加速指南
categories: [Cocoapods]
published: true
topmost: false
---

## 1. CDN 方案

CocoaPods 1.8 版本默认方案
问题: 有时候网络环境会导致 CDN 连接失败 (亲测公司网络失败, 家庭网络可用),
如果你对于自己的网络有信心，推荐使用该方案。

## 2. 镜像源方案

两个最常用的镜像地址：
https://hub.fastgit.org (推荐)
https://github.com.cnpmjs.org

### 1. 查看 repo 的源

```ruby
$ pod repo
master

- Type: git (master)
- URL: https://github.com/CocoaPods/Specs.git
- Path: /Users/weixb/.cocoapods/repos/master
  trunk
- Type: CDN
- URL: https://cdn.cocoapods.org/
- Path: /Users/weixb/.cocoapods/repos/trunk
```

trunk 是 CDN 源，可使用 pod repo remove trunk 移除, 也可在 repos 目录下手动删除 trunk 文件夹

### 2. 删除 repo 的 master 源

```ruby
pod repo remove master
```

### 3. 克隆镜像源

```ruby
git clone https://hub.fastgit.org/CocoaPods/Specs.git ~/.cocoapods/repos/master
```

### 4. (切记) 更新 repo

```ruby
pod repo update
```

### 5. 再次查看 repo 的源

```ruby
$ pod repo
master

- Type: git (master)
- URL: https://hub.fastgit.org/CocoaPods/Specs.git
- Path: /Users/weixb/.cocoapods/repos/master
  trunk
- Type: CDN
- URL: https://cdn.cocoapods.org/
- Path: /Users/weixb/.cocoapods/repos/trunk
```

### 6. 添加 pod 索引

```ruby
pod search AFNetworking
```

### 7. 在 Git 仓库中的 Podfile 文件顶部添加:

```ruby
source 'https://hub.fastgit.org/CocoaPods/Specs.git'
trunk 的方式已经成为了新的默认。如果还想使用旧的方式，就必须每个 Podfile 顶部都指定源。
```

### 8. 执行 `pod install` 和 `pod update`

---
title: Cocoapods 使用指南
categories: [Cocoapods]
published: true
topmost: false
---

### 升级 ruby 至最新版本

```ruby
ruby -v //查看当前 ruby 版本号
sudo gem update —system //升级 ruby 环境
```

### 更换 ruby 镜像

```ruby
gem sources -l ///查看当前镜像
gem sources —remove https://rubygems.org/ //移除官方镜像
gem source —a https://gems.ruby-china.org/ //添加国内镜像
```

### 安装/卸载 cocoa pods

```ruby
//安装
sudo gem install cocoapods //安装最新稳定版本
sudo gem install cocoapods --pre //安装最新版本(包含 beta 版本)
sudo gem install cocoapods -v 1.9.3 //安装指定版本
//卸载
sudo gem uninstall cocoapods //卸载整个 cocoapods
sudo gem uninstall cocoapods -v 1.9.3 ///卸载指定 cocoapods 版本
//彻底卸载
gem list --local | grep cocoapods //查看本地安装过的 cocopods 相关东西
sudo gem uninstall cocoapods-core
... //逐个删除
```

### 设置 pod 库

```ruby
pod --version //查看当前 pod 版本
pod setup //安装本地库
```

### 查找或更新库

```ruby
pod search AFNetworking //查找指定库
pod repo update AFNetworking //更新指定库
pod repo update //更新所有库
```

### 为项目添加 pod 库管理

```ruby
pod init //为项目添加 podfile，编辑完该文件后执行下述命令
pod install //为项目安装指定的第三方库
```

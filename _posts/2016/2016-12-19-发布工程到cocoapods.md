---
title: 发布组件到 cocoapods
tags: cocoapods
categories: cocoapods
---

让自己的框架支持 cocoapods

<!-- more -->

### 在 github 建立仓库,并 clone 到本地

github 不是唯一的平台,其他平台都可以,前提是项目是开源的.

### 创建.podspec 文件

打开终端,进入仓库,执行如下命令:

> pod spec create XXX

### 编辑.podspec 文件

使用 vim 或 Xcode 打开.podspec 文件,编辑如下:

```ruby
Pod::Spec.new do |s|
s.name = "XXX"
s.version = "1.0.3"
s.ios.deployment_target = '8.0'
s.summary = "some utilities"
s.homepage = "https://github.com/weixinbing/XXX"
s.license = { :type => "MIT", :file => "LICENSE" }
s.author = { "weixb" => "183292352@qq.com" }
s.social_media_url = "http://weibo.com/u/5348162268"
s.source = { :git => "https://github.com/weixinbing/XXX.git", :tag => s.version }
s.source_files = "XXX"
s.requires_arc = true
s.dependency "YTKNetwork", "~> 2.0.3"
s.dependency 'Bugly', '~> 2.4.2'
end
```

> s.name：名称，pod search 搜索的关键词,注意这里一定要和.podspec 的名称一样,否则报错
> s.version：版本号
> s.ios.deployment_target:支持的 pod 最低版本
> s.summary: 简介
> s.homepage:项目主页地址
> s.license:许可证
> s.author:作者
> s.social_media_url:社交网址
> s.source:项目的地址
> s.source_files:需要包含的源文件
> s.resources: 资源文件
> s.requires_arc: 是否支持 ARC
> s.dependency：依赖库(可以多写)

source_files:写法及含义:

> ""XXX"和“XXX/_” 表示匹配所有文件(不包含文件夹)
> “XXX/_.{h,m}” 表示匹配所有以.h 和.m 结尾的文件(不包含文件夹)
> “XXX/\*\*” 表示匹配所有子目录(不包含文件夹)

### 验证本地仓库.podspec 文件

> pod lib lint XXX.podspec

### 上传本地仓库到 Git

### 给仓库打上 tag

> git tag '1.0.0' //第一步
> git push origin --tags //推送到远程仓库

### 验证远程仓库.podspec 文件

> pod spec lint XXX.podspec

### 发布

发布前先验证是否已注册 trunk：

> pod trunk me

没有注册则使用命令：

> pod trunk register Email "Name" --verbose
> 注册完成之后会给你的邮箱发个邮件,进入邮箱邮件里面有个链接,需要点击确认一下

已注册则执行发布命令：

> pod trunk push XXX.podspec

发布成功就可以使用 pod 导入你的框架了。

### 错误搜集

遇到错误 CocoaPods was not able to update the `master` repo. If this is an unexpected issue and persists you can inspect it running `pod repo update --verbose`
执行命令`pod repo update`即可

发布成功,执行`pod search XXX`搜索

遇到错误 Unable to find a pod with name, author, summary, or descriptionmatching "XXX"的解决方法:

> 方法 1:使用终端命令
> `rm ~/Library/Caches/CocoaPods/search_index.json` 删除 cocoapods 本地缓存,再使用`pod search XXX`搜索.
> 方法 2:使用终端命令`pod setup` 更新 cocoapods 的 repo,再重复方法 1.

依赖错误, 可以使用 --use-libraries 来让验证通过.
警告错误, 可以使用 --allow-warnings 来让验证通过.

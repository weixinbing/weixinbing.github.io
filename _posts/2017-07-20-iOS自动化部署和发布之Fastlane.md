---
title: iOS自动化部署和发布之Fastlane
date: 2017-07-20
tags: 自动化
categories: iOS
---

Fastlane
<!-- more -->

官方文档：https://docs.fastlane.tools/

#### 安装
确保安装了最新版本的Xcode命令行工具：
>xcode-select --install

安装fastlane：
>使用gem命令安装
> [sudo] gem install fastlane -NV
> 使用brew命令安装
> brew cask install fastlane

cd到工程所在目录下执行：
>fastlane init

插件安装
>firim插件
>fastlane add_plugin firim
>蒲公英插件
>fastlane add_plugin pgyer


####文件配置
##### Appfile
>Appfile用来配置一些类似于AppleID、BundleID参数(参数是fastlane已经定义好的，新增的并没有用，可以在Fastfile中使用，AppleID、BundleID等其实会被一些actions直接调用，并不需要写出来传递。

```ruby
# 默认配置
app_identifier    ""
apple_id    ""
team_id    ""

# 如果lane是test换成Dev的配置
for_lane :test do
  app_identifier    ""
  apple_id    ""
  team_id    ""
end

```

##### Fastfile
>Fastfile包含分发您的应用程序所需的所有信息。可以在before_all、after_all、error中做一些操作以建立一些lane作为关键的执行逻辑，可以在其中使用fastlane内置的action，也可以调用自建action，还可以调用别的lane。
```ruby
default_platform(:ios)

platform :ios do
    desc "Description of what the lane does"
    lane :test_firim do
    # add actions here: https://docs.fastlane.tools/actions
    build_app( 
    	workspace: "xxx.xcworkspace", 
    	scheme: "xxx", 
    	export_method: "enterprise", 
	    configuration: "Debug",# Defaults to 'Release'
        output_directory: "./build",
        output_name: "xxx",
    	)
	    firim(firim_api_token: "") #上传ipa到fir.im服务器
    end

	desc "Push a new release build to the App Store"
	lane :appstore do
		build_app(workspace: "xxx.xcworkspace", scheme: "xxx")
        upload_to_app_store
    end
  
end
```
#### 执行
>fastlane test_firim


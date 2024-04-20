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

### Podfile 详解
```ruby
# 指定specs位置, 官方 CocoaPods 来源是隐含的。一旦指定了另一个来源，就需要将其包含在内。
#source 'https://github.com/artsy/Specs.git'
#source 'https://cdn.cocoapods.org/'

# 指定平台名称和版本, 平台名称有:osx 、:ios 、:tvos 、:visionos 或 :watchos。
platform :ios, '13.0'
# 禁止所有警告！
inhibit_all_warnings!

target 'PodApp' do
  # 对 Pod 使用框架
  use_frameworks!

  # Pods for PodApp
  pod 'Then'
  # pod 'Then', :inhibit_warnings => true # 禁止单个 Pod 发出警告, false为开启警告
  # pod 'Then', :modular_headers => true # 为每个 Pod 使用模块化标头, false为不使用
  # pod 'PonyDebugger', :source => 'https://cdn.cocoapods.org/' # 指定source
  # pod 'PonyDebugger', :configurations => ['Debug', 'Beta'] # 默认在所有构建配置中
  # pod 'QueryKit', :subspecs => ['Attribute', 'QuerySet'] # 指定要安装的 subSpecs 的集合, 或者指定单个: pod 'QueryKit/Attribute'
  # pod 'AFNetworking', :testspecs => ['UnitTests', 'SomeOtherTests'] # 选择性地包含测试 specs
  # pod 'AFNetworking', :path => '~/Documents/AFNetworking' # 使用本地路径中的文件, podspec 应该位于该文件夹中
  # pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git' # 指定git库, 使用 master 分支
  # pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :branch => 'dev' # 使用 dev 分支
  # pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :tag => '0.7.0' # 使用 tag
  # pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :commit => '082f8319af' # 使用 commit
  # pod 'JSONKit', :podspec => 'https://example.com/JSONKit.podspec' # 通过 HTTP 提供的 podspec 获取

end

# 配置 CocoaPods 的安装选项
install! 'cocoapods',
  :clean => true, # 默认为 true, 安装时是否清理pod 未使用的所有文件
  :deduplicate_targets => true, # 默认为 true, 是否对 Pod 目标进行重复数据删除
  :deterministic_uuids => true, # 默认为 true, 创建Pods项目时是否生成确定性UUID
  :integrate_targets => true, # 默认为 true, 是否将安装的pod自动集成到用户项目中。如果设置为 false，Pod 将被下载并安装到 Pods/ 目录，但它们不会集成到您的项目中。
  :lock_pod_sources => true, # 默认为 true, 是否锁定pod的源文件。尝试修改文件内容时，Xcode 将提示解锁文件
  :warn_for_multiple_pod_sources => true, # 默认为 true, 当多个源包含具有相同名称和版本的 Pod 时是否发出警告
  :warn_for_unused_master_specs_repo => true, # 默认为 true, 如果项目未明确指定基于 git 的主规范存储库，并且可以使用默认的 CDN，是否发出警告。

  :share_schemes_for_development_pods => false, # 默认为 false, 是否共享开发 Pod 的 Xcode 方案。
  :disable_input_output_paths => false, # 默认为 false, 是否禁用CocoaPods脚本阶段的输入和输出路径（复制框架和复制资源）
  :preserve_pod_file_structure => false, # 默认为 false, 是否保留所有 Pod 的文件结构，包括外部来源的 Pod。
  :generate_multiple_pod_projects => false, # 默认为 false, 是否为每个 pod 目标生成一个项目
  :incremental_installation => false, # 默认为 false, 是否优先使用缓存, 仅启用重新生成自上次安装以来已更改的目标及其关联项目
  :skip_pods_project_generation => false, # 默认为 false, 是否跳过生成 Pods.xcodeproj 并仅执行依赖解析和下载。
  :parallel_pod_downloads => false, # 默认为 false, 开始安装过程之前是否并行下载 Pod
  :parallel_pod_download_thread_pool_size => 40, # 默认为 40, 并行下载 pod 时使用的线程池的大小。仅当 parallel_pod_downloads 为 true 时生效。

# 配置 hooks
pre_install do |installer|
  # Do something fancy!
end
pre_integrate do |installer|
  # perform some changes on dependencies
end
post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      # config.build_settings['GCC_ENABLE_OBJC_GC'] = 'supported'
    end
    if ['PodA', 'PodB'].include? target.name
      target.build_configurations.each do |config|
        # config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '12.0'
      end
    end
  end
end
post_integrate do |installer|
  # some change after project write to disk
end
```
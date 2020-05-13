---
title: LLDB 增强插件 Chisel
categories: [Xcode]
published: true
topmost: false
---

Chisel 是一款 Facebook 出品的 LLDB 增强插件，是 LLDB 命令的集合，可帮助调试 iOS 应用。

## 安装

    brew install chisel

安装完成后, 在 `~/.lldbinit` 文件中添加如下代码

```
# ~/.lldbinit
...
command script import /usr/local/opt/chisel/libexec/fbchisellldb.py
```

重启 Xcode

## 使用

常用命令

<img src="/images/blog/2020-05-13-LLDB增强插件chisel/2020-05-13-09-41-08.png" width="100%" />

要查看所有命令，请在`LLDB`中执行 help 命令或转到 [Wiki](https://github.com/facebook/chisel/wiki).

## 参考

[Chisel Github](https://github.com/facebook/chisel)

[与调试器共舞 - LLDB 的华尔兹](https://objccn.io/issue-19-2/)

[官方指南](https://developer.apple.com/library/archive/documentation/IDEs/Conceptual/gdb_to_lldb_transition_guide/document/Introduction.html)

[How to Extend LLDB to Provide a Better Debugging Experience](https://pspdfkit.com/blog/2018/how-to-extend-lldb-to-provide-a-better-debugging-experience/)

[各种调试器收录列表](https://github.com/MattPD/cpplinks/blob/master/debugging.md)
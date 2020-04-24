---
title: Mac代理设置-Proxifier
date: 2018-03-21 21:36:36
tags: 翻墙
categories: Mac
---



Proxifier[下载](https://www.proxifier.com/)



#### 设置代理服务器

<!-- more -->

步骤如下：

![img](https://cdn-pri.nlark.com/yuque/0/2018/png/161106/1538211563117-b5bdac91-f2d0-4025-b052-7782e165e5e0.png)



shdowsocks的端口默认1080（自定义shdowsocks端口的需要修改）

![img](https://cdn-pri.nlark.com/yuque/0/2018/png/161106/1538211643295-1902f5a5-965e-4244-b65f-dcb1e11688b5.png)



#### 设置代理规则



方案1：设置全局代理

![img](https://cdn-pri.nlark.com/yuque/0/2018/png/161106/1538211966122-c599d006-56ac-4da8-be6f-079a102ce54e.png)



方案2：设置部分代理

![img](https://cdn-pri.nlark.com/yuque/0/2018/png/161106/1538211757396-227e9712-0b74-4696-b3d1-fda9f113bf6a.png)



[参考来源](https://www.zybuluo.com/yiranphp/note/611721)：一言以蔽之，如果你在终端中发起了网络请求速度很慢的话，可以先把 default 规则，设置为socks5代理（只有设置为代理，日志才会记录），然后分析一下请求的 host：port，然后就可以添加规则了，这样的话，就做到只给部分请求走代理，如果你觉得这样很麻烦，  也可以简单粗暴的，将 default 规则设置为 socks5代理，其余的规则全部禁用，那就是全部的请求走代理
---
title: iOS 设备用 wireshark 抓包
categories: [wireshark]
published: false
---

rvictl -s -l -x

rvictl -s <UDID> //开启虚拟网卡

在 wireshark 中选中 rv0 网卡

rvictl -x <UDID> //关闭虚拟网卡

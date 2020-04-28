---
title: iOS 设备用 wireshark 抓包
categories: [wireshark]
published: false
---

rvictl -s -l -x

rvictl -s <UDID> //开启虚拟网卡

在wireshark中选中rv0网卡

rvictl -x <UDID> //关闭虚拟网卡


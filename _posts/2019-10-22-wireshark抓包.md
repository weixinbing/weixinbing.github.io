---
title: iOS设备用wireshark 抓包
categories: [wireshark]
---

rvictl -s -l -x
rvictl -s <UDID> //开启虚拟网卡
在wireshark中选中rv0网卡
rvictl -x <UDID> //关闭虚拟网卡

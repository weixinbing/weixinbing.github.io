---
title: 科学上网 Just My Socks
categories: [科学上网]
published: true
topmost: false
---

[Just My Socks 官网](https://justmysocks2.net/members/index.php?language=chinese)

[Just My Socks 使用教程](https://github.com/killgcd/justmysocks/blob/master/README.md)

[免费 ss 账号](https://github.com/bannedbook/fanqiang/wiki/%E5%85%8D%E8%B4%B9ss%E8%B4%A6%E5%8F%B7)

### Shadowsocks-NG

[github](https://github.com/shadowsocks/ShadowsocksX-NG)

[下载地址](https://github.com/shadowsocks/ShadowsocksX-NG/releases/tag/v1.9.4)

### PAC

> PAC(Proxy auto-config)，用于定义浏览器该如何自动选择适当的代理服务器来访问一个网址。本质是一个 js 文件，包含了 FindProxyForURL(url, host) 函数，该返回会返回使用直连还是某个代理。

通常，要实现代理的自动分流，那么就需要有一个判断规则，根据这个规则的结果，来决定流量的走向，是走代理还是直连。常用的判断规则就两种:

- 域名
- IP 针对流量的走向不同，又分两种方式：
- 黑名单: 符合规则走代理，不符合的规则走直连;
- 白名单: 符合规则走直连，不符合的规则走代理。

关于选择可以参考：
VPS 的连接速度很快，那么可以选择白名单。如果 VPS 连接速度很慢，可以选择黑名单。

#### 规则列表

1. 域名列表

- https://github.com/gfwlist/gfwlist
- https://github.com/v2ray/domain-list-community/
- https://github.com/n0wa11/gfw_whitelist
- https://github.com/xinhugo/Free-List

其中，gfwList 是一个被 GFW 屏蔽的域名列表，这些域名都需要通过代理才能访问。白名单中的域名可以直接访问，其余的都会走代理。
gfwlist.txt 文件被 base64 编码，解码后是 ABP 格式: https://adblockplus.org/filter-cheatsheet

2. IP 列表

- https://github.com/17mon/china_ip_list
- https://github.com/v2ray/geoip/

基本都是基于 MaxMind，IPIP.NET 等第三方的数据来生成的数据，都需要定时更新。

#### ABP 语法

```js
- “!” 为行注释符
> 注释行以该符号起始作为一行注释语义，用于规则描述。
- “|” 为管线符号
> 来表示地址的最前端或最末端 比如 “|http://“ 或 |http://www.abc.com/a.js&#124; 用于精确控制匹配的开始或结束。e.g：|http://www.abc.com 等于 |http://www.abc.com* , 可以匹配以 http://www.abc.com 开头的网址。
- “||” 为子域通配符
> 方便匹配主域名下的所有子域。比如 “||www.baidu.com" 就可以不要前面的协议”http://“。e.g: ||www.abc.com 等于 www.abc.com , 只要网址中包含 www.abc.com 就可以被匹配。
- “~” 为排除标识符
> 通配符能过滤大多数广告，但同时存在误杀, 可以通过排除标识符修正误杀链接。
- “@@” 网址白名单
> 例如不拦截此条地址 @@|http://www.baidu.com/js/u.js或者 @@||www.baidu.com/js/u.js
- “*” 为字符通配符
> 能够匹配0长度或任意长度的字符串。
- “^” 为分隔符
> 可以匹配任何单个字符。
```

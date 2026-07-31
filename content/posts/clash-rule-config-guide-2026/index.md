---
title: "Clash 规则配置完全指南：分流策略、代理组与实战优化（2026版）"
date: 2026-07-31
slug: clash-rule-config-guide-2026
tags: ["教程", "新手入门", "Clash"]
description: "Clash 规则配置从入门到精通：从 DOMAIN-SUFFIX、GEOIP 等基础规则语法到 url-test、fallback、select 策略组实战详解，包括规则链匹配顺序和优先级管理。手把手从零搭建完整分流规则体系，不用再到处抄别人的 Clash 配置，看完就能独立动手配。2026下半年最新整理。"
---

> 机场买好了、客户端装上了、节点也能连——但总觉得哪里不对？刷抖音走的也是代理、访问 GitHub 走的却是直连、流媒体打开显示"地区限制"？
>
> 这些问题八成出在**规则配置**上。Clash 的规则系统是它最强大但也最容易被忽略的功能——用好了，你不需要反复切换节点、不需要手动开关代理；用不好，就是上面那种"怎么都不对"的体验。

这篇文章从零开始讲 Clash 规则体系，覆盖 Clash Meta（目前最常见的内核）、Clash Verge（桌面端主流客户端）、Stash（iOS）、Surge 规则兼容模式。

---

## 一、为什么需要规则配置？

很多新手以为 Clash = 开全局代理。其实全局模式是体验最差的用法——国内网站绕一圈出国再回来、网盘下载也被代理拖慢、银行的 App 因为走代理被风控。

**规则系统的核心价值：** 让 Clash 自动判断每个请求应该走代理还是直连，不需要你手动干预。

| 场景 | 正确行为 | 错误行为 |
|:----|:--------|:--------|
| 访问 baidu.com | 直连（国内网站） | 走代理（慢+浪费流量） |
| 访问 youtube.com | 走代理 | 直连（打不开） |
| 访问 Netflix | 走流媒体专用节点 | 走普通节点（被检测为代理） |
| 访问 网银/iCloud | 直连 | 走代理（验证码/风控） |

规则配好了，你的网络体验会从"能用"变成"好用"。

---

## 二、Clash 规则引擎的工作原理

Clash 的请求匹配流程其实不复杂：

```
用户请求 → 匹配规则列表（从上到下） → 第一个匹配的规则决定走哪个策略组 → 策略组决定走哪个节点
```

**关键原则：规则从上到下逐条匹配，匹配到第一条就停止。** 所以**精确的规则放前面，兜底的规则放最后**。

一个典型规则链长这样：

```
# 按优先级从上到下
# 1. 必须代理的（精确域名优先）
DOMAIN-SUFFIX,google.com,PROXY
DOMAIN-SUFFIX,youtube.com,PROXY

# 2. 必须直连的
DOMAIN-SUFFIX,baidu.com,DIRECT
DOMAIN-SUFFIX,weixin.qq.com,DIRECT

# 3. 流媒体走专用节点
DOMAIN-SUFFIX,netflix.com,🎬 流媒体

# 4. IP 段规则
GEOIP,CN,DIRECT

# 5. 兜底规则（未匹配的走代理）
MATCH,PROXY
```

---

## 三、规则类型详解

Clash 支持的规则类型非常丰富，每个都有适用场景。

### 3.1 DOMAIN / DOMAIN-SUFFIX（域名规则 — 最常用）

```
DOMAIN,www.google.com,PROXY              # 精确匹配单个域名
DOMAIN-SUFFIX,google.com,PROXY           # 匹配 google.com 及其所有子域名
DOMAIN-KEYWORD,google,PROXY              # 域名包含 google 就匹配
```

**推荐用法：** `DOMAIN-SUFFIX` 是最平衡的选择——一条规则管一个域名及其所有子域名。比如 `DOMAIN-SUFFIX,github.com,PROXY` 同时命中 `github.com`、`api.github.com`、`raw.githubusercontent.com`。

### 3.2 GEOIP（IP 地理位置规则）

```
GEOIP,CN,DIRECT           # 国内 IP 直连
GEOIP,US,PROXY            # 美国 IP 走代理
GEOIP,!CN,PROXY            # 非国内 IP 走代理
```

**重要：** GEOIP 的准确度取决于你的 GeoIP 数据库。Clash 会下载 `Country.mmdb` 数据库，包含了全球 IP 段的归属信息。通常的配法是 `GEOIP,CN,DIRECT` 放规则中间段。

### 3.3 IP-CIDR（IP 段规则）

```
IP-CIDR,10.0.0.0/8,DIRECT        # 内网保留段直连
IP-CIDR,17.0.0.0/8,PROXY         # Apple 服务段走代理
IP-CIDR,198.18.0.0/15,REJECT     # 回环测试段拒绝
```

IP-CIDR 通常用于处理 GeoIP 覆盖不到的场景，或者对内网段做精准管控。

### 3.4 DST-PORT / SRC-PORT（端口规则）

```
DST-PORT,80,DIRECT                 # 目标端口 80（HTTP）直连
DST-PORT,443,PROXY                 # HTTPS 走代理
DST-PORT,853,PROXY                 # DNS-over-TLS 走代理
```

端口规则很少单独用，但配合其他规则做精细控制时很有用。

### 3.5 PROCESS-NAME（进程名规则 **Clash Meta 特有**）

```
PROCESS-NAME,WeChat.exe,DIRECT     # 微信直连
PROCESS-NAME,chrome.exe,PROXY      # Chrome 走代理
PROCESS-NAME,Steam.exe,🎮 游戏    # Steam 走游戏专用节点
```

**这是 Clash Meta 最实用的功能之一。** 不同进程走不同路线——微信/企业微信直连、浏览器走代理、Steam 走游戏专项节点。Windows 上后缀要带 `.exe`，macOS 上是 App 的进程名（如 `Google Chrome`）。

### 3.6 RULE-SET（规则集 **Clash Meta 核心功能**）

```
RULE-SET,https://example.com/reject.yaml,REJECT
RULE-SET,https://example.com/proxy.yaml,PROXY
RULE-SET,https://example.com/direct.yaml,DIRECT
```

规则集允许你把规则分类成独立的 YAML 文件，远程维护。这是 2025-2026 年最主流的做法——社区维护的规则集（如 Loyalsoldier/v2ray-rules-dat、blackmatrix7/ios_rule_script）几乎覆盖了全网所有域名，每周更新。

---

## 四、代理组（Proxy Group）策略详解

规则决定了请求走哪个**策略组**，策略组再决定最终走哪个**节点**。常见的策略组类型：

### 4.1 select（手动选择）

```
proxy-groups:
  - name: "🚀 代理"
    type: select
    proxies:
      - "🇯🇵 JP 东京"
      - "🇸🇬 SG 新加坡"
      - "🇺🇸 US 洛杉矶"
```

**特点：** 你手动选一个节点，所有走这个策略组的请求都通过它。简单直观，适合"我就想用日本节点"的场景。

### 4.2 url-test（自动测速—最常用）

```
proxy-groups:
  - name: "🚀 自动选择"
    type: url-test
    proxies:
      - "🇯🇵 JP 东京"
      - "🇸🇬 SG 新加坡"
      - "🇺🇸 US 洛杉矶"
    url: "https://www.gstatic.com/generate_204"
    interval: 300
    tolerance: 50
```

**工作原理：** 每隔 `interval` 秒对所有节点测一次延迟，自动切换到延迟最低的节点。`tolerance` 参数的意思是"延迟差距在 50ms 以内的节点之间不切换"，避免频繁切换导致连接不稳定。

### 4.3 fallback（容灾切换）

```
proxy-groups:
  - name: "🚀 容灾"
    type: fallback
    proxies:
      - "🇯🇵 JP 东京"
      - "🇸🇬 SG 新加坡"
      - "🇺🇸 US 洛杉矶"
    url: "https://www.gstatic.com/generate_204"
    interval: 300
```

**特点：** 按照 proxies 列表的顺序，只用第一个可用的节点。如果第一个挂了，自动切到第二个。这在国内网络环境波动大的场景下非常实用。

### 4.4 load-balance（负载均衡）

```
proxy-groups:
  - name: "⚖️ 负载均衡"
    type: load-balance
    proxies:
      - "🇯🇵 JP 东京"
      - "🇸🇬 SG 新加坡"
    strategy: round-robin
```

**适合：** 大流量下载、BT 等场景。日常浏览不需要负载均衡。

---

## 五、实战配置模板

下面是一份完整配置的核心框架，你可以直接参考：

```yaml
# proxy-providers（节点来源——从订阅自动拉取）
proxy-providers:
  MyProvider:
    type: http
    path: ./providers/my_nodes.yaml
    url: "你的订阅链接"
    interval: 86400
    health-check:
      enable: true
      url: https://www.gstatic.com/generate_204
      interval: 300

# proxy-groups（策略组定义）
proxy-groups:
  # 流媒体专用组
  - name: "🎬 流媒体"
    type: url-test
    use:
      - MyProvider
    filter: "流媒体|Netflix|解锁"
    url: https://www.gstatic.com/generate_204
    interval: 300

  # 游戏加速组
  - name: "🎮 游戏"
    type: fallback
    use:
      - MyProvider
    filter: "游戏|游戏加速|LowPing"
    url: https://www.gstatic.com/generate_204
    interval: 300

  # 日常代理组
  - name: "🚀 代理"
    type: select
    proxies:
      - "🎬 流媒体"
      - "🎮 游戏"
      - "🚀 自动选择"
      - "DIRECT"

  - name: "🚀 自动选择"
    type: url-test
    use:
      - MyProvider
    url: https://www.gstatic.com/generate_204
    interval: 300

# rules（规则链）
rules:
  # ===== 必须代理的（精确优先） =====
  - DOMAIN-SUFFIX,google.com,🚀 代理
  - DOMAIN-SUFFIX,youtube.com,🚀 代理
  - DOMAIN-SUFFIX,github.com,🚀 代理
  - DOMAIN-SUFFIX,twitter.com,🚀 代理
  - DOMAIN-SUFFIX,facebook.com,🚀 代理
  - DOMAIN-SUFFIX,instagram.com,🚀 代理

  # ===== 流媒体 — 走专用组 =====
  - DOMAIN-SUFFIX,netflix.com,🎬 流媒体
  - DOMAIN-SUFFIX,disney.com,🎬 流媒体
  - DOMAIN-SUFFIX,hbo.com,🎬 流媒体
  - DOMAIN-SUFFIX,primevideo.com,🎬 流媒体

  # ===== 直连 — 国内服务 =====
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-SUFFIX,qq.com,DIRECT
  - DOMAIN-SUFFIX,weixin.qq.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  - DOMAIN-SUFFIX,jd.com,DIRECT
  - DOMAIN-SUFFIX,bilibili.com,DIRECT
  - DOMAIN-SUFFIX,douyin.com,DIRECT

  # ===== 游戏平台 =====
  - DOMAIN-SUFFIX,steamcontent.com,🎮 游戏
  - DOMAIN-SUFFIX,steampowered.com,🎮 游戏
  - DOMAIN-SUFFIX,epicgames.com,🎮 游戏
  - DOMAIN-SUFFIX,easebar.com,🎮 游戏

  # ===== DNS 相关 =====
  - IP-CIDR,198.18.0.1/16,REJECT,no-resolve

  # ===== GeoIP：国内 IP 直连 =====
  - GEOIP,CN,DIRECT

  # ===== 兜底 =====
  - MATCH,🚀 代理
```

这份配置用了三个核心策略组：
- **🎬 流媒体** — url-test 自动选最快的流媒体解锁节点
- **🎮 游戏** — fallback 容灾切换，确保不掉线
- **🚀 代理** — select 手动切换主策略组

---

## 六、常见问题与避坑

### 6.1 规则生效顺序搞反

**最容易犯的错误**——把 MATCH 或 GEOIP,CN 放在规则最前面，结果所有请求都直接匹配了兜底规则。

✅ 正确：精确的放前面，宽泛的放中间，兜底的放最后
❌ 错误：先把 GEOIP,CN,DIRECT 放最前面——所有国内域名都直连了，后面的规则等于白写

### 6.2 流媒体节点必须单独分组

不要用自动选择组来看流媒体——自动切换会导致 Netflix 检测到 IP 频繁变化，直接封禁。**流媒体用专属节点**，手动选一个稳定的就行。

### 6.3 国内网站走代理导致风控

银行 App、12306、企业微信、阿里旺旺——这些如果用代理访问，大概率触发风控验证码甚至被锁号。确认规则里把这些服务加到 DIRECT 组。

### 6.4 规则更新不及时

社区规则集（如 blackmatrix7/ios_rule_script）每周更新，但如果你手动写死域名，新出来的服务就不会被正确处理。建议用 RULE-SET 引用远程规则集，或者定期手动更新。

### 6.5 代理组测速 URL 不可用

有的机场封锁了 `gstatic.com`，测速一直失败。可以换：
```
url: https://www.google.com/generate_204
url: https://captive.apple.com/hotspot-detect.html
```

---

## 七、主流的规则维护方式（2026年趋势）

2026 年的主流做法已经不是手写所有规则了：

**方式一：社区规则集（推荐）**
用 `RULE-SET` 引用 `blackmatrix7/ios_rule_script` 或 `Loyalsoldier/clash-rules` 的规则集文件，覆盖绝大多数场景。你只需要写少量自己的特殊规则。

**方式二：ACL4SSR 规则**
ACL4SSR 提供了完整的规则分类（广告/代理/直连/流媒体/游戏），可以直接在线引用。适合不想折腾但配置又不想太简陋的用户。

**方式三：自维护规则**
如果对网络有特殊要求（比如公司 VPN/内网穿透），在社区规则集的基础上加自己的规则，放在规则链的最前面（优先级更高）。

不管哪种方式，核心原则一致：**精确在前、宽泛在中、兜底在尾**。

---

## 总结

Clash 规则配置没有想象中那么复杂。记住三个要点：

1. **规则是自上而下匹配的** — 精确的放前面，兜底的放最后
2. **策略组决定了体验** — 日常用 url-test 自动切换，流媒体和游戏用专用节点
3. **直连规则一定要配好** — 国内服务走直连，既快又安全

配好之后，你的 Clash 就不再只是一个"开关"，而是一个全自动的网络分流系统。不用再每天纠结"这个网站该不该开代理"——让规则替你决定。

---

### 🌐 推荐网络加速服务

如果你还没找到合适的网络加速服务，下面几家经过长期验证，节点质量和稳定性都不错：

| 服务商 | 特点 | 月费 |
|:-----|:----|:----:|
| [自由猫](https://api.huanghaiwan.com/go/自由猫) | ⭐ 主力推荐，IEPL专线+MPTCP多路复用，100+节点 | ¥25/月起 |
| [万达云](https://api.huanghaiwan.com/go/万达云) | IEPL专线，入门友好，适合新手 | ¥15/月起 |
| [肥猫云](https://api.huanghaiwan.com/go/肥猫云) | 专线线路，稳定性和性价比均衡 | ¥20/月起 |
| [Cyberguard](https://api.huanghaiwan.com/go/Cyberguard) | IEPL专线，适合流媒体解锁需求 | ¥25/月起 |

---
title: "2026科学上网客户端下载安装配置全攻略：Clash Verge/Stash/Shadowrocket/v2rayN"
date: 2026-06-28
slug: client-setup-guide-2026
tags: ["教程", "新手入门"]
description: '2026年主流科学上网客户端完整下载安装与配置教程，覆盖Windows/macOS/iOS/Android四大平台'
---

> 很多新手买好机场后卡在最后一步：订阅链接复制了，但不会导入客户端。这篇文章帮你解决这最后一公里——从下载到配置完成，每个客户端都有完整步骤。

<!--more-->

## 为什么需要一个好客户端？

机场提供的是"订阅链接"——一串以 `https://` 开头的地址，里面包含了所有服务器节点信息和你的流量凭证。客户端的作用就是读取这个链接，把节点列表展示给你，然后建立网络连接。

**选对客户端直接影响你的使用体验：**

| 对比项 | 免费客户端 | 付费客户端 |
|:-------|:----------|:----------|
| 节点切换 | 基础切换 | 自动切换/延迟测速排序 |
| 规则配置 | 有限 | 完全自定义规则 |
| 策略组 | 简单分组 | 精细化分流（按地区/应用） |
| 内存占用 | 较低 | 略高但功能更全 |
| 更新频率 | 社区驱动 | 商业团队维护 |

**建议：** 新手从免费版开始用，够用了。等上手后想折腾更多功能再考虑付费版。

---

## 快速选择：哪个客户端适合你？

| 客户端 | 平台 | 价格 | 难度 | 推荐理由 |
|:------|:----|:----|:----|:--------|
| **Clash Verge Rev** | Windows / macOS / Linux | 免费 | ⭐⭐ | 功能最全，社区活跃，全平台推荐 |
| **Stash** | iOS / macOS | ¥88 买断 | ⭐⭐ | iOS最佳选择，开箱即用体验好 |
| **Shadowrocket** | iOS | $2.99 | ⭐ | 轻量轻快，适合只想简单用 |
| **v2rayN** | Windows | 免费 | ⭐⭐⭐ | 极客向，自定义程度最高 |
| **FlClash** | Android / Windows / macOS | 免费 | ⭐⭐ | 跨平台新秀，规则引擎强大 |
| **Surfboard** | Android | 免费 | ⭐ | 安卓原生VPN，轻量稳定 |
| **Clash Meta for Android** | Android | 免费 | ⭐⭐ | 功能最全的安卓客户端 |

---

## 一、Clash Verge Rev（全平台首选推荐）

> 如果你是新手，只装这一个就够了。Clash Verge Rev 是目前最活跃的 Clash 分支，社区维护、免费开源、功能完整。

### 下载安装

**官网/下载源：**
- GitHub Releases：`https://github.com/clash-verge-rev/clash-verge-rev/releases`
- macOS 选 `.dmg`，Windows 选 `Clash.Verge_x64-setup.exe`，Linux 选 `.deb` 或 `.AppImage`

**安装注意事项：**
- macOS 首次打开可能提示"无法验证开发者"→ 系统设置 → 隐私与安全性 → 仍然打开
- Windows 安装时建议勾选"创建桌面快捷方式"

### 配置步骤

1. **打开 Clash Verge Rev**
   - 启动后右下角/顶部菜单栏会出现 Clash 图标

2. **导入订阅链接**
   - 从机场后台复制你的订阅链接
   - 点击左侧"订阅"→ 右上角"添加"按钮
   - 粘贴链接 → 点击"添加" → 点击"导入"
   - 稍等几秒，节点列表会自动加载

3. **开启代理**
   - 点击左侧"代理"→ 选择一个节点
   - 点击右上角开关开启代理
   - 默认模式为"规则"（推荐），让客户端自动分流

4. **设置为系统代理**
   - 点击"设置" → 开启"系统代理"
   - 开启后浏览器即可自动走代理，无需手动配置

### 常用功能

| 功能 | 路径 | 说明 |
|:----|:----|:-----|
| 延迟测试 | 代理页 → 右键节点 → 测速 | 批量测试所有节点延迟 |
| 自动切换 | 代理页 → 策略组 → 选择"自动" | 自动选延迟最低的节点 |
| 日志查看 | 日志页 | 查看连接详情，排查问题 |
| 快捷键 | 设置 → 快捷键 | 可设置全局开关快捷键 |

---

## 二、Stash（iOS最佳选择）

> iOS 上没有 Clash Verge，Stash 是功能最接近的替代品。基于 Clash Premium 内核开发，¥88 买断制，一次购买长期使用。

### 下载安装

- App Store 搜索"Stash"（需要美区/外区 Apple ID）
- 下载后打开，首次使用需同意 VPN 配置权限

### 配置步骤

1. **导入订阅**
   - 打开 Stash → 点击底部"配置"
   - 点击右上角"+" → "通过 URL 下载"
   - 粘贴机场订阅链接 → 确认

2. **开启连接**
   - 返回首页 → 点击"开关"按钮
   - 首次会弹出 VPN 配置授权 → 允许
   - 选择节点 → 开始使用

3. **策略组配置（进阶）**
   - Stash 支持自动切换、香港分流、故障转移等策略
   - 推荐新手使用"自动选择"策略组

### Stash vs Shadowrocket

| 对比 | Stash | Shadowrocket |
|:----|:-----|:------------|
| 价格 | ¥88 买断 | $2.99 |
| 规则引擎 | Clash Premium（强大） | Surge 规则格式（基础） |
| 策略组 | 完整支持 | 有限支持 |
| 节点切换 | 手动+自动+故障转移 | 手动为主 |
| 界面复杂程度 | 略复杂但强大 | 简单轻量 |
| 更新频率 | 定期更新 | 稳定但功能固化 |

> **选择建议：** 愿意折腾选 Stash（功能上限高），只想简单上网选 Shadowrocket。

---

## 三、Shadowrocket（iOS轻量之选）

> 俗称"小火箭"，iOS 上最出名的代理客户端。$2.99 的价格只有 Stash 的十分之一，基本功能齐全。

### 下载安装

- App Store 搜索"Shadowrocket"（需要美区/外区 Apple ID）
- $2.99 买断

### 配置步骤

1. **导入订阅**
   - 打开 Shadowrocket → 右上角"+" → 类型选"Subscribe"
   - URL 处粘贴机场订阅链接
   - 备注填个名字方便识别 → 右上角"完成"

2. **开启使用**
   - 首页选中刚添加的节点 → 点击顶部的"连接"开关
   - 首次会弹出 VPN 配置授权 → 允许

3. **配置规则（推荐）**
   - 首页 → 设置 → 按以下方式配置：
     - 开启"全局路由"（不要选"代理"，选"配置"）
     - 在"配置"中加载远程规则：URL 填入 `https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Shadowrocket/Shadowrocket.list`
   - 这样配置后，国内网站直连，国外网站走代理

---

## 四、v2rayN（Windows极客首选）

> v2rayN 是 Windows 上老牌的 V2Ray 客户端，自定义程度最高，适合喜欢折腾的用户。

### 下载安装

- GitHub Releases：`https://github.com/2dust/v2rayN/releases`
- 下载 `v2rayN.zip` → 解压到纯英文路径（不要有中文目录）
- 运行 `v2rayN.exe`

### 配置步骤

1. **导入订阅**
   - 主界面 → 服务器 → 订阅设置
   - 在"订阅地址"中输入机场链接（一行一个）
   - 点击"更新订阅" → 节点列表自动加载

2. **开启代理**
   - 右键系统托盘的 v2rayN 图标
   - 选择"系统代理" → "开启代理"
   - 选择一个节点 → 回车确认

3. **路由规则设置**
   - 点击"设置" → "路由设置"
   - 预设选择"绕过大陆" → 确定
   - 这样国内网站直连，国外网站走代理

---

## 五、FlClash（跨平台新秀）

> FlClash 是基于 Clash Meta 内核的跨平台客户端，免费开源，支持 Windows/macOS/Android。它的最大特点是内置了强大的规则引擎和漂亮的 UI。

### 下载安装

- GitHub Releases：`https://github.com/onewaytea/FlClash/releases`
- 选择对应平台的安装包

### 配置步骤

1. 打开 FlClash → 点击"订阅管理"
2. 粘贴机场订阅链接 → 点击"导入"
3. 返回首页 → 点击开关开启代理
4. 在"代理"页选择节点或策略组

FlClash 的界面比 Clash Verge Rev 更现代化，内置了延迟测试和速度测试功能，操作直观，推荐 Windows 用户作为 Clash Verge 的备选。

---

## 六、安卓客户端：Surfboard 与 Clash Meta

### Surfboard（轻量推荐）

> Surfboard 是安卓原生 VPN 客户端，极其轻量，只用 2MB 左右体积。使用 Surge 规则格式，配置简单：

1. Google Play 搜索"Surfboard"下载
2. 打开 → 点击右上角"+" → 粘贴订阅链接
3. 导入完成后点击开关即可连接

### Clash Meta for Android（功能全）

> 功能最全的安卓 Clash 客户端，支持所有 Clash 功能：

1. GitHub 搜索"Clash Meta for Android"下载 APK
2. 安装后 → 配置 → 远程配置 → 粘贴订阅链接
3. 返回首页 → 点击"启动"

---

## 快速对照表

| 场景 | 推荐客户端 | 获取方式 |
|:----|:---------|:--------|
| Windows 新手 | Clash Verge Rev | GitHub 免费下载 |
| Windows 极客 | v2rayN 或 FlClash | GitHub 免费下载 |
| macOS 新手 | Clash Verge Rev | GitHub 免费下载 |
| macOS 付费 | Stash | App Store ¥88 |
| iPhone/iPad | Stash（功能全）或 Shadowrocket（轻量） | App Store |
| Android | Surfboard（轻量）或 Clash Meta（功能全） | GitHub/Google Play |
| Linux | Clash Verge Rev | GitHub AppImage |

---

## 常见问题

### Q：导入订阅后看不到节点怎么办？

**最常见原因：** 订阅链接已失效或格式不兼容。

**排查步骤：**
1. 回机场后台检查订阅链接是否过期（部分机场每月需要手动更新）
2. 尝试用浏览器打开订阅链接——应该看到一串加密文本，如果报错说明链接有问题
3. 联系机场客服重新生成订阅链接

### Q：开启代理后国内网站打不开？

这是因为规则没设置对。在客户端设置中确保：
- 规则模式设为"规则"或"Rule"（不要选"全局"或"Global"）
- 开启"绕过大陆"（Bypass Mainland China）功能

### Q：所有节点都显示超时/失败？

有几个可能的原因：
1. **网络环境问题**：你的网络本身需要检查——试着访问一下百度、微博能不能打开
2. **防火墙拦截**：部分公司网络/校园网会拦截代理流量
3. **客户端问题**：重启客户端或者换个客户端试
4. **机场问题**：联系机场客服确认线路是否正常

### Q：免费客户端和付费客户端有区别吗？

功能上，免费客户端（Clash Verge Rev、v2rayN）的核心功能已经非常完整。付费客户端（Stash、Surge）主要优势在：配置更开箱即用、规则分流更智能化、技术支持响应更快。

对于 90% 的用户，Clash Verge Rev 完全够用。

---

## 总结

| 客户端 | 一句话总结 |
|:------|:----------|
| Clash Verge Rev | 全平台免费首选，装机必备 |
| Stash | iOS用户的首选，¥88值回票价 |
| Shadowrocket | iOS轻量之选，$2.99简单省心 |
| v2rayN | Windows极客的最爱 |
| FlClash | 跨平台新秀，UI出众功能强 |

> 💡 **新手建议：** Windows/macOS 直接装 Clash Verge Rev，iPhone 装 Stash（预算够）或 Shadowrocket（预算紧），安卓装 FlClash 或 Surfboard。全部免费/低价，一次配置永久使用。

---

## 🛩️ 机场推荐

配置好客户端后，就差一个稳定的机场了。以下是我常用和测试过的机场，按场景分类：

| 需求 | 推荐 | 起步价 | 特点 |
|:----|:----|:------|:-----|
| ⭐ 主力首选 | [自由猫 Freecat](https://api.huanghaiwan.com/go/自由猫) | ¥9/月 | MPTCP多路复用，100+节点，晚高峰稳 |
| 💰 低价入门 | [龙猫云](https://api.huanghaiwan.com/go/龙猫云) | ¥9.9/月 | 专线入门，性价比高 |
| 🎬 流媒体解锁 | [万达云](https://api.huanghaiwan.com/go/万达云) | ¥15/月 | IEPL专线，Netflix/Disney+全解锁 |
| 🔄 备用首选 | [悠兔](https://api.huanghaiwan.com/go/悠兔) | ¥17/月 | IEPL专线，199/年轻量版做备用 |
| 🚀 高端稳定 | [MESL](https://api.huanghaiwan.com/go/MESL) | ¥50/月 | IEPL专线，低调老牌 |
| 🌏 海外电商 | [肥猫云](https://api.huanghaiwan.com/go/肥猫云) | ¥20/月 | 专线稳定，TikTok优化 |

> 提示：大多数机场都提供试用期或低价套餐，建议先用最低套餐体验，确定稳定后再考虑长期订阅。

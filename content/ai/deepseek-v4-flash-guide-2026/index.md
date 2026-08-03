---
title: "DeepSeek V4-Flash 正式版指南 2026：小模型反超 Pro，API 接入与 Codex 适配全解析"
date: 2026-08-03
slug: "deepseek-v4-flash-guide-2026"
tags: ["AI", "教程", "DeepSeek", "Agent"]
description: 'DeepSeek V4-Flash 正式版 API 于 7 月 31 日全面公测，Agent 能力暴涨 6 倍。2840 亿参数 MoE 模型结构不变，只重做后训练，9 项 Agent 基准全面反超 V4-Pro 预览版，原生支持 Responses API 与 Codex 生态。一文讲清 API 接入教程、官方定价策略和旧接口迁移注意事项，无论个人开发者还是团队都值得收藏。'
---

> **更新：2026-08-03 — DeepSeek V4-Flash 正式版已于 7 月 31 日上线公测，这是 V4 系列自 4 月预览版以来最重要的一次升级。本文基于官方 API 文档、官方定价页与多家媒体报道整理。**

DeepSeek 这次不讲武德。7 月 31 日下午，官方 API 文档悄悄更新了一版发布日志，V4-Flash 正式版就这么上线了，当天直接冲上知乎热搜第一。

名字里带 Flash，听着像"轻量阉割版"，结果发布日志一摊开，9 项 Agent 基准测试全面反超三个月前的 V4-Pro 预览版。Flash 打赢 Pro，这剧本在 AI 圈不常见。更离谱的是：**模型结构和参数尺寸一点没变，只重做了一遍后训练。**

如果你在做 Agent 开发、接 API、或者单纯想找个便宜又能干活的模型，这篇值得看完。

## 太长不看版

| 维度 | 结论 |
|:----|:-----|
| 发布时间 | 2026-07-31，V4-Flash-0731 正式版 API 全面公测 |
| 模型规格 | 2840 亿总参数 MoE，激活参数仅 130 亿，上下文 1M |
| 核心变化 | 结构不变，只重做后训练；Agent 能力暴涨 |
| 最大卖点 | 原生支持 Responses API + Codex 生态深度适配 |
| 价格 | 输入 $0.14/百万 tokens（缓存命中 $0.0028），输出 $0.28/百万 tokens |
| 权重 | MIT 协议，HuggingFace 已开放 |
| 适合谁 | 开发者、Agent 应用、高性价比需求的个人和企业 |

## 这版 Flash 为什么值得关注

先把背景补上。4 月 24 日 DeepSeek 发布 V4 系列预览版，两个成员：V4-Pro（1.6 万亿参数）和 V4-Flash（2840 亿参数），都是 MIT 协议开源权重，上下文拉到 1M。发布时媒体关注点是它跑在华为、寒武纪的国产芯片上——训练芯片被限制的背景下，性能还能对标头部闭源模型。

三个月后，7 月 31 日的更新把聚光灯全抢到了 Flash 身上。**V4-Flash-0731 的模型骨架和 preview 版完全一致**，总参数 2840 亿、激活参数 130 亿、256 个路由专家，一个数字都没改。改的只有后训练——针对接口调用场景重新做了一轮专项训练。

结果就是：9 项 Agent 基准测试全面碾压自家 V4-Pro 预览版。官方只动了 V4-Flash 的 API，V4-Pro 的 API 和 App/Web 端模型都没动，V4-Pro 正式版按官方说法"将尽快发布"。

## 核心变化：骨架没换，重做后训练

很多模型升级靠堆参数、堆数据，V4-Flash 这次走的是另一条路——**训练策略调优**。

| 变化点 | 说明 |
|:------|:-----|
| 模型结构 | 与 V4-Flash-preview 完全一致（284B 总参 / 13B 激活，MoE 256 专家） |
| 后训练 | 针对 API 接口调用场景专项重做 |
| Responses API | 原生支持 OpenAI 新版接口格式 |
| Codex 适配 | 深度调优，一次配置即可在 Codex CLI / ChatGPT 桌面端 / VS Code 插件调用 |
| 其他特性 | JSON 输出、工具调用、Anthropic 兼容接口、FIM 补全（Beta） |

对开发者来说，最实在的是 Codex 适配。以前想把 DeepSeek 接进 Codex 生态，得靠中间层转换，现在官方直接把路铺平了——**Codex CLI、ChatGPT 桌面端、VS Code 的 Codex 插件都能直接调用**，省掉一堆配置文件折腾。

## Agent 能力实测：9 项基准全面反超

官方公布的 9 项基准里，V4-Flash 正式版 vs 三个月前的 V4-Pro 预览版：

| 基准测试 | V4-Flash 正式版 | V4-Pro 预览版 |
|:--------|:--------------:|:-------------:|
| Terminal Bench 2.1（终端操作） | **82.7** | 72.1 |
| DeepSWE（真实长周期编程任务） | **54.4** | 7.3 |
| NL2Repo（代码仓库理解） | **54.2** | — |
| Cybergym（网络安全攻防模拟） | **76.7** | — |
| Toolathlon Verified（工具调用） | **70.3** | — |
| Agent Last Exam（综合智能体） | **25.2** | — |
| Automation Bench Public（自动化） | **25.1** | — |
| DSBench-FullStack（全栈开发） | **68.7** | — |
| DSBench-Hard（高难度编码） | **59.6** | — |

最扎眼的是 DeepSWE：从 7.3 飙到 54.4，翻了 6 倍多。这个基准专门考 Agent 在真实长周期、多步骤编程任务里的自主解决能力，是衡量"AI 能不能真干活"的硬指标。

横向看也站得住。Terminal Bench 2.1 上，V4-Flash 的 82.7 超过了 GLM-5.2 的 81.0，逼近 Opus-4.8 的 85.0。海外评测机构 Artificial Analysis 的智能指数测试里，V4-Flash 最高推理档位拿到 50 分，同类可比模型的中位数只有 17 分。

翻译成人话：**这模型在"自己动手干活"这件事上，比三个月前的 Pro 预览版靠谱得多**——操作软件、调工具、写复杂代码，都更接近一个熟练的"数字员工"。

## 价格：便宜到没朋友

官方定价页（2026-08-01 更新）按每百万 tokens 计价：

| 计费项 | V4-Flash | V4-Pro |
|:------|:--------:|:------:|
| 输入（缓存命中） | $0.0028 | $0.003625 |
| 输入（缓存未命中） | $0.14 | $0.435 |
| 输出 | $0.28 | $0.87 |

对比一下隔壁。8 月初有报道称 GPT-5.6 系列降价——主打日常的 Tera 降 20%、主打高性价比的 Luna 降 80%。降完之后，Luna 的价格依然显著高于 V4-Flash。**论性价比，V4-Flash 现在就是第一梯队。**

两个实用提醒：

- **缓存命中价低到离谱**：$0.0028/百万 tokens，约等于白送。高频重复调用（比如固定 prompt 前缀的批量任务）记得把缓存用起来。
- **峰谷定价要来了**：官方预告将实行高峰/低谷差异化定价，高峰时段（北京时间 9:00-12:00、14:00-18:00）价格为常规价的 2 倍。生效日期待官方公告，批量任务尽量避开这几个小时能省一半钱。

## 怎么用：四种方式

### 1. 官方 App / Web

chat.deepseek.com 直接开聊。这次更新只动了 API，App 和 Web 端模型没有变化，普通用户继续用就行，不需要做任何操作。

### 2. API（OpenAI 兼容格式）

接口地址和 OpenAI 格式完全兼容，换 base_url 和 model 名就能用：

```python
from openai import OpenAI

client = OpenAI(
    api_key="你的API密钥",
    base_url="https://api.deepseek.com"
)

resp = client.chat.completions.create(
    model="deepseek-v4-flash",
    messages=[{"role": "user", "content": "用Python写一个快速排序，并解释思路"}],
    stream=False
)
print(resp.choices[0].message.content)
```

curl 也一样：

```bash
curl https://api.deepseek.com/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $DEEPSEEK_API_KEY" \
  -d '{
    "model": "deepseek-v4-flash",
    "messages": [{"role": "user", "content": "你好"}]
  }'
```

### 3. API（Anthropic 兼容格式）

用 Claude 那套 SDK 的，base_url 改成 `https://api.deepseek.com/anthropic` 即可，官方已提供 Anthropic 兼容接口。

### 4. Codex 生态

官方针对 Codex 场景做了深度适配。在 Codex CLI、ChatGPT 桌面端或 VS Code 的 Codex 插件里，把模型配置成 `deepseek-v4-flash`、base_url 指向 DeepSeek 即可直调，无需中间转换层。

## 旧接口迁移提醒（10 月 24 日截止）

**如果你是老 API 用户，这条必看。** `deepseek-chat` 和 `deepseek-reasoner` 这两个旧 API 名称需要在 10 月 24 日前迁移到新模型名（`deepseek-v4-flash` / `deepseek-v4-pro`）。官方已给出迁移窗口，代码里把 model 参数换掉、验证一下输出格式就行。

App/Web 用户不受影响，不用管这条。

## 适合谁

| 人群 | 推荐理由 |
|:----|:--------|
| 个人开发者 | 便宜、能干活，接 API 成本极低 |
| Agent 应用团队 | 原生 Responses API + 工具调用，Agent 基准全线领先 |
| 学生 / 学习者 | 免费额度 + 低价 API，练手成本几乎为零 |
| 企业降本 | 大批量调用场景，价格只有头部闭源模型的零头 |

## 竞品怎么选

| 需求 | 选谁 | 理由 |
|:----|:----|:-----|
| Agent 开发、工具调用 | **V4-Flash** | 9 项 Agent 基准领先，价格碾压 |
| 综合旗舰能力 | V4-Pro（待正式版） | 1.6T 参数，规格更高 |
| 编程助手（Codex 生态） | **V4-Flash** | 官方深度适配，开箱即用 |
| 免费体验 | 各家免费档 | 先用免费额度跑通流程再决定 |

一句话结论：**想做 Agent、想省钱、想把模型接进编码工作流，V4-Flash 正式版现在就是国产模型里最值的选择。**

## FAQ

**Q：V4-Flash 和 V4-Pro 有什么区别？**
A：V4-Flash 总参数 2840 亿、激活 130 亿，V4-Pro 总参数 1.6 万亿。规格上 Pro 更高，但 7 月 31 日这轮更新后，Flash 正式版的 Agent 能力反超了 Pro 预览版。V4-Pro 正式版还没发布。

**Q：为什么激活参数只有 130 亿还能这么强？**
A：MoE（混合专家）架构下，每次推理只激活一部分参数，但总参数池是 2840 亿。真正让这次升级起飞的是后训练策略调优——同样的骨架，训练方法对了，成绩天差地别。

**Q：旧 API 不迁移会怎样？**
A：`deepseek-chat` 和 `deepseek-reasoner` 将在 10 月 24 日后停止服务，需要在此之前把代码里的 model 参数换成新模型名。

**Q：普通用户（App/Web）需要做什么吗？**
A：不用。这次更新只涉及 V4-Flash 的 API 接口，App 和 Web 端没有任何变化。

**Q：峰谷定价什么时候生效？**
A：官方只说"即将实行"，具体生效日期以官方公告为准。高峰时段（北京时间 9:00-12:00、14:00-18:00）价格将是常规价 2 倍。

**Q：上下文长度和输出上限是多少？**
A：上下文 1M tokens，最大输出 384K tokens。长文档处理、代码库分析都够用。

**Q：权重在哪里下载？**
A：HuggingFace 已开放，MIT 协议，可商用、可魔改。

**Q：需要稳定的网络才能用吗？**
A：DeepSeek 是国内服务，直连即可。如果你同时在用 ChatGPT、Claude 这类海外工具，才需要搭配网络工具（见下方推荐表）。

## 🔧 同时用海外 AI 工具？需要稳定的网络环境

DeepSeek 本身直连就能用。但很多人的工作流是"DeepSeek 写初稿 + ChatGPT/Claude 精修"，后者需要稳定的网络环境。以下是我长期使用后推荐的方案：

| 使用场景 | 推荐方案 | 月费 | 特点 |
|:--------|:--------|:---:|:-----|
| 🥇 主力推荐 | [自由猫](https://api.huanghaiwan.com/go/自由猫) | ¥9-45 | IEPL 专线，100+ 节点，晚高峰稳定 |
| 💼 性价比之选 | [万达云](https://api.huanghaiwan.com/go/万达云) | ¥10-28 | IEPL+中转+专线三线路，适合 API 高频调用 |
| 🎬 流媒体解锁 | [SS-ID](https://api.huanghaiwan.com/go/SS-ID) | ¥20起 | IEPL 专线，5 设备，解锁 ChatGPT + 4K 视频 |
| 📱 全场景备用 | [闪狐云](https://api.huanghaiwan.com/go/闪狐云) | ¥10-39 | 中转+专线双线路，适合做备用组合 |

**组合推荐：** V4-Flash API（按量付费，日常几块钱）+ 自由猫（主力）¥25/月 + ChatGPT Plus $20——这是目前性价比最高的"国产主力 + 海外辅助"方案。

---

*本文最后更新：2026-08-03 | 相关教程：[DeepSeek 入门到精通](/ai/deepseek-guide/) | [DeepSeek 高阶组合技](/ai/deepseek-advanced-combinations/)*

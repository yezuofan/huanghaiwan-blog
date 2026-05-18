---
title: "Cursor入门到精通：AI编程的正确姿势（2026教程）"
date: 2026-05-18
slug: cursor-guide
tags: ["AI", "教程"]
---

> Cursor 是目前最火的 AI IDE，基于 VS Code 深度改造，内置 AI 对话、代码补全、多文件编辑等能力。本文从零开始，帮你把 Cursor 用到极致。

<!--more-->

## 一句话结论

| 场景 | 推荐功能 | AI模型 |
|:----|:--------|:------|
| 日常写代码 | Tab 自动补全 | Cursor Tab |
| 改一段代码 | Ctrl+K 内联编辑 | Claude / GPT-4o |
| 写一个新功能 | Composer（Ctrl+I） | Claude Sonnet |
| 调试 Bug | Chat（Ctrl+L） | Claude Sonnet |
| 跨文件重构 | Composer Agent 模式 | Claude Sonnet |
| 批量代码审查 | Chat + @Files | Claude Sonnet |

**日常用 Tab 补全（最快），写功能用 Composer（最爽），调试用 Chat（最稳）。**

---

## 快速上手：3种使用方式

### 方式 1：Cursor Tab 自动补全（最强生产力）

安装 Cursor 后，写代码时会自动出现灰色提示，按 Tab 接受：

- **单行补全**：写一半自动预测下一段
- **多行补全**：写函数/方法时自动补完整块代码
- **上下文感知**：不仅看当前文件，还看你打开的其他文件

**技巧**：换行后光标停在空白处，按 `Ctrl+Enter` 触发补全。

### 方式 2：Chat 对话（Ctrl+L）

按 `Ctrl+L` 打开对话面板，像 ChatGPT 一样和 AI 聊天讨论代码：

- 选中代码后按 `Ctrl+L` → 自动把选中代码发给 AI
- 可在对话中引用文件：输入 `@file` 选择文件
- 引用文档：输入 `@docs` 引入官方文档

**最佳实践：** 调试 Bug 时，选中报错代码 → `Ctrl+L` → "这个函数为什么会报 `IndexError`？"

### 方式 3：Composer（Ctrl+I）— 多文件编辑

Composer 是 Cursor 最强大的功能，可以同时编辑多个文件：

- **普通模式**：只改当前文件
- **Agent 模式（推荐）**：自动创建、修改、删除多个文件

---

## 核心功能详解

### 1. Tab 自动补全

| 功能 | 说明 | 快捷键 |
|:----|:-----|:------|
| Tab 补全 | 接受 AI 建议 | Tab |
| 拒绝建议 | 继续打字 | Esc |
| 换行触发生成 | 在新行等待 AI 建议 | Ctrl+Enter |
| 多行 Diff 预览 | 一次显示多行修改 | 自动 |

### 2. Ctrl+K 内联编辑

选中一段代码，按 `Ctrl+K` 输入指令，AI 直接修改选中代码：

```
选中代码 → Ctrl+K → "加错误处理" / "用async/await重写"
```

### 3. Chat（Ctrl+L）

| 功能 | 用法 |
|:----|:----|
| 代码审查 | 选中代码 → Ctrl+L → "审查这段代码" |
| 解释代码 | 选中代码 → Ctrl+L → "解释这段代码在做什么" |
| 添加测试 | 选中代码 → Ctrl+L → "为这个函数写 Jest 测试" |
| 引用文件 | @file + 文件名 → 文件内容进入上下文 |
| 引用 Web | @web + 搜索词 → 联网搜索最新文档 |

### 4. Composer Agent 模式

按 `Ctrl+I` 切换到 Agent 模式，可以实现：

- **创建新项目**：一句话生成完整项目结构
- **跨文件重构**：同时修改多个文件
- **添加功能**：在现有项目中加新功能
- **修复 Bug**：定位问题 → 修复 → 写测试

---

## 10 个实战 Prompt 模板（直接复制可用）

### 模板 1：创建 REST API

创建一个 Express.js REST API，包含用户 CRUD、SQLite 数据库、JWT 认证、错误处理中间件。

### 模板 2：写单元测试
```
为 src/utils/helpers.js 中的所有函数写 Jest 单元测试：
- 覆盖正常输入、边界值、错误输入
- 用 describe/it 组织
- Mock 外部依赖
- 测试覆盖率达到 90%+
```

### 模板 3：代码重构
```
重构这个函数：
- 拆分超过 50 行的函数
- 提取重复逻辑到公共函数
- 改用 TypeScript 类型
- 添加 JSDoc 注释
- 保持对外接口不变
```

### 模板 4：添加新功能
```
在现有项目中添加新功能：用户邮箱重置密码功能：
- 发送 6 位验证码到邮箱
- 验证码有效期 5 分钟
- 重置后强制重新登录
```

### 模板 5：性能优化
```
分析这段代码的性能瓶颈：
1. 找出 O(n²) 及以上复杂度的代码
2. 提出优化方案
3. 改为更高效的实现
4. 添加性能基准测试
5. 对比优化前后的执行时间
```

### 模板 6：数据库查询优化
```
优化这个 SQL 查询：
- 用 EXPLAIN ANALYZE 分析执行计划
- 建议需要添加的索引
- 重写 N+1 查询为 JOIN
- 标注修改前后的查询时间对比
```

### 模板 7：前端组件开发
```
用 React + TypeScript 创建一个数据表格组件：
- 支持排序、搜索过滤、分页、行选中、列宽拖动、CSV 导出
```

### 模板 8：API 文档生成
```
为所有路由生成 OpenAPI 3.0 格式 API 文档，包含请求参数和响应格式。
```

### 模板 9：Code Review 助手
```
审查这个 PR 代码：
1. 安全漏洞（SQL注入/XSS/认证绕过）
2. 性能问题（N+1查询/内存泄漏）
3. 代码质量（重复/可读性/命名）
4. 测试覆盖（缺少的测试用例）
5. 架构问题（耦合度/扩展性）
```

### 模板 10：Git Commit 信息生成
```
分析 git diff，生成 Conventional Commits 格式的 commit 信息，分类型列出：
feat/fix/refactor/docs，每行不超过 72 字。
```

---

## 7 个进阶技巧

### 1. @docs 引用文档
Cursor 可以引入第三方库的官方文档。输入 `@docs` 后选择「Add Docs」，粘贴文档 URL，Cursor 会自动抓取并索引。

### 2. .cursorrules 规则文件
在项目根目录创建 `.cursorrules` 文件，定义 Cursor 的行为规则，让 AI 遵循你的编码风格。

### 3. 项目级别上下文配置
`.cursorignore` 文件排除不需要索引的目录（node_modules/, dist/, build/），提升 AI 响应质量。

### 4. Cmd+K 内联编辑
选中一段代码 → `Cmd+K` → 输入修改指令，AI 直接修改代码，显示 Diff 对比。

### 5. Chat History 复用
Cursor 保存所有对话历史，遇到类似问题可以直接引用之前的解决方案。

### 6. Composer Checkpoint 回退
Composer 每次修改自动创建 Checkpoint。如果修改出问题，点击 Restore 回退，相当于 Composer 自动做了 git stash。

### 7. Vim 模式兼容
Cursor 完全支持 VS Code 的 Vim 插件，Vim 快捷键和 AI 补全可以同时使用。

---

## FAQ

### Q1: Cursor 免费吗？
免费套餐每月 2000 次 Tab 补全 + 50 次 Composer。Pro 版 $20/月。**个人开发免费版完全够用。**

### Q2: Cursor 和 Copilot 哪个好？

| 对比项 | Cursor | GitHub Copilot |
|:------|:------|:--------------|
| 自动补全 | ✅ 更强（多行+上下文） | ✅ 强 |
| 多文件编辑 | ✅ Composer Agent | ❌ 不支持 |
| 对话能力 | ✅ 内置 Chat | ✅ Chat 面板 |
| AI 模型 | Claude / GPT-4o 可选 | OpenAI 系列 |
| 免费额度 | 2000 Tab + 50 Composer | 有限使用 |

### Q3: Cursor 支持哪些语言？
所有主流语言都支持。Python、JavaScript/TypeScript、Java、Go、Rust、C++ 等表现最好。

### Q4: 国内用 Cursor 需要什么？
Cursor 需要访问海外 AI 服务。国内用户需要稳定的网络工具来连接：

| 方案 | 推荐指数 | 月费 | 说明 |
|:----|:-------:|:----|:-----|
| 网络工具（低延迟） | ⭐⭐⭐⭐⭐ | ~¥20-30 | 稳定连接 Cursor 服务，香港节点延迟最低 |
| API 中转 | ⭐⭐⭐ | ¥15-50/按量 | 技术门槛高，不适合新手 |

👉 [推荐稳定低延迟的网络工具 →](https://hongxingyun.club/web/#/register?code=OBEi3O69)

### Q5: Cursor 支持远程开发吗？
支持。完美兼容 VS Code 的 Remote SSH 和 Dev Containers 插件。

### Q6: Cursor 会泄露我的代码吗？
可以在 Settings → Privacy 中关闭代码训练。**商业项目建议关闭。**

---

## 优缺点总结

### ✅ 优点
- **多文件编辑强**：Composer Agent 模式是目前最好的多文件 AI 编辑体验
- **模型选择自由**：可切换 Claude Sonnet、GPT-4o
- **Tab 补全质量高**：上下文理解准确
- **VS Code 生态**：插件、主题全部兼容
- **代码审查高效**：Chat + @Files 快速理解项目

### ❌ 缺点
- Pro $20/月，比 Copilot 贵一倍
- 需要海外网络，国内需要配合网络工具
- Composer 改多文件时可能引入不存在的 API
- 长期运行占 2-3GB 内存

---

## 写在最后

Cursor 是目前个人开发者最佳的 AI 编程工具。Tab 补全 + Composer Agent 组合让日常开发效率提升 2-3 倍。

**免费版先用起来，Pro 版等确认需要批量编辑能力再升级。**

---

*本文包含推广链接，如果你通过链接注册，我们可能会获得少量佣金，但这不影响我们的评分和推荐。*
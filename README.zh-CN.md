[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-Compatible-brightgreen)](https://github.com/OpenClaw)
[![Hermes-Agent Ready](https://img.shields.io/badge/Hermes--Agent-Ready-blue)](https://github.com/Hermes-Agent)
[![Claude Code Supported](https://img.shields.io/badge/Claude%20Code-Supported-orange)](https://claude.ai/code)
[![OpenCode Compatible](https://img.shields.io/badge/OpenCode-Compatible-red)](https://github.com/open-code-ai)

# EchoMind v2.0 — 中文增强版

🌐 **English Version:** [README.md](README.md)

> 基于 [echomind_memory.skill](https://github.com/jasonatgit/echomind_memory.skill) 官方 v1.1.5 的认知架构级重构。
> 把 Hindsight 的「魂」借过来，套在 EchoMind 的「壳」里。你的 AI 不再只是「记住」，它会「思考」。

---

## 🧠 四层认知架构（碾压传统向量搜索）

大多数记忆系统只是在做一件事：**存进去，搜出来**。就像一个只有短期记忆的金鱼——知道昨天聊过，但不知道为什么聊、聊出了什么结论、下次该怎么做。

**EchoMind v2.0 借鉴 Hindsight 的认知科学架构，把记忆从「硬盘」升级为「大脑」：**

| 层 | 对应人类认知 | 做了什么 |
|:---|:---|:---|
| **🌍 世界事实** | 「火是烫的」 | 客观知识分类存储，追溯来源（手动/反射/观察） |
| **📝 经历** | 「我摸了一下那个亮的东西，被烫了」 | 完整记录每次交互的上下文和结果 |
| **🔍 观察** | 「那个光亮的东西很烫，可能是火」 | **自动从对话中提炼模式**，像侦探一样发现规律 |
| **🧠 心智模型** | 「亮的东西不要碰」 | 反复验证的观察→**自动写入系统偏好**，永久生效 |

这不是花哨的分类游戏，这是从「存储」到「认知」的本质跨越。

## ⚡ 三大核心升级

### 1. 反射引擎：从「死机」到「活脑」

原版反射引擎永远不会触发（代码里写着「当 LLM 可用时不做反射」——相当于买了一台超级计算机只当计算器用）。

**v2.0 修复**：三级降级策略，LLM 驱动的语义分析，每 8 轮对话自动触发一次「大脑复盘」：

```
对话积累 → LLM 分析 → 提取洞察 → 发现偏好 → 沉淀规则 → 提炼知识 → 生成观察
```

### 2. 观察层：从「单次记录」到「模式发现」

新增 `observation_memory` 表，每条观察自动去重、累计置信度：

```
第1次 → 「用户可能喜欢简洁回复」(conf=0.5, hit=1)
第2次 → 「嗯，又是这样」(conf=0.65, hit=2)
第3次 → 「确认了」(conf=0.75, hit=3) → **升级为心智模型**
```

### 3. 心智模型自动沉淀：从「会话记忆」到「永久人格」

同一个模式出现 **3 次**且置信度 **≥0.6**，自动写入 MEMORY.md/USER.md，永久生效。

## 🆚 竞品对比

| 维度 | 原版 EchoMind | EchoMind v2.0 | Hindsight |
|:---|:---:|:---:|:---:|
| 存储方式 | 9 张表平铺 | 11 张表 + 四层架构 | 云端 PostgreSQL |
| 反射引擎 | ❌ 从不触发 | ✅ LLM 三级降级 | ✅ 内置 |
| 模式发现 | ❌ 无 | ✅ observation_memory | ✅ 自动 |
| 心智模型 | ❌ 无 | ✅ 自动沉淀到文件 | ✅ 云端存储 |
| 离线可用 | ✅ | ✅ | ❌ 依赖 API |
| 部署成本 | 零 | 零 | ~1GB Docker |

## 📦 使用方式

```yaml
# config.yaml
memory:
  provider: echomind
  enabled: true
```

无需额外部署，无需 Docker，无需 API key。改一行配置，重启即生效。

## 🔧 修改的文件（615 行新增）

| 文件 | 改动 |
|:---|:---|
| `core/_reflective_fallback.py` | 从 200 行空壳 → 400 行完整反射引擎 |
| `core/storage/sqlite_store.py` | 新增 observation 表 + source_type 迁移 |
| `adapters/hermes_provider.py` | 反射循环 + 心智模型自动沉淀 |

## 🔗 链接

- 官方原版：https://github.com/jasonatgit/echomind_memory.skill
- 中文增强版：https://github.com/xianjianhuang/echomind-memory-cn

---

*这不是一个插件，这是具有自我反思记忆的 AI 多智能体记忆神经网络。*

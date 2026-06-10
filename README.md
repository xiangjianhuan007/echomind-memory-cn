[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-Compatible-brightgreen)](https://github.com/OpenClaw)
[![Hermes-Agent Ready](https://img.shields.io/badge/Hermes--Agent-Ready-blue)](https://github.com/Hermes-Agent)
[![Claude Code Supported](https://img.shields.io/badge/Claude%20Code-Supported-orange)](https://claude.ai/code)
[![OpenCode Compatible](https://img.shields.io/badge/OpenCode-Compatible-red)](https://github.com/open-code-ai)

# EchoMind v2.0 — Chinese Enhanced Edition

🌐 **中文版:** [README.zh-CN.md](README.zh-CN.md)

> A cognitive architecture-level fork of [echomind_memory.skill](https://github.com/jasonatgit/echomind_memory.skill) (official v1.1.5).
> Borrowing Hindsight's "soul" and wrapping it in EchoMind's "shell." Your AI doesn't just "remember" anymore — it **thinks**.

---

## 🧠 Four-Layer Cognitive Architecture (Crushes Traditional Vector Search)

Most memory systems do only one thing: **store in, search out**. Like a goldfish — it knows you talked yesterday, but has no idea why, what conclusions were reached, or what to do next time.

**EchoMind v2.0 borrows Hindsight's cognitive science architecture, upgrading memory from a "hard drive" to a "brain":**

| Layer | Human Analogy | What It Does |
|:---|:---|:---|
| **🌍 World Facts** | "Fire is hot" | Categorize objective knowledge, trace sources |
| **📝 Experience** | "I touched that bright thing and got burned" | Full context + outcome for every interaction |
| **🔍 Observation** | "That bright thing is hot, might be fire" | **Auto-extract patterns from conversations** |
| **🧠 Mental Model** | "Don't touch bright things" | Confirmed observations → **auto-write to system preferences** |

This isn't fancy categorization. This is a fundamental leap from "storage" to "cognition."

## ⚡ Three Core Upgrades

### 1. Reflection Engine: From "Dead" to "Alive"

The original reflection engine never fired (the code literally said "don't reflect when LLM is available" — like buying a supercomputer and using it as a calculator).

**v2.0 fix**: Three-tier degradation strategy, LLM-driven semantic analysis, auto-triggers a brain review every 8 conversations.

### 2. Observation Layer: From "Single Record" to "Pattern Discovery"

New `observation_memory` table, auto-deduplication, confidence accumulation:

```
1st occurrence → "User prefers concise replies" (conf=0.5, hit=1)
2nd occurrence → "Same pattern again" (conf=0.65, hit=2)
3rd occurrence → "Confirmed" (conf=0.75, hit=3) → **promoted to mental model**
```

### 3. Mental Model Auto-Sedimentation

When a pattern appears **3 times** with confidence **≥0.6**, auto-write to MEMORY.md/USER.md — permanently.

## 🆚 Comparison

| Dimension | Original EchoMind | EchoMind v2.0 | Hindsight |
|:---|:---:|:---:|:---:|
| Storage | 9 flat tables | 11 tables + 4-layer | Cloud PostgreSQL |
| Reflection Engine | ❌ Never fires | ✅ LLM 3-tier | ✅ Built-in |
| Pattern Discovery | ❌ None | ✅ observation_memory | ✅ Automatic |
| Mental Models | ❌ None | ✅ Auto-write to files | ✅ Cloud storage |
| Offline | ✅ | ✅ | ❌ API-dependent |
| Deploy Cost | Zero | Zero | ~1GB Docker |

## 📦 Usage

```yaml
# config.yaml
memory:
  provider: echomind
  enabled: true
```

No extra deployment, no Docker, no API key. Change one config line, restart, done.

## 🔧 Files Modified (615 lines added)

| File | Change |
|:---|:---|
| `core/_reflective_fallback.py` | 200-line shell → 400-line complete reflection engine |
| `core/storage/sqlite_store.py` | New observation table + source_type migration |
| `adapters/hermes_provider.py` | Reflection loop + mental model auto-sedimentation |

## 🔗 Links

- Official: https://github.com/jasonatgit/echomind_memory.skill
- Chinese Enhanced: https://github.com/xianjianhuang/echomind-memory-cn

---

*This isn't a plugin. This is an AI Multi-Agent Memory Neural Network with self-reflective memory.*

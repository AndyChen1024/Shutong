# Code Review Workflow (Codex)

本文档用于规范在本项目中使用 Codex（或类似 AI 工具）进行 Code Review 的方式。
目标不是“让 AI 说得多”，而是 **根据变更的实际风险与语义密度，获得恰当强度的反馈**。

---

## 🎯 设计目标

- 避免低价值输出（如复述 git diff、格式整理）
- 允许在低风险变更中“快速确认即可”
- 在关键变更中，获得真正有判断力的 Review
- 将 Code Review 作为**工程反馈系统**，而非仪式

---

## 🧭 Review 分级模型

本项目将 Code Review 分为三个等级：

| Level | 名称 | 适用场景 |
|------|------|----------|
| L0 | 轻量确认（Sanity Check） | 文档、规范整理、路径/命名调整、流程演练 |
| L1 | 常规 Review | 普通功能、小范围重构、中等风险改动 |
| L2 | 深度 Review | 架构调整、核心功能、生命周期/并发、dev → main |

### 30 秒判断法

> 如果你不需要“第二双眼睛”发现隐性风险 → L0
> 如果你希望获得具体改进建议 → L1
> 如果出错成本高 → L2

---

## 🟢 L0：轻量确认型 Review

**目的**
- 验证流程是否跑通
- 排除明显异常
- 允许明确回复“没有问题”

**约束**
- 禁止复述 git diff
- 禁止强制输出长分析

**Prompt 模板**

```text
Review level: L0 (sanity check only)

This is a low-risk change. I’m validating the workflow, not asking for deep analysis.
Assume I can read git diff; do NOT restate changes.

Reply briefly:
1) Safe to merge under our workflow (main is always runnable)? YES/NO
2) Any obvious anomaly or red flag? If NO, say "No red flags."
3) Is the scope reasonable for a single commit? YES/NO

Keep it short (<= 5 lines). If all good, explicitly say it's OK.
```

---

## 🟡 L1：常规 Review

**目的**
- 发现潜在问题
- 提供可执行建议
- 不做过度设计

**关注重点**
- 正确性
- 可维护性
- 是否弱化了既定工程规范（如 main 为门面）

**Prompt 模板**

```
Review level: L1 (standard review)

You are a pragmatic senior engineer reviewing this PR.

Constraints:
- Do NOT restate the diff line-by-line.
- Focus on correctness, maintainability, and workflow rules (main must stay runnable).
- Prefer minimal, actionable fixes over big refactors.

Please output:
A) Summary (2-3 sentences): what changed + overall assessment.
B) Top issues by priority:
   - P0: breaks build/run, crash risks
   - P1: correctness/perf
   - P2: maintainability/architecture boundaries
   - P3: style/nits (only if helpful)
C) Merge verdict:
   - OK to merge into dev? (YES/NO + conditions)
   - OK to merge into main? (YES/NO + conditions)

If you suggest changes, include exact code blocks or a small patch.

Context:
- Goal:
- Affected modules:
- How to test:
Now review the diff.
```

---

## 🔴 L2：深度 Review

**目的**
- 控制高风险改动
- 审视架构与长期演进成本
- 为发布/合并提供决策依据

**适用场景**
- 架构分层调整
- 核心业务逻辑
- 生命周期、并发、状态管理
- dev → main 关键合并

**Prompt 模板**

```
Review level: L2 (deep review)

Act as a strict senior reviewer. This change affects important code or architecture.

Hard rules:
- main is the showcase branch: must always build/run.
- Respect module boundaries; avoid leakage.
- Avoid speculative refactors; propose the smallest safe design.
- Do NOT restate the diff.

Please analyze:
1) Risk assessment: top 3 things that could go wrong.
2) Architecture & boundaries: any layering or coupling issues?
3) Correctness & edge cases: lifecycle, threading, state restoration.
4) Testability: 2-5 concrete test cases to add.
5) Migration/compatibility: any breaking changes? versioning implications?

Output format:
- Verdict: Block / Approve with changes / Approve
- Blocking issues (if any): numbered with minimal fix steps
- Non-blocking suggestions
- Test plan

Context:
- Goal:
- Affected modules:
- Runtime impact:
- How to test:
Now review the diff.
```

---

## ⚠️ 通用约定（所有等级适用）

- 默认假设：**提交者能自行阅读 git diff**
- 不要求 AI “一定发现问题”
- 明确允许输出结果为：
    - “Safe and low-risk”
    - “No red flags”

---

## 🧠 心法总结

- Review 的价值取决于 **变更的语义密度**
- Prompt 的职责是 **约束输出层级**
- 当 Review 价值不高时，允许 AI “少说或不说”
- 流程跑通本身，就是一次成功的工程实践

# commit-message-guidelines

# Commit Message Writing Guidelines

本文档用于说明本项目中 **commit message 的写法原则与判断方式**，  
目标不是追求“形式正确”，而是 **让提交历史在未来仍然可读、可理解、可回溯**。

---

## 🎯 设计目标

- 提交历史应当具备 **长期可读性**
- 避免无意义的冗长描述
- 允许在合适的场景下写详细说明
- 在“简短”与“信息充分”之间取得平衡

---

## 📐 基本结构（推荐）

本项目采用 Conventional Commits 风格，基本结构如下：

```text
<type>(<scope>): <subject>

<body>
```

说明：

- **subject 是必须的**
- **body 是可选的**
- 是否需要 body，取决于本次提交的“语义密度”

---

## 🏷️ Commit Type 的基本语义

常用类型的推荐含义：

- feat：新增功能（用户或业务能力）
- fix：问题修复
- docs：新增或修改文档内容
- chore：工程维护、整理、元数据变更
- refactor：不改变行为的代码重构
- test：测试相关

> 本项目中，docs 与 chore 容易产生重叠，需结合上下文判断。

---

## 🧭 docs vs chore 的判断原则

### 使用 docs: 当且仅当：

- 本次提交的**主要产出是文档内容**
- 即使没有代码变更，这次提交仍然有独立价值
- 文档本身承载新的规则、说明或知识

示例：

```
docs: establish git workflow and documentation structure
```

---

### 使用 chore: 当且仅当：

- 提交的核心目的是工程整理或维护
- 文档仅作为“顺带调整”
- 不引入新的规则或认知，仅做同步/迁移/清理

示例：

```
chore(docs): reorganize documentation directories
```

> 重要原则：**一致性优先于“绝对正确”**
> 在类似场景中，应尽量选择同一判断方式。

---

## ✂️ Commit Message 要不要写 Body？

### 常见误区

> “Commit message 应该越短越好”

更准确的说法是：

> **subject 应简短；body 是否存在，取决于是否“有必要”。**

---

## 🧠 何时应该写 Body（推荐）

当满足以下任意一条时，**推荐写 body**：

- 本次提交包含 **多类但相关的改动**
- 仅看 subject，**难以准确判断提交范围**
- 提交属于阶段性或里程碑性质
- 你希望未来 git log 本身就能说明问题

---

### 示例（合理的 body）

```
docs: align docs structure and add git workflow guidance

- add git workflow, branching examples, versioning, release checklist
- update docs index and repo references to docs/
- move commit conventions doc under docs/
```

这是一个 **“自解释型” commit**：

- subject 概括目标
- body 补充范围
- 不复述 diff 细节

---

## 🚫 何时可以省略 Body

当满足以下条件时，**可以只写 subject**：

- 改动非常单一
- 提交目的一眼可知
- 不会引起误解或歧义

示例：

```
docs: fix typo in README
```

---

## 🆚 两种常见 Commit 风格

### A. 索引型（Minimal）

```
docs: establish git workflow documentation
```

特点：

- 简洁
- 依赖 PR / diff 查看细节
- 适合高频、小步提交

---

### B. 自解释型（Descriptive）

```
docs: establish git workflow and docs structure

- add workflow and versioning guides
- reorganize documentation under docs/
```

特点：

- 单看 commit log 即可理解
- 适合规范、架构、样板工程
- **本项目推荐在“低频但重要提交”中使用**

---

## 📌 关于“是否把修改细节写进文件”

- ❌ 不建议为 commit 细节单独创建文件
- ✅ 需要长期参考的内容，应写入：
    - 文档（docs/）
    - 架构决策（ADR）

> **commit body 的职责是解释“这一次发生了什么”，**
> **而不是承担长期文档的角色。**

---

## 🧠 实用判断口诀

在写 commit message 前，可以自问一句：

> **如果只看 subject，未来的我是否会误判这次提交？**

- 如果会 → 写 body
- 如果不会 → body 可省

---

## ✅ 总结原则

- Commit message 是给“未来的你”和“协作者”看的
- 简短 ≠ 信息不足
- 允许在合适的场景下写清楚
- 一致性比纠结单次选择更重要
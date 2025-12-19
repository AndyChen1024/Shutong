# Git 工作流规范（Main 作为门面分支）

## 目标与原则

- **main**：门面分支
    - 始终可编译、可运行、可展示（随时能给别人看/拉下来就能跑）
    - 只合入“完成度足够”的内容（里程碑级别、或已验收的小功能）
- **dev**：集成分支
    - 承载日常开发集成，允许短暂不完美，但尽量保持“基本可跑”
- **feature/**：功能分支
    - 所有需求/功能/重构都在 feature 分支完成，再合入 dev

> 总结：main = 门面；dev = 工地入口；feature/* = 施工现场。

---

## 分支结构

长期分支：
- main（稳定、可发布）
- dev（开发集成）

短期分支：
- feature/<name>：新功能
- fix/<name>：非紧急 bug 修复（走 dev 流程）
- refactor/<name>：重构
- hotfix/<name>：线上/门面紧急修复（直接从 main 切）

---

## 分支命名规范

- feature/login
- feature/note-editor
- fix/startup-crash
- refactor/modularization
- hotfix/build-fails-on-main

命名建议：
- 全小写，使用 - 分隔
- 一个分支只解决一类问题（便于 review / 回滚）

---

## 日常开发流程（推荐默认流程）

### 1) 从 dev 拉出 feature 分支

```
git checkout dev
git pull
git checkout -b feature/xxx
```

### 2) 在 feature 分支上开发并提交

- 提交尽量小步、语义清晰
- 保持分支可自测（至少本地能跑）

### 3) 合并回 dev（建议用 PR / MR）

```
git checkout dev
git pull
git merge --no-ff feature/xxx
git push
```

> 推荐策略：**feature 合并必须通过检查（构建/测试）**，否则不要进 dev。

---

## 发布到 main 的流程（里程碑合并）

当 dev 达到一个可对外展示的里程碑（例如：v0.1、v0.2）：

### 1) dev 合并到 main

```
git checkout main
git pull
git merge --no-ff dev
```

### 2) 打 tag（强烈建议）

```
git tag v0.1.0
git push --tags
git push
```

### 3) 同步 main 回 dev（避免 dev 落后）

```
git checkout dev
git pull
git merge --no-ff main
git push
```

> 规则：**只有当 main 合并完成后仍然可运行，才算发布成功。**

---

## 热修复流程（保证 main 永远是门面）

当发现 main 有问题（编译失败 / 严重 bug），走 hotfix：

### 1) 从 main 切 hotfix

```
git checkout main
git pull
git checkout -b hotfix/xxx
```

### 2) 修复并合回 main

```
git checkout main
git merge --no-ff hotfix/xxx
git push
```

### 3) 把修复同步回 dev（非常重要）

```
git checkout dev
git pull
git merge --no-ff main
git push
```

---

## 合并策略与质量门槛

### main 的合并门槛（必须满足）

- 能编译、能运行（至少最小 demo）
- README/文档不严重过期（关键入口要一致）
- 变更有明确边界：可回滚、可定位

### dev 的合并门槛（建议满足）

- 尽量保持可跑（至少不阻塞其他 feature 集成）
- 大改动优先走 feature 分支，不要直接在 dev 上“裸写”

---

## 与提交规范的配合建议

建议在 PR 标题或 commit message 中体现类型，例如：
- feat: ...
- fix: ...
- refactor: ...
- chore: ...

并与分支前缀一致（feature/ 对应 feat: 等）。

---

## 什么时候创建 dev 分支

当项目满足以下任意一条，就应当创建 dev：
- main 需要“随时可跑”的承诺（门面化）
- 开始进入真实需求开发（功能迭代期）
- 需要更清晰的 review/集成节奏（Codex/PR）

创建命令：

```
git checkout main
git pull
git checkout -b dev
git push -u origin dev
```

---

## 常见约定

- 永远不要在 main 直接开发功能（除 hotfix）
- feature 分支尽量短命：开发完成尽快合入 dev
- 每次 dev -> main 都建议打 tag（形成可追溯里程碑）



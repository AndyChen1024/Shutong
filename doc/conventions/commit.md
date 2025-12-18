# Commit 规范（Conventional Commits Lite）

> 目标：让提交记录清晰可读、便于回滚/回溯、支持自动生成变更日志（可选）。

## 1. Commit Message 格式

```
<type>(<scope>): <subject>

<body> (可选)
<footer> (可选)
```

- **type**：提交类型（必填）
- **scope**：影响范围（建议填，非必填）
- **subject**：一句话说明（必填，简洁明确）
- **body**：补充原因、方案、注意事项（可选）
- **footer**：关联 issue / breaking change（可选）

### 示例

```
feat(auth): add phone login flow
fix(player): prevent crash when playlist is empty
chore: initial project structure and README
docs: update setup guide
refactor(core): extract network module
```

---

## 2. Type 列表（建议只用这些）

### ✅ 常用

- **feat**：新增功能（对用户可见的能力变化）
- **fix**：修复 bug
- **docs**：只改文档（README、doc、注释说明性文档）
- **refactor**：重构（不增功能不修 bug）
- **test**：测试相关（新增/修改测试）
- **style**：格式调整（不影响逻辑，如 ktlint、空格、换行）
- **perf**：性能优化（不改变功能，但更快/更省）
- **chore**：杂项/维护（脚本、工具、目录调整、初始化等）
- **build**：构建系统/依赖（Gradle、插件、依赖版本）
- **ci**：CI/CD 配置（GitHub Actions 等）
- **revert**：回滚提交

### ❌ 不建议

- feature（建议统一用 feat，避免混用）
- update、modify（语义太弱，不利于检索）

---

## 3. Scope 规范（推荐）

scope 用来说明“改动发生在哪”，建议保持短小一致。

适合你这个 Android 项目的 scope 示例：

- app
- core
- feature-xxx（如 feature-home / feature-login）
- build
- doc
- ci

示例：

```
feat(feature-login): add sms code login
fix(core-network): handle timeout properly
build: bump AGP to 8.4.x
docs(doc): add architecture notes
```

---

## 4. Subject 写法规则

- 用**动词开头**（add / fix / remove / update / refactor）
- **不要句号**结尾
- 尽量 **<= 72 chars**（英文）/ 中文尽量一句话说清
- 表达“做了什么”，不要写“改了一些东西”

✅ 好例子

- fix(auth): handle token refresh failure
- chore: init doc folder structure

❌ 不好例子

- update
- fix bug
- some changes

---

## 5. Body 什么时候必须写？

以下情况建议写 body（2~6 行就够）：

- 修 bug：说明复现条件、原因、修复策略
- 重构：说明动机、影响范围、风险点
- 架构/依赖调整：说明为什么改、如何验证

示例：

```
fix(player): avoid crash when playlist is empty

Cause: player assumed list size > 0.
Fix: guard empty list and show empty state.
Test: verified on emulator API 34.
```

---

## 6. 关联 Issue / PR（可选但推荐）

- 关联 issue：在 footer 写 Closes #123 / Refs #123

```
feat(search): support history suggestions

Closes #12
```

---

## 7. Breaking Change（未来可能用得到）

如果某个提交会导致外部调用/接口不兼容：

```
feat(api)!: rename user endpoint

BREAKING CHANGE: /v1/user -> /v1/users
```

---

## 8. 提交粒度建议（非常重要）

- 一次提交只做一类事情（不要 feat + refactor + 格式混在一起）
- 可运行/可回滚：提交点尽量保持项目可编译（至少主分支）
- 先 chore/docs 打地基，再逐步 feat/fix

---

# 文件放置建议

## 必放（对外规范）

- **仓库根目录：****CONTRIBUTING.md**
    - GitHub 最常见约定，别人一眼能找到。

## 建议再放一份（你自己的知识库）

- **doc/conventions/commit.md**
    - 方便你后续扩展（比如分支规范、发布流程、版本号规则等）。

### 建议的 doc 目录结构（你现在还没有子文件夹）

```
doc/
  conventions/
    commit.md
  architecture/
    overview.md
  decisions/
    adr-0001-xxx.md
```
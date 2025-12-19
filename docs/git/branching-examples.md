分支实践示例（Main 为门面）

示例 A：新增一个功能模块（feature）

目标：新增“学习记录列表”功能
1.	从 dev 开分支

```bash
git checkout dev
git pull
git checkout -b feature/study-log
```

2. 	开发与提交（小步提交）

* feat: add study log data model
* feat: render study log list screen
* fix: handle empty state

3.	合回 dev（PR 或本地合并）
```bash
git checkout dev
git pull
git merge --no-ff feature/study-log
git push
```
4.	里程碑到达后 dev → main + tag（见 versioning 文档）

---

示例 B：重构模块化（refactor）

目标：把某些共用能力从 app 挪到 core
1.	从 dev 开分支
```bash
git checkout dev
git checkout -b refactor/move-common-to-core
```
2.	建议策略

* 保证迁移后仍能编译运行
* 分步提交：先搬迁、后替换引用、再清理旧代码

3.	合回 dev
```bash
git checkout dev
git merge --no-ff refactor/move-common-to-core
```
> 重构原则：避免一次分支改太多“无关内容”，否则 review 很难。

---

示例 C：main 出现严重问题（hotfix）

目标：main 上启动就崩溃，需要立刻修复
1.	从 main 切 hotfix
```bash
git checkout main
git pull
git checkout -b hotfix/startup-crash
```
2.	修复并合回 main
```bash
git checkout main
git merge --no-ff hotfix/startup-crash
git push
```
3.	同步 main → dev
```bash
git checkout dev
git merge --no-ff main
git push
```

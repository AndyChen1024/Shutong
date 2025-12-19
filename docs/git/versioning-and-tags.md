版本号与 Tag 策略（统一规范执行）

总原则  
•   每次 dev -> main 合并，都必须打 tag•   main 的每一个 tag 都代表一个“可运行、可展示、可回滚”的里程碑  
•   tag 使用 语义化版本（SemVer）：vMAJOR.MINOR.PATCH
  
---  

SemVer 规则（简明版）

MAJOR（主版本）

当你做了 破坏性变更（Breaking Change）：  
•   API/模块对外接口不兼容  
•   重要配置/使用方式变更，导致旧用法无法运行  
•   大规模重构导致迁移成本明显

例：v1.0.0 -> v2.0.0
  
---  

MINOR（次版本）

当你增加了 向后兼容的新功能：  
•   新增 feature（默认不影响旧功能）  
•   新增模块、页面、能力  
•   增强功能但不破坏旧用法

例：v0.3.0 -> v0.4.0
  
---  

PATCH（修订版本）

当你做了 向后兼容的修复：  
•   修 bug•   小幅性能/体验修正  
•   文档小修（如果你希望文档发布也可追溯，可用 patch）

例：v0.4.1 -> v0.4.2
  
---  

0.x 阶段约定（你现在很可能在这个阶段）

在 0.y.z 阶段，允许更快迭代，但仍要有规范：  
•   0.MINOR：里程碑功能迭代（推荐你主要增长点）  
•   0.PATCH：修 bug / 小修正 / 小补丁  
•   当你认为项目具备“稳定骨架 + 可持续扩展 + 对外展示价值”时，再进入 1.0.0

---

版本号如何决定（建议流程）

每次准备 dev -> main：
1.  先判断是否 Breaking Change•   是：提升 MAJOR（或从 0.x 到 1.0.0）
2.  否则是否新增功能（对外可见）  
    •   是：提升 MINOR3.  否则：提升 PATCH  
    你可以把“对外可见”理解为：README 或演示路径需要更新。

---

Tag 类型：轻量 tag vs 注释 tag（建议用注释 tag）

建议使用 annotated tag（包含信息，更规范）：
```bash
git tag -a v0.3.0 -m "v0.3.0: add study log list feature"git push --tags
```  
  
---  

发布命令模板（标准化）

dev → main
```bash
git checkout maingit pullgit merge --no-ff dev
```  
打 tag
```bash
git tag -a vX.Y.Z -m "vX.Y.Z: <short summary>"git pushgit push --tags
```  
main → dev 同步
```bash
git checkout devgit pullgit merge --no-ff maingit push
```  
---  
可选增强：预发布版本（alpha/beta/rc）

如果你未来希望更“像产品”：  
•   v1.2.0-alpha.1  
•   v1.2.0-beta.1  
•   v1.2.0-rc.1

适用场景：你想在不污染正式版本的情况下做演示/测试。

但你的核心原则不变：  
只要合到 main，就必须是可运行门面。
  
---  

建议（强制执行的“门面纪律”）  
•   main 上 永远不做功能开发（除 hotfix）  
•   main 上每一个 tag 都应该能被 git checkout vX.Y.Z 拉起运行  
•   即使你不频繁发布，也要每次里程碑打 tag——这是未来追溯与回滚的底气
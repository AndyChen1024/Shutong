# Shutong 项目
Shutong 是一个使用 Kotlin 编写的原生 Android 应用。
本仓库的目标是成为一个“可学习/可教学的代码库（teachable codebase）”，并高度借鉴 Google 的 Now in Android（NIA）项目理念：清晰分层、单向数据流、可测试性，以及明确的模块/功能边界。

## 架构（借鉴 Now in Android）
- **UI：** Jetpack Compose（必要时使用 Material 3）。
- **状态管理：** 单向数据流（UDF）。建议每个屏幕/功能有且仅有一个状态持有者（通常是 `ViewModel`），对外暴露不可变的 UI State。
- **分层：** 让边界清晰可见：
  - `ui`：Composable、UI State 模型、导航 glue（粘合代码）
  - `domain`：Use-case 与业务规则（不依赖 Android 框架）
  - `data`：Repository 与数据源（Room/Network 等）
- **依赖方向：** `ui` → `domain` → `data`。避免“UI 直接触达数据源”。

## 项目结构与模块组织
当前形态（单模块 app）：
- 仅一个 Android App 模块：`app/`，使用 Kotlin + Jetpack Compose。
- 源码：`app/src/main/java`（包名在 `com/andychen/shutong` 下创建）
- 资源：`app/src/main/res`
- JVM 单元测试：`app/src/test/java`
- Instrumentation / Compose UI 测试：`app/src/androidTest/java`
- 文档/设计笔记：`doc/`
- 构建逻辑：Gradle wrapper 在 `gradle/`，共享设置在 `build.gradle.kts` 与 `settings.gradle.kts`

推荐的“功能优先（feature-first）”包结构：
- 将代码放在 `com.andychen.shutong.feature.<name>.<layer>` 下，其中 `<layer>` 为 `ui`、`domain` 或 `data`。
- 将跨功能共享的基础能力放在 `com.andychen.shutong.core.*`（例如 `core/ui`、`core/model`、`core/data`、`core/testing`），避免在各 feature 重复实现。

可选的未来模块化方向（项目变大后再做）：
- 参考 NIA 的拆分方式：**feature modules** + **core modules**
  - `feature/<name>`：功能相关 UI + domain wiring（把功能串起来的代码）
  - `core/<name>`：共享 UI、model、data、network、database、testing 工具等

## 构建、测试与开发命令
- 构建 Debug APK：`./gradlew :app:assembleDebug`
- 构建 Release Bundle：`./gradlew :app:bundleRelease`
- 运行单元测试：`./gradlew test`
- 运行 Android Lint：`./gradlew :app:lint`
- 快速编译检查：`./gradlew :app:compileDebugSources`
- 运行真机/模拟器测试：`./gradlew :app:connectedDebugAndroidTest`（请先启动 emulator/device）

说明：
- 使用 Android Studio（建议使用其自带的 JDK/SDK）。
- 永远使用仓库自带的 Gradle wrapper；除非有明确理由，不要随意升级 AGP/Kotlin/Compose/工具版本。

## 代码风格与命名约定
- Kotlin 优先（除非必要，不写 Java）。4 空格缩进；建议软限制 100–120 列。
- 倾向不可变 `val`、小函数、提前返回（early return），用清晰命名替代“聪明但难懂”的抽象。
- Compose：
  - 状态上提（state hoisting）；尽量保持 composable 纯净。
  - 避免在 composable body 中做重计算或副作用；使用 `remember` / `LaunchedEffect`。
  - Preview 命名为 `XxxPreview`。
- 测试包名/路径与源码保持一致。
- 依赖统一通过 version catalog（`gradle/libs.versions.toml`）管理；适用场景下优先使用 BOM。

## 测试与验证（NIA 风格的务实策略）
- 优先保障关键用户旅程的稳定性（导航、持久化、同步/网络边界、以及最关键的核心页面）。
- 相比脆弱的 Mock，更推荐 **“贴近真实的 Test Double”**：
  - Repository 通过接口对外暴露。
  - 测试中用可控的 Test Repository（提供测试专用 hook，例如控制数据流）来验证行为，而不是“验证某个方法被调用”。
- 提 PR 前建议至少运行：
  - 必跑：`./gradlew test :app:lint`
  - UI 有变更时：`./gradlew :app:connectedDebugAndroidTest`（至少一个 emulator/API level）

## 持续集成（CI）
如果项目配置了 CI，工作流文件应放在 `.github/workflows/`。
建议优先使用快速、确定性强的检查（编译/静态检查/单测），避免不必要的多 Variant 矩阵导致耗时和不稳定。

## 提交与 PR 规范
- 使用 Conventional Commits：`type(scope): subject`（例如 `feat(search): add recent queries`）。
- Commit 要小、可构建、单一目的，避免把重构和功能/修复混在一起。
- PR 建议包含：
  - Summary + Motivation
  - 验证步骤（命令 + 使用的 emulator/device）
  - UI 变更的截图/录屏
  - 关联 Issue（如适用：`Closes #123`）

## 配置与安全提示
- SDK/NDK 路径写在 `local.properties`。
- 不要提交任何 secret 或 API key；使用 build config 字段/占位符，并通过安全方式存储。
- 不提交生成文件与 IDE 文件（遵守 `.gitignore`）。

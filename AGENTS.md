# Shutong Project
Shutong is a native Android application written in Kotlin.
This repository is intended to be a “teachable codebase”, heavily inspired by Google’s Now in Android (NIA): clear layering, unidirectional data flow, testability, and explicit module/feature boundaries.

## Architecture (Now-in-Android-inspired)
- **UI:** Jetpack Compose + Material 3 where applicable.
- **State management:** Unidirectional data flow (UDF). Prefer a single state holder per screen/feature (typically a `ViewModel`) exposing immutable UI state.
- **Layering:** Keep boundaries explicit:
  - `ui`: composables + UI state models + navigation glue
  - `domain`: use-cases and business rules (no Android framework dependencies)
  - `data`: repositories + data sources (Room/Network/etc.)
- **Dependency direction:** `ui` → `domain` → `data`. Avoid “UI reaching into data sources”.

## Project Structure & Module Organization
Current shape (single-module app):
- Single Android app module in `app/` using Kotlin + Jetpack Compose.
- Source: `app/src/main/java` (packages under `com/andychen/shutong`)
- Resources: `app/src/main/res`
- Unit tests: `app/src/test/java`
- Instrumentation/Compose UI tests: `app/src/androidTest/java`
- Docs/design notes: `doc/`
- Build logic: Gradle wrapper in `gradle/`, shared settings in `build.gradle.kts` and `settings.gradle.kts`

Feature-first packaging (recommended):
- Put code under `com.andychen.shutong.feature.<name>.<layer>` where `<layer>` is `ui`, `domain`, or `data`.
- Keep shared primitives in `com.andychen.shutong.core.*` (e.g. `core/ui`, `core/model`, `core/data`, `core/testing`) instead of duplicating across features.

Optional future modularization (when the app grows):
- Follow NIA’s split of **feature modules** and **core modules**:
  - `feature/<name>`: feature-specific UI + domain wiring
  - `core/<name>`: shared UI, model, data, network, database, testing utilities

## Build, Test, and Development Commands
- Build debug APK: `./gradlew :app:assembleDebug`
- Build release bundle: `./gradlew :app:bundleRelease`
- Run unit tests: `./gradlew test`
- Run Android Lint: `./gradlew :app:lint`
- Quick compile check: `./gradlew :app:compileDebugSources`
- Run device/emulator tests: `./gradlew :app:connectedDebugAndroidTest` (ensure an emulator/device is running)

Notes:
- Use Android Studio with the bundled JDK/SDK.
- Always use the included Gradle wrapper; avoid bumping AGP/Kotlin/Compose/tool versions without an explicit reason.

## Coding Style & Naming Conventions
- Kotlin-first (Java only when required). 4-space indentation; soft 100–120 column limit.
- Prefer immutable `val`, small functions, early returns, and explicit naming over clever abstractions.
- Compose:
  - Hoist state; keep composables as pure as possible.
  - Avoid heavy work in composable bodies; use `remember` / `LaunchedEffect` appropriately.
  - Previews named `XxxPreview`.
- Tests mirror source packages.
- Dependencies come via the version catalog (`gradle/libs.versions.toml`). Prefer BOMs where appropriate.

## Testing & Verification (NIA-style pragmatism)
- Prioritize confidence in the critical user journeys (navigation, persistence, sync/network boundaries, and the most important screens).
- Prefer **realistic test doubles** over brittle mocks:
  - Keep repositories behind interfaces.
  - Provide test implementations that expose test-only hooks (e.g. controlling data streams) rather than verifying calls.
- Before opening a PR:
  - Always run: `./gradlew test :app:lint`
  - For UI changes, also run: `./gradlew :app:connectedDebugAndroidTest` on at least one emulator/API level.

## Continuous Integration
If CI workflows exist, they should live in `.github/workflows/`.
Prefer fast, deterministic checks (compile/lint/unit tests) and avoid running unnecessary variant matrices.

## Commit & Pull Request Guidelines
- Use Conventional Commits: `type(scope): subject` (e.g. `feat(search): add recent queries`).
- Keep commits small, buildable, and single-purpose.
- PRs should include:
  - Summary + motivation
  - Validation steps (commands + emulator/device used)
  - Screenshots/recordings for UI changes
  - Linked issues (e.g. `Closes #123`) when applicable

## Configuration & Security Tips
- SDK/NDK locations stay in `local.properties`.
- Never commit secrets or API keys; use build config fields/placeholders and secure storage.
- Keep generated/IDE files out of git (respect `.gitignore`).

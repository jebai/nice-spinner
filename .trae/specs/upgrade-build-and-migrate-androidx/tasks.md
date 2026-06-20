# Tasks

- [x] Task 1: 升级 Gradle Wrapper 和根 build.gradle
  - [x] 1.1: 将 gradle-wrapper.properties 中的 distributionUrl 更新为 Gradle 8.4
  - [x] 1.2: 更新根 build.gradle：AGP 升级到 8.1.2，移除 kotlin-gradle-plugin 声明，移除 jcenter()，添加 Kotlin 插件仅用于测试子项目
- [x] Task 2: 更新 gradle.properties，添加 AndroidX 配置
  - [x] 2.1: 添加 `android.useAndroidX=true`
  - [x] 2.2: 添加 `android.enableJetifier=true`
  - [x] 2.3: 添加 `android.nonTransitiveRClass=true`（AGP 8.x 推荐）
- [x] Task 3: 升级 library 模块 build.gradle
  - [x] 3.1: 移除 `kotlin-android-extensions` 插件声明（保留 `kotlin-android` 用于测试）
  - [x] 3.2: 移除 buildscript 块（AGP 版本由根项目管理）
  - [x] 3.3: 升级 compileSdkVersion 和 targetSdkVersion 到 34
  - [x] 3.4: 升级 compileOptions 的 sourceCompatibility/targetCompatibility 到 17
  - [x] 3.5: 将 `com.android.support:appcompat-v7:28.0.0` 替换为 `androidx.appcompat:appcompat:1.6.1`
  - [x] 3.6: 移除 `kotlin-stdlib-jdk7` 主代码依赖（移至 androidTestImplementation）
  - [x] 3.7: 添加 namespace 声明（AGP 8.x 要求）
- [x] Task 4: 升级 app 模块 build.gradle
  - [x] 4.1: 升级 compileSdkVersion 和 targetSdkVersion 到 34
  - [x] 4.2: 升级 compileOptions 的 sourceCompatibility/targetCompatibility 到 17
  - [x] 4.3: 将 support 依赖替换为 AndroidX 依赖
  - [x] 4.4: 添加 namespace 声明（AGP 8.x 要求）
- [x] Task 5: 迁移 library Java 源码的 Android Support import 到 AndroidX
  - [x] 5.1: NiceSpinner.java - 替换所有 `android.support.*` import 为 `androidx.*`
  - [x] 5.2: NiceSpinnerBaseAdapter.java - 替换 `android.support.*` import 为 `androidx.*`
- [x] Task 6: 迁移 app 模块的 Android Support 引用到 AndroidX
  - [x] 6.1: MainActivity.java - 替换 `android.support.v7.app.AppCompatActivity` 为 `androidx.appcompat.app.AppCompatActivity`
  - [x] 6.2: activity_main.xml - 替换 `android.support.constraint.ConstraintLayout` 为 `androidx.constraintlayout.widget.ConstraintLayout`
- [x] Task 7: 处理 Kotlin 测试文件的兼容性
  - [x] 7.1: 在 library build.gradle 中为 androidTest 保留 Kotlin 依赖支持
  - [x] 7.2: 确保 Kotlin 测试代码中的 AndroidX import 正确

# Task Dependencies
- Task 1 必须先完成（其他任务依赖新的 AGP/Gradle 版本配置）
- Task 2 必须在 Task 3、4 之前完成（AndroidX 标志需要先设置）
- Task 3 和 Task 4 可以并行执行
- Task 5 和 Task 6 可以并行执行，但应在 Task 3、4 之后
- Task 7 依赖 Task 3

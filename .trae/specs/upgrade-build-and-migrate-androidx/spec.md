# 升级构建工具链 & 迁移 AndroidX Spec

## Why
该项目使用的 AGP 3.5.1 + Gradle 5.4.1 不兼容 Java 17，在当前 Java 17 环境下无法编译。同时项目依赖了旧的 Android Support Library，需要迁移到 AndroidX。Library 模块声明了 Kotlin 依赖但主代码未使用 Kotlin，应移除。

## What Changes
- 升级 Gradle Wrapper 到 8.4（兼容 Java 17）
- 升级 AGP 到 8.1.2（兼容 Java 17 + Gradle 8.x）
- 移除 Library 模块中未使用的 Kotlin 插件和依赖（仅测试代码使用 Kotlin，测试保留 Kotlin 支持）
- 将 `android.support.*` 迁移到 `androidx.*`（Java 源码 import、XML 命名空间、依赖声明）
- 升级 compileSdkVersion / targetSdkVersion 到 34
- 升级 appcompat 到 AndroidX 对应版本
- 升级 constraint-layout 到 AndroidX 版本
- 添加 `android.useAndroidX=true` 和 `android.enableJetifier=true` 到 gradle.properties
- 移除已废弃的 `jcenter()` 仓库
- 升级 compileOptions 的 sourceCompatibility/targetCompatibility 到 Java 17

## Impact
- Affected code: 所有 Java 源文件中的 `android.support.*` import、XML 布局文件、所有 build.gradle 文件、gradle-wrapper.properties、gradle.properties
- **BREAKING**: 最低需要 Java 17 运行环境

## ADDED Requirements

### Requirement: Java 17 构建兼容
系统 SHALL 使用兼容 Java 17 的 Gradle 和 AGP 版本，确保在 Java 17 环境下可以正常编译。

#### Scenario: Java 17 下编译成功
- **WHEN** 开发者在 Java 17 环境下执行 `./gradlew build`
- **THEN** 构建成功，无版本兼容性错误

### Requirement: 移除 Library 主代码中未使用的 Kotlin 依赖
系统 SHALL 从 library 模块的主代码构建中移除 Kotlin 插件和依赖，因为主代码全部为 Java。Kotlin 仅用于 androidTest，应保留测试的 Kotlin 支持。

#### Scenario: Library 主代码不依赖 Kotlin
- **WHEN** 检查 library 模块的 build.gradle
- **THEN** 主代码编译不依赖 `kotlin-android` 和 `kotlin-android-extensions` 插件及 `kotlin-stdlib` 依赖
- **AND** androidTest 仍可使用 Kotlin 编写测试

### Requirement: 迁移到 AndroidX
系统 SHALL 将所有 `android.support.*` 引用替换为对应的 `androidx.*` 包。

#### Scenario: 源码 import 迁移完成
- **WHEN** 检查所有 Java 源文件
- **THEN** 不存在 `android.support.*` 的 import 语句
- **AND** 所有引用替换为对应的 `androidx.*` 包

#### Scenario: XML 布局迁移完成
- **WHEN** 检查所有 XML 布局文件
- **THEN** 不存在 `android.support.*` 的标签和命名空间引用

#### Scenario: 依赖声明迁移完成
- **WHEN** 检查所有 build.gradle 文件
- **THEN** 依赖声明使用 `androidx.*` 坐标而非 `com.android.support:*`

## MODIFIED Requirements

### Requirement: 构建配置版本
- Gradle Wrapper: 5.4.1 → 8.4
- AGP: 3.5.1 → 8.1.2
- compileSdkVersion: 28 → 34
- targetSdkVersion: 28 → 34
- sourceCompatibility / targetCompatibility: 1.8 → 17
- appcompat: `com.android.support:appcompat-v7:28.0.0` → `androidx.appcompat:appcompat:1.6.1`
- constraint-layout: `com.android.support.constraint:constraint-layout:1.1.3` → `androidx.constraintlayout:constraintlayout:2.1.4`

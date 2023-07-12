# 迁移到version catalogs

## 一、什么是version catalogs

version catalogs是Gradle 7.0引入的一种新特性，它可以让您以可扩容的方式添加和维护依赖项和插件。使用version catalogs，您可以在一个中央的TOML文件中定义依赖项和插件的名称和版本，然后在各个模块中以类型安全的方式引用它们。这样可以避免在多个build文件中硬编码依赖项名称和版本，也可以方便地进行依赖项的升级和管理。

## 二、为什么要迁移到version catalogs

迁移到version catalogs有以下好处：

- 提高build文件的可读性和一致性，减少重复和冗余的代码
- 支持代码补全和导航，提高开发效率和准确性
- 方便地在一个地方管理依赖项和插件的版本，避免版本冲突和不匹配的问题
- 支持多项目构建和复用，提高构建性能和稳定性

传统的方式是在gradle中定义几个常量，这种方式虽然可以做到统一定义，但是**不方便我们查找版本号**
使用version catalogs可以ctrl+mouse left**快速定位**到依赖项定义处

## 三、如何迁移到version catalogs

迁移到version catalogs的过程分为以下几个步骤：

### 3.1 创建版本目录文件

首先，在根项目的gradle文件夹中，创建一个名为libs.versions.toml的文件。Gradle默认会在这个文件中查找目录，因此我们建议使用这个默认名称。在这个文件中，添加以下部分：

```toml
[versions]
[libraries]
[plugins]
```

这些部分的含义如下：

- 在versions代码块中，定义用于保存依赖项和插件版本的变量。您可以在后续代码块（libraries和plugins代码块）中使用这些变量。
- 在libraries代码块中，定义依赖项。
- 在plugins代码块中，定义插件。

### 3.2 迁移依赖项

在libs.versions.toml文件的versions和libraries部分，为每个依赖项添加一个条目。同步您的项目，然后将build文件中的声明替换为相应的目录名称。

移除依赖项之前的build.gradle.kts文件：

```kotlin
dependencies {
    implementation("androidx.core:core-ktx:1.9.0")
}
```

在版本目录文件(`libs.versions.toml`)中定义依赖项：

```toml
[versions]
ktx = "1.9.0"

[libraries]
androidx-ktx = { group = "androidx.core", name = "core-ktx", version.ref = "ktx" }
```

在为目录中的依赖项代码块命名时建议使用kebab case（例如androidx-ktx），以便在build文件中获得更好的代码补全帮助。

在需要此依赖项的每个模块的build.gradle文件中，按照您在TOML文件中定义的名称定义依赖项。

```kotlin
dependencies {
    implementation(libs.androidx.ktx)
}
```

### 3.3 迁移插件

在libs.versions.toml文件的版本和插件部分，为每个插件添加一个条目。

同步您的项目，然后将build文件中plugins {}代码块内的声明替换为相应的目录名称。

移除插件之前的build.gradle文件：

```kotlin
// Top-level `build.gradle.kts` file
plugins {
    id("com.android.application") version "7.4.1" apply false
    id("com.android.library") version "7.4.1" apply false
    id("org.jetbrains.kotlin.android") version "1.5.31" apply false
}
```

在版本目录文件(`libs.versions.toml`)中定义插件：

```toml
[versions]
androidGradlePlugin = "7.4.1"
kotlin = "1.5.31"

[plugins]
android-application = { id = "com.android.application", version.ref = "androidGradlePlugin" }
android-library = { id = "com.android.library", version.ref = "androidGradlePlugin" }
kotlin-android = { id = "org.jetbrains.kotlin.android", version.ref = "kotlin" }
```

与依赖项一样，在为plugins代码块目录条目设置格式时建议使用kebab case（例如android-application），以便在build文件中获得更好的代码补全帮助。

在顶级和模块级build.gradle文件中定义com.android.application插件。对于来自版本目录文件的插件，请使用alias；对于并非来自版本目录文件的插件（例如惯例插件），请使用id。

```kotlin
// Top-level build.gradle
plugins {
    alias(libs.plugins.android.application) apply false
    alias(libs.plugins.android.library) apply false
    alias(libs.plugins.kotlin.android) apply false
}

// module build.gradle
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
}
```

注意：如果您使用的是低于8.1的Gradle版本，则需要在使用版本目录时为plugins {}代码块添加注解(@Suppress("DSL_SCOPE_VIOLATION"))。

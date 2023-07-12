# Jetpack导航

Mavericks提供了一个`mavericks-navigation`工件，使其能够轻松使用导航图范围内的ViewModels。

## 开始使用

在你的build.gradle文件中添加该依赖项：
```groovy
dependencies {
  implementation 'com.airbnb.android:mavericks-navigation:x.y.z'
}
```
mavericks-navigation的最新版本是[！[Maven Central](https://maven-badges.herokuapp.com/maven-central/com.airbnb.android/mavericks-navigation/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.airbnb.android/mavericks-navigation)

##初始化Mavericks

除了其他设置步骤，在[initialize Mavericks]（/setup.md#initialize）时，你需要传入一个自定义的viewModelDelegateFactory。要做到这一点，请替换：
```kotlin
Mavericks.initialize(this)
// or 
MockableMavericks.initialize(this)
```
用：
```kotlin
Mavericks.initialize(this, viewModelDelegateFactory = DefaultNavigationViewModelDelegateFactory())
// or 
MockableMavericks.initialize(this, viewModelDelegateFactory = DefaultNavigationViewModelDelegateFactory())
```

## 用法

一旦你完成了这些，你可以使用`navGraphViewModel()`委托来获得一个ViewModel，它的范围是导航图的id。

要查看其用法的例子，请查看样本[这里](https://github.com/airbnb/mavericks/blob/main/sample-navigation/src/main/java/com/airbnb/mvrx/sample/navigation/FlowFragments.kt)。
# Jetpack Navigation

Mavericks ships with a `mavericks-navigation` artifact that makes it easy to use nav graph scoped ViewModels.

## Getting Started

Add the dependency to your build.gradle file:
```groovy
dependencies {
  implementation 'com.airbnb.android:mavericks-navigation:x.y.z'
}
```
The latest version of mavericks-navigation is [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.airbnb.android/mavericks-navigation/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.airbnb.android/mavericks-navigation)

## Initialize Mavericks

In addition to the other setup steps, when you [initialize Mavericks](/setup.md#initialize), you will need to pass in a custom viewModelDelegateFactory. To do that, replace:
```kotlin
Mavericks.initialize(this)
// or 
MockableMavericks.initialize(this)
```
with:
```kotlin
Mavericks.initialize(this, viewModelDelegateFactory = DefaultNavigationViewModelDelegateFactory())
// or 
MockableMavericks.initialize(this, viewModelDelegateFactory = DefaultNavigationViewModelDelegateFactory())
```

## Usage

Once you have done that, you may use the `navGraphViewModel()` delegate to get a ViewModel scoped to the navigation graph with that id.

To view an example of its usage, check out the sample [here](https://github.com/airbnb/mavericks/blob/main/sample-navigation/src/main/java/com/airbnb/mvrx/sample/navigation/FlowFragments.kt).

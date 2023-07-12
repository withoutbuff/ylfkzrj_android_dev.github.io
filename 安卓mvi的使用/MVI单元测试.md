##单元测试
当为视图模型编写单元测试时，强迫支持的循环程序同步运行，以及控制视图模型的状态是有帮助的。

Mavericks通过可选的`mavericks-testing`工件，在JUnit 4和5测试中提供了对此的管理支持。

要使用，应用测试规则或测试扩展，通过适当的属性配置来满足你的需求。请参阅各自类的kdoc，了解最新的使用文档。

默认情况下，该规则/扩展将：
- 禁用视图模型观察者的生命周期意识
- 使得状态存储的操作是同步的
- 禁用对视图模型的调试检查
- 将TestCoroutineDispatcher换成Main coroutine dispatcher。

### JUnit 5扩展

将`MvRxTestExtension`添加到你的测试的伙伴对象中。
```kotlin
@RegisterExtension
val mavericksTestExtension = MavericksTestExtension()
```

### JUnit 4规则

添加`MvRxTestRule`规则到你的测试中。

```kotlin
@get:Rule
val mavericksRule = MavericksTestRule()
```

## Unit Testing
When writing unit tests for view models it is helpful to force the backing coroutines to run synchronously, as well as to have control over the state of the viewmodel.

Mavericks provides support for managing this in both JUnit 4 and 5 tests via the optional `mavericks-testing` artifact.

To use, apply either the test rule or test extension, configured to your needs via the appropriate properties. See the kdoc on the respective classes for the most up to date documentation on usage.

By default, the rule/extension will:
- Disable lifecycle awareness of viewmodel observers
- Make state store operations synchronous
- Disable debug checks on viewmodels
- Swap TestCoroutineDispatcher for the Main coroutine dispatcher.

### JUnit 5 Extension

Add the `MvRxTestExtension` to your test's companion object.
```kotlin
    @RegisterExtension
    val mavericksTestExtension = MavericksTestExtension()
```

### JUnit 4 Rule

Add the `MvRxTestRule` rule to your test.

```kotlin
    @get:Rule
    val mavericksRule = MavericksTestRule()
```

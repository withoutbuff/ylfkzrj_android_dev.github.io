## 尽可能使用派生属性 :id=derived

[派生属性](core-concepts.md#derived-properties)在规模上非常有用，因为它们非常容易推理和测试。只要有东西可以被建模为派生属性，你就应该把它放在那里。
例子：
``kotlin
data class CounterState(
    val count: Int = 0
) : MavericksState {
    val isEven = count % 2 == 0
}
```
```kotlin
data class SignUpState(
    val firstName: String = "",
    val lastName: String = "",
    val email: String = ""
) : MavericksState {
    val hasName = firstName.isNotBlank() && lastName.isNotBlank()

    val hasValidEmail = EMAIL_PATTERN.matches(email)

    val canSubmitForm = hasName && hasValidEmail
}
```

## 使用不可变的地图工作
使用[自定义复制和删除扩展函数](https://gist.github.com/gpeal/3cf0907fc80094a833f9baa309a1f627)，并将其与数据类类似：

`setState { copy(yourMap = yourMap.copy("a" to 1, "b" to 2)) }`

或

`setState { copy(yourMap = yourMap.delete("a", "b")) }`

## 从ViewModel.onEach驱动动画

你可以使用`onEach`回调来驱动一个动画或者在特定的时间内显示一个错误，就像这样：
```kotlin
viewModel.onAsync(YourState::yourProp, onFail = {
    binding.error.isVisible = true
    delay(4000)
    binding.error.isVisible = false
})
```

如果你的Fragment View被摧毁，Mavericks将停止发射新的值，但它不会取消任何现有的值。在这种情况下，如果Fragment View在延迟期间被销毁，当你试图访问它以隐藏4秒后的错误时，你的应用程序将崩溃。为了解决这个问题，请将该代码替换为：
```kotlin
viewModel.onAsync(YourState::yourProp, onFail = {
    viewLifecycleOwner.lifecycleScope.whenStarted {
        binding.error.isVisible = true
        delay(4000)
        binding.error.isVisible = false
    }
})
```

`whenStarted`是Jetpack的[生命周期感知的coroutine支持](https://developer.android.com/topic/libraries/architecture/coroutines#suspend)的一部分。
## Use derived props when possible :id=derived

[Derived properties](core-concepts.md#derived-properties) are extremely useful at scale because they are so easy to reason about and test. Whenever something _can_ be modeled as a derived property, you probably _should_ put it there.
Examples:
```kotlin
data class CounterState(
    val count: Int = 0
) : MavericksState {
    val isEven = count % 2 == 0
}
```
```kotlin
data class SignUpState(
    val firstName: String = "",
    val lastName: String = "",
    val email: String = ""
) : MavericksState {
    val hasName = firstName.isNotBlank() && lastName.isNotBlank()

    val hasValidEmail = EMAIL_PATTERN.matches(email)

    val canSubmitForm = hasName && hasValidEmail
}
```

## Working with immutable maps
Use [custom copy and delete extension functions](https://gist.github.com/gpeal/3cf0907fc80094a833f9baa309a1f627) and treat them similar to data classes:

`setState { copy(yourMap = yourMap.copy(“a” to 1, “b” to 2)) }`

or

`setState { copy(yourMap = yourMap.delete(“a”, “b”)) }`

## Drive animations from ViewModel.onEach

You may use an `onEach` callback to drive an animation or show an error for a specific amount of time like this:
```kotlin
viewModel.onAsync(YourState::yourProp, onFail = {
    binding.error.isVisible = true
    delay(4000)
    binding.error.isVisible = false
})
```

If your Fragment View gets destroyed, Mavericks will stop emitting new values but it will _not_ cancel any existing ones. In this case, if the Fragment view gets destroyed during the delay, your app will crash when you try and access it to hide the error after 4 seconds. To get around this, replace that code with:
```kotlin
viewModel.onAsync(YourState::yourProp, onFail = {
    viewLifecycleOwner.lifecycleScope.whenStarted {
        binding.error.isVisible = true
        delay(4000)
        binding.error.isVisible = false
    }
})
```

`whenStarted` is part of Jetpack's [lifecycle aware coroutine support](https://developer.android.com/topic/libraries/architecture/coroutines#suspend).

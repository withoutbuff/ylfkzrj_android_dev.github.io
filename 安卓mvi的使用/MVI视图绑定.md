# 视图绑定

[View Binding]（https://developer.android.com/topic/libraries/view-binding）是Google对`findViewById()`、Kotlin合成访问器和[Butterknife]（https://github.com/JakeWharton/butterknife）的替代。
虽然严格来说与Mavericks无关，但我们的示例应用程序使用自定义委托，只需一行代码就能轻松使用View Binding。
将[FragmentViewBindingDelegate.kt](https://github.com/airbnb/mavericks/blob/2.0.0/sample/src/main/java/com/airbnb/mvrx/sample/utils/FragmentViewBindingDelegate.kt)添加到你的项目中，然后你可以像这样使用它：
```kotlin
class CounterFragment : BaseFragment(R.layout.counter_fragment) {
  private val binding: CounterFragmentBinding by viewBinding()
  private val viewModel: CounterViewModel by fragmentViewModel()

  override fun invalidate() = withState(viewModel) { state ->
    binding.textView.text = "Count: ${state.count}"
  }
}
```

`by viewBinding()`委托感觉与Mavericks视图模型的委托语法非常相似，所以两者配合得很好。

# View Binding

[View Binding](https://developer.android.com/topic/libraries/view-binding) is Google's replacement for `findViewById()`, Kotlin synthetic accessors, and [Butterknife](https://github.com/JakeWharton/butterknife).
Although not strictly Mavericks related, our sample apps use custom delegates to easily use View Binding with a single line of code.
Add [FragmentViewBindingDelegate.kt](https://github.com/airbnb/mavericks/blob/2.0.0/sample/src/main/java/com/airbnb/mvrx/sample/utils/FragmentViewBindingDelegate.kt) to your project then you can use it like this:
```kotlin
class CounterFragment : BaseFragment(R.layout.counter_fragment) {
  private val binding: CounterFragmentBinding by viewBinding()
  private val viewModel: CounterViewModel by fragmentViewModel()

  override fun invalidate() = withState(viewModel) { state ->
    binding.textView.text = "Count: ${state.count}"
  }
}
```

The `by viewBinding()` delegate feels very similar to the Mavericks View Model delegate syntax so the two play well together.

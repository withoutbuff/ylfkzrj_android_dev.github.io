# Fragment Arguments

尽管Mavericks不一定要和Fragments一起使用，但它有一些帮助工具以备不时之需。

### 设置Mavericks Fragment的参数
当创建一个Fragment时：
1\. 创建一个`Parcelable`或`Serializable`对象
2\. 将Fragment参数设置为`yourArgs.asMavericksArgs()`。
```kotlin
val fragment = YourFragment()
fragment.arguments = yourArgs.asMavericksArgs()
```

###在你的Fragment中检索你的参数
在你的片段中创建一个属性：
```kotlin
private val args: YourArgsType by args()
```
这就是了!

###在 "MavericksState "的初始值中使用片段args
创建一个二级构造函数，它有一个与你的Fragment参数类型相同的参数。Mavericks会自动从你的Fragment中获取参数，并在创建你的State时使用这些参数调用这个构造函数。
```kotlin
data class MyState(
  val itemId: String,
  val item: Async<Item> = Uninitialized
) : MavericksState {
  constructor(args: YourArgs) : this(args.itemId)
}
```
# Fragment Arguments

Although Mavericks, doesn't have to be used with Fragments, it has some helpers in case it is.

### Setting Mavericks Fragment arguments
When creating a Fragment:
1. Create a `Parcelable` or `Serializable` object
2. Set the Fragment arguments to `yourArgs.asMavericksArgs()`

```kotlin
val fragment = YourFragment()
fragment.arguments = yourArgs.asMavericksArgs()
```

### Retrieving your arguments in your Fragment
Create a property in your fragment:
```kotlin
private val args: YourArgsType by args()
```
That's it!

### Using Fragment args in the initial value for `MavericksState`
Create a secondary constructor that has 1 parameter of the same type as your Fragment arguments. Mavericks will automatically get the arguments off your Fragment and call this constructor with them when creating your State.
```kotlin
data class MyState(
  val itemId: String,
  val item: Async<Item> = Uninitialized
) : MavericksState {
  constructor(args: YourArgs) : this(args.itemId)
}
```

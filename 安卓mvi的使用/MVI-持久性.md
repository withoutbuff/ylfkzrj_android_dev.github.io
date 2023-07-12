# 持久性
在Android中，有两种主要情况必须考虑持久性：

#### 配置变化/后栈
传统上，你会使用savedInstanceState来保存重要信息。然而，在Mavericks中，你会得到ViewModel的完全相同的实例，这样你就有了重新渲染视图所需的一切。一般来说，屏幕不需要在Mavericks中实现savedInstanceState。

#### 在一个新的进程中恢复
有时，Android会杀死你的进程以节省内存，并在一个新的进程中恢复你的应用程序，只保存instanceState。在这种情况下，谷歌建议只保存足够的数据，如id，以便你可以重新获取你需要的东西。当你回到你的应用程序时，Mavericks将尝试重新创建所有存在于原始进程中的ViewModels。在大多数情况下，从默认状态或片段参数（见上文）创建的初始状态已经足够了。然而，如果你想在状态中持久化单个片段，你可以在你的状态类中用`@PersistState`来注释属性。注意这些属性必须是Parcelable或Serializable，并且不能是Async对象，因为持久化和恢复加载状态会导致用户体验的破坏。

```kotlin
data class FormState(
  @PersistState val firstName: String = "",
  @PersistState val lastName: String = "",
  @PersistState val homeTown: String = ""
) : MavericksState
```

在这种情况下，如果进程被销毁并重新创建，名字、姓氏和家乡将已经被Mavericks填充到你的ViewModel的初始状态中。
# Persistence
In Android, there are two major cases in which persistence must be considered:

#### Configuration Changes / Back Stack
Traditionally, you would use savedInstanceState to save important information. However, with Mavericks, you will get the exact same instance of your ViewModel back so you have everything you need to re-render the view. Generally, screens won't need to implement savedInstanceState with Mavericks.

#### Restoration in a New Process
Sometimes, Android will kill your process to save memory and restore your app in a new process with just savedInstanceState. In this case, Google recommends saving just enough data such as ids so that you can re-fetch what you need. When you return to your app, Mavericks will attempt to recreate all ViewModels that existed in the original process. In most cases, the initial state created from default state or fragment arguments (see above) is enough. However, if you would like to persist individual pieces in state, you may annotate properties in your state class with `@PersistState`. Note that these properties must be Parcelable or Serializable and may not be Async objects because persisting and restoring loading states would lead to a broken user experience.

```kotlin
data class FormState(
  @PersistState val firstName: String = "",
  @PersistState val lastName: String = "",
  @PersistState val homeTown: String = ""
) : MavericksState
```

In this case, if the process is destroyed and recreated, the first name, last name, and home town will already be populated by Mavericks in your ViewModel's initial state.

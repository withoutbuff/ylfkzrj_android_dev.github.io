# Jetpack Compose

在Jetpack Compose的世界里，Mavericks仍然是有用的。通过Jetpack Compose，你可以在所有的用户界面上使用Compose，但在业务逻辑、数据获取、依赖注入对象图的接口等方面使用Mavericks。

要使用Mavericks与Jetpack Compose，请添加`com.airbnb.android:mavericks-compose`工件。

## 创建一个MavericksViewModel

要开始了，像以前一样创建一个MavericksViewModel。现在，通过`mavericks-compose`工件，你可以像这样在composable中获取或创建一个ViewModel：

```kotlin
@Composable
fun MyComponent() {
    val viewModel: CounterViewModel = mavericksViewModel()
    // You can now call functions on your viewModel.
}
```

默认情况下，视图模型的范围是最近的`LifecycleOwner'，但如果你需要，可以指定一个不同的范围。参考`mavericksViewModel()`的文档，了解更多关于自定义作用域的信息。

## 访问状态

如果你在你的视图模型上调用`collectAsState()`，没有参数，它将返回一个带有整个ViewModel状态类的[Compose State property](https://developer.android.com/jetpack/compose/state)，并且会在状态改变时进行更新。

你也可以传入一个状态属性引用或一个映射器函数，以获得一个[Compose State property](https://developer.android.com/jetpack/compose/state)，只针对该属性或映射器函数的变化。

推荐的方法是单独订阅一小部分状态属性，并将大的组合体分解成小的组合体。这样，当状态发生变化时，Compose可以更有效地重新组合你的UI。

```kotlin
@Composable
fun MyComponent() {
    val viewModel: CounterViewModel = mavericksViewModel()
    // All changes to your state
    val state by viewModel.collectAsState()
    // Just count.
    val count1 = viewModel.collectAsState(CounterState::count)
    // Equivalent to count but with more flexibility for nested props.
    val count2 = viewModel.collectAsState { it.count }

    Text("Count is ${state.count}")
    Text("Count is $count1")
    Text("Count is $count2")
}
```

## Compose + Mavericks 心理模型

在考虑把什么类型的代码放在Compose中，把什么放在Mavericks中时，要考虑：

1\. Compose将你的屏幕渲染成State的一个函数。
2\. Mavericks创建并更新状态。

举个具体的例子，如果你要获取天气，你可以用Mavericks从API中使用`execute`获取特定邮编的天气。

然后，你的屏幕将订阅该状态，并使用composables发出适当的用户界面。

如果用户想查询另一个地点的天气，他们将使用compose构建的用户界面，输入一个新的地点，然后点击提交。然后，那个可合成的将调用MavericksViewModel上的一个函数来更新位置，这将触发一个新的天气API`执行`调用。

该调用的结果将自动在订阅天气状态的可组合式中更新。

##范围内的ViewModels

默认情况下，`mavericksViewModel()`将获得或创建一个视图模型，其范围是最近的`LocalLifecycleOwner`。在大多数情况下，这将是最近的`NavBackStackEntry`、`Fragment`或`Activity`。如果你需要指定一个自定义的范围，传递一个不同的`LifecycleOwner`作为第一个参数。例如，要把视图模型的作用域定在`Activity'上，就把`LocalContext.current作为ComponentActivity'传递。`ComponentActivity`是`AppCompatActivity`和`FragmentActivity`的超类。

为了方便起见，`mavericksActivityViewModel()'的存在是为了将ViewModel的范围扩大到主机活动。

Mavericks也需要一个`ViewModelStoreOwner`和`SavedStateRegistryOwner`。在许多情况下（如`NavBackStackEntry`、`Fragment`和`Activity`），`LifecycleOwner`也将实现这些类，但如果你需要指定自定义所有者，你可以。只要确保它们有相同的生命周期，否则你可能会得到意想不到的行为。
# Jetpack Compose

Mavericks is still useful in a Jetpack Compose world. With Jetpack Compose, you can use Compose for all of your UI but Mavericks for your business logic, data fetching, interface to your dependency injection object graph, etc.

To use Mavericks with Jetpack Compose, add the `com.airbnb.android:mavericks-compose` artifact.

## Creating a MavericksViewModel

To get started, create a MavericksViewModel just like you always have. Now, with the `mavericks-compose` artifact, you can get or create a ViewModel in a composable like this:

```kotlin
@Composable
fun MyComponent() {
    val viewModel: CounterViewModel = mavericksViewModel()
    // You can now call functions on your viewModel.
}
```

By default, the view model will be scoped to the closest `LifecycleOwner` but you can specify a different scope if you need to. Refer to the docs for `mavericksViewModel()` for more information on custom scopes.

## Accessing State

If you call `collectAsState()` on your view model with no parameters, it will return a [Compose State property](https://developer.android.com/jetpack/compose/state) with the whole ViewModel state class and will update any time the state changes.

You can also pass in a state property reference or a mapper function to get a [Compose State property](https://developer.android.com/jetpack/compose/state) for changes to just that property or mapper function.

The recommended approach is to subscribe individually to a small set of State properties and to break down large Composables into smaller ones. This allows Compose to more efficiently recompose your UI when state changes.

```kotlin
@Composable
fun MyComponent() {
    val viewModel: CounterViewModel = mavericksViewModel()
    // All changes to your state
    val state by viewModel.collectAsState()
    // Just count.
    val count1 = viewModel.collectAsState(CounterState::count)
    // Equivalent to count but with more flexibility for nested props.
    val count2 = viewModel.collectAsState { it.count }

    Text("Count is ${state.count}")
    Text("Count is $count1")
    Text("Count is $count2")
}
```

## Compose + Mavericks Mental Model

When considering what type of code to put in Compose and what to put in Mavericks, consider:

1. Compose renders your screen as a function of State.
2. Mavericks creates and updates the state.

To use a concrete example, if you are fetching the weather, you would use Mavericks to fetch the weather for a specific zip code from an API using `execute`.

Your screen would then subscribe to that state and emit the appropriate UI using composables.

If the user wanted to look up the weather for another location, they would use the UI built with compose to type in a new location, and hit submit. That composable would then call a function on the MavericksViewModel to update the location which would then trigger a new weather API `execute` call.

The results of that call will automatically update in the composable subscribing to the weather state.

## Scoping ViewModels

By default, `mavericksViewModel()` will get or create a view model scoped to the nearest `LocalLifecycleOwner`. In most cases, this will be the closest `NavBackStackEntry`, `Fragment`, or `Activity`. If you need to specify a custom scope, pass in a different `LifecycleOwner` as the first parameter. For example, to scope a view model to the `Activity`, pass `LocalContext.current as ComponentActivity`. `ComponentActivity` is the super class of both `AppCompatActivity` and `FragmentActivity`.

As a convenience, `mavericksActivityViewModel()` exists to scope a ViewModel to the host activity.

Mavericks needs a `ViewModelStoreOwner` and `SavedStateRegistryOwner` as well. In many cases (such as `NavBackStackEntry`, `Fragment`, and `Activity`), the `LifecycleOwner` will also implement these classes but if you need to specify custom owners, you can. Just ensure that they have the same lifecycles or else you could get unexpected behavior.

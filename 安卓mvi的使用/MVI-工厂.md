# 工厂
在许多情况下，让Mavericks创建你的ViewModel和State实例就足够了。然而，有些时候你想拦截它们的创建，以注入你自己的依赖或初始状态。

## MavericksViewModelFactory
为了拦截ViewModel或State的创建，在你的ViewModel中创建一个同伴对象，并使其实现`MavericksViewModelFactory`。
```kotlin
class MyViewModel(initialState: MyState, dataStore: DataStore) : MavericksViewModel(initialState) {
    companion object : MavericksViewModelFactory<MyViewModel, MyState>
}
```

###创建一个状态类
在你的`MavericksViewModelFactory`中，覆盖`initialState(..)`并返回所需的初始状态。它将只被调用一次，当ViewModel第一次被创建时。所有后续的状态更新都将在ViewModel内部处理。

你可以用它来绑定诸如当前的userId或其他可以设置为初始值的状态属性。

### 创建一个ViewModel
在你的`MavericksViewModelFactory`中，覆盖`create(..)`并返回你的ViewModel实例。你可以用它来启用你的ViewModels中的构造函数注入等功能。更多信息见[依赖注入](dependency-injection.md)。

传递给这个函数的初始状态应该直接传递到你的ViewModel中，而无需进一步修改。

#### ViewModelContext
ViewModelContext将是 "ActivityViewModelContext "或 "FragmentViewModelContext"，这取决于ViewModel的拥有者范围。这被传递到两个工厂函数中，可以用来做一些事情，如检索应用程序、活动、片段或片段的args。任何这些都足以做一些事情，比如访问你的依赖注入图。

## 示例

```kotlin
class MyViewModel(
    initialState: MyState,
    ...
) : MavericksViewModel<MyState>(initialState) {
    ...

    companion object : MavericksViewModelFactory<MyViewModel, MyState> {

        override fun initialState(viewModelContext: ViewModelContext): MyState {
            return MyState(...)
        }

        override fun create(viewModelContext: ViewModelContext, state: MyState): MyViewModel {
            return MyViewModel(state, ...)
        }
    }
}

```

# Factories
In many cases, it is sufficient to let Mavericks create your ViewModel and State instances. However, there are times when you want to intercept their creation to inject your own dependencies or initial state.

## MavericksViewModelFactory
To intercept either ViewModel or State creation, create a companion object in your ViewModel and make it implement `MavericksViewModelFactory`
```kotlin
class MyViewModel(initialState: MyState, dataStore: DataStore) : MavericksViewModel(initialState) {
    companion object : MavericksViewModelFactory<MyViewModel, MyState>
}
```

### Creating a State class
In your `MavericksViewModelFactory`, override `initialState(...)` and return the desired initial state. It will only be called once, when the ViewModel is first created. All subsequent state updates will be handled inside the ViewModel.

You can use this to bind things like the current userId or other state properties that can be set as initial values.

### Creating a ViewModel
In your `MavericksViewModelFactory`, override `create(...)` and return your ViewModel instance. You can use this to enable things like constructor injection in your ViewModels. See [dependency injection](dependency-injection.md) for more info.

The initial state passed in to this function should be passed directly into your ViewModel without further modification.

#### ViewModelContext
ViewModelContext will either be `ActivityViewModelContext` or `FragmentViewModelContext` depending on the scope of who owns the ViewModel. This is passed into both factory functions and can be used to do things like retrieve the application, activity, fragment, or fragment args. Any of those should be enough to do things like access your dependency injection graph.

## Example

```kotlin
class MyViewModel(
    initialState: MyState,
    ...
) : MavericksViewModel<MyState>(initialState) {
    ...

    companion object : MavericksViewModelFactory<MyViewModel, MyState> {

        override fun initialState(viewModelContext: ViewModelContext): MyState {
            return MyState(...)
        }

        override fun create(viewModelContext: ViewModelContext, state: MyState): MyViewModel {
            return MyViewModel(state, ...)
        }
    }
}

```

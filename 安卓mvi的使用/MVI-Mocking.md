# Mocking
Mavericks包括模拟ViewModel状态的工具，然后可以用于自动和手动测试。

这种模拟支持是通过可选的 "mavericks-mocking "工件添加的，并包括以下工具：

- 为MavericksViewModel强制（即 "模拟"）一个特定的MavericksState，同时防止（并监听）后续的变化。
- 强制MavericksView中的所有ViewModel进入特定状态
- 生成Kotlin代码，从运行中的设备上重建状态快照，然后可用于模拟。
- 轻松修改模拟的状态以定义状态变化
- 在Launcher活动中显示所有的MaverickViews和它们的模拟状态，并能够打开任何模拟状态。

## Philosophy

在Mavericks中，每个ViewModel的State类都能完全代表和定义屏幕的内容和行为。通过加载特定的模拟，我们可以测试屏幕在该状态下的行为方式。

这可以用来在屏幕上运行自动测试，设置各种模拟状态并断言行为正确，这可以包括屏幕截图测试或验证交互时行为的测试。

此外，这在开发或手动测试应用时也很有帮助，因为应用中的任何屏幕都可以被打开到任何特定的状态，有可能避免多次点击来到达一个原本深度嵌套的屏幕。

在Kotlin源代码中定义模拟状态可以帮助存根出网络或其他异步请求，还可以确保模拟在编译时是有效的（相对于可能使用JSON或在源代码外指定模拟的模拟系统而言）。

更多技术细节，请阅读关于Airbnb如何开发这个测试基础设施的系列文章--[https://medium.com/airbnb-engineering/better-android-testing-at-airbnb-3f5b90b9c40a](https://medium.com/airbnb-engineering/better-android-testing-at-airbnb-3f5b90b9c40a)

## 初始化和配置

1\. 在你的build.gradle文件中添加一个对`mavericks-mocking`工件的额外依赖。版本划分与Mavericks核心库相同。

2\. 2.当你的应用程序被创建时，通过调用`MockableMavericks.initialize'来初始化Mavericks。(这取代了对`Mavericks.initialize`的正常调用)

3\. 3\. 使用`MockableMavericksView`接口来代替普通的`MavericksView`，并覆盖`provideMocks`函数来定义模拟状态（指导如下）。如果你使用`mavericks-rxjava2`，你可以让你的类同时实现`MvRxView`和`MockableMavericksView`。

4\. 4\. 为了支持为活动屏幕生成模拟，在视图初始化时调用`registerMockPrinter'（例如，在Fragment的`onCreate'）。

## 提供模拟状态

对于每个实现了`MockableMavericksView`的Mavericks屏幕（例如一个Fragment），你可以覆盖`provideMocks`函数，为该视图中的ViewModels定义模拟。

Mavericks嘲讽神器提供了几个扩展函数来帮助你定义这些嘲讽状态。根据视图使用的ViewModels的数量，你应该使用以下函数之一：

- `mockNoViewModels` - 如果视图没有viewmodels (它可能仍然有Fragment参数)
- `mockSingleViewModel` - 当视图有一个视图模型时
- `mockTwoViewModels` - 当视图有两个视图模型时。
- 对于更多的视图模型数量，其他函数也遵循这个命名模式。

这些函数中的每一个都遵循类似的模式，即它所需要的参数：

1\. 对于每个视图模型，该函数需要对视图中的视图模型属性的引用，以及该视图模型的 "默认 "状态（解释见下文）。
2\. 2.如果你的视图需要参数被初始化（如Fragment参数），必须提供默认参数
3\. 3.一个mock builder lambda，允许你指定除默认之外的额外mock变化（下文有详细说明）

例如，一个有单一视图模型且没有参数的Fragment可能有这样的实现：
```kotlin
class MyFragment : Fragment, MockableMavericksView {
    
    override fun provideMocks() = mockSingleViewModel(
        viewModelReference = MyFragment::myViewModelProperty,
        defaultState = myViewModelState,
        defaultArgs = null
    ) {
// 在这里可以选择声明添加状态的变化
    }
}
```

如果你的Fragment使用Mavericks范式，在Arguments中以`Mavericks.KEY_ARG`为键传递一个Parcelable数据类，那么你可以使用该类的实例作为你的默认参数。
```kotlin
class MyFragment : Fragment, MockableMavericksView {
    
    override fun provideMocks() = mockSingleViewModel(
        viewModelReference = MyFragment::myViewModelProperty,
        defaultState = myMockedViewModelState,
        defaultArgs = MyCustomArgs(id = 1)
    ) {
// 在这里可以选择声明添加状态的变化
    }
}

@Parcelize
data class MyCustomArgs(
 val id: Long
): Parcelable
```
如果你不使用`Mavericks.KEY_ARG`作为你的参数，你可以传递一个Bundle给`defaultArgs`，当Fragment被模拟时，它将被原样传递。

### 默认状态和参数

传递给顶级模拟函数的`defaultState`和`defaultArgs`参数应该被认为是该页面上数据的典型代表。一般来说，其中的任何属性都不应该是空的，有一个空的List或Collection，是未定义的，处于加载或错误状态，等等。

一个默认的模拟（命名为 "默认状态"）会根据你传入的默认状态自动为你创建。此外，如果提供了默认的args，也会自动创建一个 "默认初始化 "模拟。这将测试用默认参数和这些参数产生的状态来初始化你的ViewModel。

对默认状态的模拟变化可以描述用户可能遇到的差异，例如数据处于加载或错误状态。

这样做的目的有两个方面：

1\. 它使屏幕的典型表示标准化，然后你可以用它作为测试的基础
2\. 2.它允许在可管理的片段中轻松定义和测试各种变化。

此外，Mavericks自动添加了另一个名为 "进程重新创建后的默认状态 "的默认模拟。这是以默认状态为基础的，但有Mavericks的状态保存和恢复操作，以模拟状态对进程死亡的反应。

## Mocks是如何被使用的

一旦视图通过 "provideMocks "为其ViewModels定义了模拟，Mavericks就会在运行时处理这些模拟，并将其打包成一个方便的类来使用。

这个类提供了一个函数，可以返回一个新的视图实例，该实例用模拟中定义的参数（如果有的话）进行实例化，并且所有的ViewModels被冻结在模拟中定义的状态。

默认提供了一个Launcher活动，允许你浏览和打开你的模拟视图，但如果你想使用低级别的模拟机制进行自定义测试，你也可以访问这些机制。

###启动模拟屏幕

Mavericks提供了一个内置的入口，通过[**mock launcher**](launcher.md)来访问mock。

### Mocks的自定义用法
对于一个给定的可模拟视图，每个模拟都会创建一个`MockedViewProvider'。它包含一个lambda(`createView`)，你可以调用它来创建该视图的实例。它将被初始化为模拟中指定的参数，并自动将其ViewModels强制到该模拟中定义的状态。

要访问这些模拟视图提供者，请使用`ViewMocker.kt`文件中的函数 - `getMockVariants`或`mockVariants`。

如果你正在为你的屏幕设置自动测试，这样手动访问mock是很有帮助的。

#### 高级配置和使用

用于初始化Mavericks的`MockableMavericks`对象有默认的配置，可以满足基本的使用。然而，如果你使用mock来为你的屏幕创建自定义测试系统，你可以利用高级配置来对mock有更多的控制和可视性。

嘲讽行为是通过两个主要的全局属性来控制ViewModel的创建：

- `Mavericks.viewModelDelegateFactory`是一个全局属性。
- `Mavericks.viewModelConfigFactory`。

`MockableMavericks`对象使用`MockViewModelDelegateFactory`和`MockMavericksViewModelConfigFactory`中的实现来为这些属性设置值。你可以选择切换这些实现的设置，对其进行子类化以修改其行为，或者完全建立你自己的实现。

`MockMavericksViewModelConfigFactory'特别有助于修改自定义测试的模拟行为。可以通过铸造属性`Mavericks.viewModelConfigFactory`来访问该实现，然后可以配置其行为。

在`MockMavericksViewModelConfigFactory`中，你可以改变`mockBehavior`属性，它指定了模拟的视图模型的默认行为。就其核心而言，"mocking "意味着在ViewModel上强制执行一个特定的状态。然而，在行为上有几个细微的差别，根据你的目的来控制是很重要的。这些包括：

- 是否可以在初始的模拟状态上设置新的状态
- 如何处理视图中被注入 "existingViewModel "的ViewModel？
- 当ViewModel试图用 "execute "异步加载数据时，该怎么做？
- 如果允许新的状态变化，它们是像生产中那样异步发生，还是同步发生以进行确定性的测试？
- 被模拟的视图是否在状态变化时被订阅更新？

`mockBehavior`允许你通过指定每个模拟屏幕的默认行为来控制这些方面。此外，你可以通过使用`MockMavericksViewModelConfigFactory.pushMockBehaviorOverride`来改变现有模拟屏幕的行为。例如，在你想以完全模拟的方式启动屏幕，阻止状态变化，然后在以后允许状态变化来测试点击的情况下，这可能是有帮助的。

MavericksViewModelConfigFactory还提供了一个函数`addOnConfigProvidedListener`，你可以用来监听每个ViewModel的实例化。这有助于获得对每个ViewModel创建的钩子。

当视图被模拟时，Mavericks会在内部跟踪视图实例以及它的ViewModels，这样它就可以正确地模拟它们。为了防止这些引用在你使用完被模拟的视图后泄漏，你可以在 "MockedViewProvider "提供的 "MockedView "上调用`cleanupMockState`。

你也可以通过`MockableMavericks.mockStateHolder`访问并清除所有mock的全局存储。

## 生成模拟Generating Mocks

Mavericks ViewModel的状态是用Kotlin数据类实现的，所以状态的模拟实现是由Kotlin代码定义的，它实例化了一个状态类，并为每个数据类属性提供测试值。

为了使之有效，重要的是模拟的数据要尽可能广泛。对于有很多字段的复杂状态类来说，手动编写创建这些模拟所需的Kotlin代码是很乏味的。嘲讽有可能是成千上万行的代码。

为了帮助你创建模拟，Mavericks提供了一个模拟打印机工具，当你有一个通过ADB连接的设备正在运行你的应用程序时，你可以从你的本地计算机上运行该工具。当这个工具运行时，它将向你的应用程序发送一个意图，告诉Mavericks生成所需的Kotlin代码，为当前屏幕上的任何ViewModels重新创建状态。这些生成的代码将被脚本从设备中提取出来，并写入你机器上的本地".kt "文件，这样你就可以把它们作为模拟实现使用。

从本质上讲，这允许你捕捉任何Mavericks视图的状态快照，将其保存到源文件中，并在未来任何时候将其作为模拟视图重新加载以进行测试。

要做到这一点，你必须首先确保`MockableMavericksView.registerMockPrinter`在你的视图创建时被调用。这将在你的MavericksView上注册一个生命周期观察者，当视图处于 "开始 "的生命周期状态时，它将使用广播接收器来监听脚本的意图。

脚本本身是用Kotlin编写的，被打包成一个独立的可执行文件，你可以从Mavericks Github仓库下载。它可以在`mock_generation/MavericksMockPrinter`找到。

当你的应用程序是活的，并通过ADB连接（启用调试），通过`./mock_generation/MavericksMockPrinter`从你的电脑上运行模拟打印机工具。建议从你的应用程序的根项目目录运行这个工具，这样生成的模拟源文件可以被复制到正确的目录中。

你可以用帮助标志运行该工具 - `./MavericksMockPrinter -h`。

建议你为每个ViewModel生成并保存一个完全模拟的状态，这将是你传递给可模拟视图的`provideMocks`函数的 "默认状态"。

然后你可以通过下面描述的DSL来创建这个模拟状态的变化。这种方法减少了你需要维护的完整的mock的数量。
###高级使用说明和配置

模拟打印机使用反射来识别State类的主要构造函数属性，在运行时检查其值，并创建Kotlin代码，以相同的值来实例化该类的另一个实例。这是以递归的方式来捕获所有嵌套数据结构的状态。

这是可能的，因为Kotlin数据类有一个可预测的语法，通过命名的参数参数来构造它们的主构造函数。

然而，这也意味着，如果你的State中包含的任何类（包括任何嵌套层）不是Kotlin数据类或基元（例如Java类），那么模拟打印机将无法生成代码来准确地重建它。

你可以通过实现 "TypePrinter "接口并将你的实现添加到 "MockableMavericks.mockPrinterConfiguration.customTypePrinters "中来指示模拟打印机如何处理这些类型。你必须创建你自己的`MockPrinterConfiguration`的实例。

例如，一些应用程序可能有传统的`AutoValue`java类。Mavericks提供了一个`AutoValueTypePrinter`，它可以识别AutoValue生成的类，并知道如何正确生成代码来捕获它们的状态。

你也可以使用`MockPrinterConfiguration`来控制生成的模拟将具有哪个包名。

## 定义模拟状态的变化

一旦默认状态被设置好了，你就可以对你的参数或状态声明模拟变体。每个变体都应该被看作是一个测试--和大多数测试一样，它应该针对你的视图中的一个特定行为。

理想的情况是，模拟变量可以测试用户可能遇到的所有现实的数据变化。但通常情况下，为所有可能的数据排列组合定义变体是不现实的，也是没有帮助的--相反，尝试针对常见的情况或预期的边缘情况，如错误状态、加载或可忽略的属性。

模拟变量是通过Kotlin DSL的`state`函数来定义的。每个变体都有一个 "名称 "参数来描述它。
```kotlin
val defaultState = MyState(...)

override fun provideMocks() = mockSingleViewModel(MyFragment::MyViewModel, defaultState) {

    // Each mock is defined with the "state" function.
    // The name should describe the variation, and
    // the state it represents should be returned from the lambda
    state(name = "Null user") {
        MyState(user = null)
    }
}
```
一般来说，由于状态对象很复杂，我们不想为每个变化从头开始创建一个新的对象。相反，我们使用Kotlin的数据类copy函数，用我们想要的变化来修改 "默认状态"。

默认状态是state lambda的接收器，所以我们可以在lambda中直接调用copy

```kotlin
val defaultState = MyState(...)

override fun provideMocks() = mockSingleViewModel(MyFragment::MyViewModel, defaultState) {

    state(name = "Null user") {
        // The receiver, or "this", is the defaultState from mockSingleViewModel
        copy(user = null)
    }
}
```
像这样修改默认状态使定义一个变体变得更加容易。这就是为什么前面的章节强调了生成一个全面的默认状态的重要性。你用于测试的模拟集合可以由典型的默认状态和你可能想测试的许多微小的变化组成。

复杂的状态对象通常有很深的嵌套数据，使用复制函数来改变这些数据会很繁琐。

```kotlin
val state = MyState(
    account = Account(
        user = User(
            name = "Brian"
        )
    )
)

// Set user name to null... gross :(
state.copy(account = state.account.copy(user = state.account.user.copy(name = null)))
```
作为一个更简单的选择，你可以使用`set`函数，它是一个DSL工具，只存在于这个嘲讽环境中
```kotlin
val defaultState = MyState(
    account = Account(
        user = User(
            name = "Brian"
        )
    )
)

override fun provideMocks() = mockSingleViewModel(MyFragment::MyViewModel, defaultState) {

    state(name = "Null user name") {
        // This DSL says that we want to set the nested property 'name'
        // to be null
        set { ::account { ::user { ::name } } }.with { null }
    }
}
```
这种设置属性的DSL是通过指定一个嵌套的属性以及它应该被设置的值来实现的。这些属性使用属性引用语法来指定对象中的哪个属性应该被修改。每个lambda块代表对象层次结构中的另一个嵌套层。

注意，这只适用于Kotlin数据类。另外，由于我们的数据是不可变的，它不会修改原始状态，而是将其复制并更新指定的属性--新的对象被返回。

如果你需要改变多个属性，你可以连锁调用设置：
```kotlin
state("null user name and null email") {
   set { ::account { ::user { ::name } } }.with { null }
   .set { ::account { ::user { ::email } } }.with { null }
}
```
记住，这不会改变原始状态，所以只有返回的单一状态对象被用于新状态的变化。这意味着，如果你有多个`set`调用，它们必须用`.`链起来。
### Mocking多个ViewModels

为多个视图模型定义模拟状态变化与单个视图模型的情况非常相似。唯一的区别是，我们需要为每个状态定义一个特定的视图模型。

为此，你可以使用函数`mockTwoViewModels`和`mockThreeViewModels`（如果需要，甚至更多的视图模型也有类似的变化）。

```kotlin
provideMocks() = mockTwoViewModels(
    viewModel1Reference = SearchFragment::searchResultsViewModel,
    defaultState1 = mockSearchState,
    viewModel2Reference = SearchFragment::userAccountViewModel,
    defaultState2 = userAccountState,
    defaultArgs = SearchArgs(query = "Hawaii")
) {
    state(name = "no query, no user") {
        viewModel1 {
          setNull { ::query }
        }

        viewModel2 {
            setNull { ::user }
        }
    }
}
```
在前面的例子中，当使用单一视图模型时，`state`函数的lambda只需要为单一视图模型返回一个状态对象。

现在有了两个视图模型，lambda使用一个构建器对象作为它的接收器，我们可以通过函数`viewModel1`和`viewModel2`来指定哪个视图模型为这个变化设置一个新的状态。

默认情况下，每个视图模型继承其默认状态，所以我们可以选择只改变其中一个视图模型的状态。在这种情况下，视图模型2保持其默认状态，而视图模型1做出改变。
```kotlin
    state(name = "no query") {
        viewModel1 {
          setNull { ::query }
        }
    }
```
### 辅助函数

在`set`DSL上有一些变化，以帮助处理常见的情况。

* `setNull`将任何属性值设置为空
* `setTrue`或`setFalse`来改变一个布尔值
* `setEmpty`将一个列表属性设置为空列表
* `setZero`将一个数字属性设置为零

例如 `set { ::account { ::user { ::name } } }.with { null }`可以缩短为`setNull { ::account { :user { :name } } }`

**在一个Async属性内设置一个属性：**
添加`success`块来表示一个处于成功状态的Async属性。
```kotlin
setTrue { ::searchResults { success { ::resultCount } } }
```
**为一个Async属性的加载和失败状态定义一个模拟变量：**。
这对于在一个短行中为加载和失败创建两个模拟是很有用的
```kotlin
stateForLoadingAndFailure { ::searchResults }
```
注意，这只对State对象顶层的Async属性有效。

如果你正在模拟两个视图模型，你可以使用`viewModel1StateForLoadingAndFailure`和`viewModel2StateForLoadingAndFailure`代替。

或者，你可以单独修改加载或错误状态：
```kotlin
state("Loading") {
    setLoading { ::searchResults }
}

state("Failed") {
    setNetworkFailure { ::searchResults }
}
```
### Mocking参数

如果你的片段需要参数，那么你的mock函数必须定义默认参数：
```kotlin
mockSingleViewModel(MyFragment::MyViewModel, defaultState, defaultArgs)
```
这些参数被提供给每个模拟变量，所以当模拟片段被创建时，它被初始化了参数，然后通过视图模型覆盖模拟状态。

Mavericks视图模型会自动从片段参数创建初始状态，这也是为你测试的。一个专门的初始化模拟会使用你提供的`defaultArgs`自动创建。

如果你想为你的片段测试其他参数的初始化，你可以用`args`函数来做。
```kotlin
mockSingleViewModel(MyFragment::MyViewModel, defaultState, defaultArgs) {

    args("null id") {
        setNull { ::id }
    }
}
```
这与用state函数声明的mocks操作非常相似。默认参数是lambda的接收器，你必须返回一个新的参数实例。
这个例子假设参数是一个数据类，它使用Mavericks.KEY_ARG模式将参数传递给bundle中的一个视图。

如果你的片段直接访问参数（而不是仅仅使用它们来初始化它的MavericksState）--那么你可能想测试特定参数和状态之间的交互。你可以通过将参数传递给一个状态模拟函数来实现这个目的。
```kotlin
mockSingleViewModel(MyFragment::MyViewModel, defaultState, defaultArgs) {

    state(
       name = "null user name and args missing id",
       args = { setNull { ::id } }
    ) {
        setNull { ::listing { ::user { ::name } } }
    }
}

If args are not provided to a state variation then the default args are used.
```
### Combining Mocks

如果你的视图有很多模拟，或者有不同的默认状态或参数来测试你的模拟，你可以使用`combineMocks`函数将你的模拟分成几组。
```kotlin
override fun provideMocks() = combineMocks(
    "Standard Mocks" to standardMocks(),
    "Other Mocks" to otherMocks()
)
```
这允许我们将实现分离到单独的模拟文件中，这使得定义许多模拟变得更加简单和干净。如果你的模拟非常大，或者有很大的变化，不能通过正常的模拟变化DSL轻易捕获，这也是很有帮助的。

# Mocking
Mavericks includes tooling to mock ViewModel state, which can then be used for automated and manual testing.

This mocking support is added with the optional `mavericks-mocking` artifact, and includes tooling to:

- Force (ie "mock") a specific MavericksState for a MavericksViewModel, while preventing (and listening for) subsequent changes
- Force all ViewModels within a MavericksView to specific states
- Generate Kotlin code that reconstructs State snapshots off of a running device, which can then be used for mocking
- Easily modify mocked states to define state variations
- Display all MaverickViews and their mocked states in a Launcher activity, with the ability to open up to any mocked state

## Philosophy

With Mavericks, the State classes of each ViewModel ideally completely represent and define the content and behavior of a screen. By loading specific mocks we can test how the screen behaves with that state.

This can be used to run automated tests on a screen, setting various mock states and asserting correct behavior, which can include screenshot tests or tests that verify behavior upon interaction.

Additionally, this can be helpful when developing or manually testing the app, as any screen in the app can be opened to any specific state, potentially avoiding many clicks to reach an otherwise deeply nested screen.

Defining mock states in Kotlin source code can help to stub out network or other async requests, and also ensures that mocks are valid at compile time (as opposed to a mocking system that might use JSON or otherwise specify mocks outside of source code)

For more technical details, read the article series on how Airbnb developed this testing infrastructure - [https://medium.com/airbnb-engineering/better-android-testing-at-airbnb-3f5b90b9c40a](https://medium.com/airbnb-engineering/better-android-testing-at-airbnb-3f5b90b9c40a)

## Initialization and Configuration

1. Add an additional dependency on the `mavericks-mocking` artifact in your build.gradle file. Versioning is the same as the core Mavericks library.

2. Initialize Mavericks via a call to `MockableMavericks.initialize` when your application is created. (This replaces the normal call to `Mavericks.initialize`)

3. Use the `MockableMavericksView` interface in place of the normal `MavericksView` and override the `provideMocks` function to define mock states (guidance below). If you are using `mavericks-rxjava2` you can have your classes implement both `MvRxView` and `MockableMavericksView`.

4. To support generating mocks for an active screen, call the `registerMockPrinter` when the view is initialized (for example, in `onCreate` of the Fragment)

## Providing Mock States

For each Mavericks screen that implements `MockableMavericksView` (eg a Fragment) you can override the `provideMocks` function to define mocks for the ViewModels in that view.

The Mavericks mocking artifact provides several extension functions to help you define these mock states. Depending on how many ViewModels the view uses, you should use one of the following functions:

- `mockNoViewModels` - If the view has no viewmodels (It may still have Fragment arguments)
- `mockSingleViewModel` - When the view has one view model
- `mockTwoViewModels` - When the view has two view models
- Additional functions follow this naming pattern for higher view model counts

Each of these functions follows a similar pattern for the parameters it requires:

1. For each view model, the function requires a reference to the view model property in the view, as well as a "default" state for that view model (Explained below)
2. If your view requires arguments to be initialized (such as Fragment arguments), default arguments must be provided
3. A mock builder lambda that allows you to specify additional mock variations besides the default (Details described below)

For example, a Fragment that has a single view model and no arguments might have this implementation:
```kotlin
class MyFragment : Fragment, MockableMavericksView {
    
    override fun provideMocks() = mockSingleViewModel(
        viewModelReference = MyFragment::myViewModelProperty,
        defaultState = myViewModelState,
        defaultArgs = null
    ) {
        // Optionally declare additions state variations here
    }
}
```

If your Fragment uses the Mavericks paradigm of passing a Parcelable data class in the Arguments under the key `Mavericks.KEY_ARG` then you can use an instance of that class as your default arguments.
```kotlin
class MyFragment : Fragment, MockableMavericksView {
    
    override fun provideMocks() = mockSingleViewModel(
        viewModelReference = MyFragment::myViewModelProperty,
        defaultState = myMockedViewModelState,
        defaultArgs = MyCustomArgs(id = 1)
    ) {
        // Optionally declare additions state variations here
    }
}

@Parcelize
data class MyCustomArgs(
 val id: Long
): Parcelable
```

If you don't use the `Mavericks.KEY_ARG` for your arguments you can pass a Bundle to `defaultArgs` and it will be passed along as-is when the Fragment is mocked.

### The Default State and Arguments

The `defaultState` and `defaultArgs` parameters passed to the top level mock function should be thought of as the canonical representation of data on that page. Generally, no properties in it should be null, have an empty List or Collection, be undefined, be in a loading or error state, etc.

A default mock (named “Default state”) is created for you automatically based on the default state you pass in. Additionally, if default args are provided, a “Default initialization” mock is also automatically created. This tests initializing your ViewModel with the default arguments and the state that results from those arguments.

Mock variations to the default state can describe possible differences a user might encounter, such as data in a loading or error state.

The purpose of this is two-fold:

1. It standardizes a canonical representation of the screen which you can then use as a basis for testing
2. It allows variations to be easily defined and tested in manageable pieces

Additionally, Mavericks automatically adds another default mock named "Default state after process recreation". This is based on default state, but has the Mavericks state saving and restoring operations applied to it to simulate how the state reacts to process death.

## How Mocks Are Used

Once a view has defined mocks for its ViewModels via `provideMocks`, Mavericks processes them at runtime and packages them into a convenient class for use.

This class provides a function that returns a new instance of the view that is instantiated with the arguments defined in the mock (if any), and with all ViewModels frozen to the state defined in the mock.

A Launcher activity is provided by default that allows you to browse and open your mocked Views, but you can also access the low level mocking mechanisms if you would like to use them for custom testing.

### Launching Mocked Screens

Mavericks provides a built in entry point for accessing mocks via the [**mock launcher**](launcher.md).

### Custom Usage Of Mocks
For a given mockable view, a `MockedViewProvider` is created for each mock. This contains a lambda (`createView`) you can invoke to create an instance of that view. It will be initialized with the arguments specified in the mock, and automatically have its ViewModels forced to the states defined in that mock.

To access these mock view providers, use the functions inside the `ViewMocker.kt` file - either `getMockVariants` or `mockVariants`.

Accessing mocks manually this way can be helpful if you are setting up automated testing for your screens.

#### Advanced Configuration and Usage

The `MockableMavericks` object that is used for initializing Mavericks has default configurations that works for basic usage. However, if you are using mocks to create custom testing systems for your screens you can leverage advanced configuration to have more control and visibility into the mocks.

Mocking behavior is controlled via two main global properties that control ViewModel creation:

 - `Mavericks.viewModelDelegateFactory`
 - `Mavericks.viewModelConfigFactory`

The `MockableMavericks` object uses the implementations in `MockViewModelDelegateFactory` and `MockMavericksViewModelConfigFactory` to set values for these properties. You have the option to toggle settings on these implementations, subclass them to modify their behavior, or build your own implementations entirely.

The `MockMavericksViewModelConfigFactory` in particular is helpful for modifying mock behavior for custom tests. The implementation can be accessed by casting the property `Mavericks.viewModelConfigFactory` and its behavior can then be configured.

In `MockMavericksViewModelConfigFactory` you can change the `mockBehavior` property, which specifies the default behavior of mocked view models. At it's core, "mocking" means forcing a specific state on a ViewModel. However, there are several nuances to behavior that can be important to control based on your purposes. These include:

- Whether new state can be set over the initial mocked state
- How to handle ViewModel's in a view that are injected with "existingViewModel"
- What to do when a ViewModel attempts to load data asynchronously with `execute`
- If new state changes are allowed, do they happen async as in production, or synchronously to facility deterministic testing
- Do mocked views get subscribed update their update upon state changes

`mockBehavior` allows you to control these aspects by specifying default behavior for each mocked screen. In addition, you can change the behavior of an existing mocked screen by using `MockMavericksViewModelConfigFactory.pushMockBehaviorOverride`. For example, this can be helpful in cases where you want to start the screen as fully mocked with state changes blocked, and then later on allow state change to test clicks.

The `MavericksViewModelConfigFactory` also provides a function `addOnConfigProvidedListener` that you can use to listen for the instantiation of each ViewModel. This is helpful to get a hook into the creation of each ViewModel.

When a view is mocked, Mavericks internally tracks the View instance as well as its ViewModels, so that it can properly mock them. To prevent these references from leaking after you are done using the mocked view you can invoke `cleanupMockState` on the `MockedView` provided by the `MockedViewProvider`.

You can alternatively access and clear the entire global store of all mocks via `MockableMavericks.mockStateHolder`.

## Generating Mocks

Mavericks ViewModel state is implemented with Kotlin data classes, so a mocked implementation of a State is defined by Kotlin code that instantiates a State class and provides test values for each data class property.

For this to be effective it is important that the mocked data is as extensive as possible. For complex state classes with many fields it can be tedious to manually write the Kotlin code required to create these mocks. Mocks can potentially be thousands of lines of code.

To help you create mocks, Mavericks provides a Mock Printer tool that you can run from your local computer when you have a device connected via ADB that is running your app. When this tool is run it will send an intent to your app that will tell Mavericks to generate the Kotlin code needed to recreate the States for any ViewModels that are currently on screen. This generated code will then be pulled from the device by the script and written to local `.kt` files on your machine so that you can use them as mock implementations.

Essentially, this allows you to capture a snapshot of the State of any of your Mavericks Views, save it to a source file, and reload it as a mocked view at any time in the future for testing.

For this to work you must first make sure `MockableMavericksView.registerMockPrinter` is called when your view is created. This registers a lifecycle observer on your MavericksView that will use a Broadcast Receiver to listen for the scripts intent while the view is in the "Started" lifecycle state.

The script itself is written in Kotlin and packaged as a standalone executable that you can download from the Mavericks Github repository. It can be found at `mock_generation/MavericksMockPrinter`

While your app is live and attached via ADB (with debugging enabled), run the mock printer tool via `./mock_generation/MavericksMockPrinter` from your computer. It is recommended to run this from your app's root project directory so the generated mock source files can be copied to the right directory for you.

You can run the tool with the help flag - `./MavericksMockPrinter -h`

It is recommended that you generate and save a single fully mocked state per ViewModel, which will be the "default state" that you pass to the `provideMocks` function of your mockable View.

You can then create variations of this mocked state via the DSL described below. This approach reduces how many complete mocks you have to maintain.

### Advanced Usage Notes and Configuration

The mock printer uses reflection to identify the primary constructor properties of your State class, inspect the values at runtime, and create Kotlin code that can instantiate another instance of the class with the same values. This is done recursively to capture the state of all nested data structures.

This is possible because Kotlin data classes have a predictable syntax for their construction via named parameter arguments to their primary constructor.

However, this also means that if there is any class contained within your State (including at any nested level) that is not a Kotlin data class or primitive (eg a Java class), then the mock printer will not be able to generate code to accurately reconstruct it.

You can instruct the Mock Printer how to handle types like these by implementing the `TypePrinter` interface and adding your implementations to `MockableMavericks.mockPrinterConfiguration.customTypePrinters`. You will have to create your own instance of `MockPrinterConfiguration`.

For example, some apps may have legacy `AutoValue` java classes. Mavericks provided a `AutoValueTypePrinter` that recognizes AutoValue generated classes and knows how to properly generate code to capture their state.

You can also use the `MockPrinterConfiguration` to control which package name the generated mocks will have.

## Defining Mock State Variations

Once default state has been set up, you can declare mock variations to your arguments or state. Each variation should be thought of as a test - and like most tests, it should target one specific behavior in your View.

Ideally mock variations would test all realistic data permutations that a user might encounter. Often though this is not realistic or helpful to define variations for all possible data permutations - instead, try to target common cases or expected edge cases such as error states, loading, or nullable properties.

Mock variations are defined via a Kotlin DSL with the `state` function. Each variation has a "name" parameter that describes it.

```kotlin
val defaultState = MyState(...)

override fun provideMocks() = mockSingleViewModel(MyFragment::MyViewModel, defaultState) {

    // Each mock is defined with the "state" function.
    // The name should describe the variation, and
    // the state it represents should be returned from the lambda
    state(name = "Null user") {
        MyState(user = null)
    }
}
```

Generally, since State objects are complex we don't want to create a new one from scratch for each variation. Instead, we use Kotlin's data class copy function to modify the "default state" with the change we want.

The default state is the receiver of the state lambda, so we can call copy directly in the lambda

```kotlin
val defaultState = MyState(...)

override fun provideMocks() = mockSingleViewModel(MyFragment::MyViewModel, defaultState) {

    state(name = "Null user") {
        // The receiver, or "this", is the defaultState from mockSingleViewModel
        copy(user = null)
    }
}
```

Modifying the default state like this makes it much easier to define a variation. This is why earlier sections emphasize the importance of generating a comprehensive default state. Your collection of mocks for testing can consist of the canonical default state along with the many slight variations that you may want to test.

Complex state objects often have deeply nested data, which can be tedious to change using the copy function.

```kotlin
val state = MyState(
    account = Account(
        user = User(
            name = "Brian"
        )
    )
)

// Set user name to null... gross :(
state.copy(account = state.account.copy(user = state.account.user.copy(name = null)))
```

As a simpler alternative you can use the `set` function, which is a DSL tool that exists only within this mocking context

```kotlin
val defaultState = MyState(
    account = Account(
        user = User(
            name = "Brian"
        )
    )
)

override fun provideMocks() = mockSingleViewModel(MyFragment::MyViewModel, defaultState) {

    state(name = "Null user name") {
        // This DSL says that we want to set the nested property 'name'
        // to be null
        set { ::account { ::user { ::name } } }.with { null }
    }
}
```

This DSL for setting a property works by specifying one nested property along with the value it should be set to. The properties use the property reference syntax to specify which property in the object should be modified. Each lambda block represents another nesting layer in the object hierarchy.

Note that this ONLY works for Kotlin data classes. Also, since our data is immutable it doesn't modify the original state, but copies it with the specified property updated - the new object is returned.

If you need to change multiple properties you can chain set calls:

```kotlin
state("null user name and null email") {
   set { ::account { ::user { ::name } } }.with { null }
   .set { ::account { ::user { ::email } } }.with { null }
}
```

Remember, this doesn't mutate the original state, so only the single state object that is returned is used for the new state variation. This means that if you have multiple `set` calls they must be chained with a `.`

### Mocking Multiple ViewModels

Defining mock state variations for multiple view models is very similar to the case with a single view model. The only difference is that for each state we need to define a specific view model.

For this you can use the functions `mockTwoViewModels` and `mockThreeViewModels` (similar variations exist for even more view models if needed).

```kotlin
provideMocks() = mockTwoViewModels(
    viewModel1Reference = SearchFragment::searchResultsViewModel,
    defaultState1 = mockSearchState,
    viewModel2Reference = SearchFragment::userAccountViewModel,
    defaultState2 = userAccountState,
    defaultArgs = SearchArgs(query = "Hawaii")
) {
    state(name = "no query, no user") {
        viewModel1 {
          setNull { ::query }
        }

        viewModel2 {
            setNull { ::user }
        }
    }
}
```

When using a single view model in earlier examples the `state` function's lambda just had to return a single state object for the single view model.

Now with two view models the lambda uses a builder object as its receiver, which we can use to specify which view model to set a new state for this variation, via the functions `viewModel1` and `viewModel2`.

By default each view model inherits its default state, so we can choose to change the state of only one of the view model's. In this case view model 2 keeps its default state for this varation, while view model 1 makes a change.

```kotlin
    state(name = "no query") {
        viewModel1 {
          setNull { ::query }
        }
    }
```

### Helper Functions

There are a few variations on the `set` DSL to help with common cases.

* `setNull` to set any property value to null
* `setTrue` or `setFalse` to change a Boolean value
* `setEmpty` to set a List property to an empty list
* `setZero` to set a number property to zero

For example `set { ::account { ::user { ::name } } }.with { null }` could be shortened to `setNull { ::account { ::user { ::name } } }`

**Setting a property inside an Async property:**
Add the `success` block to represent an Async property in the Success state.
```kotlin
setTrue { ::searchResults { success { ::resultCount } } }
```

**Define a mock variation for the loading and failure states of an Async property:**
This is useful for creating two mocks, for loading and failure, in a single short line
```kotlin
stateForLoadingAndFailure { ::searchResults }
```

Note that this only works for Async properties at the top level of the State object.

If you are mocking two view models you can instead use `viewModel1StateForLoadingAndFailure` and `viewModel2StateForLoadingAndFailure`

Alternatively you can individually modify loading or error state:

```kotlin
state("Loading") {
    setLoading { ::searchResults }
}

state("Failed") {
    setNetworkFailure { ::searchResults }
}
```

### Mocking Arguments

If your fragment takes arguments, then your mock function must define default arguments:
```kotlin
mockSingleViewModel(MyFragment::MyViewModel, defaultState, defaultArgs)
```

These arguments are provided to every mock variation, so that when the mocked fragment is created it is initialized with the arguments, and then has the mocked state overlaid via the viewmodel.

Mavericks viewmodels automatically create initial state from fragment arguments, and this is tested for you as well. A dedicated initialization mock is automatically created using the `defaultArgs` you provide.

If you would like to test other argument initializations for your fragment you can do that with the `args` function.

```kotlin
mockSingleViewModel(MyFragment::MyViewModel, defaultState, defaultArgs) {

    args("null id") {
        setNull { ::id }
    }
}
```

This operates very similarly to mocks declared with the state function. The default arguments are the receiver to the lambda and you must return a new instance of your arguments.
This example assumes that the arguments are a data class that uses the Mavericks.KEY_ARG pattern for passing arguments to a view in a bundle.

If your fragment accesses arguments directly (instead of just using them to initialize it's MavericksState) - then you may want to test interactions between specific arguments and state. You can do that by passing arguments to a state mock function.

```kotlin
mockSingleViewModel(MyFragment::MyViewModel, defaultState, defaultArgs) {

    state(
       name = "null user name and args missing id",
       args = { setNull { ::id } }
    ) {
        setNull { ::listing { ::user { ::name } } }
    }
}

If args are not provided to a state variation then the default args are used.
```

### Combining Mocks

If your View has many mocks, or there are different default States or Arguments that your mocks are tested with, you can split your mocks into groups using the `combineMocks` function.

```kotlin
override fun provideMocks() = combineMocks(
    "Standard Mocks" to standardMocks(),
    "Other Mocks" to otherMocks()
)
```

This allows us to separate the implementations into separate mock files, which makes defining many mocks simpler and cleaner. This can also be helpful if your mocks are very large, or have large variations that can't be easily captured via the normal mock variation DSL.




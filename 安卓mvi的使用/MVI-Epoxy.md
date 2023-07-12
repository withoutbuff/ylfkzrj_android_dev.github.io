# Epoxy
环氧树脂

[Epoxy](https://github.com/airbnb/epoxy)是Airbnb的一个开源库，它使构建复杂的布局变得更加容易。它建立在RecyclerView之上，并允许你以声明式的、反应式的方式处理多种视图类型。

### Epoxy的理念
Epoxy使用不可变的 "模型 "来描述数据和视图之间的接口。一个 "EpoxyModel "类存在于你的布局中的每一个独特的视图，并定义了视图如何被创建、样式化和与数据绑定。Epoxy提供了一个强大的注释处理器，可以根据自定义视图或数据绑定布局为你生成这些模型。

为了创建一个页面的布局，EpoxyModels是按照视图出现的顺序来声明的，并且要将数据绑定到它们。这被称为 "构建模型"。由于这些模型是不可改变的，所以每当数据发生变化时，它们必须被重建，以提供描述视图的新快照。

这看起来像这样（从Mavericks的样本应用程序中采用）：

```kotlin
fun invalidate() {
    recyclerView.requestModelBuild()
}

fun buildModels() = withState(viewModel) { state ->
  header {
    id("header")
    title("Hello World")
  }

  state.items.forEach { item ->
    itemCard {
        id(item.id)
        title(item.title)
        clickListener { _ ->
            navigateToItemDetails(item.id)
        }
    }
  }
}
```

###与Mavericks结合
Epoxy的模型构建模式执行的是单向数据流。当状态发生变化时，模型会被异步重建，Epoxy会运行一个后台diff来找出变化的内容。然后在主线程上将变化分派给RecyclerView。这与Mavericks的反应式、基于状态的方法完美契合。

我们在Airbnb使用的一种模式，也是我们在示例应用中分享的，就是每个片段都包含一个EpoxyController，定义了模型的构建方式。Mavericks会自动管理这一点，并在状态改变和视图失效时重建模型。这可以建立在基础片段中，因此单个功能片段在声明一个新页面时的开销很小。

一个简略的版本是这样的：（完整的代码可以在示例应用程序中找到）
```kotlin
abstract class BaseEpoxyFragment : Fragment(R.layout.base_epoxy_fragment) {

    abstract val epoxyController: EpoxyController

    override fun onViewCreated() {
        recyclerView.setController(epoxyController)
    }

    override fun invalidate() {
        recyclerView.requestModelBuild()
    }
}
```

然后，我们可以很容易地创建一个新的页面来扩展我们的基础片段：
```kotlin
class HelloWorldEpoxyFragment : BaseFragment() {
    private val viewModel: HelloWorldViewModel by fragmentViewModel()

    override fun epoxyController() = simpleController(viewModel) { state ->
        marquee {
            id("marquee")
            title(state.title)
        }
    }
}
```

### Epoxy的好处
虽然Epoxy为我们提供了一个漂亮的语法来声明UI，但它在引擎盖下还有很多其他的东西，可以提高它在Mavericks中的实用性。

- Epoxy支持在后台线程上建立模型和扩散。Mavericks使用的基础片段自动提供了这个功能，所以所有的Epoxy渲染都是在主线程之外进行的。你可以触发大量的状态变化而不必担心性能问题。
- Epoxy只更新视图中发生变化的部分。如果一个状态变化导致了一个更新的标题，只有你的视图的标题文本被反弹。只有你的图片改变了？很好，其他的都不需要更新。
- 示例代码中用于声明视图的函数，如`itemCard`，是由Epoxy生成的Kotlin扩展函数。Epoxy为你生成了几乎所有的模板，所以你可以简单地创建一个带有数据绑定的xml布局，并立即在Mavericks片段中使用它。
- Epoxy与[Paris](https://github.com/airbnb/paris)集成，允许通过模型声明对你的视图进行完全的风格控制。


# Epoxy

[Epoxy](https://github.com/airbnb/epoxy) is an open source library by Airbnb that makes it easier to build complex  layouts. It builds on top of RecyclerView, and allows you to work with multiple view types in a declarative, reactive way.

### Epoxy's Philosophy
Epoxy uses immutable "models" to describe the interface between data and view. An `EpoxyModel` class exists for each unique view in your layout, and defines how the view is created, styled, and bound to data. Epoxy provides a powerful annotation processor to generate these models for you based on custom views or databinding layouts.

To create the layout of a page, EpoxyModels are declared in the order that views should appear, with the data that should be bound to them. This is called "building models". Since the models are immutable they must be rebuilt whenever the data changes to provide a new snapshot that describes the view.

This looks like this (adopted from the Mavericks sample app):

```kotlin
fun invalidate() {
    recyclerView.requestModelBuild()
}

fun buildModels() = withState(viewModel) { state ->
  header {
    id("header")
    title("Hello World")
  }

  state.items.forEach { item ->
    itemCard {
        id(item.id)
        title(item.title)
        clickListener { _ ->
            navigateToItemDetails(item.id)
        }
    }
  }
}
```

### Combining with Mavericks
The model building pattern of Epoxy enforces a one way data flow. When a state change occurs, models are rebuilt asynchronously and Epoxy runs a background diff to figure out what changed. Changes are then dispatched on the main thread to the RecyclerView. This fits in perfectly with the reactive, state based approach of Mavericks.

A pattern we use at Airbnb, and that we share in the sample app, is that each fragment contains an EpoxyController that defines how models are built. Mavericks automatically manages this, and rebuilds the models whenever the state changes and the view is invalidated. This can be built into a base fragment, so that individual feature fragments have very little overhead in declaring a new page.

An abridged version looks like this: (The complete code can be found in the sample app)
```kotlin
abstract class BaseEpoxyFragment : Fragment(R.layout.base_epoxy_fragment) {

    abstract val epoxyController: EpoxyController

    override fun onViewCreated() {
        recyclerView.setController(epoxyController)
    }

    override fun invalidate() {
        recyclerView.requestModelBuild()
    }
}
```

We can then create a new page very easily be extending our base fragment:
```kotlin
class HelloWorldEpoxyFragment : BaseFragment() {
    private val viewModel: HelloWorldViewModel by fragmentViewModel()

    override fun epoxyController() = simpleController(viewModel) { state ->
        marquee {
            id("marquee")
            title(state.title)
        }
    }
}
```

### Benefits of Epoxy
While Epoxy gives us a pretty syntax for declaring UI, it has a lot of other things going on under the hood that improve its usefulness with Mavericks.

- Epoxy supports building models and diffing on a background thread. The base fragment Mavericks uses provides this automatically so all of the Epoxy rendering happens off the main thread. You can trigger a large amount of state changes and not have to worry about performance.
- Epoxy only updates the parts of a view that changed. If a state change resulted in an updated header title, only the header text of your view is rebound. Only your picture changed? Great, nothing else needs to be updated.
- The functions in the example code for declaring a view, such as `itemCard`, are Kotlin extension functions generated by Epoxy. Epoxy generates almost all the boilerplate for you, so you can simply create an xml layout with databinding and use it immediately in a Mavericks fragment.
- Epoxy integrates with [Paris](https://github.com/airbnb/paris) to allow full styling control of your views through the model declaration.



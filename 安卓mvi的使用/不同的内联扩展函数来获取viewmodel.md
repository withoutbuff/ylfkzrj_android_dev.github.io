#####MavericksViewModel是一种用于MVI开发的类，它可以管理和更新MavericksState，以及提供一个状态流供其他类订阅。

| 函数 | 异同 | 使用场景 | 优点 | 缺点 |
| --- | --- | --- | --- | --- |
| `fragmentViewModel `| 用于在Fragment中**获取或创建一个新的**MavericksViewModel实例，这个实例的生命周期和**Fragment相同**。 | 每个Fragment都需要独立的ViewModel的情况。 | 可以避免ViewModel的数据混乱或泄露。 | 不能和其他Fragment共享数据和状态。 |
| `parentFragmentViewModel `| 用于在Fragment中**获取或创建一个和父Fragment**共享的MavericksViewModel实例，这个实例的生命周期和**父Fragment相同**。 | 子Fragment和父Fragment需要共享数据和状态的情况。 | 可以简化数据传递和更新。 | 需要遍历所有的父Fragment来寻找或创建ViewModel实例，可能会影响性能。 |
|` targetFragmentViewModel `| 用于在Fragment中**获取或创建一个和目标Fragment**（通过`setTargetFragment`设置）共享的MavericksViewModel实例，这个实例的生命周期和**目标Fragment相同**。 | 两个不相关的Fragment需要共享数据和状态的情况。 | 可以灵活地指定目标Fragment。 | 需要处理目标Fragment不存在或没有对应ViewModel的异常情况。 |
| `existingViewModel` | 用于在Fragment中**获取一个已经存在的**MavericksViewModel实例，这个实例的生命周期和**Activity相同**。 | 在流程中间的页面，不能作为流程的入口点。 | 可以保证获取到正确的ViewModel实例。 | 需要处理没有找到对应ViewModel的异常情况。 |
| `viewModel `| 用于在Activity中**获取或创建一个新的**MavericksViewModel实例，这个实例的生命周期和**Activity相同**。 | 任何Activity中使用ViewModel的情况。 | 可以方便地初始化和获取ViewModel实例。 | 可能会造成ViewModel的数据过大或不一致。 |

##`fragmentViewModel`函数
有三个泛型类型参数：

- `T: Fragment`的子类，也必须实现`MavericksView`接口。
- `VM: MavericksViewModel`的子类，用于指定要获取的ViewModel的类型。
- `S: MavericksState`的子类，用于指定ViewModel的状态类型。

`fragmentViewModel`函数有两个可选参数：

- `viewModelClass`: 用于指定ViewModel的类对象，默认为VM::class。
- `keyFactory`: 用于生成ViewModel的唯一标识符，默认为viewModelClass.java.name。

`fragmentViewModel`函数的返回值是一个MavericksDelegateProvider，它是一个接口，用于提供一个lazy delegate来获取或创建ViewModel。`fragmentViewModel`函数内部调用了viewModelDelegateProvider函数，传入了viewModelClass，keyFactory，existingViewModel = false（表示不使用已存在的ViewModel），以及一个lambda表达式，用于创建一个新的ViewModel实例。这个lambda表达式使用了MavericksViewModelProvider.get函数，传入了viewModelClass，stateClass，viewModelContext（包含了activity和args），key，以及initialStateFactory（用于初始化状态）。

Fragment可以通过一个lazy delegate来获取或创建一个新的MavericksViewModel实例，并且可以指定ViewModel和State的类型以及keyFactory。
```kotlin
inline fun <T, reified VM : MavericksViewModel<S>, reified S : MavericksState> T.fragmentViewModel(
    viewModelClass: KClass<VM> = VM::class,
    crossinline keyFactory: () -> String = { viewModelClass.java.name }
): MavericksDelegateProvider<T, VM> where T : Fragment, T : MavericksView =
    viewModelDelegateProvider(
        viewModelClass,
        keyFactory,
        existingViewModel = false
    ) { stateFactory ->
        MavericksViewModelProvider.get(
            viewModelClass = viewModelClass.java,
            stateClass = S::class.java,
            viewModelContext = FragmentViewModelContext(
                activity = requireActivity(),
                args = _fragmentArgsProvider(),
                fragment = this
            ),
            key = keyFactory(),
            initialStateFactory = stateFactory
        )
    }
```

```kotlin
inline fun <T, reified VM : MavericksViewModel<S>, reified S : MavericksState> T.parentFragmentViewModel(
    viewModelClass: KClass<VM> = VM::class,
    crossinline keyFactory: () -> String = { viewModelClass.java.name }
): MavericksDelegateProvider<T, VM> where T : Fragment, T : MavericksView =
    viewModelDelegateProvider(
        viewModelClass,
        keyFactory,
        existingViewModel = true 
        // 'existingViewModel'设置为true。虽然这个函数在两种情况下都能工作，
       //即已存在或新建的viewmodel，但是支持两种情况会比较困难，所以我们只测试常见的情况，
        //即“已存在”。我们不能确定这个fragment是否被设计为在非已存在的情况下使用（比如它可能需要参数）
        
    ) { stateFactory ->
        if (parentFragment == null) {
            // 使用ViewModelDoesNotExistException，这样模拟框架可以拦截并模拟这种情况下的viewmodel。
            throw ViewModelDoesNotExistException(
                "There is no parent fragment for ${this::class.java.name} so view model ${viewModelClass.java.name} could not be found." // ${this::class.java.name}没有父fragment，所以找不到view model ${viewModelClass.java.name}。
            )
        }
        var parent: Fragment? = parentFragment
        val key = keyFactory()
        while (parent != null) {
            try {
                return@viewModelDelegateProvider MavericksViewModelProvider.get(
                    viewModelClass = viewModelClass.java,
                    stateClass = S::class.java,
                    viewModelContext = FragmentViewModelContext(
                        activity = this.requireActivity(),
                        args = _fragmentArgsProvider(),
                        fragment = parent
                    ),
                    key = key,
                    forExistingViewModel = true 
                    // 对于已存在的viewmodel，设置为true。
                )
            } catch (e: ViewModelDoesNotExistException) {
                parent = parent.parentFragment 
                // 如果在当前父fragment中找不到viewmodel，就继续向上遍历父fragment。
            }
        }

        // 没有找到viewmodel。在最顶层的父fragment中创建一个新的。
        var topParentFragment = parentFragment
        while (topParentFragment?.parentFragment != null) {
            topParentFragment = topParentFragment.parentFragment
        }
        val viewModelContext = FragmentViewModelContext(
            requireActivity(),
            _fragmentArgsProvider(),
            topParentFragment!!
        )

        MavericksViewModelProvider.get(
            viewModelClass = viewModelClass.java,
            stateClass = S::class.java,
            viewModelContext = viewModelContext,
            key = keyFactory(),
            initialStateFactory = stateFactory
        )
    }
```
```kotlin
inline fun <T, reified VM : MavericksViewModel<S>, reified S : MavericksState> T.targetFragmentViewModel(
    viewModelClass: KClass<VM> = VM::class,
    crossinline keyFactory: () -> String = { viewModelClass.java.name }
): MavericksDelegateProvider<T, VM> where T : Fragment, T : MavericksView =
    viewModelDelegateProvider(
        viewModelClass,
        keyFactory,
        existingViewModel = true
    ) { stateFactory ->

        @Suppress("DEPRECATION")
        val targetFragment =
            requireNotNull(targetFragment) { "There is no target fragment for ${this::class.java.name}!" }

        MavericksViewModelProvider.get(
            viewModelClass = viewModelClass.java,
            stateClass = S::class.java,
            viewModelContext = FragmentViewModelContext(
                activity = requireActivity(),
                args = targetFragment._fragmentArgsProvider(),
                fragment = targetFragment
            ),
            key = keyFactory(),
            initialStateFactory = stateFactory
        )
    }
```
```kotlin
inline fun <T, reified VM : MavericksViewModel<S>, reified S : MavericksState> T.existingViewModel(
    viewModelClass: KClass<VM> = VM::class,
    crossinline keyFactory: () -> String = { viewModelClass.java.name }
): MavericksDelegateProvider<T, VM> where T : Fragment, T : MavericksView =
    viewModelDelegateProvider(
        viewModelClass,
        keyFactory,
        existingViewModel = true
    ) { stateFactory ->

        MavericksViewModelProvider.get(
            viewModelClass = viewModelClass.java,
            stateClass = S::class.java,
            viewModelContext = ActivityViewModelContext(
                requireActivity(),
                _fragmentArgsProvider()
            ),
            key = keyFactory(),
            initialStateFactory = stateFactory,
            forExistingViewModel = true
        )
    }
```
```kotlin
inline fun <T, reified VM : MavericksViewModel<S>, reified S : MavericksState> T.activityViewModel(
    viewModelClass: KClass<VM> = VM::class,
    noinline keyFactory: () -> String = { viewModelClass.java.name }
): MavericksDelegateProvider<T, VM> where T : Fragment, T : MavericksView =
    viewModelDelegateProvider(
        viewModelClass,
        keyFactory,
        existingViewModel = false
    ) { stateFactory ->

        MavericksViewModelProvider.get(
            viewModelClass = viewModelClass.java,
            stateClass = S::class.java,
            viewModelContext = ActivityViewModelContext(
                activity = requireActivity(),
                args = _fragmentArgsProvider()
            ),
            key = keyFactory(),
            initialStateFactory = stateFactory
        )
    }
```
```kotlin
inline fun <T, reified VM : MavericksViewModel<S>, reified S : MavericksState> T.viewModel(
    viewModelClass: KClass<VM> = VM::class,
    crossinline keyFactory: () -> String = { viewModelClass.java.name }
): Lazy<VM> where T : ComponentActivity = lifecycleAwareLazy(this) {
    MavericksViewModelProvider.get(
        viewModelClass = viewModelClass.java,
        stateClass = S::class.java,
        viewModelContext = ActivityViewModelContext(this, intent.extras?.get(Mavericks.KEY_ARG)),
        key = keyFactory()
    )
}
```


##每个函数的代码示例，假设有一个OrderViewModel和一个OrderState用于管理订单的数据和状态。

#####- fragmentViewModel: 在Fragment中使用这个函数，可以这样写：

```kotlin
class OrderFragment: Fragment(), MavericksView {

  // 获取或创建一个新的OrderViewModel实例
  private val viewModel: OrderViewModel by fragmentViewModel()

  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
    // Inflate the layout for this fragment
    return inflater.inflate(R.layout.fragment_order, container, false)
  }

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // 绑定视图和数据
    binding.viewModel = viewModel
    binding.lifecycleOwner = viewLifecycleOwner

    // 设置点击事件
    binding.buttonConfirm.setOnClickListener {
      viewModel.confirmOrder()
    }
  }

  override fun invalidate() {
    // 更新UI
    withState(viewModel) { state ->
      binding.textViewOrderStatus.text = state.orderStatus
      binding.textViewOrderPrice.text = state.orderPrice.toString()
    }
  }
}
```

#####- parentFragmentViewModel: 在子Fragment中使用这个函数，可以这样写：

```kotlin
class OrderDetailFragment: Fragment(), MavericksView {

  // 获取或创建一个和父Fragment共享的OrderViewModel实例
  private val viewModel: OrderViewModel by parentFragmentViewModel()

  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
    // Inflate the layout for this fragment
    return inflater.inflate(R.layout.fragment_order_detail, container, false)
  }

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // 绑定视图和数据
    binding.viewModel = viewModel
    binding.lifecycleOwner = viewLifecycleOwner

    // 设置点击事件
    binding.buttonEdit.setOnClickListener {
      viewModel.editOrder()
    }
  }

  override fun invalidate() {
    // 更新UI
    withState(viewModel) { state ->
      binding.textViewOrderDetail.text = state.orderDetail
      binding.textViewOrderNote.text = state.orderNote
    }
  }
}
```

#####- targetFragmentViewModel: 在Fragment中使用这个函数，可以这样写：

```kotlin
class OrderSummaryFragment: Fragment(), MavericksView {

  // 获取或创建一个和目标Fragment（通过setTargetFragment设置）共享的OrderViewModel实例
  private val viewModel: OrderViewModel by targetFragmentViewModel()

  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
    // Inflate the layout for this fragment
    return inflater.inflate(R.layout.fragment_order_summary, container, false)
  }

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    // 绑定视图和数据
    binding.viewModel = viewModel
    binding.lifecycleOwner = viewLifecycleOwner

    // 设置点击事件
    binding.buttonPay.setOnClickListener {
      viewModel.payOrder()
      findNavController().navigate(R.id.action_orderSummaryFragment_to_paymentFragment)
    }
  }

  override fun invalidate() {
    // 更新UI
    withState(viewModel) { state ->
      binding.textViewOrderSummary.text = state.orderSummary
      binding.textViewTotalPrice.text = state.totalPrice.toString()
      binding.textViewDiscount.text = state.discount.toString()
      binding.textViewTax.text = state.tax.toString()
      binding.textViewFinalPrice.text = state.finalPrice.toString()
    }
  }
}
```

#####- existingViewModel: 在Fragment中使用这个函数，可以这样写：

```kotlin
class PaymentFragment: Fragment(), MavericksView {

  // 获取一个已经存在的OrderViewModel实例，如果没有找到，会抛出异常
  private val viewModel: OrderViewModel by existingViewModel()

  override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
  ): View? {
    // Inflate the layout for this fragment
    return inflater.inflate(R.layout.fragment_payment, container, false)
  }

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

     // 绑定视图和数据
     binding.viewModel = viewModel
     binding.lifecycleOwner = viewLifecycleOwner

     // 设置点击事件
     binding.buttonSubmit.setOnClickListener {
       viewModel.submitPayment()
       findNavController().navigate(R.id.action_paymentFragment_to_thankYouFragment)
     }
   }

   override fun invalidate() {
     // 更新UI
     withState(viewModel) { state ->
       binding.textViewFinalPrice.text = state.finalPrice.toString()
       binding.editTextCardNumber.setText(state.cardNumber)
       binding.editTextCardName.setText(state.cardName)
       binding.editTextCardExpiry.setText(state.cardExpiry)
       binding.editTextCardCvv.setText(state.cardCvv)
     }
   }
}
```

#####- viewModel: 在Activity中使用这个函数，可以这样写：

```kotlin
class OrderActivity : AppCompatActivity() {

   // 获取或创建一个新的OrderViewModel实例
   private val viewModel : OrderViewModel by viewModel()

   override fun onCreate(savedInstanceState : Bundle?) {
       super.onCreate(savedInstanceState)

       setContentView(R.layout.activity_order)

       // 设置标题栏文字为订单号码
       supportActionBar?.title = "Order #${viewModel.state.orderNumber}"
   }
}
```

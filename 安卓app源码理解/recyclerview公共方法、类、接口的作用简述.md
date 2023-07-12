#公共set方法

| 功能分类 | set 方法 | 使用场景 |
| :---: | :---: | :---: |
| RecyclerView 性能 | setHasFixedSize() <br> setRecycledViewPool() <br> setViewCacheExtension() <br> setItemViewCacheSize() | 当 RecyclerView 的大小不会随着内容变化时，可以调用 setHasFixedSize() 来提高性能 <br> 当 RecyclerView 中有多种类型的 ViewHolder 时，可以调用 setRecycledViewPool() 来共享回收池 <br> 当 RecyclerView 中有特殊类型的 ViewHolder 时，可以调用 setViewCacheExtension() 来自定义缓存策略 <br> 当 RecyclerView 中的子项数量较少时，可以调用 setItemViewCacheSize() 来增加缓存大小 |
| RecyclerView 的 UI | setLayoutManager() <br> setItemAnimator() <br> setItemDecoration() <br> setEdgeEffectFactory() | 调用 setLayoutManager() 来设置 RecyclerView 的布局方式，如线性、网格或瀑布流 <br> 调用 setItemAnimator() 来设置 RecyclerView 的子项动画效果，如添加、删除或移动时的动画 <br> 调用 setItemDecoration() 来设置 RecyclerView 的子项装饰效果，如分割线、背景或阴影 <br> 调用 setEdgeEffectFactory() 来设置 RecyclerView 的边缘效果，如滑动到边缘时的波纹或光晕 |
| ItemView  | setHasStableIds() <br> setPreserveFocusAfterLayout() | 调用 setHasStableIds() 来设置 RecyclerView 的 Adapter 是否有稳定的 ID ，即每个子项的 ID 是否在数据集改变时保持不变，这可以提高性能和动画效果 <br> 调用 setPreserveFocusAfterLayout() 来设置 RecyclerView 在布局变化后是否保持焦点，这可以提高用户体验和无障碍功能 |
| 设置监听回调 | setAdapter() <br> setRecyclerListener() <br> setOnFlingListener() <br> setOnScrollListener() <br> setOnChildAttachStateChangeListener() | 调用 setAdapter() 来设置 RecyclerView 的 Adapter ，即数据和视图的绑定器，这是必须的步骤 <br> 调用 setRecyclerListener() 来设置 RecyclerView 的 RecyclerListener ，即监听回收事件的回调，这可以用于释放资源或更新状态 <br> 调用 setOnFlingListener() 来设置 RecyclerView 的 OnFlingListener ，即监听滑动事件的回调，这可以用于处理滑动手势或启动滚动动画 <br> 调用 setOnScrollListener() 来设置 RecyclerView 的 OnScrollListener ，即监听滚动事件的回调，这可以用于实现上拉加载更多或悬浮标题等功能 <br> 调用 setOnChildAttachStateChangeListener() 来设置 RecyclerView 的 OnChildAttachStateChangeListener ，即监听子项附着状态变化的回调，这可以用于实现拖拽排序或侧滑删除等功能 |


- **public void setAdapter(Adapter adapter)**：用于设置 RecyclerView 的 Adapter ，即数据和视图的绑定器。
- **public void setHasFixedSize(boolean hasFixedSize)**：用于设置 RecyclerView 是否有固定的大小，如果有，可以优化性能。
- **public void setItemAnimator(ItemAnimator animator)**：用于设置 RecyclerView 的 ItemAnimator ，即子项的动画效果。
- **public void setItemViewCacheSize(int size)**：用于设置 RecyclerView 的 ItemView 缓存的大小，即可以复用的子项的数量。
- **public void setLayoutManager(LayoutManager layout)**：用于设置 RecyclerView 的 LayoutManager ，即子项的布局方式。
- **public void setOnFlingListener(OnFlingListener onFlingListener)**：用于设置 RecyclerView 的 OnFlingListener ，即监听滑动事件的回调。
- **public void setRecyclerListener(RecyclerListener listener)**：用于设置 RecyclerView 的 RecyclerListener ，即监听回收事件的回调。
- **public void setEdgeEffectFactory(EdgeEffectFactory edgeEffectFactory)**：用于设置 RecyclerView 的 EdgeEffectFactory ，即滚动到边缘时的效果。
- **public void setAccessibilityDelegateCompat(RecyclerViewAccessibilityDelegate accessibilityDelegate)**：用于设置 RecyclerView 的 AccessibilityDelegate ，即辅助功能的代理类。
- **public void setChildDrawingOrderCallback(ChildDrawingOrderCallback childDrawingOrderCallback)**：用于设置 RecyclerView 的 ChildDrawingOrderCallback ，即子项绘制顺序的回调。
- **public void setClipToPadding(boolean clipToPadding)**：用于设置 RecyclerView 是否剪切掉与其内边距重叠的子项。
- **public void setNestedScrollingEnabled(boolean enabled)**：用于设置 RecyclerView 是否支持嵌套滚动。
- **public void setOnScrollListener(OnScrollListener listener)**：用于设置 RecyclerView 的 OnScrollListener ，即滚动事件的监听器。
- **public void setRecycledViewPool(RecycledViewPool pool)**：用于设置 RecyclerView 的 RecycledViewPool ，即回收视图的缓存池。
- **public void setScrollingTouchSlop(int slopConstant)**：用于设置 RecyclerView 的滚动触摸阈值，即触发滚动所需的最小移动距离。
- **public void setViewCacheExtension(ViewCacheExtension extension)**：用于设置 RecyclerView 的 ViewCacheExtension ，即自定义的视图缓存扩展类¹²³。
- **public void setHasStableIds(boolean hasStableIds)**：用于设置 RecyclerView 的 Adapter 是否有稳定的 ID ，即每个子项的 ID 是否在数据集改变时保持不变。
- **public void setPreserveFocusAfterLayout(boolean preserveFocusAfterLayout)**：用于设置 RecyclerView 在布局变化后是否保持焦点。
- **public void setScrollStateChangeListener(ScrollStateChangeListener listener)**：用于设置 RecyclerView 的 ScrollStateChangeListener ，即滚动状态变化的监听器。


#公共get方法
| 功能分类 | get 方法 | 使用场景 |
| :---: | :---: | :---: |
| RecyclerView 性能 | getViewCacheExtension() <br> getItemViewType() <br> getAdapterPosition() <br> getLayoutPosition() <br> getItemId() <br> getItemAnimator() <br> getItemDecorationCount() <br> getItemViewCacheSize() | 调用 getViewCacheExtension() 来获取 RecyclerView 的 ViewCacheExtension ，即自定义的视图缓存扩展类 <br> 调用 getItemViewType() 来获取 RecyclerView 中指定位置的子项的类型，该类型是在 Adapter 中定义的 <br> 调用 getAdapterPosition() 来获取 RecyclerView 中当前 ViewHolder 的位置，该位置是在 Adapter 中定义的 <br> 调用 getLayoutPosition() 来获取 RecyclerView 中当前 ViewHolder 的位置，该位置是在布局中定义的 <br> 调用 getItemId() 来获取 RecyclerView 中当前 ViewHolder 的 ID ，该 ID 是在 Adapter 中定义的 <br> 调用 getItemAnimator() 来获取 RecyclerView 的 ItemAnimator ，即子项的动画效果 <br> 调用 getItemDecorationCount() 来获取 RecyclerView 的 ItemDecoration 的数量，即子项的装饰效果 <br> 调用 getItemViewCacheSize() 来获取 RecyclerView 的 ItemView 缓存的大小，即可以复用的子项的数量 |
| RecyclerView 的 UI | getFocusedChild() <br> getMaxFlingVelocity() <br> getMinFlingVelocity() <br> getOnFlingListener() <br> getPreserveFocusAfterLayout() <br> getRecyclerListener() | 调用 getFocusedChild() 来获取 RecyclerView 中当前获得焦点的子视图 <br> 调用 getMaxFlingVelocity() 来获取 RecyclerView 的最大滑动速度 <br> 调用 getMinFlingVelocity() 来获取 RecyclerView 的最小滑动速度 <br> 调用 getOnFlingListener() 来获取 RecyclerView 的 OnFlingListener ，即监听滑动事件的回调 <br> 调用 getPreserveFocusAfterLayout() 来获取 RecyclerView 在布局变化后是否保持焦点 <br> 调用 getRecyclerListener() 来获取 RecyclerView 的 RecyclerListener ，即监听回收事件的回调 |
| ItemView 的 UI | getHasStableIds() <br> getLayoutDirection() <br> getLayoutFrozen() <br> getLayoutManager() <br> getNestedScrollAxes() <br> getScrollState() | 调用 getHasStableIds() 来获取 RecyclerView 的 Adapter 是否有稳定的 ID ，即每个子项的 ID 是否在数据集改变时保持不变 <br> 调用 getLayoutDirection() 来获取 RecyclerView 的布局方向，即水平或垂直 <br> 调用 getLayoutFrozen() 来获取 RecyclerView 的布局是否被冻结，即是否可以响应触摸事件或滚动事件 <br> 调用 getLayoutManager() 来获取 RecyclerView 的 LayoutManager ，即子项的布局方式 <br> 调用 getNestedScrollAxes() 来获取 RecyclerView 的嵌套滚动的方向，即水平或垂直 <br> 调用 getScrollState() 来获取 RecyclerView 的滚动状态，即空闲、拖拽或滑动 |
| 设置监听回调 | getChildAdapterPosition() <br> getChildItemId() <br> getChildLayoutPosition() <br> getChildViewHolder() <br> findViewHolderForAdapterPosition() <br> findViewHolderForItemId() <br> findViewHolderForLayoutPosition() | 调用 getChildAdapterPosition() 来获取 RecyclerView 中指定子视图的位置，该位置是在 Adapter 中定义的 <br> 调用 getChildItemId() 来获取 RecyclerView 中指定子视图的 ID ，该 ID 是在 Adapter 中定义的 <br> 调用 getChildLayoutPosition() 来获取 RecyclerView 中指定子视图的位置，该位置是在布局中定义的 <br> 调用 getChildViewHolder() 来获取 RecyclerView 中指定子视图的 ViewHolder ，即封装了子视图和数据的对象 <br> 调用 findViewHolderForAdapterPosition() 来获取 RecyclerView 中指定位置的 ViewHolder ，该位置是在 Adapter 中定义的 <br> 调用 findViewHolderForItemId() 来获取 RecyclerView 中指定 ID 的 ViewHolder ，该 ID 是在 Adapter 中定义的 <br> 调用 findViewHolderForLayoutPosition() 来获取 RecyclerView 中指定位置的 ViewHolder ，该位置是在布局中定义的 |

- **public Adapter getAdapter()**：用于获取 RecyclerView 的 Adapter ，即数据和视图的绑定器 。
- **public int getChildAdapterPosition(View child)**：用于获取 RecyclerView 中指定子视图的位置，该位置是在 Adapter 中定义的 。
- **public int getChildLayoutPosition(View child)**：用于获取 RecyclerView 中指定子视图的位置，该位置是在布局中定义的。
- **public long getChildItemId(View child)**：用于获取 RecyclerView 中指定子视图的 ID ，该 ID 是在 Adapter 中定义的。
- **public ViewHolder getChildViewHolder(View child)**：用于获取 RecyclerView 中指定子视图的 ViewHolder ，即视图的封装容器。
- **public LayoutManager getLayoutManager()**：用于获取 RecyclerView 的 LayoutManager ，即子项的布局方式。
- **public RecycledViewPool getRecycledViewPool()**：用于获取 RecyclerView 的 RecycledViewPool ，即回收视图的缓存池。
- **public int getScrollState()**：用于获取 RecyclerView 的滚动状态，即是否正在滚动或惯性滚动。
- **public ViewCacheExtension getViewCacheExtension()**：用于获取 RecyclerView 的 ViewCacheExtension ，即自定义的视图缓存扩展类。
- **public int getItemViewType(int position)**：用于获取 RecyclerView 中指定位置的子项的类型，该类型是在 Adapter 中定义的。
- **public int getAdapterPosition()**：用于获取 RecyclerView 中当前 ViewHolder 的位置，该位置是在 Adapter 中定义的。
- **public int getLayoutPosition()**：用于获取 RecyclerView 中当前 ViewHolder 的位置，该位置是在布局中定义的。
- **public long getItemId()**：用于获取 RecyclerView 中当前 ViewHolder 的 ID ，该 ID 是在 Adapter 中定义的。
- **public ItemAnimator getItemAnimator()**：用于获取 RecyclerView 的 ItemAnimator ，即子项的动画效果。
- **public int getItemDecorationCount()**：用于获取 RecyclerView 的 ItemDecoration 的数量，即子项的装饰效果。
- **public int getItemViewCacheSize()**：用于获取 RecyclerView 的 ItemView 缓存的大小，即可以复用的子项的数量。
- **public View getFocusedChild()**：用于获取 RecyclerView 中当前获得焦点的子视图。
- **public int getMaxFlingVelocity()**：用于获取 RecyclerView 的最大滑动速度。
- **public int getMinFlingVelocity()**：用于获取 RecyclerView 的最小滑动速度。
- **public OnFlingListener getOnFlingListener()**：用于获取 RecyclerView 的 OnFlingListener ，即监听滑动事件的回调。
- **public boolean getPreserveFocusAfterLayout()**：用于获取 RecyclerView 在布局变化后是否保持焦点。
- **public RecyclerListener getRecyclerListener()**：用于获取 RecyclerView 的 RecyclerListener ，即监听回收事件的回调。


#公共内部类

| 功能分类 | 公共内部类 | 使用场景 |
| :---: | :---: | :---: |
| RecyclerView 性能 | Adapter<VH extends ViewHolder> <br> RecyclerView.AdapterDataObserver <br> RecycledViewPool <br> Recycler | 用于定义 RecyclerView 的 Adapter ，即数据和视图的绑定器，这是必须的步骤 <br> 用于定义 RecyclerView 的 AdapterDataObserver ，即监听数据变化的观察者，这可以用于更新 UI 或状态 <br> 用于定义 RecyclerView 的 RecycledViewPool ，即回收视图的缓存池，这可以用于提高性能和复用率 <br> 用于定义 RecyclerView 的 Recycler ，即回收和复用视图的类，这可以用于释放资源或更新状态 |
| RecyclerView 的 UI |EdgeEffectFactory <br> LayoutManager <br> SmoothScroller | 用于定义 RecyclerView 的 EdgeEffectFactory ，即边缘效果的工厂类，这可以用于自定义滑动到边缘时的效果 <br> 用于定义 RecyclerView 的 LayoutManager ，即子项的布局方式，这可以用于自定义线性、网格或瀑布流等布局 <br> 用于定义 RecyclerView 的 SmoothScroller ，即平滑滚动的类，这可以用于自定义滚动速度或目标位置 |
| ItemView 的 UI | ItemAnimator <br> ItemDecoration | 用于定义 RecyclerView 的 ItemAnimator ，即子项的动画效果，这可以用于自定义添加、删除或移动时的动画 <br> 用于定义 RecyclerView 的 ItemDecoration ，即子项的装饰效果，这可以用于自定义分割线、背景或阴影等效果 |


- **ItemDecoration**：用于为 RecyclerView 的每个子项添加装饰，例如分隔线或背景色。
- **LayoutManager**：用于管理 RecyclerView 的子项的布局方式，例如线性、网格或瀑布流。
- **ViewHolder**：用于封装 RecyclerView 的子项的视图，并提供绑定数据的方法。
- **Adapter**：用于创建和绑定 RecyclerView 的子项的视图，并提供数据集的大小。
- **OnItemTouchListener**：用于监听 RecyclerView 的触摸事件，例如点击、滑动或拖动。
- **RecyclerListener**：用于监听 RecyclerView 的回收事件，例如当一个子项被回收时。
- **SmoothScroller**：用于实现 RecyclerView 的平滑滚动效果，可以自定义滚动速度、目标位置等。
- **SnapHelper**：用于实现 RecyclerView 的对齐效果，可以让子项在滚动停止时自动对齐到某个位置。
- **public static class RecyclerView.Adapter<VH extends RecyclerView.ViewHolder>**：用于定义 RecyclerView 的 Adapter ，即数据和视图的绑定器。
- **public static abstract class RecyclerView.AdapterDataObserver**：用于定义 RecyclerView 的 AdapterDataObserver ，即监听数据变化的观察者。
- **public static class RecyclerView.EdgeEffectFactory**：用于定义 RecyclerView 的 EdgeEffectFactory ，即边缘效果的工厂类。
- **public static class RecyclerView.ItemAnimator**：用于定义 RecyclerView 的 ItemAnimator ，即子项的动画效果。
- **public static abstract class RecyclerView.ItemDecoration**：用于定义 RecyclerView 的 ItemDecoration ，即子项的装饰效果。
- **public static abstract class RecyclerView.LayoutManager**：用于定义 RecyclerView 的 LayoutManager ，即子项的布局方式。
- **public static class RecyclerView.Adapter<VH extends RecyclerView.ViewHolder>**：用于定义 RecyclerView 的 Adapter ，即数据和视图的绑定器。
- **public static abstract class RecyclerView.AdapterDataObserver**：用于定义 RecyclerView 的 AdapterDataObserver ，即监听数据变化的观察者。


#公共内部接口
| 功能分类 | 公共内部接口 | 使用场景 |
| :---: | :---: | :---: |
| RecyclerView 的 UI | ChildDrawingOrderCallback <br>OnFlingListener <br> OnScrollListener | 用于定义 RecyclerView 的 ChildDrawingOrderCallback ，即子项绘制顺序的回调，这可以用于自定义子项的层次关系或遮盖效果 <br> 用于定义 RecyclerView 的 OnFlingListener ，即监听滑动事件的回调，这可以用于处理滑动手势或启动滚动动画 <br> 用于定义 RecyclerView 的 OnScrollListener ，即监听滚动事件的回调，这可以用于实现上拉加载更多或悬浮标题等功能 |
| ItemView 的 UI | OnChildAttachStateChangeListener <br> OnItemTouchListener | 用于定义 RecyclerView 的 OnChildAttachStateChangeListener ，即监听子项附着状态变化的回调，这可以用于实现拖拽排序或侧滑删除等功能 <br> 用于定义 RecyclerView 的 OnItemTouchListener ，即监听子项触摸事件的回调，这可以用于处理点击、长按或拖拽等事件 |
- **public static interface RecyclerView.ChildDrawingOrderCallback**：用于定义 RecyclerView 的 ChildDrawingOrderCallback ，即子项绘制顺序的回调。
- **public static interface RecyclerView.OnChildAttachStateChangeListener**：用于定义 RecyclerView 的 OnChildAttachStateChangeListener ，即监听子项附着状态变化的回调。
- **public static interface RecyclerView.ChildDrawingOrderCallback**：用于定义 RecyclerView 的 ChildDrawingOrderCallback ，即子项绘制顺序的回调。
- **public static class RecyclerView.EdgeEffectFactory**：用于定义 RecyclerView 的 EdgeEffectFactory ，即边缘效果的工厂类。
- **public static class RecyclerView.ItemAnimator**：用于定义 RecyclerView 的 ItemAnimator ，即子项的动画效果。
- **public static abstract class RecyclerView.ItemDecoration**：用于定义 RecyclerView 的 ItemDecoration ，即子项的装饰效果。
- **public static abstract class RecyclerView.LayoutManager**：用于定义 RecyclerView 的 LayoutManager ，即子项的布局方式。
- **public static interface RecyclerView.OnChildAttachStateChangeListener**：用于定义 RecyclerView 的 OnChildAttachStateChangeListener ，即监听子项附着状态变化的回调。


#继承接口
- **public interface NestedScrollingChild**：用于定义 RecyclerView 作为嵌套滚动的子视图的接口，该接口包含了一些控制嵌套滚动的方法，如 startNestedScroll() ， stopNestedScroll() ， dispatchNestedScroll() 等。
- **public interface NestedScrollingChild2**：用于定义 RecyclerView 作为嵌套滚动的子视图的接口，该接口是 NestedScrollingChild 的扩展，增加了对指针输入设备（如触摸屏）的支持。
- **public interface NestedScrollingChild3**：用于定义 RecyclerView 作为嵌套滚动的子视图的接口，该接口是 NestedScrollingChild2 的扩展，增加了对嵌套滚动消耗和偏移量的支持。
- **public interface ScrollingView**：用于定义 RecyclerView 作为可滚动视图的接口，该接口包含了一个获取水平和垂直滚动范围的方法 computeHorizontalScrollRange() 和 computeVerticalScrollRange()。
- **public interface NestedScrollingParent**：用于定义 RecyclerView 作为嵌套滚动的父视图的接口，该接口包含了一些协调嵌套滚动的方法，如 onNestedScrollAccepted() ， onNestedPreScroll() ， onNestedScroll() 等。
- **public interface NestedScrollingParent2**：用于定义 RecyclerView 作为嵌套滚动的父视图的接口，该接口是 NestedScrollingParent 的扩展，增加了对指针输入设备（如触摸屏）的支持。
- **public interface NestedScrollingParent3**：用于定义 RecyclerView 作为嵌套滚动的父视图的接口，该接口是 NestedScrollingParent2 的扩展，增加了对嵌套滚动消耗和偏移量的支持。


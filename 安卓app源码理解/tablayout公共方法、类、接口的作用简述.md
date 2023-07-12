###一、简单的示例代码

#####1.动态实现
######核心代码
```
  TabLayout tabLayout = ...;
  tabLayout.addTab(tabLayout.newTab().setText("Tab 1"));
  tabLayout.addTab(tabLayout.newTab().setText("Tab 2"));
  tabLayout.addTab(tabLayout.newTab().setText("Tab 3"));
```
######简例代码
```
// 动态实现方式，通过代码创建TabLayout和Tab，并添加到布局中
val tabLayout = TabLayout(this) // 创建一个TabLayout对象
tabLayout.layoutParams = LinearLayout.LayoutParams(
    LinearLayout.LayoutParams.MATCH_PARENT,
    LinearLayout.LayoutParams.WRAP_CONTENT
) // 设置布局参数
tabLayout.tabGravity = TabLayout.GRAVITY_FILL // 设置标签的重力为填充
tabLayout.tabMode = TabLayout.MODE_FIXED // 设置标签的模式为固定

val tabIcons = arrayOf( // 创建一个图标数组
    R.drawable.ic_home,
    R.drawable.ic_search,
    R.drawable.ic_profile
)

val tabTexts = arrayOf( // 创建一个文本数组
    getString(R.string.home),
    getString(R.string.search),
    getString(R.string.profile)
)

for (i in 0..2) { // 循环创建三个标签，并添加到TabLayout中
    val tab = tabLayout.newTab() // 创建一个新的标签对象
    tab.setIcon(tabIcons[i]) // 设置标签的图标
    tab.setText(tabTexts[i]) // 设置标签的文本
    tabLayout.addTab(tab) // 添加标签到TabLayout中
}

val container = findViewById<LinearLayout>(R.id.container) // 获取一个容器视图
container.addView(tabLayout) // 将TabLayout添加到容器视图中

// 添加一个监听器，当标签的选择状态发生改变时，打印日志信息
tabLayout.addOnTabSelectedListener(object : TabLayout.OnTabSelectedListener {
    override fun onTabSelected(tab: TabLayout.Tab?) {
        Log.d("TabLayout", "onTabSelected: ${tab?.text}")
    }

    override fun onTabUnselected(tab: TabLayout.Tab?) {
        Log.d("TabLayout", "onTabUnselected: ${tab?.text}")
    }

    override fun onTabReselected(tab: TabLayout.Tab?) {
        Log.d("TabLayout", "onTabReselected: ${tab?.text}")
    }
})
```
#####2.静态实现
######核心代码
```
  <com.google.android.material.tabs.TabLayout
          android:layout_height="wrap_content"
          android:layout_width="match_parent">
 
      <com.google.android.material.tabs.TabItem
              android:text="@string/tab_text"/>
 
      <com.google.android.material.tabs.TabItem
              android:icon="@drawable/ic_android"/>
 
  </com.google.android.material.tabs.TabLayout>
```
######简例代码
```
// 静态实现方式，直接在布局文件中添加TabLayout和TabItem
<com.google.android.material.tabs.TabLayout
    android:id="@+id/tab_layout"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:tabGravity="fill"
    app:tabMode="fixed">

    <com.google.android.material.tabs.TabItem
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:icon="@drawable/ic_home"
        android:text="@string/home" />

    <com.google.android.material.tabs.TabItem
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:icon="@drawable/ic_search"
        android:text="@string/search" />

    <com.google.android.material.tabs.TabItem
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:icon="@drawable/ic_profile"
        android:text="@string/profile" />

</com.google.android.material.tabs.TabLayout>
```

#####3.搭配viewpager实现
######核心代码
```
  <androidx.viewpager.widget.ViewPager
      android:layout_width="match_parent"
      android:layout_height="match_parent">
 
      <com.google.android.material.tabs.TabLayout
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top" />
 
  </androidx.viewpager.widget.ViewPager>
```
#####1.公共set方法

| 分类 | 方法名 | 作用简述 | 使用场景 |
| --- | --- | --- | --- |
| 布局 | setInlineLabel(boolean inline) | 设置是否使用内联标签，即标签文本和图标是否在同一行。 | 改变标签的内联样式。 |
| 布局 | setInlineLabelResource(int inlineResourceId) | 设置是否使用内联标签，从资源文件中获取布尔值。 | 改变标签的内联样式。 |
| 布局 | setScrollPosition(int position, float positionOffset, boolean updateSelectedText) | 设置当前滚动位置，并选择是否更新选中的标签文本。 | 滚动到指定位置的标签。 |
| 布局 | setTabGravity(int gravity) | 设置标签的重力，即标签在水平方向上的对齐方式。可能的值有GRAVITY_CENTER和GRAVITY_FILL。 | 改变标签在水平方向上的对齐方式。 |
| 布局 | setTabIndicatorGravity(int indicatorGravity) | 设置指示器的重力，即指示器在垂直方向上的对齐方式。可能的值有INDICATOR_GRAVITY_BOTTOM、INDICATOR_GRAVITY_CENTER、INDICATOR_GRAVITY_STRETCH和INDICATOR_GRAVITY_TOP。 | 改变指示器在垂直方向上的对齐方式。 |
| 布局 | setTabMode(int mode) | 设置标签的模式，即标签是否可以滚动。可能的值有MODE_FIXED和MODE_SCROLLABLE。 | 改变标签是否可以滚动。 |
| 样式 | setSelectedTabIndicator(int resId) | 设置选中的标签指示器的资源ID，如果为0，则不显示指示器。 | 改变选中的标签指示器的样式。 |
| 样式 | setSelectedTabIndicator(Drawable drawable) | 设置选中的标签指示器的Drawable对象，如果为null，则不显示指示器。 | 改变选中的标签指示器的样式。 |
| 样式 | setSelectedTabIndicatorColor(int color) | 设置选中的标签指示器的颜色。 | 改变选中的标签指示器的颜色。 |
| 样式 | setSelectedTabIndicatorGravity(int indicatorGravity) | 设置选中的标签指示器的重力，即指示器在垂直方向上的对齐方式。可能的值有INDICATOR_GRAVITY_BOTTOM、INDICATOR_GRAVITY_CENTER、INDICATOR_GRAVITY_STRETCH和INDICATOR_GRAVITY_TOP。这个方法已经被setTabIndicatorGravity(int indicatorGravity)取代，但是仍然有效果。 | 改变选中的标签指示器在垂直方向上的对齐方式。 |
| 样式 | setTabIconTint(ColorStateList iconTint) | 设置所有标签图标的着色器，如果为null，则不着色图标。 | 改变所有标签图标的颜色或透明度。 |
| 样式 | setTabIconTintResource(int iconTintResourceId) | 设置所有标签图标的着色器，从资源文件中获取颜色状态列表。如果为0，则不着色图标。 | 改变所有标签图标的颜色或透明度。 |
| 样式 | setTabIconTintMode(PorterDuff.Mode mode) | 设置所有标签图标的着色模式，如果为null，则使用SRC_IN模式。这个方法只有在设置了着色器时才有效果。| 改变所有标签图标的着色效果。 |
| 样式 | setTabIndicatorFullWidth(boolean tabIndicatorFullWidth) | 设置是否使用完整宽度的指示器。 | 改变指示器是否使用完整宽度。 |
| 样式 | setTabRippleColor(ColorStateList rippleColor) | 设置所有标签点击时的涟漪颜色，如果为null，则使用默认的涟漪颜色。 | 改变所有标签点击时的涟漪颜色。 |
| 样式 | setTabRippleColorResource(int rippleColorResourceId) | 设置所有标签点击时的涟漪颜色，从资源文件中获取颜色状态列表。如果为0，则使用默认的涟漪颜色。 | 改变所有标签点击时的涟漪颜色。 |
| 样式 | setTabTextColors(int normalColor, int selectedColor) | 设置所有标签文本的颜色，分别为正常状态和选中状态的颜色。 | 改变所有标签文本的颜色。 |
| 样式 | setTabTextColors(ColorStateList textColor) | 设置所有标签文本的颜色状态列表，如果为null，则使用默认的文本颜色。 | 改变所有标签文本的颜色随状态变化而变化。 |
| 列表操作 | addOnTabSelectedListener(OnTabSelectedListener listener) | 添加一个监听器，当任何标签的选择状态发生改变时，会回调这个监听器。 | 监听标签的选择或取消选择事件，并做相应的操作。 |
| 列表操作 | addTab(Tab tab) | 添加一个新的标签到布局中，不会选中这个标签。 | 增加一个新的标签。 |
| 列表操作 | addTab(Tab tab, boolean setSelected) | 添加一个新的标签到布局中，并选择是否选中这个标签。 | 增加一个新的标签，并设置是否选中它。 |
| 列表操作 | addTab(Tab tab, int position) | 添加一个新的标签到布局中的指定位置，不会选中这个标签。 | 增加一个新的标签到指定位置。 |
| 列表操作 | addTab(Tab tab, int position, boolean setSelected) | 添加一个新的标签到布局中的指定位置，并选择是否选中这个标签。 | 增加一个新的标签到指定位置，并设置是否选中它。 |
| 列表操作 | clearOnTabSelectedListeners() | 移除所有已经添加过的监听器。 | 取消监听标签的选择或取消选择事件。 |
| 列表操作 | newTab() | 创建并返回一个新的空白标签，这个方法只是创建了一个新的标签对象，并没有添加到布局中，需要通过addTab(Tab tab)方法来添加。| 创建一个新的空白标签，并设置它的文本或图标等属性。 |
| 列表操作 | removeAllTabs() | 移除所有已经添加过的标签，并清空内部状态。| 清空所有标签。 |
| 列表操作 | removeOnTabSelectedListener(OnTabSelectedListener listener) | 移除指定的监听器，如果没有添加过这个监听器，不会有任何效果。| 取消监听指定监听器对应的事件。 |
| 列表操作 | removeTab(Tab tab) | 移除指定的标签，如果没有添加过这个标签，不会有任何效果。| 删除指定的标签。 |
| 列表操作 | removeTabAt(int position) | 移除指定位置的标签，如果位置无效，不会有任何效果。| 删除指定位置的标签。 |
| 列表操作 | selectTab(Tab tab) | 选中指定的标签，如果没有添加过这个标签，不会有任何效果。如果已经有其他选中的标签，会取消选中它们，并回调相应的监听器方法。如果传入null，则取消选中所有已经选中的标签，并回调相应的监听器方法。| 选中或取消选中某个或所有已经添加过的

#####2.公共get方法
| 分类 | 方法名 | 作用简述 | 使用场景 |
| --- | --- | --- | --- |
| 布局 | getTabGravity() | 返回标签的重力，即标签在水平方向上的对齐方式。可能的值有GRAVITY_CENTER和GRAVITY_FILL。 | 改变标签在水平方向上的对齐方式。 |
| 布局 | getTabIndicatorGravity() | 返回指示器的重力，即指示器在垂直方向上的对齐方式。可能的值有INDICATOR_GRAVITY_BOTTOM、INDICATOR_GRAVITY_CENTER、INDICATOR_GRAVITY_STRETCH和INDICATOR_GRAVITY_TOP。 | 改变指示器在垂直方向上的对齐方式。 |
| 布局 | getTabMode() | 返回标签的模式，即标签是否可以滚动。可能的值有MODE_FIXED和MODE_SCROLLABLE。 | 改变标签是否可以滚动。 |
| 样式 | getTabIconTint() | 返回标签图标的着色器，如果没有设置，返回null。 | 改变标签图标的颜色或透明度。 |
| 样式 | getTabIconTintMode() | 返回标签图标的着色模式，如果没有设置，返回null。 | 改变标签图标的着色效果。 |
| 样式 | getTabIndicatorFullWidth() | 返回是否使用完整宽度的指示器。 | 改变指示器是否使用完整宽度。 |
| 样式 | getTabRippleColor() | 返回标签点击时的涟漪颜色，如果没有设置，返回null。 | 改变标签点击时的涟漪颜色。 |
| 样式 | getTabRippleColorStateList() | 返回标签点击时的涟漪颜色状态列表，如果没有设置，返回null。 | 改变标签点击时的涟漪颜色随状态变化而变化。 |
| 样式 | getTabTextColors() | 返回标签文本的颜色状态列表，如果没有设置，返回null。 | 改变标签文本的颜色随状态变化而变化。 |
| 列表操作 | getSelectedTabPosition() | 返回当前选中的标签的位置，如果没有选中的标签，返回-1。 | 判断用户选择了哪个标签，并根据不同的标签显示不同的内容。 |
| 列表操作 | getTabAt(int index) | 返回指定位置的标签，如果位置无效，返回null。 | 获取某个标签，并调用它的方法来改变它的文本或图标。 |
| 列表操作 | getTabCount() | 返回标签的数量。 | 遍历所有的标签或判断是否有标签。 |

#####3.公共内部类
| 分类 | 类名 | 作用简述 | 使用场景 |
| --- | --- | --- | --- |
| 监听器 | TabLayout.BaseOnTabSelectedListener<T extends TabLayout.Tab> | 这个接口已经被废弃，使用TabLayout.OnTabSelectedListener代替。它是一个监听器接口，当一个标签的选择状态发生改变时，会回调相应的方法。 | 监听标签的选择或取消选择事件，并做相应的操作。 |
| 监听器 | TabLayout.OnTabSelectedListener | 这个接口是一个监听器接口，当一个标签的选择状态发生改变时，会回调相应的方法。它有三个方法：onTabSelected(Tab tab)、onTabUnselected(Tab tab)和onTabReselected(Tab tab)。 | 监听标签的选择或取消选择事件，并做相应的操作。 |
| 标签 | TabLayout.Tab | 这个类是一个标签，在TabLayout中显示。它有一些方法来设置或获取它的文本、图标、标签、自定义视图等属性。它也有一些方法来选中或取消选中它。 | 创建和修改标签，并设置或获取它的属性。 |
| 视图 | TabLayout.TabView | 这个类是一个包含TabLayout.Tab实例的线性布局，用于在TabLayout中显示。它是一个内部类，不对外公开。 | 在TabLayout中显示和布局标签。 |
| 视图 | TabLayout.TabItem | 这个类是一种特殊的视图，在TabLayout中可以显式声明一个标签，并设置它的文本或图标属性。它是一个内部类，不对外公开。 | 在布局文件中直接添加和设置标签。 |

具体内部方法

| 分类 | 类名 | 方法名 | 作用简述 | 使用场景 |
| --- | --- | --- | --- | --- |
| 监听器 | TabLayout.BaseOnTabSelectedListener<T extends TabLayout.Tab> | onTabSelected(Tab tab) | 当一个标签被选中时，回调这个方法。 | 监听标签被选中事件，并做相应的操作。 |
| 监听器 | TabLayout.BaseOnTabSelectedListener<T extends TabLayout.Tab> | onTabUnselected(Tab tab) | 当一个标签被取消选中时，回调这个方法。 | 监听标签被取消选中事件，并做相应的操作。 |
| 监听器 | TabLayout.BaseOnTabSelectedListener<T extends TabLayout.Tab> | onTabReselected(Tab tab) | 当一个已经选中的标签再次被选中时，回调这个方法。 | 监听标签再次被选中事件，并做相应的操作。 |
| 监听器 | TabLayout.OnTabSelectedListener | onTabSelected(Tab tab) | 当一个标签被选中时，回调这个方法。 | 监听标签被选中事件，并做相应的操作。 |
| 监听器 | TabLayout.OnTabSelectedListener | onTabUnselected(Tab tab) | 当一个标签被取消选中时，回调这个方法。 | 监听标签被取消选中事件，并做相应的操作。 |
| 监听器 | TabLayout.OnTabSelectedListener | onTabReselected(Tab tab) | 当一个已经选中的标签再次被选中时，回调这个方法。 | 监听标签再次被选中事件，并做相应的操作。 |
| 标签 | TabLayout.Tab | getBadge() | 返回这个标签上的徽章，如果没有设置徽章，返回null。徽章是一种显示数字或文本的小圆形视图，用于提示用户有新消息或未读通知等。| 获取或修改这个标签上的徽章。 |
| 标签 | TabLayout.Tab | getCustomView() | 返回这个标签上的自定义视图，如果没有设置自定义视图，返回null。自定义视图是一种可以替代默认的文本和图标视图的视图，用于显示更复杂或不同样式的内容。| 获取或修改这个标签上的自定义视图。 |
| 标签 | TabLayout.Tab | getIcon() | 返回这个标签上的图标，如果没有设置图标，返回null。图标是一种可以在文本左边或上面显示的小图片，用于增加视觉效果或区分不同类型的内容。| 获取或修改这个标签上的图标。 |
| 标签 | TabLayout.Tab | getParent() | 返回这个标签所属的TabLayout对象，如果没有添加到任何TabLayout对象中，返回null。| 获取或修改这个标签所属的TabLayout对象。 |
| 标签 | TabLayout.Tab | getPosition() | 返回这个标签在TabLayout对象中的位置，如果没有添加到任何TabLayout对象中，返回-1。| 获取或修改这个标签在TabLayout对象中的位置。 |
| 标签 | TabLayout.Tab | getTag() | 返回这个标签上设置的任意对象，如果没有设置任意对象，返回null。任意对象是一种可以关联到这个标签上的用户自定义数据，用于存储额外信息或状态等。| 获取或修改这个标签上设置的任意对象。 |
| 标签 | TabLayout.Tab | getText() | 返回这个标签上的文本，如果没有设置文本，返回null。文本是一种可以在图标右边或下面显示的文字，用于描述或命名这个标签的内容。| 获取或修改这个标签上的文本。 |
| 标签 | TabLayout.Tab | getOrCreateBadge() | 返回这个标签上的徽章，如果没有设置徽章，就创建一个新的徽章并返回。徽章是一种显示数字或文本的小圆形视图，用于提示用户有新消息或未读通知等。| 获取或创建这个标签上的徽章。 |
| 标签 | TabLayout.Tab | getTabLabelVisibility() | 返回这个标签的标签可见性，即当只有图标时，是否显示文本。可能的值有TAB_LABEL_VISIBILITY_UNLABELED和TAB_LABEL_VISIBILITY_LABELED。| 获取或修改这个标签的标签可见性。 |
| 标签 | TabLayout.Tab | getTabLabelVisibility() | 返回这个标签的标签可见性，即当只有图标时，是否显示文本。可能的值有TAB_LABEL_VISIBILITY_UNLABELED和TAB_LABEL_VISIBILITY_LABELED。| 获取或修改这个标签的标签可见性。 |
| 标签 | TabLayout.Tab | getTabLabelVisibility() | 返回这个标签的标签可见性，即当只有图标时，是否显示文本。可能的值有TAB_LABEL_VISIBILITY_UNLABELED和TAB_LABEL_VISIBILITY_LABELED。| 获取或修改这个标签的标签可见性。 |
| 标签 | TabLayout.Tab | isInlineLabel() | 返回这个标签是否使用内联标签，即标签文本和图标是否在同一行。| 获取或修改这个标签是否使用内联标签。 |
| 标签 | TabLayout.Tab | isSelected() | 返回这个标签是否被选中。| 获取或修改这个标签是否被选中。 |
| 标签 | TabLayout.Tab | removeBadge() | 移除这个标签上的徽章，如果没有设置徽章，不会有任何效果。徽章是一种显示数字或文本的小圆形视图，用于提示用户有新消息或未读通知等。| 移除这个标签上的徽章。 |
| 标签 | TabLayout.Tab | select() | 选中这个标签，如果已经被选中，不会有任何效果。如果已经有其他选中的标签，会取消选中它们，并回调相应的监听器方法。如果没有添加到任何TabLayout对象中，不会有任何效果。| 选中这个标签
| 标签 | TabLayout.Tab | setBadgeDrawable(Drawable badgeDrawable) | 设置这个标签上的徽章为一个Drawable对象，如果为null，则移除徽章。徽章是一种显示数字或文本的小圆形视图，用于提示用户有新消息或未读通知等。| 设置或移除这个标签上的徽章。 |
| 标签 | TabLayout.Tab | setBadgeText(CharSequence badgeText) | 设置这个标签上的徽章文本，如果为null，则移除徽章。徽章是一种显示数字或文本的小圆形视图，用于提示用户有新消息或未读通知等。| 设置或移除这个标签上的徽章文本。 |
| 标签 | TabLayout.Tab | setContentDescription(CharSequence contentDescription) | 设置这个标签的内容描述，用于辅助功能服务。内容描述是一种可以描述这个标签的含义或作用的文字，用于帮助视障或听障用户理解和操作这个标签。| 设置或修改这个标签的内容描述。 |
| 标签 | TabLayout.Tab | setCustomView(int resId) | 设置这个标签上的自定义视图为一个资源ID，如果为0，则移除自定义视图。自定义视图是一种可以替代默认的文本和图标视图的视图，用于显示更复杂或不同样式的内容。| 设置或移除这个标签上的自定义视图。 |
| 标签 | TabLayout.Tab | setCustomView(View view) | 设置这个标签上的自定义视图为一个View对象，如果为null，则移除自定义视图。自定义视图是一种可以替代默认的文本和图标视图的视图，用于显示更复杂或不同样式的内容。| 设置或移除这个标签上的自定义视图。 |
| 标签 | TabLayout.Tab | setIcon(int resId) | 设置这个标签上的图标为一个资源ID，如果为0，则移除图标。图标是一种可以在文本左边或上面显示的小图片，用于增加视觉效果或区分不同类型的内容。| 设置或移除这个标签上的图标。 |
| 标签 | TabLayout.Tab | setIcon(Drawable icon) | 设置这个标签上的图标为一个Drawable对象，如果为null，则移除图标。图标是一种可以在文本左边或上面显示的小图片，用于增加视觉效果或区分不同类型的内容。| 设置或移除这个标签上的图标。 |
| 标签 | TabLayout.Tab | setIconTintList(ColorStateList iconTint) | 设置这个标签上的图标着色器，如果为null，则不着色图标。着色器是一种可以改变图片颜色或透明度的效果，用于适应不同状态或主题等。| 设置或取消这个标签上的图标着色器。 |
| 标签 | TabLayout.Tab | setIconTintMode(PorterDuff.Mode iconTintMode) | 设置这个标签上的图标着色模式，如果为null，则使用SRC_IN模式。着色模式是一种可以改变图片着色效果的方式，用于实现不同风格或动画等。这个方法只有在设置了着色器时才有效果。| 设置或修改这个标签上的图标着色模式。 |
| 标签 | TabLayout.Tab | setInlineLabel(boolean inline) | 设置是否使用内联标签，即标签文本和图标是否在同一行。| 设置或修改是否使用内联
| 标签 | TabLayout.Tab | setTag(Object tag) | 设置这个标签上的任意对象，如果为null，则移除任意对象。任意对象是一种可以关联到这个标签上的用户自定义数据，用于存储额外信息或状态等。| 设置或移除这个标签上的任意对象。 |
| 标签 | TabLayout.Tab | setText(int resId) | 设置这个标签上的文本为一个资源ID，如果为0，则移除文本。文本是一种可以在图标右边或下面显示的文字，用于描述或命名这个标签的内容。| 设置或移除这个标签上的文本。 |
| 标签 | TabLayout.Tab | setText(CharSequence text) | 设置这个标签上的文本为一个CharSequence对象，如果为null，则移除文本。文本是一种可以在图标右边或下面显示的文字，用于描述或命名这个标签的内容。| 设置或移除这个标签上的文本。 |
| 标签 | TabLayout.Tab | setTabLabelVisibility(int tabLabelVisibility) | 设置这个标签的标签可见性，即当只有图标时，是否显示文本。可能的值有TAB_LABEL_VISIBILITY_UNLABELED和TAB_LABEL_VISIBILITY_LABELED。| 设置或修改这个标签的标签可见性。 |
| 视图 | TabLayout.TabView | getTab() | 返回这个视图对应的TabLayout.Tab对象，如果没有对应的对象，返回null。| 获取或修改这个视图对应的TabLayout.Tab对象。 |
| 视图 | TabLayout.TabView | onInitializeAccessibilityNodeInfo(AccessibilityNodeInfo info) | 初始化这个视图的辅助功能节点信息，用于辅助功能服务。辅助功能节点信息是一种可以描述这个视图的属性和行为的数据结构，用于帮助视障或听障用户理解和操作这个视图。| 初始化或修改这个视图的辅助功能节点信息。 |
| 视图 | TabLayout.TabView | onMeasure(int widthMeasureSpec, int heightMeasureSpec) | 测量这个视图及其子视图的大小，用于布局服务。测量是一种可以确定视图所需宽度和高度的过程，用于适应不同屏幕尺寸和方向等。| 测量或修改这个视图及其子视图的大小。 |
| 视图 | TabLayout.TabView | setSelected(boolean selected) | 设置这个视图是否被选中，用于状态服务。选中是一种可以表示视图是否被用户激活或选择的状态，用于改变视觉效果或触发事件等。| 设置或修改这个视图是否被选中。 |
| 视图 | TabLayout.TabView | update() | 更新这个视图的内容，根据对应的TabLayout.Tab对象的属性。| 更新或修改这个视图的内容。 |
| 视图 | TabLayout.TabItem | TabItem(Context context) | 构造一个新的TabItem对象，传入一个上下文对象。上下文对象是一种可以提供应用环境信息和资源的对象，用于创建和初始化视图等。| 创建一个新的TabItem对象。 |
| 视图 | TabLayout.TabItem | TabItem(Context context, AttributeSet attrs) | 构造一个新的TabItem对象，传入一个上下文对象和一个属性集合。属性集合是一种可以包含视图的样式和行为属性的数据结构，用于从布局文件中解析视图等。| 创建一个新的TabItem对象，并设置它的属性。 |

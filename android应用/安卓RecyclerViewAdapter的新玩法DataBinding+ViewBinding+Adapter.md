直奔主题
首先来说一下，自定义RecyclerViewAdapter的基本步骤，不管是什么写法，这些都是定式：
##1.写一个类继承RecyclerView.Adapter
```
class DemoRecyclerViewAdapter(
    var listDemoItem: ArrayList<listDemoItem>,
    val context: Context
) :
    RecyclerView.Adapter<ItemDemoRecyclerViewBinding.MyViewHolder>() {
}
```
写一个内部类，继承RecyclerView.ViewHolder
```
    class MyViewHolder(val binding: ItemDemoRecyclerViewBinding) :
        RecyclerView.ViewHolder(binding.root)
```


##2.复写（Override）一些必须复写的函数，这些函数都是抽象函数，所以如果子类不是抽象类，就必须要复写。
不用担心，编译器都会有提示的
![提示有错误.png](https://upload-images.jianshu.io/upload_images/24860325-5b7370ada4b45e8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
而且源码里很清楚，有三个抽象函数
![三个抽象函数.png](https://upload-images.jianshu.io/upload_images/24860325-1662328e65be8091.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在这里，我们直接用快捷键ctr+o，就会有弹出框，里面是所有能复写的父类或接口的方法：
![可以复写的.png](https://upload-images.jianshu.io/upload_images/24860325-62d998abf445097b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
直接用鼠标点击需要复写的方法就好了。

##3.向相应的方法里写入业务逻辑

onCreateViewHolder：
这个方法里创建视图：
所以我们要返回一个viewHolder。
这里我们直接用自定义的viewHolder类，新建一个实例，构造函数的参数，是我们ViewBinding。（ViewBinding可以dotcall的方式，访问XML文件中，带有id的控件。）
```
    override fun onCreateViewHolder(
        parent: ViewGroup,
        viewType: Int
    ): MyViewHolder {

        return MyViewHolder(
            ItemDemoRecyclerViewBinding.inflate(
                LayoutInflater.from(parent.context),
                parent,
                false
            )
        )
    }
```
onBindViewHolder
生成好了视图以后，在这里写点击事件，绑定、更新视图属性。
```
 override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        with(holder) {
            with(binding) {
                (listDemoItem[position]).apply {
                    itemSettingsTextViewTitle.text = title
                    var onClockListener: View.OnClickListener = View.OnClickListener {
                    }
                    when (this) {
                        is Demo1 -> {
                            itemImageViewSettingsProfile.setImageResource(R.drawable.demo1)//图片自己找吧
                            onClockListener = View.OnClickListener {
                              //添加点击事件的逻辑
                            }
                        }
                        is Demo2 -> {
                            itemImageViewSettingsProfile.setImageResource(R.drawable.demo2)
                            onClockListener = View.OnClickListener {
                                //添加点击事件的逻辑
                            }
                        }
                        is Demo3 -> {
                            itemImageViewSettingsProfile.setImageResource(R.drawable.demo3)
                            onClockListener = View.OnClickListener {
                                //添加点击事件的逻辑
                            }
                        }
                        is BaseDemoSettings -> {
                            itemImageViewSettingsProfile.setImageResource(R.drawable.demo)
                            onClockListener = View.OnClickListener {
                                //添加点击事件的逻辑
                            }
                        }
                    }
                    root.setOnClickListener(onClockListener)
                }
            }
        }
    }
```

getItemCount
Kotlin的简洁优雅，使得这个函数变成了一行代码
```
override fun getItemCount() = listDemoItem.size
```

总结一下，和传统的在viewHolder里写入大量的findViewById不同，直接使用ViewBinding使得代码非常简洁。

其实listDemoItem参数，可以使用DataBinding的方式写。
viewModel类
```
class DemoModel : ViewModel() {
    private val _listDemoItem =
        MutableLiveData<ArrayList<DemoRecyclerViewAdapter.BaseDemoItem>>().apply {
            value = ArrayList()
        }
    val listDemoItem : LiveData<ArrayList<DemoRecyclerViewAdapter.BaseDemoItem>> =
        _listDemoItem 

    fun initlistDemoItem (context: Context) {
        with(ArrayList<DemoRecyclerViewAdapter.BaseDemoItem>()) {
            this.add(DemoRecyclerViewAdapter.Demo1(context))
            this.add(DemoRecyclerViewAdapter.Demo2(context))
            this.add(DemoRecyclerViewAdapter.Demo3(context))
            _listDemoItem .postValue(this)
        }
    }
}
```
使用
```
//以下是在fragment里的写法，activity里把requireActivity()替换成this@MainActivity
ViewModelProvider.AndroidViewModelFactory(requireActivity().application)
            .create(DemoModel ::class.java).apply {
                    with(
                        DemoRecyclerViewAdapter(
                            listDemoItem .value!!,
                            requireActivity()
                        )
                    ) {
                        recyclerViewDeomList.adapter = this
                        this@apply.listDemoItem .observe(
                            viewLifecycleOwner,
                            Observer {
                                this.setData(it)
                            })
                    }
                    initlistDemoItem (requireActivity())
            }
```

#下面是所有类的源代码
```
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerViewDeomList"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
        app:layout_constraintBottom_toBottomOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

 DemoRecyclerViewAdapter.kt
```
class DemoRecyclerViewAdapter(
    var listDemoItem: ArrayList<listDemoItem>,
    val context: Context
) :
    RecyclerView.Adapter<ItemDemoRecyclerViewBinding.MyViewHolder>() {

    class MyViewHolder(val binding: ItemDemoRecyclerViewBinding) :
        RecyclerView.ViewHolder(binding.root)

    override fun onCreateViewHolder(
        parent: ViewGroup,
        viewType: Int
    ): MyViewHolder {

        return MyViewHolder(
            ItemDemoRecyclerViewBinding.inflate(
                LayoutInflater.from(parent.context),
                parent,
                false
            )
        )
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        with(holder) {
            with(binding) {
                (listDemoItem[position]).apply {
                    itemSettingsTextViewTitle.text = title
                    var onClockListener: View.OnClickListener = View.OnClickListener {
                    }
                    when (this) {
                        is Demo1 -> {
                            itemImageViewSettingsProfile.setImageResource(R.drawable.demo1)//图片自己找吧
                            onClockListener = View.OnClickListener {
                              //添加点击事件的逻辑
                            }
                        }
                        is Demo2 -> {
                            itemImageViewSettingsProfile.setImageResource(R.drawable.demo2)
                            onClockListener = View.OnClickListener {
                                //添加点击事件的逻辑
                            }
                        }
                        is Demo3 -> {
                            itemImageViewSettingsProfile.setImageResource(R.drawable.demo3)
                            onClockListener = View.OnClickListener {
                                //添加点击事件的逻辑
                            }
                        }
                        is BaseDemoSettings -> {
                            itemImageViewSettingsProfile.setImageResource(R.drawable.demo)
                            onClockListener = View.OnClickListener {
                                //添加点击事件的逻辑
                            }
                        }
                    }
                    root.setOnClickListener(onClockListener)
                }
            }
        }
    }

    override fun getItemCount() = listDemoItem.size

    fun setData(list: ArrayList<listDemoItem>) {
        listDemoItem= list
        notifyDataSetChanged()
    }

    abstract class listDemoItem(val title: String)

    class Demo1(context: Context) : listDemoItem(
        context.resources.getString(
            R.string.item_title_demo1
        )
    )

    class Demo2(context: Context) : listDemoItem(
        context.resources.getString(
            R.string.item_title_demo2
        )
    )

    class Demo3(context: Context) : listDemoItem(
        context.resources.getString(
            R.string.item_title_demo3
        )
    )
}
```
item_demo_recycler_view.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingTop="@dimen/spacing_small"
    android:paddingBottom="@dimen/spacing_small">

    <ImageView
        android:id="@+id/itemImageViewSettingsProfile"
        android:layout_width="20dp"
        android:layout_height="20dp"
        android:layout_marginStart="@dimen/spacing_normal"
        android:scaleType="fitCenter"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintDimensionRatio="1:1"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/itemSettingsTextViewTitle"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="@string/pre_text_view_artist"
        android:layout_marginStart="@dimen/spacing_normal"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toRightOf="@+id/itemImageViewSettingsProfile"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
最后在activity或者fragment等等地方使用
```
with(ArrayList<DemoRecyclerViewAdapter.BaseDeviceDetailSettingsItem>()) {
            this.add(DemoRecyclerViewAdapter.Demo1(context))
            this.add(DemoRecyclerViewAdapter.Demo2(context))
            this.add(DemoRecyclerViewAdapter.Demo3(context))
            DemoRecyclerViewAdapter(
                            this,
                            requireActivity()//fragment里用这个
//activity类里面，用 //this@MainActivity替换requireActivity()
                        )
        }

```
在string.xml里添加
![添加在resources标签里](https://upload-images.jianshu.io/upload_images/24860325-b52cec9ddcc4201a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
    <string name="item_title_demo1">demo1</string>
    <string name="item_title_demo2">demo2</string>
    <string name="item_title_demo3">demo3</string>
```

Bottom Sheet对话框似乎正在取代常规的Android对话框和菜单。底部对话框是一个组件，它从屏幕底部滑上来，展示你的应用程序中的额外内容。

底部对话框就像一个由用户行为触发的消息框。谷歌、Facebook和Twitter等公司已经在其应用程序中实现了这一功能。

底部对话框被用于音乐、支付和文件管理应用程序中。就应用而言，任何Android 组件，包括`TextView`、`ImageView`、`RecyclerViews`、`Buttons`和`Text input`都可以包含在Bottom Sheet中。这使它变得相当有活力。

例如，它可以帮助显示从数据库中获取的数据。只要适合你的应用周期，底部对话框可以应用于许多情况下。

#1.底部对话框的类型

底层对话框的两种主要类型是`多功能`和`长周期`对话框；

#####1.1多功能底部对话框

它具有与alert dialog类似的特征。当被触发时（通过用户的操作），它从当前屏幕的底部滑上来。一个多功能对话框可以包含一个功能列表。

这些元素在被点击时可以对应于一些动作。多功能底部对话框阻止与屏幕其他部分的交互，以表示焦点的转移。当用户点击视图之外或按下返回键时，它就会被取消。

我们没有像长周期对话框那样用CoordinatorLayout来包装Modal Bottom Sheet，而是像普通对话框那样动态地创建它。

一个优秀的多功能底部对话框的例子是Google Drive应用程序。

![图片.png](https://upload-images.jianshu.io/upload_images/24860325-14ce5c1f9a63d4fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


或这个付款底部对话框的例子。

![图片.png](https://upload-images.jianshu.io/upload_images/24860325-ed92209099b4f462.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


多功能对话框是内联菜单和简单对话框的一个很好的替代品。它们为更多的内容、图标和更多的屏幕操作提供了额外的空间。

#####1.2长周期底部对话框

持久底部对话框提供关于当前屏幕的补充内容。它是作为CoordinatorLayout的一个子节点。

容器的一部分是可见的，为用户提供更多的内容。与多功能对话框不同，长周期底部对话框小组件对当前屏幕内容是永久性的。

下面是一个Google Maps应用程序中的长周期底部对话框对话框的例子。

![图片.png](https://upload-images.jianshu.io/upload_images/24860325-7cc3f02e71a474c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#2.实现功能

#####2.1创建一个新的Android studio项目。为了实现Bottom Sheet对话框，你需要导入material库。

在你的app.gradle文件中导入以下库。
```
implementation 'com.google.android.material:material:1.2.1'
```

同步该项目以下载库。这将使所有需要的功能在你的项目中可用。

我们将讨论如何使用Android studio实现两种类型的底部对话框对话框。

实现多功能的底部对话框对话框

使用BottomsheetDialog

#####2.2准备布局

为了显示对话框，你需要一个XML文件来安排对话框的内容。你可以选择使用任何适合对话框的小部件。这些视图可以包括`RecyclerView`、`ImageViews`、`Text`、`Inputs`和`Button`。

确保你生成一些矢量图像，就像下面XML文件中的ImageView所展示的那样。或者查看这个GitHub资源库以获得更多参考。https://github.com/kimkimani/ModalBottomSheet

下面是我将用来实现多功能的底部对话框对话框的bottom_sheet_dialog_layout.xml布局。
```
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout

 xmlns:android="http://schemas.android.com/apk/res/android"

 android:orientation="vertical"

 android:layout_width="match_parent"

 android:layout_height="match_parent">

 <LinearLayout

 android:layout_width="match_parent"

 android:layout_height="wrap_content"

 android:orientation="horizontal"

 android:id="@+id/download"

 android:background="?android:attr/selectableItemBackground"

 android:padding="8dp">

 <ImageView

 android:layout_width="24dp"

 android:layout_height="24dp"

 android:src="@drawable/ic_baseline_cloud_download_24"

 android:layout_margin="8dp"/>

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:text="Download File"

 android:layout_gravity="center_vertical"

 android:padding="8dp"/>

 </LinearLayout>

 <LinearLayout

 android:layout_width="match_parent"

 android:layout_height="wrap_content"

 android:orientation="horizontal"

 android:id="@+id/shareLinearLayout"

 android:background="?android:attr/selectableItemBackground"

 android:padding="8dp">

 <ImageView

 android:layout_width="24dp"

 android:layout_height="24dp"

 android:src="@drawable/ic_baseline_share_24"

 android:layout_margin="8dp"/>

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:text="Share"

 android:layout_gravity="center_vertical"

 android:padding="8dp"/>

 </LinearLayout>

 <LinearLayout

 android:layout_width="match_parent"

 android:layout_height="wrap_content"

 android:orientation="horizontal"

 android:id="@+id/uploadLinearLaySout"

 android:background="?android:attr/selectableItemBackground"

 android:padding="8dp">

 <ImageView

 android:layout_width="24dp"

 android:layout_height="24dp"

 android:src="@drawable/ic_baseline_add_to_drive_24"

 android:layout_margin="8dp"/>

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:text="Upload To Google Drive"

 android:layout_gravity="center_vertical"

 android:padding="8dp"/>

  </LinearLayout>

 <LinearLayout

 android:layout_width="match_parent"

 android:layout_height="wrap_content"

 android:orientation="horizontal"

 android:id="@+id/copyLinearLayout"

 android:background="?android:attr/selectableItemBackground"

 android:padding="8dp">

 <ImageView

 android:layout_width="24dp"

 android:layout_height="24dp"

 android:src="@drawable/ic_baseline_file_copy_24"

 android:layout_margin="8dp"/>

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:text="Copy Link"

 android:layout_gravity="center_vertical"

 android:padding="8dp"/>

 </LinearLayout>

 <LinearLayout

 android:layout_width="match_parent"

 android:layout_height="wrap_content"

 android:orientation="horizontal"

 android:id="@+id/delete"

 android:background="?android:attr/selectableItemBackground"

 android:padding="8dp">

 <ImageView

 android:layout_width="24dp"

 android:layout_height="24dp"

 android:src="@drawable/ic_baseline_delete_24"

 android:layout_margin="8dp"/>

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:text="Delete File Selection"

 android:layout_gravity="center_vertical"

 android:padding="8dp"/>

 </LinearLayout>

</LinearLayout>
```
对话框是由一个指定的用户动作触发的。在本教程中，我们将包括一个按钮并使用它的onClick监听器来触发对话框。

继续并在你的 activity_main.xml 文件中添加一个按钮。
```
<Button

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:id="@+id/button"

 android:backgroundTint="@color/purple_500"

 android:fontFamily="serif"

 android:text="Show Dialog"

 android:textColor="#FFFFFF"

 android:textSize="18sp"

 tools:ignore="MissingConstraints"

 tools:layout_editor_absoluteX="116dp"

tools:layout_editor_absoluteY="28dp"/>
```

#####2.3在activity中初始化底层对话框

初始化按钮并在onCreate函数中设置onClick监听器。当按钮被点击时，我们将显示对话框。创建一个函数showBottomSheetDialog()并在按钮的onClick监听器中调用它，如下所示。
```
Button mBottton = findViewById(R.id.button);

 mBottton.setOnClickListener(new View.OnClickListener() {

 @Override

 public void onClick(View v) {

 showBottomSheetDialog()

 }

});

在showBottomSheetDialog()函数中，初始化底部对话框。使用setContentView方法初始化 bottom_sheet_dialog_layout.xml。
```
#####2.4声明所有的组件，并按底层对话框布局中指定的id调用它们。最后，我们将使用bottomSheetDialog.show()来显示对话框。
```
private void showBottomSheetDialog() {

 final BottomSheetDialog bottomSheetDialog = new BottomSheetDialog(this);

 bottomSheetDialog.setContentView(R.layout.bottom_sheet_dialog);

 LinearLayout copy = bottomSheetDialog.findViewById(R.id.copyLinearLayout);

 LinearLayout share = bottomSheetDialog.findViewById(R.id.shareLinearLayout);

 LinearLayout upload = bottomSheetDialog.findViewById(R.id.uploadLinearLayout);

 LinearLayout download = bottomSheetDialog.findViewById(R.id.download);

 LinearLayout delete = bottomSheetDialog.findViewById(R.id.delete);

 bottomSheetDialog.show();

}
```
#####2.5运行该应用程序以测试 "底部对话框 "是否有效。点击该按钮应该会触发对话框从底部滑到顶部。

![图片.png](https://upload-images.jianshu.io/upload_images/24860325-f591e65905939201.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#####2.6设置onClick监听器

对话框布局中的每个元素都可以被分配一个动作。当一个项目被点击时，它将按照代码中的指定重定向给用户。

在我们的应用程序中，当一个元素被点击时，我们将显示一个Toast消息。你可以在你未来的应用程序中进行修改，以引导用户到不同的活动。

在bottomSheetDialog.show()的正上方添加以下OnClickListeners。
```
copy.setOnClickListener(new View.OnClickListener() {

 @Override

 public void onClick(View v) {

 Toast.makeText(getApplicationContext(), "Copy is Clicked ", Toast.LENGTH_LONG).show();

 bottomSheetDialog.dismiss();

 }

});

share.setOnClickListener(new View.OnClickListener() {

 @Override

 public void onClick(View v) {

 Toast.makeText(getApplicationContext(), "Share is Clicked", Toast.LENGTH_LONG).show();

 bottomSheetDialog.dismiss();

 }

});

upload.setOnClickListener(new View.OnClickListener() {

 @Override

 public void onClick(View v) {

 Toast.makeText(getApplicationContext(), "Upload is Clicked", Toast.LENGTH_LONG).show();

 bottomSheetDialog.dismiss();

 }

});

download.setOnClickListener(new View.OnClickListener() {

 @Override

 public void onClick(View v) {

 Toast.makeText(getApplicationContext(), "Download is Clicked", Toast.LENGTH_LONG).show();

 bottomSheetDialog.dismiss();

 }

});

delete.setOnClickListener(new View.OnClickListener() {

 @Override

 public void onClick(View v) {

 Toast.makeText(getApplicationContext(), "Delete is Clicked", Toast.LENGTH_LONG).show();

 bottomSheetDialog.dismiss();

 }

});
```
我们使用bottomSheetDialog.dismiss()来关闭对话框，一旦有元素被点击。

你也可以设置一个更明显的动作，指示你的应用程序在对话框被关闭时做一些事情。例如，应用程序可以启动一个新的activity。
```
bottomSheetDialog.setOnDismissListener(new DialogInterface.OnDismissListener() {

 @Override

 public void onDismiss(DialogInterface dialog) {

 // Instructions on bottomSheetDialog Dismiss

 }

});
```
![图片.png](https://upload-images.jianshu.io/upload_images/24860325-a2849354f0d1f9ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


BottomSheetDialogFragment 一个Fragment可以被显示为一个Bottom Sheet对话框。继续创建一个新的Fragment，叫它BottomSheetFragment。你可以选择启动一个新的项目。

创建一个新的片段将生成一个与之相关的XML文件。继续并在其中包括你的布局设计。使用 bottom_sheet_dialog_layout.xml 中指定的相同布局。为这个Fragment加载布局，如下图所示。
```
public class BottomSheetFragment extends Fragment {

 public BlankFragment() {

 }

 @Override

 public void onCreate(Bundle savedInstanceState) {

 super.onCreate(savedInstanceState);

 }

 @Override

 public View onCreateView(LayoutInflater inflater, ViewGroup container,Bundle savedInstanceState) {

 View view = inflater.inflate(R.layout.bottom_sheet_dialog, container, false);

 return view;

 }

}
```
在 activity_main.xml 中添加一个按钮，声明它，并按照前面的步骤设置 OnClick Listener。

我们想在按钮被点击的时候打开这个Fragment。

在按钮的OnClick Listener中指出以下代码块。
```
Button bottton = findViewById(R.id.button);

bottton.setOnClickListener(new View.OnClickListener() {

 @Override

 public void onClick(View v) {

 BlankFragment blankFragment = new BlankFragment();

 blankFragment.show(getSupportFragmentManager(),blankFragment.getTag());

 }

});
```
我们需要将这个Fragment转换为Bottom Sheet。我们需要继承BottomSheetDialogFragment而不是Fragment。
```
public class BottomSheetFragment extends BottomSheetDialogFragment {

}
```
当你运行该应用程序时，它应该显示如下所示的对话框。

![图片.png](https://upload-images.jianshu.io/upload_images/24860325-5a6865d19e39db3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在GitHub上查看用于实现两个多功能对话框的代码。

实现长周期底层页面对话框

布置底层页面的设计

我们将使用一个简单的登录界面的例子。我们将使用一个长周期对话框将其滑入主屏幕，而不是在常规activity布局中显示它。

我已经创建了一个bottom_sheet_dialog_layout.xml文件，并包括以下简单的登录布局。
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

 xmlns:app="http://schemas.android.com/apk/res-auto"

 android:id="@+id/bottom_sheet_layout"

 android:layout_width="match_parent"

 android:orientation="vertical"

 android:layout_height="wrap_content">

 <LinearLayout

 android:id="@+id/bottom_sheet_header"

 android:layout_width="match_parent"

 android:layout_height="wrap_content"

 android:background="@color/purple_500"

 android:orientation="horizontal"

 app:layout_constraintEnd_toEndOf="parent"

 app:layout_constraintStart_toStartOf="parent"

 app:layout_constraintTop_toTopOf="parent">

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:layout_weight="3"

 android:fontFamily="serif"

 android:padding="24dp"

 android:textSize="18sp"

 android:text="Welcome Back. Please Login"

 android:textColor="@android:color/white" />

 <ImageView

 android:id="@+id/bottom_sheet_arrow"

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:layout_gravity="center"

 android:layout_weight="1"

 app:srcCompat="@drawable/ic_baseline_keyboard_arrow_up_24" />

 </LinearLayout>

 <androidx.constraintlayout.widget.ConstraintLayout

 android:background="@color/teal_200"

 android:layout_width="match_parent"

 android:layout_height="match_parent">

 <LinearLayout

 android:layout_width="match_parent"

 android:layout_height="match_parent"

 android:filterTouchesWhenObscured="false"

 android:gravity="center_horizontal"

 android:orientation="vertical"

 app:layout_constraintBottom_toBottomOf="parent"

 app:layout_constraintEnd_toEndOf="parent"

 app:layout_constraintStart_toStartOf="parent"

 app:layout_constraintTop_toTopOf="parent">

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:fontFamily="serif"

 android:gravity="center_horizontal"

 android:text="login"

 android:textSize="36sp" />

 <LinearLayout

 android:layout_width="match_parent"

 android:layout_height="wrap_content"

 android:layout_marginTop="24dp"

 android:orientation="horizontal"

 android:padding="10dp">

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:layout_weight="1"

 android:textColor= "@color/purple_500"

 android:fontFamily="serif"

 android:text="username"

 android:textSize="24sp" />

 <EditText

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:layout_weight="1"

 android:ems="10"

 android:fontFamily="serif"

 android:hint="enter_email_address"

 android:inputType="textEmailAddress" />

 </LinearLayout>

 <LinearLayout

 android:layout_width="match_parent"

 android:layout_height="wrap_content"

 android:orientation="horizontal"

 android:padding="10dp">

 <TextView

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:layout_weight="1"

 android:textColor= "@color/purple_500"

 android:fontFamily="serif"

 android:text="password"

 android:textSize="24sp" />

 <EditText

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:layout_weight="1"

 android:ems="10"

 android:fontFamily="serif"

 android:hint="enter_password"

 android:inputType="textPassword" />

 </LinearLayout>

 <Button

 android:layout_width="wrap_content"

 android:layout_height="wrap_content"

 android:layout_marginTop="16dp"

 android:layout_marginBottom="50dp"

 android:fontFamily="serif"

 android:text="login"

 android:backgroundTint="@color/purple_500"

 android:textColor="#FFFFFF"

 android:textSize="18sp" />

 </LinearLayout>

 </androidx.constraintlayout.widget.ConstraintLayout>

</LinearLayout>
```
![图片.png](https://upload-images.jianshu.io/upload_images/24860325-ffe492ee194c715d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这还不是一个底部对话框。它只是一个普通的布局。要把这个布局转变为底部对话框（Bottom Sheet）对话框，应该在根布局中添加一些声明。

这些声明将控制 Bottom Sheet 的行为。你可以从这里了解更多关于这些属性的信息。
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"

 xmlns:app="http://schemas.android.com/apk/res-auto"

 android:id="@+id/bottom_sheet_layout"

 android:layout_width="match_parent"

 android:orientation="vertical"

 android:layout_height="wrap_content"

 app:behavior_hideable="false"

 app:behavior_peekHeight="62dp"

 app:layout_behavior="com.google.android.material.bottomsheet.BottomSheetBehavior">
```
底部对话框的行为标志包括；

app:layout_behavior - 将BottomSheetBehavior应用到XML文件中。这被分配给com.google.android.material.bottomsheet。这是最重要的BottomSheetBehavior属性，因为它将一个给定的布局定义为Bottom Sheet对话框。

app:behavior_hideable - 取一个布尔值。如果为真，用户可以通过向下滑动来拖动和隐藏对话框。如果是假的，对话框将漂浮在屏幕上，不能隐藏。

app:behavior_peekHeight - 它定义了用户可见的底部对话框的高度。

记住要添加一个用于访问布局的ID。

为了有效地实现Bottom Sheet，它必须是CoordinatorLayout的一个子节点。要做到这一点，请转到你的主XML文件。这可能是一个activity或fragment。在我们的例子中，它将是 activity_main.xml。

下面是做这件事的代码。
```
<?xml version="1.0" encoding="utf-8"?>

<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"

 xmlns:app="http://schemas.android.com/apk/res-auto"

 xmlns:tools="http://schemas.android.com/tools"

 android:layout_width="match_parent"

 android:layout_height="match_parent"

 tools:context=".MainActivity">

 <androidx.coordinatorlayout.widget.CoordinatorLayout

 android:layout_width="match_parent"

 android:layout_height="match_parent">

 <include layout="@layout/bottom_sheet_dialog_layout" />

 </androidx.coordinatorlayout.widget.CoordinatorLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```
记住要包括我们设计的Bottom Sheet。用CoordinatorLayout把它包起来。

![图片.png](https://upload-images.jianshu.io/upload_images/24860325-e74f3f289fd9b31a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


展开和折叠底部对话框

为了控制对话框的滑动和折叠，我们使用状态。底部对话框有几种你需要了解的状态。它们包括

STATE_EXPANDED - 对话框可见到其最大定义高度。

STATE_COLLAPSED - 对话框根据设定的peekHeight可见。

STATE_DRAGGING - 用户正在向上和向下拖动对话框。

STATE_SETTLING - 显示对话框在一个特定的下沉高度。这可以是peekHeight、展开的高度，如果对话框被隐藏，则为零。

STATE_HIDDEN - 对话框不可见。

我们要做的最后一件事是监听对话框的状态。我们使用BottomSheetCallback来检测任何状态变化。

声明以下参数：

private LinearLayout mBottomSheetLayout;

private BottomSheetBehavior sheetBehavior;

private ImageView header_Arrow_Image;

我们将把OnClick监听器分配给箭头型状的矢量图像。当点击时，我们要展开或收起对话框。
```
header_Arrow_Image.setOnClickListener(new View.OnClickListener() {

 @Override

 public void onClick(View v) {

 if(sheetBehavior.getState() != BottomSheetBehavior.STATE_EXPANDED){

 sheetBehavior.setState(BottomSheetBehavior.STATE_EXPANDED);

 } else {

 sheetBehavior.setState(BottomSheetBehavior.STATE_COLLAPSED);

 }

 }

});
```
实现一个BottomSheetCallback以监听BottomSheetBehavior状态。
```
sheetBehavior.addBottomSheetCallback(new BottomSheetBehavior.BottomSheetCallback() {

 @Override

 public void onStateChanged(@NonNull View bottomSheet, int newState) {

 }

 @Override

 public void onSlide(@NonNull View bottomSheet, float slideOffset) {

 header_Arrow_Image.setRotation(slideOffset * 180);

 }

});
```
onStateChanged告诉应用程序根据状态在对话框上发生了什么。 onSlide将旋转箭头图像（同时从下往上滑动），直到STATE_EXPANDED达到其最大高度。

在另一边，当STATE_COLLAPSED达到peekHeight时，图像将旋转到它的原始状态。

运行应用程序

![图片.png](https://upload-images.jianshu.io/upload_images/24860325-ffa1292cf51ba5d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在GitHub上查看用于实现长周期对话框的代码。

结论

底部对话框是显示菜单和对话框的一种独特方式。它提供了更多的空间来包含内容。底层页面对话框可以容纳不同的组件。

请查看Material文档以了解更多关于底部对话框的信息。

编码愉快。

同行评审贡献：Wanja Mike
翻译：本人
格式校对：本人
原文链接：[Implementing Bottom Sheet Dialogs using Android Studio | Engineering Education (EngEd) Program | Section](https://www.section.io/engineering-education/bottom-sheet-dialogs-using-android-studio/)

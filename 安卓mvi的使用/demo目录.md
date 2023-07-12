为了帮助你轻松访问你的模拟，包括`mavericks-launcher`工件并使用`MavericksLauncherActivity`。这个活动会自动聚合你的App中所有可模拟的Mavericks视图，并将它们和它们的可用模拟一起显示。

![mock_launcher_home.png](https://upload-images.jianshu.io/upload_images/24860325-84fe1ba15c123061.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![mock_launcher_detail_page.png](https://upload-images.jianshu.io/upload_images/24860325-ce9279ab77bcc24e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![mock_launcher_opened_fragment.png](https://upload-images.jianshu.io/upload_images/24860325-5d8ccb56482a5bde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


你可以选择一个片段并进行模拟，它将在一个新的活动中被打开。

要访问这个启动器活动，请调用`MavericksLauncherActivity.show(context)'。

## 自定义

默认情况下，这将在一个普通的活动中加载每个片段，但如果你的片段希望有一个特定的主机活动，你可以通过设置`MavericksLauncherMockActivity.activityToShowMock`到你想要的任何活动类来定制它们被启动的活动。

如果你这样设置一个自定义活动，它应该使用`MavericksLauncherMockActivity.showNextMockFromActivity`函数来显示模拟的视图，一旦活动被创建。比如说

```kotlin
class LauncherActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        if (savedInstanceState == null) {
                MavericksLauncherMockActivity.showNextMockFromActivity(
                    activity = this,
                    showView = { mavericksView ->
                       // Use whatever custom code you want to show your Mavericks View
                     }
                )
        }
    }
}
```

## 从命令行打开
MavericksLauncherActivity是导出的，所以你可以通过命令行的adb打开它。

`adb shell am start -n com.your.application.package/com.airbnb.mvrx. launcher.MavericksLauncherActivity`。
把com.your.application.package改成你的应用程序的包即可。

支持两个用于定制的字符串插件：

`viewNameToOpen` - 按名称打开一个特定的mavericks视图或mock。这不需要完全匹配，包含这个字符串的视图的第一个FQN将被打开。如果没有找到匹配的视图，第一个匹配的mock名称将被打开。匹配是不分大小写的。

`adb shell am start -n com.airbnb.sample/com.airbnb.mvrx. launcher.MavericksLauncherActivity --es viewNameToOpen MyFragment`。

`viewNamePatternToTest` - 匹配所有名称包含此字符串的视图和模拟。为它们启动一个测试流程，以检查它们是否能打开而不崩溃。匹配是不区分大小写的。

`adb shell am start -n com.airbnb.sample/com.airbnb.mvrx. launcher.MavericksLauncherActivity --es viewNamePatternToTest com_my_package`。

###多个字词
如果你想在模式中传递多个单词，你需要用引号来包裹参数。你可能需要引用完整的命令，这样才能发挥作用

`adb shell "am start -n com.airbnb.sample/com.airbnb.mvrx. launcher.MavericksLauncherActivity --es viewNameToOpen 'Default state'"`。

你可能也会发现在你的bash配置文件中定义函数是很有帮助的，这样可以更容易地调用这些函数。

```
# 打开启动器
function mavericks_launcher() {
  adb shell am start -n com.your.application.package/com.airbnb.mvrx.launcher.MavericksLauncherActivity
}

# 测试符合给定模式的片段和mock。
# 即：mavericks_test my_fragment
function mavericks_test() {
  adb shell am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNamePatternToTest $1
}


# 测试应用程序中的所有mock
function mavericks_test_all() {
  adb shell am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNamePatternToTest ""
}

# 打开第一个符合模式的片段
# 即：mavericks_open MyFragment
function mavericks_open() {
  adb shell am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNameToOpen $1
}
```

### Jetpack导航支持

默认情况下，启动器在为jetpack导航配置的应用程序中不工作，因为它不知道如何用你的导航图启动一个片段。你可以用你的导航图定义一个自定义的活动，代替启动器使用。请看mvrx示例模块中的LauncherActivity的例子。

## 从启动器中自动测试模拟对象

在Launcher活动中，Fragment工具条上有一个 "Test All "按钮，点击该按钮后，将打开每个系列的mock，检查初始化时的崩溃情况。

这提供了一个理智的检查，即每个模拟可以在没有崩溃的情况下被打开和加载。

默认情况下，它会在 "MavericksLauncherTestMocksActivity "活动中打开每个模拟，但这一行为可以通过为 "MavericksLauncherTestMocksActivity.provideIntentToTestMock "设置一个自定义意图来改变。

这所提供的测试是相当天真和基本的，但可以通过未来的工作来扩展。

## 注意事项
由于每个被模拟的片段都是独立启动的，它必须能够独立运作，而不需要假设任何其他片段或活动的存在。例如，它不能假设它的父片段或父活动是什么。

这意味着，如果你需要片段间的通信，你不能假设另一个片段存在，而是必须使用解耦的方法，如Dagger注入的类。

请注意，同样的限制适用于在自动测试套件中运行的片段，所以所有Mavericks视图都应该遵循这一点。

## 它是如何工作的
Mavericks检查应用程序的Dex文件，以找到所有可模拟的Mavericks视图以及它们声明的模拟状态和参数。这个过程可能需要一些时间，所以Fragment和mock信息被保存在共享偏好中，以便在随后的应用启动中更快地加载。

当一个可模拟的视图被选中显示时，所选择的模拟状态将被应用到它身上（同样，使用与测试中相同的技术）。这是通过一个模拟的状态存储来完成的，它允许我们强制一个特定的状态，同时阻断对设置状态的外部调用。这可以防止正常的ViewModel或Fragment的初始化覆盖模拟状态。

如果Fragment使用了一个现有的ViewModel，那么我们就覆盖它，使之像activityViewModel一样。这是必要的，因为我们综合地跳到了一个流程的中间，所以之前的视图模型不会存在。

在片段初始化时启动的网络（或其他异步操作）被阻断，这样它们就不会覆盖强制模拟状态。一旦初始化结束，模拟的ViewModel状态存储被切换到一个功能存储，这样屏幕就可以像正常一样被交互，并且允许未来因用户交互而进行的状态更新。
To help you easily access your mocks, include the `mavericks-launcher` artifact and use `MavericksLauncherActivity`. This activity automatically aggregates all mockable Mavericks views in your App and displays them with their available mocks.

![Launcher Home](/images/mock_launcher_home.png) ![Launcher Detail Screen](/images/mock_launcher_detail_page.png) ![Fragment opened from launcher](/images/mock_launcher_opened_fragment.png)

You can select a Fragment and mock and it will be opened in a new Activity.

To access this Launcher Activity, call `MavericksLauncherActivity.show(context)`.

## Customization

By default this loads each Fragment in a plain Activity, but if your Fragments expect a specific host Activity you can customize the Activity they are launched in by setting `MavericksLauncherMockActivity.activityToShowMock` to whichever Activity class you want.

If you set a custom activity this way it should use the `MavericksLauncherMockActivity.showNextMockFromActivity` function to display the mocked view once the Activity is created. For example:

```kotlin
class LauncherActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        if (savedInstanceState == null) {
                MavericksLauncherMockActivity.showNextMockFromActivity(
                    activity = this,
                    showView = { mavericksView ->
                       // Use whatever custom code you want to show your Mavericks View
                     }
                )
        }
    }
}
```

## Opening from command line
MavericksLauncherActivity is exported, so you can open it via adb on the command line.

`adb shell am start -n com.your.application.package/com.airbnb.mvrx.launcher.MavericksLauncherActivity`
Just change com.your.application.package to the package of your application.

There is support for two String extras for customization:

`viewNameToOpen` - Open a specific mavericks view or mock by name. This doesn't have to match exactly, the first FQN of a view that contains this string will be opened. If a matching view is not found, the first mock name to match is opened. Matching is case insensitive.

`adb shell am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNameToOpen MyFragment`

`viewNamePatternToTest` - Matches all views and mocks whose name contains this string. A test flow is started for them to check that they can open without crashing. Matching is case insensitive.

`adb shell am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNamePatternToTest com_my_package`

### Multiple Words
If you would like to pass multiple words in the pattern, you need to wrap the arguments in quotes. You may need to quote the full command for this to work

`adb shell "am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNameToOpen 'Default state'"`

You may also find it helpful to define functions in your bash profile to make it easier to call these.

```
# Open the launcher
function mavericks_launcher() {
  adb shell am start -n com.your.application.package/com.airbnb.mvrx.launcher.MavericksLauncherActivity
}

# Test fragments and mocks that match the given pattern.
# ie: mavericks_test my_fragment
function mavericks_test() {
  adb shell am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNamePatternToTest $1
}

# Test all mocks in the app
function mavericks_test_all() {
  adb shell am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNamePatternToTest ""
}

# Open the first fragment that matches the pattern
# ie: mavericks_open MyFragment
function mavericks_open() {
  adb shell am start -n com.airbnb.sample/com.airbnb.mvrx.launcher.MavericksLauncherActivity --es viewNameToOpen $1
}
```

### Jetpack Navigation Support

By default the launcher does not work in an app configured for jetpack navigation, since it cannot know how to start a fragment with your navigation graph. You can define a custom activity with your nav graph that the launcher will use instead. See LauncherActivity in the mvrx sample module for an example.

## Automated testing of mocks from the launcher

From the Launcher activity a "Test All" button is available in the Fragment toolbar that, when clicked, will open each mock in series to check for crashes upon initialization.

This offers a sanity check that each mock can be opened and loaded without crashes.

By default this opens each mock in the `MavericksLauncherTestMocksActivity` activity, but that behavior can be changed via setting a custom intent for `MavericksLauncherTestMocksActivity.provideIntentToTestMock`

The testing offered by this is fairly naive and basic, but could be expanded with future work.

## Caveats
Since each mocked fragment is launched in isolation it must be able to function independently, without assuming any other fragments or activities are present. For example, it must not make assumptions about what its parent fragment or parent activity is.

This means that if you need inter-fragment communication you cannot assume another fragment exists, but instead must use decoupled approaches, such as a Dagger injected class.

Note that the same restriction applies for fragments being run in automatic test suites, so all Mavericks views should follow this.

## How It Works
Mavericks inspects the app Dex files to find all mockable Mavericks views and their declared mock states and arguments. This process can take some time, so Fragment and mock information is saved in shared preferences for faster loading on subsequent app starts.

When a mockable view is selected for display the chosen mock state is applied to it (again, using the same technique as in tests). This is done via a mocked State Store which allows us to force a certain state, while blocking external calls to set state. This prevents normal ViewModel or Fragment initialization from overriding the mock state.

If the Fragment uses an existingViewModel then we override that to instead act like activityViewModel. This is necessary since we synthetically jumped into the middle of a flow so previous view models won't exist.

Network (or other Async operations) that are started on fragment initialization are blocked so that they don't override the forced mock state. Once initialization is over the mocked ViewModel state store is switched to a functional store so that the screen can be interacted with like normal, and future state updates due to user interaction are allowed.

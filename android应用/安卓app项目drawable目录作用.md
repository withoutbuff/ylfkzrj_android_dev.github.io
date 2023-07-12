可绘制资源是图形的一般概念，可以绘制到屏幕上，您可以使用getDrawable（int）等API检索，也可以应用到另一个具有“android:drawable”和“android:icon”等属性的XML资源。有几种不同类型的抽屉：



###位图文件



创建BitmapDrawable



创建NinePatchDrawable




一个Drawable，用于管理一系列其他Drawable。这些是按数组顺序绘制的，因此索引最大的元素将绘制在顶部。创建





一个XML文件，它为不同的状态引用不同的位图图形（例如，在按下按钮时使用不同的图像）。创建StateListDrawable



###级别列表



一个XML文件，用于定义一个可绘制文件，该文件管理多个备用可绘制文件（每个可绘制文件都分配了一个最大数值）。创建LevelListDrawable



###可绘制过渡



一个XML文件，用于定义可在两个可绘制资源之间交叉淡入淡出的可绘制资源。创建可过渡绘制



###嵌入式可绘制



一个XML文件，用于定义一个可绘制对象，并将另一个可绘图对象插入指定的距离。当视图需要比视图的实际边界小的背景绘制时，这很有用。



###可绘制的夹子



一个XML文件，它定义了一个可绘制文件，并根据该可绘制文件的当前级别值剪裁另一个可绘图文件。创建ClipDrawable



###可绘制比例

一个XML文件，该文件定义了一个可绘制文件，该可绘制文件根据其当前级别值更改另一个可绘图文件的大小。创建可缩放绘制



###可绘制形状



一个XML文件，用于定义几何形状，包括颜色和渐变。创建GradientDrawable



另请参阅动画资源文档，了解如何创建AnimationDrawable


A drawable resource is a general concept for a graphic that can be drawn to the screen and which you can retrieve with APIs such as getDrawable(int) or apply to another XML resource with attributes such as `android:drawable` and `android:icon`. There are several different types of drawables:

###Bitmap File

Creates a BitmapDrawable

Creates a NinePatchDrawable


A Drawable that manages an array of other Drawables. These are drawn in array order, so the element with the largest index is be drawn on top. Creates 



An XML file that references different bitmap graphics for different states (for example, to use a different image when a button is pressed). Creates a StateListDrawable

###Level List

An XML file that defines a drawable that manages a number of alternate Drawables, each assigned a maximum numerical value. Creates a LevelListDrawable

###Transition Drawable

An XML file that defines a drawable that can cross-fade between two drawable resources. Creates a TransitionDrawable

###Inset Drawable

An XML file that defines a drawable that insets another drawable by a specified distance. This is useful when a View needs a background drawable that is smaller than the View's actual bounds.

###Clip Drawable

An XML file that defines a drawable that clips another Drawable based on this Drawable's current level value. Creates a ClipDrawable

###Scale Drawable
An XML file that defines a drawable that changes the size of another Drawable based on its current level value. Creates a ScaleDrawable

###Shape Drawable

An XML file that defines a geometric shape, including colors and gradients. Creates a GradientDrawable

Also see the Animation Resource document for how to create an AnimationDrawable

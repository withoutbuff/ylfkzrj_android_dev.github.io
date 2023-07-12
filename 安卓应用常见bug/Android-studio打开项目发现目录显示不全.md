有的时候，我们打开一个新的项目，会出现项目目录显示不全的问题，明明目录里面有代码，而且Android studio也能正确识别出这个目录是项目的工作目录。
如下图所示，项目代码显示不出来，只能显示一些项目配置文件
![目录不全](https://upload-images.jianshu.io/upload_images/24860325-be7d3498fe787e84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

原因是`.idea`文件夹里面的配置文件 modules.xml 损坏了
这个时候需要我们找到项目目录，**手动删除目录下的.idea**
删除后重新打开项目，我们可以正常看到项目目录和代码了

`.idea`文件夹是隐藏目录，需要手动在文件浏览器中勾选**显示隐藏文件夹**

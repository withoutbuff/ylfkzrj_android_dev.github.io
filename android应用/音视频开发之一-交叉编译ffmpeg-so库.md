# 音视频开发之一-交叉编译ffmpeg so库

## 1. ffmpeg简介

ffmpeg是一个开源的音视频处理框架，它提供了丰富的功能和接口，可以实现音视频的编解码、转换、滤镜、播放、推流等操作。ffmpeg由以下几个主要组件构成：

- libavcodec：一个包含了各种音视频编解码器的库，支持多种格式和协议。
- libavformat：一个负责封装和解封装音视频数据的库，可以处理多种容器格式。
- libavfilter：一个负责对音视频进行各种滤镜处理的库，例如裁剪、旋转、水印等。
- libavdevice：一个提供了访问捕获和渲染设备的接口的库，例如摄像头、麦克风、屏幕等。
- libavutil：一个包含了一些通用的工具函数和结构体的库，例如日志、错误码、内存管理等。
- libswscale：一个负责对图像进行缩放和色彩转换的库。
- libswresample：一个负责对音频进行重采样和混合的库。

## 2. ffmpeg源码下载地址

ffmpeg是一个开源项目，它的源码托管在GitHub上，可以通过以下命令克隆到本地：

```bash
git clone https://github.com/FFmpeg/FFmpeg.git
```

也可以直接从GitHub上下载zip压缩包或者tar.gz压缩包。

## 3. ffmpeg的编译命令

为了在安卓平台上使用ffmpeg，我们需要使用NDK（Native Development Kit）工具链来交叉编译出适用于不同CPU架构（如armeabi-v7a, arm64-v8a, x86, x86_64等）的so（shared object）库文件。NDK工具链提供了gcc或clang两种编译器来进行交叉编译。

ffmpeg本身提供了一个configure脚本来配置编译选项，并生成makefile文件。我们可以通过在终端中运行以下命令来查看configure脚本支持的所有选项：

```bash
./configure --help
```

其中比较重要的选项有：

- --prefix：指定生成文件的输出目录，默认为当前目录下的install文件夹。
- --enable-shared：指定生成动态链接库（so文件），默认为否。
- --disable-static：指定不生成静态链接库（a文件），默认为否。
- --enable-small：指定优化生成文件大小，可能会牺牲一些性能，默认为否。
- --disable-all：指定禁用所有组件，默认为否。
- --enable-gpl：指定启用GPL许可证下的组件，默认为否。
- --enable-nonfree: 指定启用非自由组件，默认为否。
- --enable-libx264: 指定启用libx264编码器，默认为否。
- --enable-libx265: 指定启用libx265编码器，默认为否。

除此之外，还有一些针对安卓平台特有的选项：

- --target-os=android: 指定目标操作系统为安卓，默认为linux。
- --cross-prefix: 指定交叉编译器的前缀，例如arm-linux-androideabi-。
- --arch: 指定目标CPU架构，例如arm, arm64, x86, x86_64等。
- --cpu: 指定目标CPU型号，例如cortex-a8, cortex-a53等。
- --sysroot: 指定系统根目录，通常为NDK中的platforms目录下对应的API级别和架构的目录。
- --extra-cflags: 指定额外的C编译器参数，例如-I, -D等。
- --extra-ldflags: 指定额外的链接器参数，例如-L, -l等。

根据不同的需求和配置，我们可以组合出不同的configure命令来编译ffmpeg。例如，以下命令可以编译出一个支持x264和x265编码器，并且适用于arm64-v8a架构的ffmpeg库：

```bash
./configure \
--prefix=install/arm64-v8a \
--enable-shared \
--disable-static \
--enable-small \
--disable-all \
--enable-gpl \
--enable-nonfree \
--enable-libx264 \
--enable-libx265 \
--target-os=android \
--cross-prefix=aarch64-linux-android- \
--arch=aarch64 \
--cpu=cortex-a53 \
--sysroot=$NDK/platforms/android-21/arch-arm64/ \ 
--extra-cflags="-I$NDK/sysroot/usr/include/aarch64-linux-android -I$X264/include -I$X265/include" \ 
--extra-ldflags="-L$X264/lib -L$X265/lib"
```

其中"\$NDK"是NDK工具链所在的目录，"\$X264"和X265是"\$libx264"和libx265库所在的目录。

运行configure命令后，会生成一个config.h文件和一个makefile文件。我们可以通过运行以下命令来查看config.h文件中定义了哪些宏³：

```bash
cat config.h | grep '#define'
```

我们可以通过运行以下命令来查看makefile文件中定义了哪些变量和规则：

```bash
cat makefile | grep '='
```

如果没有出现错误信息，则说明configure成功。接下来我们可以通过运行以下命令来开始编译过程：

```bash
make -j4
```

其中-j4表示使用4个线程进行并行编译，可以加快速度。

如果没有出现错误信息，则说明make成功。接下来我们可以通过运行以下命令来将生成的文件复制到指定的输出目录：

```bash
make install
```

这样就完成了一次交叉编译过程。我们可以重复上述步骤，并修改相应的选项来针对不同的CPU架构进行交叉编译。

## 4. ffmpeg的编译脚本

为了方便管理和重用ffmpeg的交叉编译过程，我们可以将上述步骤封装成一个脚本文件，并传入一些参数来控制不同的选项。以下是一个示例脚本：

```bash
#!/bin/bash

# 定义变量
NDK=/home/ndk/android-ndk-r21d # NDK工具链所在目录，请根据实际情况修改
TOOLCHAIN=$NDK/toolchains/llvm/prebuilt/linux-x86_64 # 工具链子目录，请根据实际情况修改
API=21 # API级别，请根据实际情况修改

function build_android {
echo "Compiling FFmpeg for $CPU"
./configure \
--prefix=$PREFIX \
--enable-shared \
--disable-static \
--disable-doc \
--disable-ffmpeg \
--disable-ffplay \
--disable-ffprobe \
--disable-symver \
--cross-prefix=$CROSS_PREFIX \
--target-os=android \
--arch=$ARCH \
--cpu=$CPU \
--cc=$CC \ 
--cxx=$CXX \ 
--enable-cross-compile \ 
--sysroot=$SYSROOT \ 
$ADDITIONAL_CONFIGURE_FLAG
make clean
make -j4
make install
echo "The Compilation of FFmpeg for $CPU is completed"
}

# armv8-a
ARCH=arm64
CPU=armv8-a
CC=$TOOLCHAIN/bin/aarch64-linux-android$API-clang
CXX=$TOOLCHAIN/bin/aarch64-linux-android$API-clang++
SYSROOT=$NDK/toolchains/llvm/prebuilt/linux-x86_64/sysroot # 系统根目录，请根据实际情况修改
CROSS_PREFIX=$TOOLCHAIN/bin/aarch64-linux-android-
PREFIX=$(pwd)/android/$CPU # 输出目录，请根据实际情况修改
ADDITIONAL_CONFIGURE_FLAG=
build_android

# x86_64
ARCH=x86_64
CPU=x86-64
CC=$TOOLCHAIN/bin/x86_64-linux-android$API-clang 
CXX=$TOOLCHAIN/bin/x86_64-linux-android$API-clang++ 
SYSROOT=$NDK/toolchains/llvm/prebuilt/linux-x86_64/sysroot # 系统根目录，请根据实际情况修改
CROSS_PREFIX=$TOOLCHAIN/bin/x86_64-linux-android-
PREFIX=$(pwd)/android/$CPU # 输出目录，请根据实际情况修改 
ADDITIONAL_CONFIGURE_FLAG= --enable-asm --enable-yasm --disable-mmx --disable-inline-asm 
build_android

# 其他架构可以类似地添加和修改参数
```
这个脚本定义了一个build_android函数，用来执行configure, make和make install命令，并传入不同的参数。然后针对不同的CPU架构，定义了一些变量，并调用build_android函数。这样就可以一次性编译出多个架构的ffmpeg库。

我们可以将这个脚本保存为一个文件，例如build_ffmpeg.sh，并赋予可执行权限。然后在终端中运行以下命令来开始编译过程：
```
./build_ffmpeg.sh
```
如果没有出现错误信息，则说明编译成功。我们可以在输出目录下找到生成的ffmpeg库文件。

##5. ffmpeg的使用方法
在Android Studio中，我们可以通过以下步骤来引用和使用ffmpeg库：

在app模块下创建一个jniLibs文件夹，并将生成的ffmpeg库文件复制到对应的子文件夹中，例如arm64-v8a, x86_64等。
在app模块下创建一个src/main/cpp文件夹，并在其中创建一个native-lib.cpp文件，用来编写调用ffmpeg库函数的本地代码。
在app模块下创建一个src/main/java/com/example/ffmpegdemo/MainActivity.java文件，用来编写调用本地代码的Java代码。
在app模块下创建一个CMakeLists.txt文件，用来配置NDK相关参数。
在app模块下打开build.gradle文件，在defaultConfig节点下添加以下内容：
```
externalNativeBuild {
    cmake {
        cppFlags ""
    }
}
ndk {
    abiFilters 'arm64-v8a', 'x86_64' // 根据需要选择支持的架构，请与jniLibs中保持一致 
}
```
在app模块下打开build.gradle文件，在android节点下添加以下内容：
```
externalNativeBuild {
    cmake {
        path "CMakeLists.txt" // 指定CMakeLists.txt所在路径，请根据实际情况修改
    }
}
```
在native-lib.cpp文件中，添加以下内容：
```
#include <jni.h>
#include <string>
#include <android/log.h>

// 引入ffmpeg头文件
extern "C" {
#include "libavcodec/avcodec.h"
#include "libavformat/avformat.h"
#include "libavutil/log.h"
}

#define LOG_TAG "FFmpegDemo"
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR, LOG_TAG, __VA_ARGS__)

// 定义一个本地函数，用来打印ffmpeg版本信息
extern "C" JNIEXPORT jstring JNICALL
Java_com_example_ffmpegdemo_MainActivity_getFFmpegVersion(
        JNIEnv* env,
        jobject /* this */) {
    std::string version = av_version_info();
    LOGE("FFmpeg version: %s", version.c_str());
    return env->NewStringUTF(version.c_str());
}
```
在MainActivity.java文件中，添加以下内容：
```
package com.example.ffmpegdemo;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    // 加载本地库文件
    static {
        System.loadLibrary("native-lib");
    }

    // 声明本地函数，用来调用本地代码
    public native String getFFmpegVersion();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 获取文本视图对象，并设置文本为ffmpeg版本信息
        TextView tv = findViewById(R.id.sample_text);
        tv.setText(getFFmpegVersion());
    }
}
```
在CMakeLists.txt文件中，添加以下内容：
```
cmake_minimum_required(VERSION 3.4.1)

# 设置变量，指定ffmpeg库所在目录，请根据实际情况修改 
set(distribution_DIR ${CMAKE_SOURCE_DIR}/../../../../jniLibs)

# 添加子目录，指定源码所在目录，请根据实际情况修改 
add_subdirectory(${distribution_DIR}/${ANDROID_ABI} ${CMAKE_CURRENT_BINARY_DIR}/ffmpeg)

# 添加本地库，指定源码所在文件，请根据实际情况修改 
add_library( native-lib SHARED src/main/cpp/native-lib.cpp )

# 查找系统库，并赋值给变量 
find_library( log-lib log )

# 链接本地库和系统库 
target_link_libraries( native-lib ffmpeg ${log-lib} )
```
同步项目并运行应用，在模拟器或真机上查看效果。如果一切正常，应该可以看到文本视图显示了ffmpeg的版本信息，并且在Logcat中也可以看到相应的日志输出。
以上就是一个简单的示例，展示了如何在Android Studio中引用和使用ffmpeg库。我们可以根据不同的需求，在native-lib.cpp文件中编写更多的调用ffmpeg库函数的代码，并在MainActivity.java文件中编写更多的调用本地代码的代码。例如，我们可以实现视频播放器、视频编辑器、视频转码器等功能。




### 安装环境

下载sdk: https://developer.android.com/studio/command-line/sdkmanager

1. 从 Android Studio 下载页面中下载最新的“command line tools only”软件包，然后将其解压缩。
2. 将解压缩的 cmdline-tools 目录移至您选择的新目录，例如 android_sdk。这个新目录就是您的 Android SDK 目录。
3. 在解压缩的 cmdline-tools 目录中，创建一个名为 latest 的子目录。
4. 将原始 cmdline-tools 目录内容（包括 lib 目录、bin 目录、NOTICE.txt 文件和 source.properties 文件）移动到新创建的 latest 目录中。现在，您就可以从这个位置使用命令行工具了。


列出可选项
```
.\sdkmanager.bat --list
```

安装 ndk:
```
.\sdkmanager.bat --install 'platform-tools'
.\sdkmanager.bat --install 'ndk;25.1.8937393'
.\sdkmanager.bat --install 'build-tools;33.0.1'
.\sdkmanager.bat --install 'emulator'
.\sdkmanager.bat --install 'platforms;android-33'
.\sdkmanager.bat --install 'system-images;android-33;google_apis;x86_64'
.\sdkmanager.bat --install 'system-images;android-33;google_apis;arm64-v8a'
```

创建模拟器
```
.\avdmanager.bat --help
.\avdmanager.bat list
.\avdmanager.bat create avd -d 45 -n android -k 'system-images;android-33;google_apis;x86_64' -f
```

启动模拟器 $sdk/emulator 目录下
```
emulator -list-avds
emulator -avd android -netdelay none -netspeed full -memory 2048
```

### 不用IDE创建android项目

参考这个文档来手动创建项目即可，本项目的目录结构与文档的不一样

https://developer.android.com/studio/build


构建apk
```sh
./gradlew assembleDebug
```

安装引用或者调式. 或者也可以使用 adb install 进行安装
```
./gradlew installDebug
```

### 安卓ndk开发

https://developer.aliyun.com/article/781762

通过 ndk-build

创建 Android.mk 和 Application.mk 文件。
新建一个项目，在 app 目录下（任目录下都可以）新建 jni 文件，添加 Android.mk 和 Application.mk 文件，以及 com_haohao_hellojni_MyJNI.h 文件（运用上一小节的方法生成）。

Android.mk
```Makefile
LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)

# 要生成的.so库名称。java代码System.loadLibrary("hello");加载的就是它
LOCAL_MODULE := hello

# C++文件
LOCAL_SRC_FILES := hello.cpp

include $(BUILD_SHARED_LIBRARY)
```

Application.mk
```Makefile
# 不写 APP_ABI 会生成全部支持的平台,目前支持：armeabi arm64-v8a armeabi-v7a
# APP_ABI := armeabi arm64-v8a armeabi-v7a mips mips64 x86 x86_64
APP_ABI := armeabi arm64-v8a armeabi-v7a
```


生成 .so 文件。
在 jni 目录下（配置好NDK环境变量）直接执行 ndk-build ， 生成 .so 文件。


配置项目工程。
在 main 目录下新建 jniLibs 目录，并拷贝 armeabi arm64-v8a armeabi-v7a 文件夹，运行 proj 。

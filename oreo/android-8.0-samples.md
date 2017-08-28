# 代码示例
使用下面的示例代码来了解Android 8.0的兼容性和APIs。要在Android Studio中下载示例代码，选择**文件->新建->导入示例**菜单选项。
> **注意：**这些可下载的项目被设计为使用Gradle和Android Studio来操作。

### 通知渠道
**NotificationChannels示例** - Android 8.0添加了通知渠道支持，允许应用将通知组织到专题的分类。每个分类可以拥有自己的提醒风格，同时用户能够按照自己的兴趣选择开启或关闭某一分类。此示例展示如何创建渠道及合适地发起通知。

从GitHub获取：[Java](https://github.com/googlesamples/android-NotificationChannels/) | [Kotlin](https://github.com/googlesamples/android-NotificationChannels/tree/master/kotlinApp)

### 自动填充框架
**AutofillFramework示例** - 本示例展示了Android 8.0引入的自动填充框架的使用。它包含了一个需要被自动填充的客户Activities的实现及为此客户Activities提供自动填充数据的服务。

### 画中画模式
**PictureInPicture示例** - 本示例展示手持设备上画中画模式的基本使用。本示例播放了一则视频。当应用切换到画中画模式，视频会继续保持播放。在画中画屏幕界面上，应用显示一个动作项用于控制暂停和恢复视频。

从GitHub获取：[Java](https://github.com/googlesamples/android-PictureInPicture/) | (Kotlin)(http://github.com/googlesamples/android-PictureInPicture/tree/master/kotlinApp)

### 可下载的字体
**DownloadableFonts** - 本示例展示如何使用Android 8.0引入的可下载字体功能。可下载字体允许应用程序从某个Provider请求特定的字体，而不是一起打包字体或者自己下载。这意味着不比再单独地将字体作为asset打包。

从GitHub获取：[Java](http://github.com/googlesamples/android-DownloadableFonts/) | [Kotlin](http://github.com/googlesamples/android-DownloadableFonts/tree/master/kotlinApp)

**EmojiCompat** - 本示例展示Emoji兼容支持库的使用。你可以使用这个库阻止应用以□的形式显示不存在的emoji表情。你可以使用随应用打包的或者下载的emoji字体。本示例展示了两者的使用方式。

从GitHub获取：[Java](http://github.com/googlesamples/android-EmojiCompat/) | [Kotlin](http://github.com/googlesamples/android-EmojiCompat/tree/master/kotlinApp)

### 后台执行限制
**Bluetooth Advertisements示例** - Bluetooth Advertisements示例被更新以适配Android 8.0的后台执行限制。此示例之前版本创建了一个后台服务用以g广播蓝牙低功耗信号，此进程现在以前台服务方式启动以确保执行。

从GitHub获取：[Java](https://github.com/googlesamples/android-BluetoothAdvertisements/)

### 后台位置限制
**LocationUpdatesPendingIntent示例** - 展示了如何使用`PendingIntent`来请求位置更新。对于目标是Android 7.0但是运行在Android 8.0上的应用，开发者可以使用`PendingIntent.getService()`或者`PendingIntent.getBroadcast()`。对于目标是Android 8.0的应用，由于后台启动服务的限制，`PendingIntent.getService()`将不能工作。当目标设备是Android 8.0，开发者应该使用`PendingIntent.getBroadcast()`。

从GitHub获取：[Java](https://github.com/googlesamples/android-play-location/tree/master/LocationUpdatesPendingIntent/)

**LocationUpdatesForegroundService示例** - 展示当应用Activities不可见时，如果使用前台服务获取位置更新。对于运行在Android 8.0上的应用（即使目标是Android 7.0（API Level 24）或者更低），后台更新被限制为每小时几次。使用前台服务是一种获取更频繁更新的方法。

从GitHub获取：[Java](https://github.com/googlesamples/android-play-location/tree/master/LocationUpdatesForegroundService/)

### AAudio
**AAudio Echo示例** - AAudio是一个新的NDK API接口，它使得一些高级的音频应用程序能够在支持的设备访问低延迟的音频。本示例展示如何创建输入和输出流、配置循坏播放。

从GitHub获取：[C++](https://github.com/googlesamples/android-audio-high-performance/tree/master/aaudio)

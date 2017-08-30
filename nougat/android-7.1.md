# Android 7.1于开发者
Android 7.1更新为用户和开发者引入了各种新功能和特性。本文档重点介绍对开发者而言的新特性。

### <a id="shortcuts"></a>应用程序快捷方式
您可以使用新的快捷方式功能直接将用户从启动器带入到您的应用的核心操作。用户只需长按您的应用程序的启动器图标即可显示应用程序的快捷方式，然后轻点快捷方式便可跳转到相关操作。这些快捷方式是吸引用户的好方法，它们可在用户启动您的应用程序之前展示您的应用程序的功能。

每个快捷方式关联一个intent，每个intent启动一个特定的操作或任务，您可以为任何可以用intent表示的操作创建一个快捷方式。例如，您可以创建发送新短信，预约，播放视频，继续游戏，加载地图位置等intent。

您可以通过将快捷方式添加到APK中的资源文件的方式来静态创建应用程序的快捷方式，也可以在运行时动态添加。静态快捷方式是通用操作的理想选择，动态快捷方式可让您根据用户的偏好，行为，位置等突出某些操作。您可以在每个应用程序中最多提供五个快捷方式。但请注意，某些启动器应用程序不会显示您为应用程序注册的每个快捷方式。

在您的应用程序添加快捷方式后，这些快捷方式便可以在任何支持它们的启动器上使用，例如Pixel启动器（Pixel设备上的默认启动器），Now启动器（Nexus设备上的默认启动器）以及其他提供支持的启动器。

任何应用程序都可以创建快捷方式，任何启动器应用都可以添加对快捷方式的支持。Android 7.1为应用程序注册快捷方式和启动器读取注册的快捷方式提供了API。有关详细信息，请参阅[应用程序快捷方式开发者文档](https://developer.android.com/guide/topics/ui/shortcuts.html)。
![](images/android-7.1/shortcuts.png)

### 图像键盘支持
用户经常想与emojis表情，贴纸和其他各种富内容进行通信。在以前的Android版本中，软键盘（也称为[输入法编辑器](https://developer.android.com/guide/topics/text/creating-input-method.html)或IME）只能向应用程序发送unicode emojis表情。对于富内容，应用程序或者必须构建无法在其他应用程序中使用的应用程序相关的emoji表情，或者使用变通方法，如通过[轻松共享操作](https://developer.android.com/training/sharing/shareaction.html)或剪贴板发送图片。

现在在Android 7.1中，Android SDK包含了Commit Content API，它提供了一种通用方式，以便IME将图像和其他富内容直接发送到应用程序内的文本编辑器。该API也可在v13支持库25.0.0版本中使用。

使用此API，您可以构建从任何输入法接受富内容的短消息应用程序，以及可向任何应用程序发送富内容的输入法程序。有关详细信息，请参阅[图像键盘支持开发人员文档](https://developer.android.com/guide/topics/text/image-keyboard.html)。

### 新专业Emoji表情
在Android 7.1中，我们添加了新的表情符号，代表了更广泛的女性和男性职业。新的表情符号在我们现有的男性表情符号和女性表情符号之间带来平衡，并且有各种肤色。

如果您是输入法或即时通讯应用程序开发人员，您应该开始将这些表情符号纳入您的应用程序。您可以通过调用Paint.hasGlyph()动态检查新的emoji字符。
![](images/android-7.1/new-emoji-7.1_2x.png)

### 增强动态壁纸元数据
现在，您可以向任何壁纸预览组件（例如壁纸选择器应用程序）提供有关动态壁纸的元数据。您可以显示现有元数据属性，如标签，说明和作者以及上下文URL和标题的新属性，以将用户链接到有关壁纸的更多信息。

更多详情，请参阅[Android开发者博客](https://android-developers.blogspot.com/2016/10/android-71-developer-preview.html)。

### 圆形图标资源
应用程序现在可以定义圆形启动器图标，这些图标用在支持它们的设备上。当启动器请求应用程序图标时，framework将返回`android：icon`或`android：roundIcon`，具体取决于设备编译配置。因此，应用程序应该确保在响应启动器intent时同时定义`android：icon`和`android：roundIcon`资源。您可以使用[Image Asset Studio](https://developer.android.com/studio/write/image-asset-studio.html#access)来设计圆形图标。

您应该确保在新的支持圆形图标的设备上测试您的应用程序，查看您的圆形应用程序图标的外观及其显示方式。一种资源测试方式是运行使用Google API模拟器系统（目标API级别为25级）的[Android模拟器](https://developer.android.com/studio/run/emulator.html)。您还可以通过在Google Pixel设备上安装应用程序来测试图标。

有关设计应用启动器图标的详细信息，请参阅[材料设计指南](https://material.google.com/style/icons.html#icons-product-icons)。

### 存储管理Intent
应用程序现在可以启动`ACTION_MANAGE_STORAGE` intent将用户带到系统的**剩余空间**界面。例如，如果应用程序需要的空间大于当前可用空间，则可以使用此intent让用户删除不需要的应用程序和内容，以释放足够的空间。

### 改进的VR线程调度
Android 7.1提供了新功能来改善VR线程调度。这是非常有用的，因为虚拟现实应用程序对延迟非常敏感。

应用程序现在可以指定一个线程为VR线程。当应用程序处于[VR模式](https://developer.android.com/about/versions/nougat/android-7.0.html#vr)时，系统将更积极地调度线程以最小化延迟。一个进程同时只能有一个VR线程，并且系统可能会限制该线程的运行时间。当应用程序未处于VR模式时，此设置无效。

要将线程指定为VR线程，请调用新的`ActivityManager.setVrThread()`方法。

### 演示用户提示
应用程序现在可以检查设备是否作为演示用户运行。

应用程序可以调用新的`UserManager.isDemoUser()`方法来查看应用程序是否在演示用户沙箱中运行。这允许应用程序自定义潜在客户的初始体验。例如，当作为演示用户运行时，应用程序可能会为用户提供更多帮助，或者更详细地解释其功能。

### 运营商和电话应用程序可用API
系统现在为运营商和电话应用程序提供了新的电话功能，包括：
* 多终端呼叫
* CDMA语音隐私属性
* Visual Voicemail的源类型支持
* 运营商配置选项，用于管理视频电话

### 可穿戴设备新屏幕密度
Android现在对可穿戴设备支持几个新的屏幕密度，这些屏幕密度更匹配一些设备的物理规格。这样，您可以根据需要，针对要显示的屏幕对可穿戴设备应用程序中的图片做微调。

新的设备密度是：
* `DENSITY_260`
* `DENSITY_300`
* `DENSITY_340`

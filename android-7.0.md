# Android 7.0于开发者
Android 7.0牛轧糖为用户和开发者引入了各种新功能和特性。本文档重点介绍对开发者而言的新特性。

请务必查看[Android 7.0行为变更](#Android-7-0行为变更)，以了解平台变更对您的应用程序的影响。

要了解更多Android 7.0的用户功能，请访问[www.android.com](https://www.android.com)。

### 多窗口支持
Android 7.0中，我们向平台引入了一种全新且非常需要的多任务功能 -- 多窗口支持。

用户现在可以在屏幕上一次弹出打开两个应用程序。
- 在运行Android 7.0的手机和平板上，用户可以在分屏模式下以并列或者叠加的方式同时运行2个应用程序。用户可以通过拖拽应用程序之间的分隔线来调整应用程序的窗口大小。
- 在Android TV设备上，应用程式可以开启[画中画模式](https://developer.android.com/preview/features/picture-in-picture.html)，以便用户在浏览或与其他应用程序互动时，可以继续显示内容。

特别是在平板或者其他大屏设备上，多窗口支持为您提供了吸引用户的新方法。您甚至可以在您的应用程序中开启`拖放`功能以便用户可以很方便地拖拽内容到您的应用程序中或者从您的应用程序中拖拽内容 -- 一种绝佳的提升用户体验的方式。

在您的应用程序中添加多窗口支持以及配置它如何处理多窗口显示是很简单的。例如，您可以指定您的Activity的最小允许尺寸，防止用户将Activity大小调整为小于该尺寸。您也可以在您的应用程序中禁止多窗口显示，以确保系统只会以全屏模式展示您的应用程序。

更多信息请参阅[多窗口支持开发者文档](https://developer.android.com/preview/features/multi-window.html)。

### 通知增强
Android 7.0中，我们重新设计了通知功能，以使它更容易、方便使用。一些变更如下：
- **模板更新**：我们正在更新通知模板，以强调图片和头像。开发者做极小的代码调整就能利用新模板。
- **消息样式定制**：您可以使用`MessagingStyle`类来定制更多与通知相关的用户界面标签。您可以配置消息、会话标题和内容视图。
- **通知分组**：系统可以将消息分组在一起，例如按照消息主题显示分组。用户能够对一组通知执行操作，如清除或存档。如果您之前实现过Android Wear的通知，您可能已经熟悉这种模式了。
- **直接回复**：对于即时通讯应用程序，Android系统支持内联回复，以便用户可以在通知界面直接快速回复短信或者文字消息。
- **自定义视图**：当在通知中使用自定义视图时，2个新的APIs使您能够自定义系统装饰，如通知标题和操作。

要了解如何实现这些新特性，请参阅[通知](https://developer.android.com/preview/features/notification-updates.html)指南。

### 分析指导的JIT/AOT编译器
Android 7.0中，我们添加了一个包含针对ART代码分析的即时（JIT）编译器，它会随着Android应用程序的运行，不断提升应用程序的性能。JIT编译器是对ART当前AOT编译器的补充，有助于提高运行时性能，节省存储空间，加快应用程序和系统的更新速度。

分析指导的编译器允许ART能够根据每个应用程序的真实使用情况及设备状况来管理每个应用程序的JIT/AOT编译器。例如，ART分析并维护每个应用程序最常使用的方法，然后通过预编译或者缓存这些方法以获得最佳性能。它会留下应用程序的其他部分直到使用时才编译。

除了提高应用程序关键部分的性能，分析指导的编译器还能够帮助减少应用程序的整体内存占用，包括相关联的二进制文件。此功能在低内存设备上尤其重要。

ART以一种对设备电池影响最小的方式管理分析指导的编译器。它仅在设备空闲和充电时才进行预编译，从而通过提前完成那些工作节省时间和电量。

### 快速应用安装
ART即时编译器一个明显的好处是应用程序安装和系统更新的速度。即使是在Android 6.0上需要几分钟时间来优化和安装的大型应用程序现在也可以在几秒内装上。系统更新也更快，因为没有更多的优化步骤。

### 移动时Doze
Android 6.0引入了Doze，一种通过在设备空闲时（如闲置在桌上或抽屉里时）延迟应用程序的CPU和网络活动来节省电量的系统模式。

现在Android 7.0中，Doze更进一步，移动时也省电。任何时候屏幕关闭一段时间、设备不插电，Doze就会应用一部分熟悉的CPU和网络限制于应用程序。这意味着即使用户将设备放在口袋里也可以省电。

即使设备是使用电池，只要屏幕关闭一段时间，Doze就会限制网络访问、推迟任务和同步。在短暂的窗口维护期，应用程序才被允许访问网路和执行其他被推迟的任务。打开屏幕或插电会使设备退出Doze。

当设备静止、屏幕关闭并使用电池一段时间，Doze会在[PowerManager.WakeLock](https://developer.android.com/reference/android/os/PowerManager.WakeLock.html)，[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)警报和GPS/Wi-Fi扫描上应用完整的CPU和网络限制。

无论设备移动与否，应用程序适配Doze机制的最佳做法都是相同的，因此如果您已经更新了应用程序来优雅地处理Doze机制，那么您就已经设置好了。如果没有，请立即开始[应用程序适配Doze机制](https://developer.android.com/training/monitoring-device-state/doze-standby.html#assessing_your_app)。

### Svelte计划：后台优化
Svelte计划持续致力于最大限度地减少Android生态范围内各设备的系统和应用程序的内存使用。在Android 7.0中，Svelte计划专注于优化应用程序在后台的运行方式上。

后台处理是大多数应用程序的重要组成部分。处理得当，它能够带来惊艳的用户体验 -- 及时、快速和情景感知。处理不当，后台处理可能会消耗不必要内存（和电量），并影响其他应用程序和系统的性能。

从Android 5.0开始，[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)已成为一种以对用户有利的方式执行后台工作的首选方式。应用程序可以调度作业，同时允许系统根据内存、电源和连接条件进行优化。JobScheduler对此提供了控制和简化，我们希望所有的应用程序都使用它。

另一个好的选择是[GcmNetworkManager](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmNetworkManager)，Google Play服务的一部分，它提供了类似的作业调度功能，并能兼容低版本Android系统。

我们正继续扩展`JobScheduler`和`GCMNetworkManager`来满足更多的使用场景，如在Android 7.0中，您可以基于Content Provider的内容变动来安排后台工作。同时，我们开始弃用一些可能降低系统性能（尤其在低内存设备上）的旧模式。

Android 7.0中我们移除了3个常用的隐式广播：[CONNECTIVITY_ACTION](https://developer.android.com/reference/android/net/ConnectivityManager.html#CONNECTIVITY_ACTION)，[ACTION_NEW_PICTURE](https://developer.android.com/reference/android/hardware/Camera.html#ACTION_NEW_PICTURE)，[ACTION_NEW_VIDEO](https://developer.android.com/reference/android/hardware/Camera.html#ACTION_NEW_VIDEO)，因为它们可以一次唤醒多个应用程序的后台进程，同时消耗内存和电量。如果您的应用程序接收这些广播，利用Android 7.0迁移至`JobScheduler`和相关API。

请参阅[后台优化](https://developer.android.com/topic/performance/background-optimization.html)文档了解详细信息。

### SurfaceView
Android 7.0在[SurfaceView](https://developer.android.com/reference/android/view/SurfaceView.html)类中引入了同步移动。在某些情况下，它提供比[TextureView](https://developer.android.com/reference/android/view/TextureView.html)更好的电池性能，如当渲染视频或3D内容时，若应用程序滚动，或者对视频位置做动画，使用[SurfaceView](https://developer.android.com/reference/android/view/SurfaceView.html)比[TextureView](https://developer.android.com/reference/android/view/TextureView.html)消耗更少的电量。

[SurfaceView](https://developer.android.com/reference/android/view/SurfaceView.html)类带来更节电的屏幕合成，因为它在专用硬件中被合成，并与应用程序窗口内容合成分离。因此，它比[TextureView](https://developer.android.com/reference/android/view/TextureView.html)产生更少的中间产物。

[SurfaceView](https://dAPIseveloper.android.com/reference/android/view/SurfaceView.html)对象的内容位置现在与包含它的应用程序内容同步更新。 这种变化的一个结果是，对[SurfaceView](https://developer.android.com/reference/android/view/SurfaceView.html)中正在播放的视频做简单的转换或缩放，它不会随着视图的移动产生黑条。

从Android 7.0开始，我们强烈推荐您使用[SurfaceView](https://developer.android.com/reference/android/view/SurfaceView.html)替换[TextureView](https://developer.android.com/reference/android/view/TextureView.html)以节省功耗。

### 流量节省
在移动设备的生命周期里，蜂窝数据计划的成本通常超过设备本身的成本。对于许多用户，蜂窝数据是他们想要节省的昂贵资源。。

Android 7.0引入流量节省模式，这是一种新的系统服务，可帮助减少应用程序使用蜂窝数据，无论是漫游，接近结算周期结束还是小型的预付费数据包。流量节省让用户能控制应用程序如何使用蜂窝数据，并在流量节省启用时，让开发者能提供更有效的服务。当用户在设置中开启流量节省，并且设备连接到计费网络，系统会阻止后台数据的使用，并通知应用程序在前台尽可能使用较少的数据，如限制流式传输比特率，降低图片质量，延迟乐观预缓存等。用户可以将特定应用加入白名单，以便在流量节省开启时允许后台计费的数据使用。

Android 7.0通过扩展[ConnectivityManager](https://developer.android.com/reference/android/net/ConnectivityManager.html)向应用程序提供了一种获取用户流量节省设置和监听流量节省设置改变的方式。所有的应用程序都应该检查用户是否已启动流量节省模式，并尽力限制前台和后台的数据使用。

### Vulkan APIs
Android 7.0将[Vulkan™](http://www.khronos.org/vulkan)（一种新的3D渲染API）集成进了平台。和[OpenGL™ ES](https://www.khronos.org/opengles/)一样，Vulkan也是由Khronos Group维护的一个3D图形和渲染的开放标准。

Vulkan是从头开始设计的，以最大限度地减少驱动程序中的CPU开销，并允许您的应用程序更直接的控制GPU操作。Vulkan还通过允许多个线程一次执行诸如命令缓冲器构造的工作来实现更好的并行化。

Vulkan开发工具和库已经被打包进Android 7.0 NDK。 它们包括：
* 头文件
* 验证层（调试库）
* SPIR-V着色器编译器
* SPIR-V运行时着色器编译库

Vulkan仅适用于配备有Vulkan功能硬件（例如Nexus 5X，Nexus 6P和Nexus Player）的设备上的应用程序。 我们正在与合作伙伴紧密合作，尽快将Vulkan推向更多设备。

更多信息，请参阅[API文档](https://developer.android.com/ndk/guides/graphics/index.html)。

### 快速设置图块APIs
快速设置是一种流行且简单的暴露关键设置和操作的方式（直接从通知窗口中）。在Android 7.0中，我们扩大了快速设置的范围，使其更加实用和方便。

我们添加了更多空间以容纳更多的快速设置图块，用户可以通过向左或向右滑动来访问分页显示区域。我们还让用户能够控制快速设置图块的显示方式以及显示的位置 - 用户只需通过拖放即可添加或移动图块。

对于开发人员，Android 7.0还添加了一个新的API，可让您定义自己的快速设置图块，以便用户轻松访问应用程序中的主要控制和操作。

快速设置图块保留用于紧急需要或经常使用的控制或操作，不应用作启动应用程序的快捷方式。

一旦定义好快速设置图块，您可以将其展示给用户，用户可以通过拖放将其添加到快速设置中。

有关创建应用程序图块的信息，请参阅[图块](https://developer.android.com/reference/android/service/quicksettings/Tile.html)参考文档。

### 号码屏蔽
Android 7.0现在支持平台层的号码屏蔽，并提供了一个框架API，让服务提供商维护一个号码屏蔽列表。 默认短信应用，默认手机应用和运营商应用可以读取和写入号码屏蔽列表。其他应用程序无法访问此列表。

通过将号码屏蔽作为平台的标准功能，Android为应用程序提供了一种一致的方式，以支持各种设备上的号码屏蔽。应用程序还可以利用的其他好处包括：
* 呼叫被屏蔽号码，文字上也会被屏蔽
* 通过设备的”备份和恢复“功能，即使恢复出厂设置，被屏蔽的号码还会存在
* 多个应用程序可以共享一个号码屏蔽列表

此外，通过Android整合运营商应用程序，意味着运营商能能读取设备上的号码屏蔽列表，并为用户执行服务端阻止，以阻止不需要的呼叫和文本通过任何介质（例如VOIP端点或转发电话）到达用户。

更新信息，请参阅[BlockedNumberContract](https://developer.android.com/reference/android/provider/BlockedNumberContract.html)的参考文档。

### 来电过滤
Android 7.0允许默认的电话应用程序过滤来电。电话应用程序通过实现新的`CallScreeningService`来实现此功能。`CallScreeningService`允许电话应用程序基于[来电的详情](https://developer.android.com/reference/android/telecom/Call.Details.html)执行的许多操作，如：
* 拒绝来电
* 不允许来电显示在最近通话中
* 不向用户显示该来电的通知

更多信息，请参阅[CallScreeningService](https://developer.android.com/reference/android/telecom/CallScreeningService.html)的参考文档。

### 多区域支持，更多语言
Android 7.0现在允许用户在设置中选择多个区域，以便更好地支持双语用例。应用程序可以使用新的API获取用户所选的区域，然后为多区域用户提供更复杂的用户体验，例如显示多种语言的搜索结果，而不是提供以用户已知的语言翻译的网页。应用程序可以通过调用`LocaleList.GetDefault()`获取用户设置的多区域列表。 为了支持更多的区域设置，Android 7.0正在改变它解析资源的方式。请确保测试并验证您的应用程序按照预期使用新的资源解析逻辑。

要了解新的资源解析行为和您应遵循的最佳做法，请参阅[多语言支持](https://developer.android.com/guide/topics/resources/multilingual-support.html)。

### 新的Emojis
Android 7.0引入了更多的emojis表情和emojis相关的功能，包括肤色选择和变体选择。如果您的应用程序支持emojis表情，请按照下列指南来使用这些emojis相关的功能。
* 在插入emojis表情之前，请检查设备是否包含该emojis表情。要检查系统字体中存在哪些emojis表情，请使用[hasGlyph(String)](https://developer.android.com/reference/android/graphics/Paint.html#hasGlyph&#40;java.lang.String&#41;)方法。
* 检查emojis表情是否支持变体选择。变体选择器允许您以彩色或黑白来显示某个emojis表情。在移动设备上，应用应以彩色而不是黑白表示emojis表情。但是，如果您的应用程序是在文本中显示emojis表情，则应使用黑白变体。要确定某个emojis表情是否有变化，请使用变体选择器。有关完整的、包含变体的字符列表，请查看[Unicode文档关于变体](http://www.unicode.org/Public/9.0.0/ucd/StandardizedVariants-9.0.0d1.txt)的emojis变体序列部分。
* 检查emojis表情是否支持肤色。Android 7.0允许用户根据自己的喜好修改emojis表情的渲染肤色。输入法应用程序应该为具有多种肤色的emojis表情提供可视指示，并应允许用户选择他们喜欢的肤色。要确定哪个系统emojis表情支持肤色选择，请使用[hasGlyph(String)](https://developer.android.com/reference/android/graphics/Paint.html#hasGlyph&#40;java.lang.String&#41;)方法。您可以通过阅读[Unicode文档](http://unicode.org/emoji/charts/full-emoji-list.html)来确定哪些emojis表情应该使用肤色。

### ICU4J APIs
现在，Android 7.0在框架的`android.icu`包下提供了一个[ICU4J](http://site.icu-project.org/) API的子集。迁移很容易，大多数情况下只需要将命名空间从`com.java.icu`更改为`android.icu`。如果您已经在应用程序中使用ICU4J软件包，切换到Android框架提供的`android.icu` API则可以大幅减小APK体积。

要了解有关Android ICU4J API的更多信息，请参阅[ICU4J支持](https://developer.android.com/guide/topics/resources/icu4j-framework.html)。

### WebView

#### Chrome + WebView一起
从Android 7.0的Chrome 51版本开始，设备的Chrome APK将为Android系统WebViews提供渲染。这种方法不仅可以提高设备本身的内存使用，还可以减少保持WebView更新所需的带宽（因为只要Chrome保持启用状态，独立的WebView APK将不再更新）。

您可以通过启用“开发者选项”并选择WebView实现来选择WebView提供程序。您可以使用安装在设备上的任何兼容Chrome版本（开发，测试或稳定）或独立Webview APK作为WebView提供程序。

#### 多进程
从Android 7.0中的Chrome 51版本开始，当开发者选项“Multiprocess WebView”被启用时，WebView将在单独的沙箱进程中运行Web内容。

我们正在寻找有关兼容性和运行时性能的反馈以期在后续Android版本中默认启用多进程WebView。在此版本中，启动时间、总内存使用和软件渲染性能方面都在预期范围内。

如果您发现多进程模式有一些不可预期的问题，请告诉我们。请与[Chromium错误跟踪器](https://bugs.chromium.org/p/chromium/issues/entry?template=Webview%20Bugs)上与WebView小组联系。

#### Javascript在网页加载之前运行
从以Android 7.0为目标平台的应用程序开始，当新网页被加载时，Javascript上下文将被重置。当前，该Javascript上下文将被延续到第一页在新的WebView实例中被加载完成。

想要在WebView中插入Javascript的开发人员应该在页面开始加载后执行脚本。

#### 地理位置调用安全
从以Android 7.0为目标平台的应用程序开始，地理位置API将只允许安全源（通过HTTPS）调用。此策略旨在当用户使用不安全的连接时保护用户的隐私。

#### 使用WebView Beta测试
WebView会定期更新，因此我们建议您经常使用WebView的测试版通道测试与您的应用程序的兼容性。要开始在Android 7.0上测试WebView的预发版本，请下载并安装Chrome开发者版或Chrome测试版，然后选择它作为如上所述的开发者选项下的WebView实现。请通过[Chromium错误跟踪器](https://bugs.chromium.org/p/chromium/issues/entry?template=Webview%20Bugs)报告问题，以便我们可以在新版WebView发布之前修复它们。

如果您有任何其他问题或疑问，请随时通过[G +社区](https://plus.google.com/communities/105434725573080290360)与WebView小组联系。

### OpenGL™ ES 3.2 API
Android 7.0为OpenGL ES 3.2增加了框架接口和平台支持，包括：
* 来自[Android Extension Pack](https://www.khronos.org/registry/gles/extensions/ANDROID/ANDROID_extension_pack_es31a.txt)（AEP）的所有扩展（`EXT_texture_sRGB_decode`除外）。
* 用于HDR和延迟着色的浮点帧缓冲区。
* BaseVertex绘制调用以实现更好的批处理和流式传输。
* 强大的缓冲访问控制，减少WebGL开销。
Android 7.0上的OpenGL ES 3.2框架API随`GLES32`类一起提供。当使用OpenGL ES 3.2，请务必在manifest文件中使用`<uses-feature>`标记和`android：glEsVersion`属性声明需求。

有关使用OpenGL ES的信息，包括如何在运行时检查设备支持的OpenGL ES版本，请参阅[OpenGL ES API指南](https://developer.android.com/guide/topics/graphics/opengl.html)。

### Android TV录制
Android 7.0通过增加新的录制API已经能够录制和播放来自Android TV输入服务的内容。基于现有的时移API，TV输入服务可以控制记录何种频道数据，记录会话如何保存，以及管理用户与记录内容的交互。

更新信息，请参阅[Android TV录制API](https://developer.android.com/training/tv/tif/content-recording.html)。

### Android for Work
Android for Work为运行Android 7.0的设备添加了许多新功能和API。 以下是一些亮点：有关更改的完整列表，请参阅[Android for Work更新](https://developers.google.com/android/work/overview)。

#### 工作资料安全质询
以Android 7.0 SDK为目标平台的资料所有者能够为运行在工作资料里的应用设置分离的安全质询。当用户尝试打开任何工作应用程序时，工作质询显示出来。成功完成安全质询将解锁工作资料并在必要时解密。对于资料所有者，`ACTION_SET_NEW_PASSWORD`提示用户设置工作质询，`ACTION_SET_NEW_PARENT_PROFILE_PASSWORD`提示用户设置设备锁定。

资料所有者可以使用`setPasswordQuality()`，`setPasswordMinimumLength()`和相关方法为工作质询设置不同的密码策略（例如PIN需要多长时间，或者是否能够使用指纹解锁资料）。资料所有者还可以使用由新的`getParentProfileInstance()`方法返回的`DevicePolicyManager`实例设置设备锁定。此外，资料所有者可以使用新的`setOrganizationColor()`和`setOrganizationName()`方法自定义工作质询的凭据屏幕。

#### 关闭工作模式
在有工作资料的设备上，用户可以切换工作模式。当管理用户临时关闭工作模式，这会禁用工作资料相关应用、后台同步和通知。这包括资料所有者应用程序。当关闭工作模式，系统将会显示一个持续状态图标，提醒用户他们无法启动工作应用程序。桌面启动器也将指示工作应用和窗口小部件无法访问。

#### 永远处于VPN
设备所有者和资料所有者可以确保工作应用总是通过指定的VPN进行连接。设备启动后，系统自动启动该VPN。

新的`DevicePolicyManager`设备策略方法是`setAlwaysOnVpnPackage()`和`getAlwaysOnVpnPackage()`。

由于VPN服务可以无需应用程序交互，完全由系统直接绑定，因此VPN客户端需要处理`始终开启VPN`的新入口点。如前所述，服务通过Intent过滤器匹配`android.net.VpnService`动作告知系统。

用户还可以使用`设置>更多>Vpn`手动设置实现了`VPNService`方法的VPN客户端始终启用`始终开启VPN`。从“设置”启用`始终开启VPN`的选项仅在VPN客户端以API级别24时为目标平台时可用。

#### 自定义配置
应用程序可以使用公司颜色和徽标自定义资料所有者和设备所有者的配置流程。`DevicePolicyManager.EXTRA_PROVISIONING_MAIN_COLOR`自定义颜色。`DevicePolicyManager.EXTRA_PROVISIONING_LOGO_URI`自定义公司徽标。

### 辅助功能增强
Android 7.0现在直接在开机向导程序的欢迎屏幕上提供视觉设置。这使得用户更容易发现和配置其设备上的辅助功能，包括放大手势，字体大小，显示大小和话语提示。

由于这些辅助功能的展示位置更加突出，您的用户更有可能在启用它们的情况下试用您的应用程序。确保在启用这些设置的情况下提前测试您的应用程序。您可以从设置>辅助功能启用。

此外，在Android 7.0中，无障碍服务现在可以帮助运动损伤的用户触摸屏幕。新的API允许构建具有面部跟踪，眼睛跟踪，点扫描等功能的服务，以满足这些用户的需求。

有关详细信息，请参阅[GestureDescription](https://developer.android.com/reference/android/accessibilityservice/GestureDescription.html)的参考文档。

### 直接启动
直接启动能够改善设备启动时间，并且使注册的应用程序即使在意外重启后仍只具有受限的功能。例如，如果一个加密的设备在用户休眠时重新启动，其设定的提醒，消息和来电仍然可以继续正常地通知到用户。这也意味着无障碍服务在设备重启后也可以立即可用。

直接启动利用Android 7.0的文件加密功能，为系统和应用程序数据启用细粒度加密策略。系统对选定的系统数据和显式注册的应用程序数据使用设备加密存储。而默认情况下，所有其他系统数据，用户数据，应用和应用数据使用证书加密存储。

在引导时，系统以受限模式启动，只能访问设备加密的数据，并且无法访问应用或数据。如果您有要在此模式下运行的组件，可以通过在manifest中设置标志来注册它们。重新启动后，系统通过广播`LOCKED_BOOT_COMPLETED` intent来激活已注册的组件。系统会确保注册的设备加密的应用程序数据在解锁前可用。其他所有数据直到用户确认使用锁屏凭据解密后方可使用。

有关详细信息，请参阅[直接引导](https://developer.android.com/preview/features/direct-boot.html)。

### 密钥认证
Android 7.0引入了密钥认证，一种新的安全工具，可帮助您确保存储在[设备的硬件支持的密钥库](https://source.android.com/security/keystore/)中的密钥对正确保护您的应用程序使用的敏感信息。通过使用此工具，您将对您的应用与驻留在安全硬件中的密钥进行互动更加放心，即使运行您应用的设备已经被root。如果您在应用中使用硬件支持的密钥库中的密钥，则应该使用此工具，尤其是在使用密钥验证应用中的敏感信息时。

密钥证明允许您验证RSA或EC密钥对是否已创建并存储在设备的受信任执行环境（TEE）中的设备的硬件支持的密钥库中。该工具还允许您使用设备外服务（例如应用程序的后端服务器）来确定并强力验证密钥对的使用和有效性。这些功能提供了一个额外的安全级别来保护密钥对，即使有人根植设备或危及设备上运行的Android平台的安全性。

> 注意：只有少数运行Android 7.0的设备支持硬件级密钥认证;所有其他运行Android 7.0的设备都使用软件级密钥验证。在生产级环境中验证设备的硬件支持密钥的属性之前，应确保设备支持硬件级密钥验证。为此，您应该检查认证证书链是否包含由Google认证根密钥签署的根证书，并且密钥描述数据结构中的attestationSecurityLevel元素设置为TrustedEnvironment安全级别。

有关详细信息，请参阅[Key Attestation](https://developer.android.com/training/articles/security-key-attestation.html)开发者文档。

### 网络安全配置
在Android 7.0中，应用程序可以通过使用声明性网络安全配置，而不是使用常规的易错编程API（例如X509TrustManager），以达到不进行任何代码修改，安全地定制其安全（HTTPS，TLS）连接的行为。

支持的功能包括：
* 自定义信任锚。允许应用程序自定义为其安全连接信任哪些证书颁发机构（CA）。例如，信任特定的自签名证书或一组受限制的公共CA.
* 仅调试覆盖。让应用程序开发人员安全地调试其应用程序的安全连接，而不会增加安装风险。
* 明文流量选择停用。使应用程序保护自身免受意外使用明文流量的影响。
* 证书固定。高级功能，允许应用程序限制哪些服务器密钥可被信任用于安全连接。
有关信息，请参考[网络安全配置](https://developer.android.com/training/articles/security-config.html)。

### 默认可信任证书机构
默认情况下，以Android 7.0为目标平台的应用仅信任系统提供的证书，不再信任用户添加的证书颁发机构（CA）的证书。以Android 7.0（API级别24）为目标的应用程序，如果希望信任用户新增的CA，则应使用[网路安全设定](https://developer.android.com/training/articles/security-config.html)来指定用户添加的CA如何被信任。

### APK签名方案v2
Android 7.0推出了应用签名方案v2，这是一种新的应用程序签名方案，可提供更快的应用安装速度，更有效地防止未经授权的APK文件更改。默认情况下，Android Studio 2.2和Gradle 2.2的Android插件使用APK签名方案v2和使用JAR签名的传统签名方案签名您的应用程序。

虽然我们建议您将APK签名方案v2应用于您的应用程序，但此新方案不是强制性的。如果您的应用在使用APK签名方案v2时无法正确构建，您可以禁用新方案。禁用将会导致Android Studio 2.2和Gradle 2.2的Android插件仅使用传统的签名方案对应用进行签名。 要仅使用传统方案进行签名，请打开模块的`build.gradle`文件，然后将`v2SigningEnabled false`行添加到release签名配置中：
```gradle
android {
    ...
    defaultConfig { ... }
    signingConfigs {
      release {
        storeFile file("myreleasekey.keystore")
        storePassword "password"
        keyAlias "MyReleaseKey"
        keyPassword "password"
        v2SigningEnabled false
      }
    }
  }
```
> 注意：如果您使用APK签名方案v2签署应用程序，并进一步变更应用程序，应用程序的签名将无效。因此，请在使用APK签名方案v2签名应用之前（而不是之后）使用zipalign等工具。

更多信息，请阅读描述如何在Android Studio中[对应用进行签名](https://developer.android.com/studio/publish/app-signing.html#release-mode)的Android Studio文档，以及如何使用Gradle Android插件[配置应用签名的构建文件](https://developer.android.com/studio/build/build-variants.html#signing)。

### 受限的目录访问
Android 7.0中，应用程序可以使用新的API请求访问特定的外部存储目录，包括可移动媒体（如SD卡）上的目录。新的API大大简化了您的应用程序访问标准外部存储目录（如Pictures目录）的方式。类似相册之类的应用可以使用这些API，而不是使用`READ_EXTERNAL_STORAGE`（该授权允许访问所有目录）或存储访问框架（能使用户导航到该目录）。

此外，新的API简化了用户向您的应用授予外部存储访问权限的步骤。当您使用新的API时，系统使用简单的权限UI，清楚地详细说明应用程序请求访问的目录。

有关详细信息，请参阅[受限的目录访问](https://developer.android.com/training/articles/scoped-directory-access.html)开发人员文档。

### 键盘快捷键助手
Android 7.0中，用户可以按**Meta +** /来触发键盘快捷键页面，该页面显示系统和当前获得焦点的应用程序提供的所有快捷键。如果存在快捷键，系统将从应用程序的菜单自动检索这些快捷键。您还可以通过覆盖`onProvideKeyboardShortcuts()`方法为快捷键页面提供自己微调过的快捷键列表。

> **注意**：不是所有的键盘都有**Meta**键：在Macintosh键盘上，它是**Command**键，在Windows键盘上，它是**Windows**键，在Pixel C和Chrome操作系统键盘上，它是**Search**键 。

要在应用程序的任何位置触发键盘快捷键助手，请从相关Activity中调用`requestShowKeyboardShortcuts()`。

### 自定义指针API
Android 7.0引入了自定义指针API，它允许您自定义指针的外观，可见性和行为。当用户使用鼠标或触摸板进行UI对象交互时，此功能特别有用。默认指针使用标准图标。此API还包括一些高级功能，如根据特定的鼠标或触摸板移动更改指针图标的外观。

要设置指针图标，请覆盖`View`类的`onResolvePointerIcon()`方法。此方法使用`PointerIcon`对象来绘制与特定运动事件对应的图标。

### 持续性能API
对于长期运行的应用程序，性能可能会大幅波动，因为当设备组件达到其温度上限时，系统会限制片上系统引擎。这种波动为应用开发者创建高性能、长期运行的应用带来了一个变动的目标。

为了解决这些限制，Android 7.0开始支持持续性能模式，使OEM厂商能够提供关于支持长期运行应用程序的设备性能的提示。应用程序开发人员可以使用这些提示来调整应用程序，以在长时间内实现可预测的、水平一致的设备性能。

应用开发人员只能在Nexus 6P设备上的Android 7.0中试用此新API。要使用此功能，请为要在持续性能模式下运行的窗口设置持续性能窗口标志。使用`Window.setSustainedPerformanceMode()`方法设置此标志。当窗口失去对焦时，系统自动禁用此模式。

### VR支持
Android 7.0为新的VR模式增加了平台支持和优化，让开发者能为用户构建高品质的移动VR体验。有许多性能增强，包括访问VR应用程序独占的CPU核心。在您的应用程序中，您可以利用适用于VR的智能头部跟踪和立体声通知。最重要的是，Android 7.0提供了极低延迟的图形。有关构建适用于Android 7.0的VR应用的完整信息，请参阅[Google VR SDK Android版](https://developers.google.com/vr/android/)。

### 打印服务增强
Android 7.0中，打印服务开发者现在可以显示打印机和打印作业相关的额外信息。

当列出所有打印机时，打印服务现在可以通过两种方式为每个打印机设置图标：
* 您可以通过调用`setIconResourceId()`从资源ID设置图标。
* 您可以通过调用`etHasCustomPrinterIcon()`来显示网络中的图标，并实现`onRequestCustomPrinterIcon()`来设置图标被请求的回调。
此外，您可以通过调用`setInfoIntent()`来提供每个打印机显示额外信息的`Activity`。

您可以分别通过调用`setProgress()`和`setStatus()`来指定打印作业通知中打印作业的进度和状态。

### 帧度量API
帧度量API允许应用程序监视其UI渲染的性能。该API通过暴露流式发布/订阅API来传输应用程序当前窗口的帧校时信息。返回的数据等同于`adb shell dumpsys gfxinfo framestats`显示的数据，但不限于最近120帧。

您可以使用帧度量API来测量发布产品的交互级UI性能，而无需使用USB连接。此API允许以比`adb shell dumpsys gfxinfo`高得多的粒度收集数据。这种更高的粒度是可能的，因为系统可以收集应用中特定交互的数据，而不需要捕获整个应用程序性能的全局摘要，或清除任何全局状态。您可以使用此功能收集性能数据，并捕获应用程序实际使用的UI性能回归。

要监视窗口，请实现[OnFrameMetricsAvailableListener.onFrameMetricsAvailable](https://developer.android.com/reference/android/view/Window.OnFrameMetricsAvailableListener.html#onFrameMetricsAvailable&#40;android.view.Window, android.view.FrameMetrics, int&#41;)回调方法并将其注册在要监视的窗口上。

该API还提供了一个[FrameMetrics](https://developer.android.com/reference/android/view/FrameMetrics.html)对象，其中包含渲染子系统报告帧生命周期中各种关键的的时间点数据。目前支持的度量为：`UNKNOWN_DELAY_DURATION`，`INPUT_HANDLING_DURATION`，`ANIMATION_DURATION`，`LAYOUT_MEASURE_DURATION`，`DRAW_DURATION`，`SYNC_DURATION`，`COMMAND_ISSUE_DURATION`，`SWAP_BUFFERS_DURATION`，`TOTAL_DURATION`和`FIRST_DRAW_FRAME`。

### 虚拟文件
在以前的Android版本中，应用可以使用存储访问框架以允许用户从云存储帐户（如Google云端硬盘）中选择文件。 然而却没有一种方法能表示没有直接字节码表示的文件，每个文件都需要提供输入流。

Android 7.0在存储访问框架中添加了虚拟文件的概念。虚拟文件功能允许您的[DocumentsProvider](https://developer.android.com/reference/android/provider/DocumentsProvider.html)返回可以与[ACTION_VIEW](https://developer.android.com/reference/android/content/Intent.html#ACTION_VIEW) Intent一起使用的文档URI，即使这些文件没有直接的字节码表示。Android 7.0还允许您为用户文件提供备用格式，虚拟或其他。

有关打开虚拟文件的更多信息，请参阅[存储访问框架指南中的打开虚拟文件](https://developer.android.com/guide/topics/providers/document-provider.html#virtual)。

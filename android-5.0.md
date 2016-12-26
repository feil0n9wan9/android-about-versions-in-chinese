# Android 5.0 APIs

Android 5.0 [LOLLIPOP][180] 为用户和应用开发者提供了新功能。本文旨在介绍其中最值得关注的新 API。  

如果您有已发布的应用，请务必看一看 [Android 5.0][181] 行为变更，了解您的应用应该考虑的变化。即使您不使用新的 API 或者不围绕着新功能做开发，这些行为变更仍可能会影响您的应用在 Android 5.0 设备上的表现。  

如需详细了解新平台功能，请参阅 [Android Lollipop][182] 重要内容。

[180]: https://developer.android.google.cn/reference/android/os/Build.VERSION_CODES.html#LOLLIPOP)
[181]: https://developer.android.google.cn/about/versions/android-5.0-changes.html
[182]: https://developer.android.google.cn/about/versions/lollipop.html

####着手开发
要着手开发 Android 5.0 应用，您必须先[获得 Android SDK](https://developer.android.google.cn/studio/index.html)，然后使用 [SDK 管理器](https://developer.android.google.cn/tools/help/sdk-manager.html)下载 Android 5.0 SDK Platform 和系统映像。  

####更新目标 API 级别
要进一步优化您的应用在运行 Android 5.0 的设备上的性能，请将您的 [targetSdkVersion](https://developer.android.google.cn/guide/topics/manifest/uses-sdk-element.html#target) 设置为 "21"，在 Android 5.0 系统映像上安装您的应用并进行测试，然后发布更新了此变更的应用。  

您可以通过在代码中添加条件，在执行您的 [minSdkVersion](https://developer.android.google.cn/guide/topics/manifest/uses-sdk-element.html#min) 不支持的 API 之前检查系统 API 级别，实现在使用 Android 5.0 API 的同时仍为旧版本提供支持。要详细了解如何保持向后兼容性，请阅读[支持不同平台版本](https://developer.android.google.cn/training/basics/supporting-devices/platforms.html)。  

如需了解有关 API 级别工作方式的详细信息，请阅读[什么是 API 级别](https://developer.android.google.cn/guide/topics/manifest/uses-sdk-element.html#ApiLevels)？  

####重要的行为变更
如果您之前发布过 Android 应用，请注意您的应用可能受到 Android 5.0 变化的影响。  

如需了解完整信息，请参阅[Android 5.0变更](https://developer.android.google.cn/about/versions/android-5.0-changes.html) 。

###用户界面
####Material Design 支持
Android 5.0 添加了对 Android 的新 Material Design 样式的支持。您可以创建具有 Material Design 功能的应用，实现动态视觉效果，利用其中的 UI 元素转换赋予用户自然的感觉。此支持包括：  

* Material Design 主题
* 视图阴影
* [RecyclerView](https://developer.android.google.cn/reference/android/support/v7/widget/RecyclerView.html) 小部件
* 可绘制动画和造型效果
* Material Design 动画和 Activity 转换效果
* 针对基于视图状态的视图属性的动画生成器
* 可自定义的 UI 小部件和具有可由您控制的调色板的app bars
* 基于 XML 矢量图形的动画和非动画可绘制对象

要详细了解如何为您的应用添加 Material Design 功能，请参阅 [Material Design](https://developer.android.google.cn/training/material/index.html)。

####最近使用的应用屏幕中的并发文档和 Activity
在之前的版本中，[最近使用的应用屏幕][1]只能为最近与用户交互过的每个应用显示一项任务。现在，您的应用可以根据需要为其他并发文档 Activity 打开更多任务。此功能简化了多任务处理，通过在所有应用中提供一致的切换体验，让用户能够在最近使用的应用屏幕中的各个 Activity 和文档之间快速切换。此类并行任务示例可能包括：网络浏览器应用中打开的标签页、效率类应用中的文档、游戏中的并行对局或信息应用中的聊天。您的应用可以通过 [ActivityManager.AppTask][2] 类管理它的任务。

为插入逻辑换行符以便系统将您的 Activity 视为新任务，请在使用 [startActivity()][3] 启动 Activity 时使用 [FLAG_ACTIVITY_NEW_DOCUMENT][4]。您还可以通过在manifest清单文件中将 [`<activity>`][5] 元素的 documentLaunchMode 属性设置为 "intoExisting" 或 "always" 来获得此行为。

为避免使最近使用的应用屏幕变得混乱，您可以在应用中设置该屏幕中可显示的任务数上限。要实现此目的，请设置 [`<application>`][6] 属性 [android:maxRecents][7]。目前可指定的上限为每位用户 50 个任务（RAM 较低设备为 25 个）。

可将最近使用的应用屏幕中的任务设置为在重启后保留。要控制持久化行为，请使用 [android:persistableMode][8] 属性。您还可以通过调用 [setTaskDescription()][9] 方法，更改 Activity 在最近使用的应用屏幕中的视觉属性，如 Activity 的颜色、标签和图标。

[1]: https://developer.android.google.cn/guide/components/recents.html
[2]: https://developer.android.google.cn/reference/android/app/ActivityManager.AppTask.html
[3]: https://developer.android.google.cn/reference/android/app/Activity.html#startActivity(android.content.Intent)
[4]: https://developer.android.google.cn/reference/android/content/Intent.html#FLAG_ACTIVITY_NEW_DOCUMENT
[5]: https://developer.android.google.cn/guide/topics/manifest/activity-element.html
[6]: https://developer.android.google.cn/guide/topics/manifest/application-element.html
[7]: https://developer.android.google.cn/reference/android/R.attr.html\#maxRecents
[8]: https://developer.android.google.cn/reference/android/R.attr.html#persistableMode
[9]: https://developer.android.google.cn/reference/android/app/Activity.html#setTaskDescription(android.app.ActivityManager.TaskDescription)

####WebView 更新
Android 5.0 将 [WebView][10] 实现更新至 Chromium M37，增强了安全性和稳定性，并修复了一些问题。运行在 Android 5.0 上的 [WebView][11] 的默认用户代理字符串已更新，以纳入 37.0.0.0 作为版本号。

此版本引入了 [PermissionRequest][12] 类，让您的应用可以通过 [getUserMedia()][13] 等网络 API 授予 [WebView][14] 访问相机和麦克风之类受保护资源的权限。您的应用必须对这些资源拥有相应的 Android 权限，才能向 [WebView][15] 授予权限。

借助新的 [onShowFileChooser()][16] 方法，您现在可以在 [WebView][17] 中使用输入表单字段，然后启动文件选择器从 Android 设备中选择图像和文件。

此外，此版本还提供了对 [WebAudio][18]、[WebGL][19] 和 [WebRTC][20] 开放标准的支持。要详细了解此版本包含的新功能，请参阅 [WebView for Android][21]。

[10]: https://developer.android.google.cn/reference/android/webkit/WebView.html
[11]: https://developer.android.google.cn/reference/android/webkit/WebView.html
[12]: https://developer.android.google.cn/reference/android/webkit/PermissionRequest.html
[13]: https://developer.mozilla.org/en-US/docs/NavigatorUserMedia.getUserMedia
[14]: https://developer.android.google.cn/reference/android/webkit/WebView.html
[15]: https://developer.android.google.cn/reference/android/webkit/WebView.html
[16]: https://developer.android.google.cn/reference/android/webkit/WebChromeClient.html#onShowFileChooser%28android.webkit.WebView,%20android.webkit.ValueCallback%3Candroid.net.Uri%5B%5D%3E,%20android.webkit.WebChromeClient.FileChooserParams%29
[17]: https://developer.android.google.cn/reference/android/webkit/WebView.html
[18]: http://webaudio.github.io/web-audio-api/
[19]: https://www.khronos.org/webgl/
[20]: http://www.webrtc.org/
[21]: https://developer.chrome.com/multidevice/webview/overview

####屏幕采集和共享
Android 5.0 引入了新的 [android.media.projection][22] API，让您可以为应用添加屏幕采集和屏幕共享功能。例如，如果您想在视频会议应用中启用屏幕共享，便可使用此功能。

新增的 [createVirtualDisplay()][23] 方法允许您的应用将主屏幕（the default display）的内容采集到一个 [Surface][24] 对象中，然后您的应用便可将其发送至整个网络。该 API 只允许采集非安全屏幕内容，不允许采集系统音频。要开始采集屏幕，您的应用必须先使用通过 [createScreenCaptureIntent()][25] 方法获得的 [Intent][26] 启动屏幕采集对话框，请求用户授予权限。

如需查看新 API 使用方法的示例，请参阅示例项目中的 MediaProjectionDemo 类。

[22]: https://developer.android.google.cn/reference/android/media/projection/package-summary.html
[23]: https://developer.android.google.cn/reference/android/media/projection/MediaProjection.html#createVirtualDisplay%28java.lang.String,%20int,%20int,%20int,%20int,%20android.view.Surface,%20android.hardware.display.VirtualDisplay.Callback,%20android.os.Handler%29
[24]: https://developer.android.google.cn/reference/android/view/Surface.html
[25]: https://developer.android.google.cn/reference/android/media/projection/MediaProjectionManager.html#createScreenCaptureIntent()
[26]: https://developer.android.google.cn/reference/android/content/Intent.html

###通知
####锁定屏幕通知

Android 5.0 中的锁定屏幕(lock screen)可以显示通知。用户可以通过“Settings” 选择是否允许在安全的锁定屏幕(lock screen)上显示敏感的通知内容。

您的应用可以控制在安全锁定屏幕上显示的通知中可见信息的详细程度。要控制可见性级别，请调用 [setVisibility()][27]，然后指定以下值之一：

* [VISIBILITY_PRIVATE][28]：显示通知图标等基本信息，但隐藏通知的完整内容。
* [VISIBILITY_PUBLIC][29]：显示通知的完整内容。
* [VISIBILITY_SECRET][30]：不显示任何内容，甚至不显示通知图标。

当可视性级别为 [VISIBILITY_PRIVATE][31] 时，您还可以提供隐藏个人详情的删减版通知内容。例如，短信应用可能会显示一条通知，指出“您有3 条新短信”，但是隐藏了短信内容和发件人。要提供此替换版本的通知，请先使用 [Notification.Builder][32] 创建替换通知。创建专用通知对象时，请通过 [setPublicVersion()][33] 方法为其附加替换通知。

[27]: https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setVisibility(int)
[28]: https://developer.android.google.cn/reference/android/app/Notification.html#VISIBILITY_PRIVATE
[29]: https://developer.android.google.cn/reference/android/app/Notification.html#VISIBILITY_PUBLIC
[30]: https://developer.android.google.cn/reference/android/app/Notification.html#VISIBILITY_SECRET
[31]: https://developer.android.google.cn/reference/android/app/Notification.html#VISIBILITY_PRIVATE
[32]: https://developer.android.google.cn/reference/android/app/Notification.Builder.html
[33]: https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setPublicVersion(android.app.Notification)

####通知元数据
Android 5.0 使用与您的应用通知关联的元数据，以更智能的方式对通知排序。要设置元数据，请在构建通知时调用 [Notification.Builder][34] 中的下列方法：

* [setCategory()][35]：当设备处于“优先”模式时，指示系统如何处理应用通知（例如，通知代表来电、即时通讯还是闹铃）。
* [setPriority()][36]：标记通知的重要性是高于还是低于普通通知。如果优先级字段设置为 [PRIORITY_MAX][37] 或[PRIORITY_HIGH][38] 的通知还有声音或振动，则会将其显示在小型浮动窗口中。
* [addPerson()][39]：让您可以添加一名或多名与通知有关的人员。您的应用可以使用此名单指示系统将指定人员发出的通知归成一组，或者将这些人员发出的通知视为更重要的通知。

[34]: https://developer.android.google.cn/reference/android/app/Notification.Builder.html
[35]: https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setCategory(java.lang.String)
[36]: https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setPriority(int)
[37]: https://developer.android.google.cn/reference/android/app/Notification.html#PRIORITY_MAX
[38]: https://developer.android.google.cn/reference/android/app/Notification.html#PRIORITY_HIGH
[39]: https://developer.android.google.cn/reference/android/app/Notification.Builder.html#addPerson(java.lang.String)

###图形
####对 OpenGL ES 3.1 的支持
Android 5.0 添加了对 OpenGLES 3.1 的Java 接口和native层面的支持。OpenGL ES 3.1 中提供的主要新功能包括：

* 计算着色器
* 单独的着色器对象
* 间接绘制命令
* 多重采样和模具纹理
* 着色语言改进
* 高级混合模式和调试专用扩展
* 向后兼容 OpenGL ES 2.0 和 3.0

Android 上 OpenGL ES 3.1 的 Java 接口由 [GLES31][40] 提供。使用 OpenGL ES 3.1 时，请务必在清单文件中使用 [`<uses-feature>`][41] 标记和 android:glEsVersion 属性对其进行声明。例如：

	<manifest>
    	<uses-feature android:glEsVersion="0x00030001" />
    	...
	</manifest>
如需了解有关如何使用 OpenGL ES（包括如何在运行时检查设备支持的 OpenGL ES 版本）的详细信息，请参阅 [OpenGL ES API 指南][42]。

[40]: https://developer.android.google.cn/reference/android/opengl/GLES31.html
[41]: https://developer.android.google.cn/guide/topics/manifest/uses-feature-element.html
[42]: https://developer.android.google.cn/guide/topics/graphics/opengl.html

####Android 扩展包
除了 OpenGL ES 3.1 外，此版本还提供了一个扩展包，其中包括对高级图形功能的Java 接口和native层面的支持。Android 将这些扩展视作单个软件包。（如果存在 ANDROID_extension_pack_es31a 扩展，您的应用可以假定扩展包中的所有扩展都存在，只需使用一条 #extension 语句便可启用着色语言功能。）

该扩展包支持：

* 对着色器存储缓冲区、images和atomics的有保证的Fragment着色器支持（在 OpenGL ES 3.1 中，Fragment着色器支持为可选支持）。
* 镶嵌和几何着色器
* ASTC (LDR) 纹理压缩格式
* Per-sample interpolation and shading
* 帧缓冲区中每个颜色附件采用不同混合模式

该扩展包的 Java 接口随 [GLES31Ext][43] 提供。在您的应用清单中，您可以将应用声明为必须安装在支持该扩展包的设备上。例如：

	<manifest>
	    <uses-feature android:name=“android.hardware.opengles.aep”
	        android:required="true" />
	    ...
	</manifest>

[43]: https://developer.android.google.cn/reference/android/opengl/GLES31Ext.html

###媒体
####用于高级相机功能的 Camera API
Android 5.0 引入了新的 [android.hardware.camera2][44] API 来简化精细照片采集和图像处理。您现在可以使用 [getCameraIdList()][45] 通过编程方式访问可供系统使用的相机设备，以及使用 [openCamera()][46] 连接特定设备。要开始采集图像，请创建一个 [CameraCaptureSession][47] 并指定用于发送已采集图像的 [Surface][48] 对象。可将 [CameraCaptureSession][49] 配置为进行单张拍摄或多张连拍。

要在采集新图像时得到通知，请实现 [CameraCaptureSession.CaptureCallback][50] 侦听器，并把它设置在您的图像采集请求中。现在，当系统完成图像采集请求时，会调用您的 [CameraCaptureSession.CaptureCallback][51] 侦听器 [onCaptureCompleted()][52]这个方法，并在 [CaptureResult][53] 中为您提供图像采集元数据。

[CameraCharacteristics][54] 类可让您的应用检测到设备上可用的相机功能。该对象的 [INFO_SUPPORTED_HARDWARE_LEVEL][55] 属性代表相机的功能级别。

* 所有设备都至少支持 [INFO_SUPPORTED_HARDWARE_LEVEL_LEGACY][56] 硬件级别，该级别具有的能力大致与弃用的 [Camera][57] API 相当。
* 支持 [INFO_SUPPORTED_HARDWARE_LEVEL_FULL][58] 硬件级别的设备可手动控制采集和后期处理，以及以高帧速率采集高分辨率图像。

要了解如何使用更新后的 [Camera][59] API，请参阅此版本中的 Camera2Basic 和 Camera2Video 实现示例。

[44]: https://developer.android.google.cn/reference/android/hardware/camera2/package-summary.html
[45]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraManager.html#getCameraIdList()
[46]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraManager.html#openCamera%28java.lang.String,%20android.hardware.camera2.CameraDevice.StateCallback,%20android.os.Handler%29
[47]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraCaptureSession.html
[48]: https://developer.android.google.cn/reference/android/view/Surface.html
[49]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraCaptureSession.html
[50]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraCaptureSession.CaptureCallback.html
[51]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraCaptureSession.CaptureCallback.html
[52]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraCaptureSession.CaptureCallback.html#onCaptureCompleted%28android.hardware.camera2.CameraCaptureSession,%20android.hardware.camera2.CaptureRequest,%20android.hardware.camera2.TotalCaptureResult%29
[53]: https://developer.android.google.cn/reference/android/hardware/camera2/CaptureResult.html
[54]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraCharacteristics.html
[55]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraCharacteristics.html#INFO_SUPPORTED_HARDWARE_LEVEL
[56]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraMetadata.html#INFO_SUPPORTED_HARDWARE_LEVEL_LEGACY
[57]: https://developer.android.google.cn/reference/android/hardware/Camera.html
[58]: https://developer.android.google.cn/reference/android/hardware/camera2/CameraMetadata.html#INFO_SUPPORTED_HARDWARE_LEVEL_FULL
[59]: https://developer.android.google.cn/reference/android/hardware/camera2/package-summary.html

####音频回放

此版本加入了对 [AudioTrack][60] 的下列更改：

* 您的应用现在可以提供浮点格式 ([ENCODING_PCM_FLOAT][61]) 的音频数据。这可以实现更大的动态范围、更一致的精度和更多的动态余量。浮点算法在进行中间计算时特别有用。回放端点为音频数据使用位深度更低的整型格式。（在Android 5.0中，部分内部管道尚未采用浮点格式。）
* 您的应用现在可以提供音频数据作为 [ByteBuffer][62]，数据使用的格式与 [MediaCodec][63] 提供的格式相同。
* [WRITE_NON_BLOCKING][64] 选项可简化某些应用的缓冲和多线程处理。

[60]: https://developer.android.google.cn/reference/android/media/AudioTrack.html
[61]: https://developer.android.google.cn/reference/android/media/AudioFormat.html#ENCODING_PCM_FLOAT
[62]: https://developer.android.google.cn/reference/java/nio/ByteBuffer.html
[63]: https://developer.android.google.cn/reference/android/media/MediaCodec.html
[64]: https://developer.android.google.cn/reference/android/media/AudioTrack.html#WRITE_NON_BLOCKING

####媒体回放控制
使用新增的通知和媒体 API 可确保系统 UI 了解您的媒体回放情况，并可提取和显示专辑封面。现在，可以利用新增的 [MediaSession][65] 类和 [MediaController][66] 类更轻松地在整个 UI 和服务范围内控制媒体回放。

新增的 [MediaSession][65] 类替代了弃用的 [RemoteControlClient][67] 类，提供一套回调方法来处理传输控制和媒体按钮。如果您的应用提供媒体回放，并运行在 Android [TV][68] 或 [Wear][69] 平台上，请使用 [MediaSession][65] 类，通过同样的回调方法来处理您的传输控制。

现在，您可以使用新增的 [MediaController][66] 类开发自己的媒体控制器应用。该类可通过您的应用的 UI 进程，以线程安全方式监控和控制媒体回放。请在创建控制器时指定一个 [MediaSession.Token][70] 对象，以便您的应用可与给定 [MediaSession][65] 交互。您可以利用 [MediaController.TransportControls][71] 方法，通过发送 [play()][72]、[stop()][73]、[skipToNext()][74] 和 [setRating()][75] 等命令来控制该会话上的媒体回放。对于控制器，您还可以注册一个 [MediaController.Callback][76] 对象来侦听该会话上的元数据和状态变化。

此外，您还可以利用新增的 [Notification.MediaStyle][77] 类创建允许将回放控制与媒体会话绑定的丰富通知。

[65]: https://developer.android.google.cn/reference/android/media/session/MediaSession.html
[66]: https://developer.android.google.cn/reference/android/media/session/MediaController.html
[67]: https://developer.android.google.cn/reference/android/media/RemoteControlClient.html
[68]: https://developer.android.google.cn/tv/index.html
[69]: https://developer.android.google.cn/wear/index.html
[70]: https://developer.android.google.cn/reference/android/media/session/MediaSession.Token.html
[71]: https://developer.android.google.cn/reference/android/media/session/MediaController.TransportControls.html
[72]: https://developer.android.google.cn/reference/android/media/session/MediaController.TransportControls.html#play()
[73]: https://developer.android.google.cn/reference/android/media/session/MediaController.TransportControls.html#stop()
[74]: https://developer.android.google.cn/reference/android/media/session/MediaController.TransportControls.html#skipToNext()
[75]: https://developer.android.google.cn/reference/android/media/session/MediaController.TransportControls.html#setRating(android.media.Rating)
[76]: https://developer.android.google.cn/reference/android/media/session/MediaController.Callback.html
[77]: https://developer.android.google.cn/reference/android/app/Notification.MediaStyle.html

####媒体浏览
Android 5.0 引入了通过新增的 [android.media.browse][78] API 让应用能够浏览其他应用媒体内容库的功能。要公开您应用内的媒体内容，请扩展 [MediaBrowserService][79] 类。您实现的 [MediaBrowserService][79] 应提供对 [MediaSession.Token][80] 的访问权限，以便应用能播放您的服务提供的媒体内容。

要与媒体浏览器服务交互，请使用 [MediaBrowser][81] 类。在您创建 [MediaBrowser][81] 实例时为 [MediaSession][82] 指定组件名称。然后，您的应用便可利用该浏览器实例连接到关联的服务并获得 [MediaSession.Token][80] 对象，以播放通过该服务公开的内容。

[78]: https://developer.android.google.cn/reference/android/media/browse/package-summary.html
[79]: https://developer.android.google.cn/reference/android/service/media/MediaBrowserService.html
[80]: https://developer.android.google.cn/reference/android/media/session/MediaSession.Token.html
[81]: https://developer.android.google.cn/reference/android/media/browse/MediaBrowser.html
[82]: https://developer.android.google.cn/reference/android/media/session/MediaSession.html

###存储
####目录选择
Android 5.0 扩展了[存储访问框架][83]，允许用户选择整个目录子树，从而授予应用对所含全部文档的读/写权限，无需用户逐项确认。

要选择目录子树，请生成并发送一个 [OPEN_DOCUMENT_TREE][84] intent。系统会显示所有支持子树选择的 [DocumentsProvider][85] 实例，并允许用户浏览和选择目录。返回的 URI 代表对所选子树的访问权限。然后，您就可以使用 [buildChildDocumentsUriUsingTree()][86] 和 [buildDocumentUriUsingTree()][87] 以及 [query()][88] 来探索子树。

新增的 [createDocument()][89] 方法允许您在该子树下的任何位置新建文档或目录。要管理现有文档，请使用 [renameDocument()][90] 和 [deleteDocument()][91]。检查 [COLUMN_FLAGS][92] 以验证provider是否支持这些调用，然后再发出调用。

如果您要实现 [DocumentsProvider][93] 并想支持子树选择，请实现 [isChildDocument()][94] 并在您的 [COLUMN_FLAGS][92] 中加入 [FLAG_SUPPORTS_IS_CHILD][95]。

Android 5.0 还在共享的存储空间上引入了新的软件包专属目录，您的应用可在其中放置供加入到 [MediaStore][96] 中的媒体文件。新增的 [getExternalMediaDirs()][97] 返回所有共享存储设备上这些目录的路径。您的应用无需额外权限便可访问返回的路径，这与 [getExternalFilesDir()][98] 类似。平台会定期扫描这些目录中的新媒体，但您也可利用 [MediaScannerConnection][99] 显式扫描新内容。

[83]: https://developer.android.google.cn/guide/topics/providers/document-provider.html
[84]: https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_OPEN_DOCUMENT_TREE
[85]: https://developer.android.google.cn/reference/android/provider/DocumentsProvider.html
[86]: https://developer.android.google.cn/reference/android/provider/DocumentsContract.html#buildChildDocumentsUriUsingTree%28android.net.Uri,%20java.lang.String%29
[87]: https://developer.android.google.cn/reference/android/provider/DocumentsContract.html#buildDocumentUriUsingTree%28android.net.Uri,%20java.lang.String%29
[88]: https://developer.android.google.cn/reference/android/content/ContentResolver.html#query%28android.net.Uri,%20java.lang.String%5B%5D,%20java.lang.String,%20java.lang.String%5B%5D,%20java.lang.String%29
[89]: https://developer.android.google.cn/reference/android/provider/DocumentsContract.html#createDocument%28android.content.ContentResolver,%20android.net.Uri,%20java.lang.String,%20java.lang.String%29
[90]: https://developer.android.google.cn/reference/android/provider/DocumentsContract.html#renameDocument%28android.content.ContentResolver,%20android.net.Uri,%20java.lang.String%29
[91]: https://developer.android.google.cn/reference/android/provider/DocumentsProvider.html#deleteDocument(java.lang.String)
[92]: https://developer.android.google.cn/reference/android/provider/DocumentsContract.Document.html#COLUMN_FLAGS
[93]: https://developer.android.google.cn/reference/android/provider/DocumentsProvider.html
[94]: https://developer.android.google.cn/reference/android/provider/DocumentsProvider.html#isChildDocument%28java.lang.String,%20java.lang.String%29
[95]: https://developer.android.google.cn/reference/android/provider/DocumentsContract.Root.html#FLAG_SUPPORTS_IS_CHILD
[96]: https://developer.android.google.cn/reference/android/provider/MediaStore.html
[97]: https://developer.android.google.cn/reference/android/content/Context.html#getExternalMediaDirs()
[98]: https://developer.android.google.cn/reference/android/content/Context.html#getExternalFilesDir(java.lang.String)
[99]: https://developer.android.google.cn/reference/android/media/MediaScannerConnection.html

###无线和连接
####多个网络连接
Android 5.0 提供了新的多网络 API，允许您的应用动态扫描具有特定能力的可用网络，并与它们建立连接。当您的应用需要 SUPL、彩信或运营商计费网络等专业化网络时，或者您想使用特定类型的传输协议发送数据时，就可以使用此功能。

要从您的应用以动态方式选择并连接网络，请执行以下步骤：

1. 创建一个 [ConnectivityManager][100]。
2. 使用 [NetworkRequest.Builder][101] 类创建一个 [NetworkRequest][102] 对象，并指定您的应用感兴趣的网络功能和传输类型。
3. 要扫描合适的网络，请调用 [requestNetwork()][103] 或 [registerNetworkCallback()][104]，并传入 [NetworkRequest][102] 对象和 [ConnectivityManager.NetworkCallback][105] 的实现。如果您想在检测到合适的网络时主动切换到该网络，请使用 [requestNetwork()][103] 方法；如果只是接收已扫描网络的通知而不需要主动切换，请改用 [registerNetworkCallback()][104] 方法。

当系统检测到合适的网络时，它会连接到该网络并调用 [onAvailable()][106] 回调。您可以使用回调中的 [Network][107] 对象来获取有关网络的更多信息，或者引导通信使用所选网络。

[100]: https://developer.android.google.cn/reference/android/net/ConnectivityManager.html
[101]: https://developer.android.google.cn/reference/android/net/NetworkRequest.Builder.html
[102]: https://developer.android.google.cn/reference/android/net/NetworkRequest.html
[103]: https://developer.android.google.cn/reference/android/net/ConnectivityManager.html#requestNetwork%28android.net.NetworkRequest,%20android.net.ConnectivityManager.NetworkCallback%29
[104]: https://developer.android.google.cn/reference/android/net/ConnectivityManager.html#registerNetworkCallback(android.net.NetworkRequest,%20android.net.ConnectivityManager.NetworkCallback)
[105]: https://developer.android.google.cn/reference/android/net/ConnectivityManager.NetworkCallback.html
[106]: https://developer.android.google.cn/reference/android/net/ConnectivityManager.NetworkCallback.html#onAvailable(android.net.Network)
[107]: https://developer.android.google.cn/reference/android/net/Network.html

####蓝牙低功耗
Android 4.3 为发挥核心作用的[蓝牙低功耗][108]（蓝牙 LE）引入了平台支持。在 Android 5.0 中，Android 设备现在可以发挥蓝牙 LE 外围设备的作用。应用可以利用此功能让附近设备发现它。例如，您可以开发这样的应用：让设备发挥计步器或健康监测仪的作用，并与其他蓝牙 LE 设备进行数据通信。

新增的 [android.bluetooth.le][109] API 让您的应用可以发布广告、扫描响应以及与附近的蓝牙 LE 设备建立连接。要使用新增的广告和扫描功能，请在您的manifest清单文件中添加 [BLUETOOTH_ADMIN][110] 权限。当用户更新您的应用或从 Play 商店下载您的应用时，会被要求向您的应用授予以下权限：“蓝牙连接信息：允许应用控制蓝牙，包括向周围的蓝牙设备广播信息和接收信息”

要启动蓝牙 LE 广播，以便其他设备能发现您的应用，请调用 [startAdvertising()][111]，并传入 [AdvertiseCallback][112] 类的实现。回调对象会收到广播操作成功或失败的报告。

Android 5.0 引入了 [ScanFilter][113] 类，让您的应用可以只扫描其感兴趣的特定类型设备。要开始扫描蓝牙 LE 设备，请调用 [startScan()][114]，并传入筛选器列表。在方法调用中，您还必须提供 [ScanCallback][115] 的实现，以便在发现蓝牙 LE 广播时进行报告。

[108]: https://developer.android.google.cn/guide/topics/connectivity/bluetooth-le.html
[109]: https://developer.android.google.cn/reference/android/bluetooth/le/package-summary.html
[110]: https://developer.android.google.cn/reference/android/Manifest.permission.html#BLUETOOTH_ADMIN
[111]: https://developer.android.google.cn/reference/android/bluetooth/le/BluetoothLeAdvertiser.html#startAdvertising(android.bluetooth.le.AdvertiseSettings,%20android.bluetooth.le.AdvertiseData,%20android.bluetooth.le.AdvertiseCallback)
[112]: https://developer.android.google.cn/reference/android/bluetooth/le/AdvertiseCallback.html
[113]: https://developer.android.google.cn/reference/android/bluetooth/le/ScanFilter.html
[114]: https://developer.android.google.cn/reference/android/bluetooth/le/BluetoothLeScanner.html#startScan(android.bluetooth.le.ScanCallback)
[115]: https://developer.android.google.cn/reference/android/bluetooth/le/ScanCallback.html

####NFC 增强功能
Android 5.0 添加这些增强功能是为了扩大 NFC 的使用范围和提高 NFC 的使用灵活性：

* Android Beam 现已出现在 share 菜单中。
* 您的应用可通过调用 [invokeBeam()][116] 来调用用户设备上的 Android Beam 进行数据分享。这样一来，用户不必手动用设备接触另一台具有 NFC 功能的设备，便可完成数据传送。
* 您可以使用新增的 [createTextRecord()][117] 方法来创建一条包含 UTF-8 文本数据的 NDEF 记录。
* 如果您要开发支付应用，现在可以通过调用[registerAidsForService()][118] 动态注册 NFC 应用 ID (AID)。您还可以使用 [setPreferredService()][119] 来设置应在特定 Activity 位于前台时使用的首选卡模拟服务。

[116]: https://developer.android.google.cn/reference/android/nfc/NfcAdapter.html#invokeBeam(android.app.Activity)
[117]: https://developer.android.google.cn/reference/android/nfc/NdefRecord.html#createTextRecord(java.lang.String,%20java.lang.String)
[118]: https://developer.android.google.cn/reference/android/nfc/cardemulation/CardEmulation.html#registerAidsForService%28android.content.ComponentName,%20java.lang.String,%20java.util.List%3Cjava.lang.String%3E%29
[119]: https://developer.android.google.cn/reference/android/nfc/cardemulation/CardEmulation.html#setPreferredService(android.app.Activity,%20android.content.ComponentName)

###Volta 项目
除了提供新功能外，Android 5.0 还重视电池寿命的改善。可以利用新增的 API 和工具来了解和优化您的应用的功耗。

####计划排定作业
Android 5.0 新增了一个 [JobScheduler][120] API，允许您定义一些在稍后或指定条件下（如设备充电时）以异步方式运行在系统中的作业，从而优化电池寿命。下列情形下，作业计划排定功能很有用：

* 应用具有不面向用户并且可以推迟的作业
* 应用具有您更愿意在设备插入电源时再进行的作业
* 应用具有一项需要接入网络或连接 WLAN 的任务。
* 应用具有多项您希望定期以批处理方式运行的任务。

一个作业单位由一个 [JobInfo][121] 对象封装。该对象指定计划排定标准。  
使用 [JobInfo.Builder][122] 类可配置应如何运行已排计划的任务。您可以安排任务在特定条件下运行，例如：

* 在设备充电时启动
* 在设备连入无限流量网络时启动
* 在设备空闲时启动
* 在特定期限前或以最低延迟完成

例如，您可以添加一段如下代码，在无限流量网络上运行您的任务：
```java
JobInfo uploadTask = new JobInfo.Builder(mJobId,
                                         mServiceComponent /* JobService component */)
        .setRequiredNetworkCapabilities(JobInfo.NetworkType.UNMETERED)
        .build();
JobScheduler jobScheduler =
        (JobScheduler) context.getSystemService(Context.JOB_SCHEDULER_SERVICE);
jobScheduler.schedule(uploadTask);
```
如果设备有稳定的电源（也就是说，设备已插入电源超过 2 分钟，并且电池处于[健康水平][123]），系统将运行任何已做好运行准备的计划作业，无论作业期限是否已过。

要查看如何使用 [JobScheduler][120] API 的示例，请参阅此版本中的 JobSchedulerSample 实现示例。

[120]: https://developer.android.google.cn/reference/android/app/job/JobScheduler.html
[121]: https://developer.android.google.cn/reference/android/app/job/JobInfo.html
[122]: https://developer.android.google.cn/reference/android/app/job/JobInfo.Builder.html
[123]: https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_BATTERY_OKAY

####电池使用开发者工具
新增的 dumpsys batterystats 命令可生成值得关注的设备电池使用情况统计数据，这些数据按唯一身份用户 ID (UID) 加以组织。统计数据包括：

* 电池相关事件的历史记录
* 设备的全局统计数据
* 每个 UID 和系统组件的近似耗电情况
* 每个应用的每数据包移动 ms
* 系统 UID 汇总统计数据
* 应用 UID 汇总统计数据

可使用 --help 选项来了解各种输出定制选项的相关信息。例如，要打印设备上次充电后某个给定应用软件包的电池使用情况统计信息，请运行以下命令：

	$ adb shell dumpsys batterystats --charged <package-name>

您可以使用[电池耗电历史][124]工具对 dumpsys 命令输出的数据进行处理，根据日志生成用电相关事件的 HTML 可视化形式。这些信息可方便您了解和诊断任何电池相关问题。

[124]: https://github.com/google/battery-historian

###工作场所和教育领域中的 Android
####托管配置
Android 5.0 提供了用于在企业环境内运行应用的新功能。如果用户已有个人帐户，则[设备管理员][125]可启动托管配置进程，向设备添加共存但独立的托管配置文件。与托管配置文件关联的应用与非托管应用一并出现在用户的启动器、最近使用的应用屏幕和通知中。

要启动托管配置进程，请通过 [Intent][126] 发送 [ACTION_PROVISION_MANAGED_PROFILE][127]。如果调用成功，系统会触发 [onProfileProvisioningComplete()][128] 回调。然后您就可以调用 [setProfileEnabled()][129] 来启用此托管配置文件。

默认情况下，托管配置文件中只启用了一小部分应用。您可以通过调用 [enableSystemApp()][130] 在托管配置文件中安装更多应用。

如果您要开发启动器应用，可以使用新增的 [LauncherApps][131] 类获取可为当前用户启动的 Activity 以及任何关联托管配置文件的列表。您的启动器可通过向可绘制图标追加工作徽章，以醒目方式显示托管应用。要检索带徽章的图标，请调用 [getUserBadgedIcon()][132]。

要查看如何使用新功能，请参阅此版本中的 BasicManagedProfile 实现示例。

[125]: https://developer.android.google.cn/guide/topics/admin/device-admin.html
[126]: https://developer.android.google.cn/reference/android/content/Intent.html
[127]: https://developer.android.google.cn/reference/android/app/admin/DevicePolicyManager.html#ACTION_PROVISION_MANAGED_PROFILE
[128]: https://developer.android.google.cn/reference/android/app/admin/DeviceAdminReceiver.html#onProfileProvisioningComplete(android.content.Context,%20android.content.Intent)
[129]: https://developer.android.google.cn/reference/android/app/admin/DevicePolicyManager.html#setProfileEnabled(android.content.ComponentName)
[130]: https://developer.android.google.cn/reference/android/app/admin/DevicePolicyManager.html#enableSystemApp(android.content.ComponentName,%20android.content.Intent)
[131]: https://developer.android.google.cn/reference/android/content/pm/LauncherApps.html
[132]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#getUserBadgedIcon(android.graphics.drawable.Drawable,%20android.os.UserHandle)

####设备所有者
Android 5.0 引入了部署设备所有者应用的功能。设备所有者是一种专业化类型的[设备管理员][133]，额外拥有在设备上创建和移除二级用户以及配置全局设置的能力。您的设备所有者应用可以使用 [DevicePolicyManager][134] 类中的方法对托管设备上的配置、安全性和应用进行精细控制。一台设备在同一时间只能有一名活动的设备所有者。

要部署和激活设备所有者，您必须在设备处于未配置状态时执行从编程应用到设备的 NFC 数据传送。此数据传送发送的信息与[托管配置][135]中所述配置 intent 中发送的信息相同。

[133]: https://developer.android.google.cn/guide/topics/admin/device-admin.html
[134]: https://developer.android.google.cn/reference/android/app/admin/DevicePolicyManager.html
[135]: https://developer.android.google.cn/about/versions/android-5.0.html#ManagedProvisioning

####固定屏幕
Android 5.0 引入了一个全新的固定屏幕 API，可让您暂时限制用户离开您的任务或被通知打断。举例来说，如果您要开发一款教育应用来支持 Android 上的高风险评估要求，或者您要开发单一用途或公用电话亭模式下的应用，便可使用此 API。您的应用激活固定屏幕后，在其退出该模式前，用户将无法看到通知，无法访问其他应用，也无法返回主屏幕。

激活固定屏幕的方式有两种：

* 手动方式：用户可以在 Settings > Security > Screen Pinning 中启用固定屏幕，然后通过触摸最近使用的应用屏幕中的绿色大头针图标选择其想固定的任务。
* 编程方式：要以编程方式激活固定屏幕，请在您的应用内调用 [startLockTask()][136]。如果请求应用不是设备所有者，系统会提示用户进行确认。设备所有者应用可以调用 [setLockTaskPackages()][137] 方法，无需执行用户确认步骤便可使应用变为可固定应用。

激活任务锁定时，会发生以下行为：
* 状态栏空白，并隐藏用户通知和状态信息。
* “主屏幕”按钮和“最近用过的应用”按钮处于隐藏状态。
* 其他应用无法启动新 Activity
* 当前应用可以启动新 Activity，前提是这样做不会创建新任务。
* 当设备所有者调用固定屏幕时，用户将一直锁定于您的应用，直至应用调用 [stopLockTask()][138]。
* 如果固定屏幕是由并非设备所有者的其他应用启动的 Activity，或者是由用户直接启动，则用户可通过同时按住“Back”按钮和“Recent”按钮退出。

[136]: https://developer.android.google.cn/reference/android/app/Activity.html#startLockTask()
[137]: https://developer.android.google.cn/reference/android/app/admin/DevicePolicyManager.html#setLockTaskPackages(android.content.ComponentName,%20java.lang.String[])
[138]: https://developer.android.google.cn/reference/android/app/Activity.html#stopLockTask()

###打印框架
####将 PDF 渲染成位图
您现在可以利用新增的 [PdfRenderer][139] 类，将 PDF 文档页面渲染成位图图像后进行打印。您必须指定一个可查找（即内容可随机访问的） [ParcelFileDescriptor][140]，系统会在其上写入可打印内容。您的应用可通过 [openPage()][141] 获得要渲染的页面，然后调用 [render()][142] 将打开的 [PdfRenderer.Page][143] 转换成位图。如果您只想将文档的一部分转换成位图图像（例如，为了实现[平铺渲染][144]以便放大文档），还可以设置其他参数。

要查看新 API 使用方法的示例，请参阅 PdfRendererBasic 示例。

[139]: https://developer.android.google.cn/reference/android/graphics/pdf/PdfRenderer.html
[140]: https://developer.android.google.cn/reference/android/os/ParcelFileDescriptor.html
[141]: https://developer.android.google.cn/reference/android/graphics/pdf/PdfRenderer.html#openPage(int)
[142]: https://developer.android.google.cn/reference/android/graphics/pdf/PdfRenderer.Page.html#render(android.graphics.Bitmap,%20android.graphics.Rect,%20android.graphics.Matrix,%20int)
[143]: https://developer.android.google.cn/reference/android/graphics/pdf/PdfRenderer.Page.html
[144]: http://en.wikipedia.org/wiki/Tiled_rendering

###系统
####应用使用情况统计信息
现在可以利用新增的 [android.app.usage][145] API 访问 Android 设备上的应用使用历史记录。此 API 提供比已弃用的 [getRecentTasks()][146] 方法更为详细的使用信息。要使用此 API，您必须先在清单中声明 "android.permission.PACKAGE_USAGE_STATS" 权限。用户还必须通过 Settings > Security > Apps 为该应用启用访问使用情况的权限。

系统以应用为单位收集使用数据，按天、周、月和年汇总数据。系统保留这些数据的最长持续时间如下：

* 每日数据：7 天
* 每周数据：4 周
* 每月数据：6 个月
* 每年数据：2 年

系统会为每个应用记录以下数据：

* 最后一次使用应用的时间
* 在该时间间隔（以天、周、月或年为单位）内应用位于前台的总时长
* 一天之中当组件（以软件包和 Activity 名称标识）转入前台或后台时记录的时间戳
* 设备配置发生变化（如设备屏幕方向因旋转而发生变化）时记录的时间戳

[145]: https://developer.android.google.cn/reference/android/app/usage/package-summary.html
[146]: https://developer.android.google.cn/reference/android/app/ActivityManager.html#getRecentTasks(int,%20int)

###测试与辅助工具
####测试与辅助工具改进
Android 5.0 添加了以下测试与无障碍功能支持：

* 新增的 [getWindowAnimationFrameStats()][147]和 [getWindowContentFrameStats()][148]方法可采集窗口动画和内容的帧统计信息。这些方法让您可以编写仪器测试，以评估应用渲染帧时的刷新频率是否足以提供流畅的用户体验。
* 新增的 [executeShellCommand()][149] 方法让您可以在仪器测试中执行 shell 命令。命令的执行方式与从已连接到设备的主机运行 adb shell 类似，允许您使用 dumpsys、am、content 和 pm 等基于 shell 的工具。
* 使用无障碍功能 API 的无障碍服务和测试工具（如 [UiAutomator][150]）现在可以检索视力健全的用户可与之交互的屏幕上各窗口属性的相关详细信息。要检索 [AccessibilityWindowInfo][151] 对象列表，请调用新增的 [getWindows()][152] 方法。
* 新增的 [AccessibilityNodeInfo.AccessibilityAction][153] 类允许您定义要在 [AccessibilityNodeInfo][154] 上执行的标准或自定义操作。新增的 [AccessibilityNodeInfo.AccessibilityAction][153] 类取代了以前在 [AccessibilityNodeInfo][154] 中提供的与操作有关的 API。
* Android 5.0 可对您应用内的文本语音转换合成进行更精细的控制。新增的 [Voice][155] 类允许您的应用使用关联了特定语言区域、质量和延时评级以及文本语音转换引擎专属参数的语音配置文件。

[147]: https://developer.android.google.cn/reference/android/app/UiAutomation.html#getWindowAnimationFrameStats()
[148]: https://developer.android.google.cn/reference/android/app/UiAutomation.html#getWindowContentFrameStats(int)
[149]: https://developer.android.google.cn/reference/android/app/UiAutomation.html#executeShellCommand(java.lang.String)
[150]: https://developer.android.google.cn/tools/help/uiautomator/index.html
[151]: https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityWindowInfo.html
[152]: https://developer.android.google.cn/reference/android/accessibilityservice/AccessibilityService.html#getWindows()
[153]: https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityNodeInfo.AccessibilityAction.html
[154]: https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityNodeInfo.html
[155]: https://developer.android.google.cn/reference/android/speech/tts/Voice.html

###IME
####更方便的输入语言切换
从 Android 5.0 开始，用户可以更方便地在平台支持的所有[输入法编辑器 (IME)][156] 之间切换。执行指定的切换操作（通常是触摸软键盘上的地球图标）可在所有此类 IME 中循环切换。此行为变更是由 [shouldOfferSwitchingToNextInputMethod()][157] 方法实现的。

此外，框架现在会检查下一个 IME 是否具有切换机制（并进而检查该 IME 是否支持切换到其后的 IME）。具有切换机制的 IME 将不会循环切换到不具有该机制的 IME。此行为变更是由 [switchToNextInputMethod()][158] 方法实现的。

要查看如何使用更新后的 IME 切换 API 的示例，请参阅此版本中更新后的软键盘实现示例。要详细了解如何实现 IME 切换，请参阅[创建输入法][159]。

[156]: https://developer.android.google.cn/guide/topics/text/creating-input-method.html
[157]: https://developer.android.google.cn/reference/android/view/inputmethod/InputMethodManager.html#shouldOfferSwitchingToNextInputMethod(android.os.IBinder)
[158]: https://developer.android.google.cn/reference/android/view/inputmethod/InputMethodManager.html#switchToNextInputMethod(android.os.IBinder,%20boolean)
[159]: https://developer.android.google.cn/guide/topics/text/creating-input-method.html

###清单声明
####可声明的必备功能
现在支持在 [`<uses-feature>`][160] 元素中使用以下值，以便您确保只在提供应用所需功能的设备上安装您的应用。

* [FEATURE_AUDIO_OUTPUT][161]
* [FEATURE_CAMERA_CAPABILITY_MANUAL_POST_PROCESSING][162]
* [FEATURE_CAMERA_CAPABILITY_MANUAL_SENSOR][163]
* [FEATURE_CAMERA_CAPABILITY_RAW][164]
* [FEATURE_CAMERA_LEVEL_FULL][165]
* [FEATURE_GAMEPAD][166]
* [FEATURE_LIVE_TV][167]
* [FEATURE_MANAGED_USERS][168]
* [FEATURE_LEANBACK][169]
* [FEATURE_OPENGLES_EXTENSION_PACK][170]
* [FEATURE_SECURELY_REMOVES_USERS][171]
* [FEATURE_SENSOR_AMBIENT_TEMPERATURE][172]
* [FEATURE_SENSOR_HEART_RATE_ECG][173]
* [FEATURE_SENSOR_RELATIVE_HUMIDITY][174]
* [FEATURE_VERIFIED_BOOT][175]
* [FEATURE_WEBVIEW][176]

[160]: https://developer.android.google.cn/guide/topics/manifest/uses-feature-element.html
[161]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_AUDIO_OUTPUT
[162]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_CAMERA_CAPABILITY_MANUAL_POST_PROCESSING
[163]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_CAMERA_CAPABILITY_MANUAL_SENSOR
[164]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_CAMERA_CAPABILITY_RAW
[165]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_CAMERA_LEVEL_FULL
[166]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_GAMEPAD
[167]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_LIVE_TV
[168]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_MANAGED_USERS
[169]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_LEANBACK
[170]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_OPENGLES_EXTENSION_PACK
[171]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_SECURELY_REMOVES_USERS
[172]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_SENSOR_AMBIENT_TEMPERATURE
[173]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_SENSOR_HEART_RATE_ECG
[174]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_SENSOR_RELATIVE_HUMIDITY
[175]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_VERIFIED_BOOT
[176]: https://developer.android.google.cn/reference/android/content/pm/PackageManager.html#FEATURE_WEBVIEW

####用户权限
现在，[`<uses-permission>`][177] 元素中支持以下权限，以声明您的应用访问特定 API 所需的权限。
* [BIND_DREAM_SERVICE][178]：如果针对的是 API 级别 21 及更高级别，则[互动屏保][179]服务必须获得该权限才能确保只有系统可与其绑定。

[177]: https://developer.android.google.cn/guide/topics/manifest/uses-permission-element.html
[178]: https://developer.android.google.cn/reference/android/Manifest.permission.html#BIND_DREAM_SERVICE
[179]: https://developer.android.google.cn/about/versions/android-4.2.html#Daydream

# Android O功能和APIs
Android O为用户和开发者引入多种新功能。本文重点介绍面向开发者的新功能。

请务必查阅[Android O行为变更](behavior-changes.html)以了解平台变更可能影响您的应用的领域。

### 通知
在Android O中，我们已重新设计通知，以便为管理通知行为和设置提供更轻松和更统一的方式。这些变更包括：
* 通知渠道：Android O引入了通知渠道，其允许您为要显示的每种通知类型创建用户可自定义的渠道。用户界面将通知渠道称之为通知类别。要了解如何实现通知渠道的信息，请参阅[通知渠道](features/notification-channels.html)指南。
* 休眠：用户可以将通知置于休眠状态，以便稍后重新显示它。重新显示时通知的重要程度与首次显示时相同。应用可以移除或更新已休眠的通知，但更新休眠的通知并不会使其重新显示。
* 通知超时：现在，使用`Notification.Builder.setTimeout()`创建通知时您可以设置超时。您可以使用此方法指定一个持续时间，过了该持续时间后取消通知。如果需要，您可以在指定的超时持续时间之前取消通知。
* 通知清除：系统现在可区分通知是由用户清除，还是由应用移除。要查看清除通知的方式，您应实现[NotificationListenerService](https://developer.android.google.cn/reference/android/service/notification/NotificationListenerService.html)类的新[onNotificationRemoved()](https://developer.android.google.cn/reference/android/service/notification/NotificationListenerService.html#onNotificationRemoved(android.service.notification.StatusBarNotification, android.service.notification.NotificationListenerService.RankingMap, int))方法。
* 背景颜色：您现在可以设置和启用通知的背景颜色。只能在用户必须一眼就能看到的持续任务的通知中使用此功能。例如，您可以为与驾车路线或正在进行的通话有关的通知设置背景颜色。您还可以使用[Notification.Builder.setColor()](https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setColor(int))设置所需的背景颜色。这样做将允许您使用[Notification.Builder.setColorized()](https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setColorized(boolean))启用通知的背景颜色设置。
消息样式：现在，使用[MessagingStyle](https://developer.android.google.cn/reference/android/app/Notification.MessagingStyle.html)类的通知可在其折叠形式中显示更多内容。对于与消息有关的通知，您应使用[MessagingStyle](https://developer.android.google.cn/reference/android/app/Notification.MessagingStyle.html)类。您还可以使用新的[addHistoricMessage()](https://developer.android.google.cn/reference/android/app/Notification.MessagingStyle.html#addHistoricMessage(android.app.Notification.MessagingStyle.Message))方法，通过向与消息相关的通知添加历史消息为会话提供上下文。

![](../images/notifications-inline-controls1.png) ![](../images/notifications-inline-controls2.png)
**图 1.** 右侧屏幕所示为 Android O 中通知的嵌入式控件。

### 自动填充框架
帐号创建、登录和信用卡交易需要时间并且容易出错。在使用要求执行此类重复性任务的应用时，用户很容易遭受挫折。

Android O通过引入自动填充框架，简化了登录和信用卡表单之类表单的填写工作。在用户选择接受自动填充之后，新老应用都可使用自动填充框架。

您可以采取某些措施，优化您的应用使用此框架的方式。如需了解详细信息，请参阅[自动填充框架概览](features/autofill.html)。

### 画中画模式
Android O允许以画中画(PIP)模式启动Activity。PIP是一种特殊的多窗口模式，最常用于视频播放。目前，PIP模式可用于Android TV，而 Android O则让该功能可进一步用于其他Android设备。

当某个 Activity 处于 PIP 模式时，它会处于暂停状态，但仍应继续显示内容。因此，您应确保您的应用在[onPause()](https://developer.android.google.cn/reference/android/app/Activity.html#onPause())处理程序中进行处理时不会暂停播放。相反，您应在[onStop()](https://developer.android.google.cn/reference/android/app/Activity.html#onStop())中暂停播放视频，并在[onStart()](https://developer.android.google.cn/reference/android/app/Activity.html#onStart())中继续播放。如需了解详细信息，请参阅多窗口生命周期。

要指定您的Activity可以使用PIP模式，请在清单中将`android:supportsPictureInPicture`设置为true。（从Android O开始，如果您打算在 Android TV或其他Android设备上支持PIP模式，则无需将`android:resizeableActivity`设置为true；只有在您的Activity支持其他多窗口模式时，才需要设置`android:resizeableActivity。`）

### API变更
Android O =引入一种新的对象`android.app.PictureInPictureArgs`，您可以将该对象传递给PIP方法来指定某个Activity在其处于PIP模式时的行为。此对象还指定了各种属性，例如Activity的首选纵横比。

现在，在[添加画中画](https://developer.android.google.cn/training/tv/playback/picture-in-picture.html)中介绍的现有PIP方法可用于所有Android设备，而不仅限于Android TV。此外，Android O还提供以下方法来支持PIP模式：

* `Activity.enterPictureInPictureMode(PictureInPictureArgs args)`：将Activity置于画中画模式。Activity的纵横比和其他配置设置均由`args`指定。如果`args`中的任何字段为空，系统将使用您上次调用`Activity.setPictureInPictureArgs()`时所设置的值。
指定的Activity被置于屏幕的一角，屏幕剩余部分则被屏幕之前显示的上一Activity所填满。进入PIP模式的Activity将进入暂停状态，但仍保持已启动状态。如果用户点按此PIP Activity，系统将显示一个菜单供用户操作，而在Activity处于PIP状态期间，不会理会任何触摸事件。
* `Activity.setPictureInPictureArgs()`更新Activity的PIP配置设置。如果Activity目前处于PIP模式，则会更新此设置；如果 Activity的纵横比发生变化，这非常有用。如果Activity不处于PIP模式，则会使用这些配置设置，而不会考虑您调用的`enterPictureInPictureMode()`方法。

### 处理字体
Android O推出一项新功能，即XML中的字体，允许您使用字体作为资源。这意味着，不再需要以资产的形式捆绑字体。字体在`R`文件中编译，并且作为一种资源，可自动用于系统。然后，您可以利用一种新的资源类型`font`来访问这些字体。Android O还提供了一种机制，可用来检索与系统字体有关的信息并提供文件描述符。如需了解有关以资源形式使用字体以及检索系统字体有关的详细信息，请参阅[处理字体](features/working-with-fonts.html)。

### 自适应图标
Android O 引入自适应启动器图标。自适应图标支持视觉效果，可在不同设备型号上显示为各种不同的形状。要了解如何创建自适应图标，请参阅[自适应图标](features/adaptive-icons.html)预览功能指南。

### 颜色管理
图像应用的Android开发者现在可以利用支持广色域彩色显示的新设备。要显示广色域图像，应用需要在其清单（每个Activity）中启用一个标志，并加载具有嵌入的广域彩色配置文件（AdobeRGB、Pro Photo RGB、DCI-P3 等）的位图。

### WLAN感知
Android O新增了对WLAN感知的支持，此技术基于周边感知联网(NAN)规范。在具有相应WLAN感知硬件的设备上，应用和附近设备可以通过WLAN进行搜索和通信，无需依赖互联网接入点。我们正在与硬件合作伙伴合作，以尽快将WLAN感知技术应用于设备。有关如何将WLAN感知集成到您的应用中的详细信息，请参阅[WLAN感知](features/wifi-aware.html)。

### 配套设备配对
在尝试通过蓝牙、BLE和WLAN与配套设备配对时，Android O提供的API允许您自定义配对请求对话框。如需了解详细信息，请参阅[配套设备配对](features/companion-device-pairing.html)。

如需了解有关在Android上使用蓝牙的详细信息，请参阅[蓝牙](https://developer.android.google.cn/guide/topics/connectivity/bluetooth.html)指南。有关对蓝牙所作的特定于[Android O的变更](behavior-changes.html)，请参阅Android O行为变更页面的[蓝牙](https://developer.android.google.cn/guide/topics/connectivity/bluetooth.html)部分。

### WebView API
Android O提供多种API，帮助您管理在应用中显示网页内容的[WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)对象。这些API可增强应用的稳定性和安全性，它们包括：
* Version API
* Google SafeBrowsing API
* Termination Handle API
* Renderer Importance API
如需了解有关这些API用法的更多信息，请参阅[管理WebView](https://developer.android.google.cn/preview/features/managing-webview.html)。

### 固定快捷方式和小部件
Android O引入了快捷方式和小部件的应用内固定功能。在您的应用中，您可以根据用户权限为支持的启动器创建固定的快捷方式和小部件。

如需了解详细信息，请参阅[固定快捷方式和小部件](https://developer.android.google.cn/preview/features/pinning-shortcuts-widgets.html)预览功能指南。

### 无障碍功能
Android O支持开发者使用以下无障碍功能创建自己的无障碍服务：
* 语言检测

  要识别“文本到语音转换(TTS)”工具已在某个文本范围内识别的语言，请使用`TextClassificationManager.detectLanguages()`。此方法出现在Android O中引入的[TextClassificationManager](https://developer.android.google.cn/reference/android/view/textclassifier/TextClassificationManager.html)类中。您可以使用生成的`android.view.textclassifier.TextLanguage`对象列表来确定哪些文本区域已被指定为某种特定的语言以及 TTS 有多大把握为某个特定的文本子集指定语言。
* 无障碍功能按钮

  通过在[android:accessibilityFlags](https://developer.android.google.cn/reference/android/accessibilityservice/AccessibilityServiceInfo.html#attr_android:accessibilityFlags)属性中设置[FLAG_REQUEST_ACCESSIBILITY_BUTTON](https://developer.android.google.cn/reference/android/accessibilityservice/AccessibilityServiceInfo.html#FLAG_REQUEST_ACCESSIBILITY_BUTTON)标志，您的服务可以请求在系统的导航区域显示无障碍功能按钮。此按钮为用户从设备上的任何屏幕启动服务功能提供了一种快速的方式。您的服务可以使用[registerAccessibilityButtonCallback()](https://developer.android.google.cn/reference/android/accessibilityservice/AccessibilityButtonController.html#registerAccessibilityButtonCallback(android.accessibilityservice.AccessibilityButtonController.AccessibilityButtonCallback))注册按钮交互回调。

  > **注**：此功能仅适用于提供软件渲染导航区域的设备。使用 isAccessibilityButtonAvailable() 方法和 onAvailabilityChanged() 回调可跟踪无障碍功能按钮的可用性。请确保您的服务的用户在无障碍功能按钮不可用时可以通过其他方式访问相关功能。

* 指纹手势

  您的无障碍服务也可以响应替代的输入机制，即沿设备的指纹传感器按特定方向滑动（上、下、左和右）。要获取有关这些交互的回调，请完成以下顺序步骤：

  1.声明[USE_FINGERPRINT](https://developer.android.google.cn/reference/android/Manifest.permission.html#USE_FINGERPRINT)权限和`AccessibilityServiceInfo.CAPABILITY_CAN_CAPTURE_FINGERPRINT_GESTURES`功能。
  2. 在[android:accessibilityFlags](https://developer.android.google.cn/reference/android/accessibilityservice/AccessibilityServiceInfo.html#attr_android:accessibilityFlags)属性中设置`AccessibilityServiceInfo.FLAG_CAPTURE_FINGERPRINT_GESTURES`标志。
  3. 使用[registerFingerprintGestureCallback()](https://developer.android.google.cn/reference/android/accessibilityservice/FingerprintGestureController.html#registerFingerprintGestureCallback(android.accessibilityservice.FingerprintGestureController.FingerprintGestureCallback, android.os.Handler))注册回调。

  请记住，并非所有设备都包含指纹传感器。您可以使用[isHardwareDetected()](https://developer.android.google.cn/reference/android/hardware/fingerprint/FingerprintManager.html#isHardwareDetected())方法识别设备是否支持此传感器。即使对于包含指纹传感器的设备，您的服务也只有在指纹传感器不用于身份验证目的时才可使用它。要识别此传感器何时可用，请调用[isGestureDetectionAvailable()](https://developer.android.google.cn/reference/android/accessibilityservice/FingerprintGestureController.html#isGestureDetectionAvailable())方法并实现[onGestureDetectionAvailabilityChanged()](https://developer.android.google.cn/reference/android/accessibilityservice/FingerprintGestureController.FingerprintGestureCallback.html#onGestureDetectionAvailabilityChanged(boolean))回调。

* 字词级突出显示
  要确定[TextView](https://developer.android.google.cn/reference/android/widget/TextView.html)对象中可见字符的位置，您可以在[EXTRA_DATA_TEXT_CHARACTER_LOCATION_KEY](https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityNodeInfo.html#EXTRA_DATA_TEXT_CHARACTER_LOCATION_KEY)中将其作为第一个参数传递到[refreshWithExtraData()](https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityNodeInfo.html#refreshWithExtraData(java.lang.String, android.os.Bundle))中。随后会更新您为[refreshWithExtraData()](https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityNodeInfo.html#refreshWithExtraData(java.lang.String, android.os.Bundle))提供的作为第二个参数的[Bundle](https://developer.android.google.cn/reference/android/os/Bundle.html)对象，使之包含一个可打包的[Rect](https://developer.android.google.cn/reference/android/graphics/Rect.html)对象数组。每个[Rect](https://developer.android.google.cn/reference/android/graphics/Rect.html)对象代表某个特定字符的边界框。
* 提示文本
  您的服务可以使用[AccessibilityNodeInfo](https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityNodeInfo.html)类中的[getHintText()](https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityNodeInfo.html#getHintText())方法，获取[EditText](https://developer.android.google.cn/reference/android/widget/EditText.html)对象的提示文本。即使某个特定的[EditText](https://developer.android.google.cn/reference/android/widget/EditText.html)对象当前未显示提示文本，[getHintText()](https://developer.android.google.cn/reference/android/view/accessibility/AccessibilityNodeInfo.html#getHintText())方法仍将为您的服务提供提示文本。
* 连续的手势分派
  您的服务现在可以使用[GestureDescription.StrokeDescription](https://developer.android.google.cn/reference/android/accessibilityservice/GestureDescription.StrokeDescription.html)构造函数中的最后一个参数`isContinued`，指定属于同一设定手势的笔划的顺序。

如需了解有关如何让您的应用更便于访问的更多信息，请参阅[无障碍功能](https://developer.android.google.cn/guide/topics/ui/accessibility/index.html)。

### 权限
Android O中引入了一项新权限`android.permission.ANSWER_PHONE_CALLS`，使用此权限，应用可按设定的方式接听拨入的电话。此权限被划分为[危险](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous)类别，属于[PHONE](https://developer.android.google.cn/reference/android/Manifest.permission_group.html#PHONE)权限组。

要在应用中处理拨入的电话，您可以使用[TelecomManager](https://developer.android.google.cn/reference/android/telecom/TelecomManager.html)类中的`acceptRingingCall()`方法。

### 内容提供程序分页
我们已更新内容提供程序以支持加载大型数据集，每次加载一页。例如，一个具有大量图像的照片应用可查询要在页面中显示的数据的子集。内容提供程序返回的每个结果页面由一个 Cursor 对象表示。客户端和提供程序必须实现分页才能利用此功能。

如需了解有关内容提供程序变更的详细信息，请参阅[ContentProvider](https://developer.android.google.cn/reference/android/content/ContentProvider.html)和[ContentProviderClient](https://developer.android.google.cn/reference/android/content/ContentProviderClient.html)。

### 媒体增强功能
#### 媒体指标
新的`getMetrics()`方法将返回一个包含配置和性能信息的[Bundle](https://developer.android.google.cn/reference/android/os/Bundle.html)对象，用一个包含属性和值的地图表示。为以下媒体类定义`getMetrics()`方法：
* [MediaPlayer.getMetrics()](https://developer.android.google.cn/reference/android/media/MediaPlayer.html#getMetrics())
* [MediaRecorder.getMetrics()](https://developer.android.google.cn/reference/android/media/MediaRecorder.html#getMetrics())
* [MediaCodec.getMetrics()](https://developer.android.google.cn/reference/android/media/MediaCodec.html#getMetrics())
* [MediaExtractor.getMetrics()](https://developer.android.google.cn/reference/android/media/MediaExtractor.html#getMetrics())

为每个实例单独收集指标，并持续到实例的生命周期结束为止。如果没有可用的指标，则此方法将返回null。返回的实际指标取决于类。

#### MediaPlayer
Android O为MediaPlayer类添加了多种新方法。这些方法可以从多个方面增强您的应用处理媒体播放的能力：
* 通过控制[缓冲行为](https://developer.android.google.cn/preview/features/media-player.html#buffering)改进性能的功能。
* 在[搜索](https://developer.android.google.cn/preview/features/media-player.html#seeking)帧时进行精细控制。
* 播放[受数字版权管理保护的](https://developer.android.google.cn/preview/features/media-player.html#drm"")材料的功能。

#### 音频录制器
* 音频录制器现在支持对流式传输有用的MPEG2_TS格式：
  ```java
  mMediaRecorder.setOutputFormat(MediaRecorder.OutputFormat.MPEG_2_TS);
  ```
  请参阅[MediaRecorder.OutputFormat](https://developer.android.google.cn/reference/android/media/MediaRecorder.OutputFormat.html)
* [MediaMuxer](https://developer.android.google.cn/reference/android/media/MediaMuxer.html)现在可以处理任意数量的音频和视频流，而不再仅限于一个音频曲目和/或一个视频曲目。使用[addTrack()](https://developer.android.google.cn/reference/android/media/MediaMuxer.html#addTrack(android.media.MediaFormat))可混录所需的任意数量的曲目。
* [MediaMuxer](https://developer.android.google.cn/reference/android/media/MediaMuxer.html)还可以添加一个或多个包含用户定义的每帧信息的元数据曲目。元数据的格式由您的应用定义。仅对MP4容器支持元数据曲目。

元数据可以用于离线处理。例如，传感器的陀螺仪信号可以用于执行视频稳定操作。

在添加元数据曲目时，曲目的MIME格式必须以前缀“application/”开头。除了数据不是来源于`MediaCodec`以外，写入元数据的操作与写入视频/音频数据相同。相反，应用将包含相关时间戳的`ByteBuffer`传递给[writeSampleData()](https://developer.android.google.cn/reference/android/media/MediaMuxer.html#writeSampleData(int, java.nio.ByteBuffer, android.media.MediaCodec.BufferInfo))方法。时间戳必须和视频及音频曲目处于相同的时基。

生成的MP4文件使用ISOBMFF的12.3.3.2部分定义的`TextMetaDataSampleEntry`，指示元数据的MIME格式。在使用[MediaExtractor](https://developer.android.google.cn/reference/android/media/MediaExtractor.html)提取包含元数据曲目的文件时，元数据的MIME格式将提取到[MediaFormat](https://developer.android.google.cn/reference/android/media/MediaFormat.html)中。

### 多显示器支持
从Android O开始，此平台为多显示器提供增强的支持。如果Activity支持多窗口模式，并且在具有多显示器的设备上运行，则用户可以将Activity从一个显示器移动到另一个显示器。当应用启动Activity时，此应用可指定Activity应在哪个显示器上运行。

> 注：如果Activity支持多窗口模式，则Android O将为该Activity自动启用多显示器支持。您应测试您的应用，确保它在多显示器环境下可正常运行。

每次只有一个Activity可以处于继续状态，即使此应用具有多个显示器。具有焦点的Activity将处于继续状态，所有其他可见的Activity均暂停，但不会停止。如需了解有关当多个Activity可见时活动生命周期的详细信息，请参阅[多窗口生命周期](https://developer.android.google.cn/guide/topics/ui/multi-window.html#lifecycle)。

当用户将 Activity 从一个显示器移动到另一个显示器时，系统将调整 Activity 大小，并根据需要发起运行时变更。您的 Activity 可以自行处理配置变更，或允许系统销毁包含该 Activity 的进程，并以新的尺寸重新创建它。如需了解详细信息，请参[阅处理配置变更](https://developer.android.google.cn/guide/topics/resources/runtime-changes.html)。

#### API变更
[ActivityOptions](https://developer.android.google.cn/reference/android/app/ActivityOptions.html)提供两个新方法以支持多个显示器：

[setLaunchDisplayId()](https://developer.android.google.cn/reference/android/app/ActivityOptions.html#setLaunchDisplayId(int))

指定 Activity 在启动后应显示在哪个显示器上。

[getLaunchDisplayId()](https://developer.android.google.cn/reference/android/app/ActivityOptions.html#getLaunchDisplayId())

返回 Activity 的当前启动显示器。

#### 工具更新
对adb shell进行了扩展，以支持多个显示器。shell start命令现在可用于启动Activity，并指定Activity的目标显示器：
```shell
adb shell start <activity_name> --display <display_id>
```

### 新的帐号访问和Discovery API
Android O对应用访问用户帐号的方式引入多项改进。对于由身份验证器管理的帐号，身份验证器在决定对应用隐藏帐号还是显示帐号时可以使用自己的政策。Android系统跟踪可以访问特定帐号的应用。

在以前的Android版本中，想要跟踪用户帐号列表的应用必须获取有关所有帐号的更新，包括具有不相关类型的帐号。Android O添加了 [addOnAccountsUpdatedListener(android.accounts.OnAccountsUpdateListener, android.os.Handler, boolean, java.lang.String[])](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#addOnAccountsUpdatedListener(android.accounts.OnAccountsUpdateListener, android.os.Handler, boolean, java.lang.String[]))方法，其允许应用指定应接收帐号变更的帐号类型列表。

#### API变更
AccountManager提供六个新方法以帮助身份验证器管理哪些应用可以查看帐号：
* [setAccountVisibility(android.accounts.Account, java.lang.String, int)](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#setAccountVisibility(android.accounts.Account, java.lang.String, int))：针对特定用户帐号和软件包组合设置可见性级别。
* [getAccountVisibility(android.accounts.Account, java.lang.String)](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#getAccountVisibility(android.accounts.Account, java.lang.String))：获取特定用户帐号和软件包组合的可见性级别。
* [getAccountsAndVisibilityForPackage(java.lang.String, java.lang.String)](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#getAccountsAndVisibilityForPackage(java.lang.String, java.lang.String))：允许身份验证器获取帐号和给定软件包的可见性级别。
[getPackagesAndVisibilityForAccount(android.accounts.Account)](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#getPackagesAndVisibilityForAccount(android.accounts.Account))：允许身份验证器获取存储的给定帐号的可见性值。
[addAccountExplicitly(android.accounts.Account, java.lang.String, android.os.Bundle, java.util.Map<java.lang.String, java.lang.Integer>)](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#addAccountExplicitly(android.accounts.Account, java.lang.String, android.os.Bundle, java.util.Map<java.lang.String, java.lang.Integer>))：允许身份验证器初始化帐号的可见性值。
[addOnAccountsUpdatedListener(android.accounts.OnAccountsUpdateListener, android.os.Handler, boolean, java.lang.String[])](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#addOnAccountsUpdatedListener(android.accounts.OnAccountsUpdateListener, android.os.Handler, boolean, java.lang.String[]))：将[OnAccountsUpdateListener](https://developer.android.google.cn/reference/android/accounts/OnAccountsUpdateListener.html)侦听器添加到[AccountManager](https://developer.android.google.cn/reference/android/accounts/AccountManager.html)对象。无论设备上的帐号列表何时发生变化，系统都将调用此侦听器。

Android O引入两个特殊的软件包名称值，以使用[setAccountVisibility(android.accounts.Account, java.lang.String, int)](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#setAccountVisibility(android.accounts.Account, java.lang.String, int))方法指定未设置的应用的可见性级别。[PACKAGE_NAME_KEY_LEGACY_VISIBLE](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#PACKAGE_NAME_KEY_LEGACY_VISIBLE)可见性值应用于具有[GET_ACCOUNTS](https://developer.android.google.cn/reference/android/Manifest.permission.html#GET_ACCOUNTS)权限的应用，并且其目标Android版本低于Android O，或其签名与针对任意Android版本的身份验证器匹配。[PACKAGE_NAME_KEY_LEGACY_NOT_VISIBLE](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#PACKAGE_NAME_KEY_LEGACY_NOT_VISIBLE)为之前未设置的应用提供默认的可见性值，对于此类应用，[PACKAGE_NAME_KEY_LEGACY_NOT_VISIBLE](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#PACKAGE_NAME_KEY_LEGACY_NOT_VISIBLE)不适用。

如需了解有关新的帐号访问和Discovery API的详细信息，请参阅对[AccountManager]((https://developer.android.google.cn/reference/android/accounts/AccountManager.html)) 和[OnAccountsUpdateListener](https://developer.android.google.cn/reference/android/accounts/OnAccountsUpdateListener.html)的参考。

### AnimatorSet
从Android O开始，[AnimatorSet](https://developer.android.google.cn/reference/android/animation/AnimatorSet.html) API现在支持寻道和倒播功能。寻道功能允许您将动画的位置设置为指定的时间点处。如果您的应用包含可撤消的动作的动画，倒播功能会很有用。现在，您不必定义两组独立的动画，而只需反向播放同一组动画。

### 自动调整TextView的大小
Android O允许您根据TextView的大小自动设置文本展开或收缩的大小。这意味着，在不同屏幕上优化文本大小或者优化包含动态内容的文本大小比以往简单多了。如需了解有关如何在Android O中自动调整TextView的大小的详细信息，请参阅[自动调整TextView的大小](https://developer.android.google.cn/preview/features/autosizing-textview.html)。

### 应用类别
在适当的情况下，Android O允许每个应用声明它们所属的类别。这些类别用于将应用呈现给用户的用途或功能相同的应用归类在一起，例如按流量消耗、电池消耗和存储消耗将应用归类。您可以在`<application>`清单标记中设置`android:appCategory`属性，定义应用的类别。

### 新的StrictMode检测程序
Android O添加了三个新的StrictMode检测程序，帮助识别应用可能出现的错误：
* [detectUnbufferedIo()](https://developer.android.google.cn/reference/android/os/StrictMode.ThreadPolicy.Builder.html#detectUnbufferedIo())将检测您的应用何时读取或写入未缓冲的数据，这可能极大影响性能。
* [detectContentUriWithoutPermission()](https://developer.android.google.cn/reference/android/os/StrictMode.VmPolicy.Builder.html#detectContentUriWithoutPermission())将检测您的应用在其外部启动Activity时何时意外忘记向其他应用授予权限。
* [detectUntaggedSockets()](https://developer.android.google.cn/reference/android/os/StrictMode.VmPolicy.Builder.html#detectUntaggedSockets())将检测您的应用何时使用网络流量，而不使用[setThreadStatsTag(int)](https://developer.android.google.cn/reference/android/net/TrafficStats.html#setThreadStatsTag(int))将流量标记用于调试目的。

### 缓存数据
Android O围绕缓存数据提供更加出色的导航和性能。现在，每个应用均获得一定的磁盘空间配额，用于存储[getCacheQuotaBytes(UUID)](https://developer.android.google.cn/reference/android/os/storage/StorageManager.html#getCacheQuotaBytes(java.util.UUID))返回的缓存数据。

当系统需要释放磁盘空间时，将开始从超过配额最多的应用中删除缓存文件。因此，如果将您的缓存数据量始终保持低于配额的水平，则在必须清除系统中的某些文件时，您的缓存文件将能坚持到最后。系统在决定删除您的应用中的哪些缓存文件时，将首先考虑删除最旧的文件（由修改时间决定）。

您还可以针对每个目录启用两种新行为，以控制系统如何释放缓存数据：
* `StorageManager.setCacheBehaviorAtomic()`可用于指示某个目录及其所有内容应作为一个不可分割的整体进行删除。
* [setCacheBehaviorTombstone(File, boolean)](https://developer.android.google.cn/reference/android/os/storage/StorageManager.html#setCacheBehaviorTombstone(java.io.File, boolean))可用于指示不应删除某个目录内的文件，而应将它们截断到0字节长度，使空文件保持完好。

最后，在需要为大文件分配磁盘空间时，可考虑使用新的[allocateBytes(FileDescriptor, long)](https://developer.android.google.cn/reference/android/os/storage/StorageManager.html#allocateBytes(java.io.FileDescriptor, long)) API，它将自动清除属于其他应用的缓存文件（根据需要），以满足您的请求。在确定设备是否有足够的磁盘空间保存您的新数据时，请调用[getAllocatableBytes(UUID)](https://developer.android.google.cn/reference/android/os/storage/StorageManager.html#getAllocatableBytes(java.util.UUID))而不要使用[getUsableSpace()](https://developer.android.google.cn/reference/java/io/File.html#getUsableSpace())，因为前者会考虑系统要为您清除的任何缓存数据。

### 增强的媒体文件访问功能
[存储访问框架(SAF)](https://developer.android.google.cn/guide/topics/providers/document-provider.html)允许应用显示自定义[DocumentsProvider](https://developer.android.google.cn/reference/android/provider/DocumentsProvider.html)，后者可以为其他应用提供访问数据源中的文件的权限。事实上，文档提供程序甚至可以提供驻留在网络存储区或使用[媒体传输协议(MTP)](https://en.wikipedia.org/wiki/Media_Transfer_Protocol)等协议的文件的访问权限。

但是，访问远程数据源中的大媒体文件面临一些挑战。媒体播放器需要以寻址方式访问来自文档提供程序的文件。当大媒体文件驻留在远程数据源上时，文档提供程序必须事先提取所有数据，并创建快照文件描述符。媒体播放器无法播放没有文件描述符的文件，因此在文档提供程序完成文件下载前，无法开始播放。

从Android O开始，存储访问框架允许[自定义文档提供程序](https://developer.android.google.cn/guide/topics/providers/create-document-provider.html)为驻留在远程数据源中的文件创建可寻址的文件描述符。SAF 可打开文件，获取原生可寻址的文件描述符。然后 SAF 向文档提供程序提交离散字节请求。此功能使文档提供程序可以返回媒体播放器应用请求的准确字节范围，而不必事先缓存整个文件。

要使用此功能，您需要调用新的[StorageManager.openProxyFileDescriptor()](https://developer.android.google.cn/reference/android/os/storage/StorageManager.html#openProxyFileDescriptor(int, android.os.ProxyFileDescriptorCallback, android.os.Handler))方法。[openProxyFileDescriptor()](https://developer.android.google.cn/reference/android/os/storage/StorageManager.html#openProxyFileDescriptor(int, android.os.ProxyFileDescriptorCallback, android.os.Handler))方法可接受[ProxyFileDescriptorCallback](https://developer.android.google.cn/reference/android/os/ProxyFileDescriptorCallback.html)对象作为回调。任何时候，当客户端应用对文档提供程序返回的文件描述符执行文件操作时，SAF都会调用回调。

### 企业中的Android
Android企业版为运行Android O的设备引入多种新功能和API。我们改进了个人资料所有者和设备所有者管理模式，使其比以往任何时候都更强大、高效，也更容易设置。我们还启用了全新的部署方案。

一些值得关注的突出功能包括：
* 在企业拥有的设备上使用托管个人资料的功能。
* 大刀阔斧地改进了工作资料，采用易于使用的设置流程，显著缩短设置时间。
* 基于文件的企业级加密管理。
* 应用管理API委派。
* 如需了解有关Android O中面向Android企业版的新API和新功能的更多信息，请参阅[企业中的Android](https://developer.android.google.cn/preview/features/work.html)页面。

### Java编程语言更新
在Android O中，我们将为Android添加OpenJDK Java语言功能。我们将添加来自OpenJDK 8的`java.time`以及`java.nio.file`和`java.lang.invoke`，包括来自OpenJDK 7的`MethodHandle`。请查阅新程序包中的[API 差异报告](https://developer.android.google.cn/sdk/api_diff/o-dp1/changes.html)。

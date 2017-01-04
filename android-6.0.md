# Android 6.0 APIs
Android 6.0（M）为用户和应用开发者提供了新功能。本文档介绍了最值得注意的API。

#### 开始开发
要开始为Android 6.0构建应用，您必须先[获取Android SDK](https://developer.android.com/studio/index.html)。然后使用[SDK Manager](https://developer.android.com/studio/intro/update.html)下载Android 6.0 SDK平台和系统映像。

#### 更新您的目标API级别
为了更好地为运行Android的设备优化您的应用，请将[targetSdkVersion](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#target)设置为`“23”`，在Android系统映像上安装您的应用，测试它，然后使用此更改发布更新的应用。

您可以使用Android API，同时还支持旧版本，方法是在您的代码中添加条件，以便在执行[minSdkVersion](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#min)不支持的API之前检查系统API级别。 要了解有关保持向后兼容性的更多信息，请阅读[支持不同的平台版本](https://developer.android.com/training/basics/supporting-devices/platforms.html)。

有关API levels如何工作的更多信息，请阅读[什么是API Level](https://developer.android.com/training/basics/supporting-devices/platforms.html)？

### 指纹认证
此版本提供了新的API，可让您通过在支持的设备上使用指纹扫描来验证用户，将这些API与[Android密钥库系统](https://developer.android.com/training/articles/keystore.html)结合使用。

要通过指纹扫描对用户进行身份验证，请获取新的[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)类的实例并调用[authenticate()](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#authenticate&#40;android.hardware.fingerprint.FingerprintManager.CryptoObject, android.os.CancellationSignal, int, android.hardware.fingerprint.FingerprintManager.AuthenticationCallback, android.os.Handler&#41;)方法。您的应用程式必须在配有指纹传感器的相容装置上执行。您必须为应用程式实施指纹验证流程的使用者介面，并在使用者介面中使用标准的Android指纹图示。 Android指纹图标（`c_fp_40px.png`）包含在指纹对话框示例中。如果您正在开发使用指纹身份验证的多个应用，请注意每个应用都必须独立验证用户的指纹。

要在您的应用中使用此功能，请首先在清单中添加[USE_FINGERPRINT](https://developer.android.com/reference/android/Manifest.permission.html#USE_FINGERPRINT)权限。
```
<uses-permission
        android：name =“android.permission.USE_FINGERPRINT”/>
```
要查看指纹身份验证的应用实现，请参阅[指纹对话框示例](https://developer.android.com/samples/FingerprintDialog/index.html)。有关如何将这些身份验证API与其他Android API结合使用的演示，请参阅[指纹和付款API](https://developer.android.com/about/versions/marshmallow/android-6.0.html#confirm-credential)视频。

如果您要测试此功能，请按照下列步骤操作：
1. 安装Android SDK工具版本24.3，如果你还没有这样做。
2. 通过转到**设置** > **安全** > **指纹**，在模拟器中注册新指纹，然后按照注册说明进行操作。
使用仿真器通过以下命令模拟指纹触摸事件。使用相同的命令在锁定屏幕或应用程序中模拟指纹触摸事件。
```
adb -e emu finger touch <finger_id>
```
在Windows上，您可能必须运行`telnet 127.0.0.1 <emulator-id>`，后跟`finger touch <finger_id>`。

### 确认凭据
您的应用程式可根据使用者上次解锁装置的日期来验证使用者。 此功能使用户不必记住额外的应用程序专用密码，并避免您需要实现自己的身份验证用户界面。 您的应用程序应将此功能与用于用户身份验证的公共密钥或密钥实现结合使用。

要设置在用户成功验证后可以重复使用相同密钥的超时持续时间，请在设置[KeyGenerator](https://developer.android.com/reference/javax/crypto/KeyGenerator.html)或[KeyPairGenerator](https://developer.android.com/reference/java/security/KeyPairGenerator.html)时调用新的[setUserAuthenticationValidityDurationSeconds()](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationValidityDurationSeconds&#40;int&#41;)方法。

避免过度显示重新身份验证对话框 - 您的应用应首先尝试使用加密对象，如果超时过期，请使用[createConfirmDeviceCredentialIntent()](https://developer.android.com/reference/android/app/KeyguardManager.html#createConfirmDeviceCredentialIntent&#40;java.lang.CharSequence, java.lang.CharSequence&#41;)方法重新验证应用中的用户。

要查看此功能的应用实现，请参阅[确认凭据示例](https://developer.android.com/samples/ConfirmCredential/index.html)。

### 应用程序链接
此版本通过提供更强大的应用程序链接来增强Android的意图系统。 此功能可让您将应用程式与您拥有的网域相关联。 基于此关联，平台可以确定用于处理特定web链接的默认应用，并跳过提示用户选择应用。 要了解如何实现此功能，请参阅[处理应用程序链接](https://developer.android.com/training/app-links/index.html)。

### 应用程式的自动备份
系统现在可以为应用程序执行自动完全数据备份和恢复。 您的应用必须定位到Android 6.0（API级别23）才能启用此行为; 您不需要添加任何其他代码。 如果用户删除其Google帐户，则其备份数据也会被删除。 要了解此功能的工作原理以及如何配置要在文件系统上备份的内容，请参阅[为应用程序配置自动备份](https://developer.android.com/guide/topics/data/autobackup.html)。

### 直接共享
此版本为您提供API，使用户能够直观，快速地共享。 现在，您可以定义在应用中启动特定活动的直接共享目标。 这些直接共享目标通过共享菜单向用户公开。 此功能允许用户在其他应用内与目标（例如联系人）共享内容。 例如，直接共享目标可以在另一个社交网络应用中启动活动，这使得用户能够直接向该应用中的特定朋友或社区共享内容。

要启用直接共享目标，必须定义一个扩展[ChooserTargetService类](https://developer.android.com/reference/android/service/chooser/ChooserTargetService.html)的类。在清单中声明您的服务。在该声明中，使用[SERVICE_INTERFACE](https://developer.android.com/reference/android/Manifest.permission.html#BIND_CHOOSER_TARGET_SERVICE)操作指定[BIND_CHOOSER_TARGET_SERVICE](https://developer.android.com/reference/android/service/chooser/ChooserTargetService.html#SERVICE_INTERFACE)权限和intent过滤器。

以下示例显示如何在清单中声明[ChooserTargetService类](https://developer.android.com/reference/android/service/chooser/ChooserTargetService.html)。
```
<service android:name=".ChooserTargetService"
        android:label="@string/service_name"
        android:permission="android.permission.BIND_CHOOSER_TARGET_SERVICE">
    <intent-filter>
        <action android:name="android.service.chooser.ChooserTargetService" />
    </intent-filter>
</service>
```
对于要公开给[ChooserTargetService类](https://developer.android.com/reference/android/service/chooser/ChooserTargetService.html)的每个活动，在应用清单中添加一个名称为`android.service.chooser.chooser_target_service`的元数据元素。
```
<activity android:name=".MyShareActivity”
        android:label="@string/share_activity_label">
    <intent-filter>
        <action android:name="android.intent.action.SEND" />
    </intent-filter>
<meta-data
        android:name="android.service.chooser.chooser_target_service"
        android:value=".ChooserTargetService" />
</activity>
```

### 语音交互
此版本提供了一个新的语音交互API，与[语音操作](https://developers.google.com/voice-actions/)一起，允许您在应用程序中构建对话语音体验。调用[isVoiceInteraction()](https://developer.android.com/reference/android/app/Activity.html#isVoiceInteraction&#40;&#41;)方法来确定语音操作是否触发了您的活动。如果是这样，您的应用程序可以使用[VoiceInteractor](https://developer.android.com/reference/android/app/VoiceInteractor.html)类来请求用户的语音确认，从选项列表中选择等等。

大多数语音交互起源于用户语音动作。然而，语音交互活动也可以在没有用户输入的情况下开始。例如，通过语音交互启动的另一个应用程序也可以发送启动语音交互的意图。要确定您的活动是从用户语音查询启动还是从其他语音交互应用启动，请调用[isVoiceInteractionRoot()](https://developer.android.com/reference/android/app/Activity.html#isVoiceInteractionRoot&#40;&#41;)方法。如果另一个应用程式启动您的活动，该方法会传回false。然后，您的应用可能会提示用户确认他们打算执行此操作。

要详细了解实施语音操作，请参阅[语音操作开发者网站](https://developers.google.com/voice-actions/interaction/)。

### 协助API
此版本为用户提供了一种通过助理与您的应用互动的新方式。 要使用此功能，用户必须启用助理才能使用当前上下文。一旦启用，用户可以通过长按**主屏幕**按钮在任何应用程序中召唤助理。

通过设置[FLAG_SECURE](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#FLAG_SECURE)标志，您的应用程序可以选择不与助理共享当前上下文。除了平台传递给助理的标准信息集之外，您的应用程序还可以通过使用新的[AssistContent](https://developer.android.com/reference/android/app/assist/AssistContent.html)类来共享其他信息。

要向助理提供您应用的其他上下文，请按照以下步骤操作：
* 实现[Application.OnProvideAssistDataListener](https://developer.android.com/reference/android/app/Application.OnProvideAssistDataListener.html)接口。
* 使用[registerOnProvideAssistDataListener()](https://developer.android.com/reference/android/app/Application.html#registerOnProvideAssistDataListener&#40;android.app.Application.OnProvideAssistDataListener&#41;)注册此侦听器。
* 为了提供特定于活动的上下文信息，覆盖[onProvideAssistData()](https://developer.android.com/reference/android/app/Activity.html#onProvideAssistData&#40;android.os.Bundle&#41;)回调和可选的新的[onProvideAssistContent()](https://developer.android.com/reference/android/app/Activity.html#onProvideAssistContent&#40;android.app.assist.AssistContent&#41;)回调。

### 可用的存储设备
在此版本中，用户可以使用SD卡等外部存储设备。采用外部存储设备对设备进行加密和格式化，就像内部存储一样。此功能允许用户在存储设备之间移动这些应用程序的应用程序和私人数据。当移动应用程序时，系统遵循清单中的[android:installLocation](https://developer.android.com/guide/topics/manifest/manifest-element.html#install)首选项。

如果您的应用程序访问以下API或字段，请注意，当应用程序在内部和外部存储设备之间移动时，它们返回的文件路径将动态更改。在构建文件路径时，强烈建议您始终动态调用这些API。不要使用硬编码文件路径或保留之前构建的完全限定文件路径。

[Context](https://developer.android.com/reference/android/content/Context.html)方法：
* [getFilesDir()](https://developer.android.com/reference/android/content/Context.html#getFilesDir&#40;&#41;)
* [getCacheDir()](https://developer.android.com/reference/android/content/Context.html#getCacheDir&#40;&#41;)
* [getCodeCacheDir()](https://developer.android.com/reference/android/content/Context.html#getCodeCacheDir&#40;&#41;)
* [getDatabasePath()](https://developer.android.com/reference/android/content/Context.html#getDatabasePath&#40;java.lang.String&#41;)
* [getDir()](https://developer.android.com/reference/android/content/Context.html#getDir&#40;java.lang.String, int&#41;)
* [getNoBackupFilesDir()](https://developer.android.com/reference/android/content/Context.html#getNoBackupFilesDir&#40;&#41;)
* [getFileStreamPath()](https://developer.android.com/reference/android/content/Context.html#getFileStreamPath&#40;java.lang.String&#41;)
* [getPackageCodePath()](https://developer.android.com/reference/android/content/Context.html#getPackageCodePath&#40;&#41;)
* [getPackageResourcePath()](https://developer.android.com/reference/android/content/Context.html#getPackageResourcePath&#40;&#41;)

[ApplicationInfo](https://developer.android.com/reference/android/content/pm/ApplicationInfo.html)字段：
* [dataDir](https://developer.android.com/reference/android/content/pm/ApplicationInfo.html#dataDir)
* [sourceDir](https://developer.android.com/reference/android/content/pm/ApplicationInfo.html#sourceDir)
* [nativeLibraryDir](https://developer.android.com/reference/android/content/pm/ApplicationInfo.html#nativeLibraryDir)
* [publicSourceDir]()
* [splitSourceDir](https://developer.android.com/reference/android/content/pm/ApplicationInfo.html#splitSourceDirs)
* [splitPublicSourceDirs](https://developer.android.com/reference/android/content/pm/ApplicationInfo.html#splitPublicSourceDirs)

要调试此功能，您可以通过运行以下命令启用通过USB On-The-Go（OTG）电缆连接到Android设备的USB驱动器：
```
$ adb shell sm set-force-adoptable true
```

### 通知
此版本针对通知功能引入了下列API变更：
* 新增了[INTERRUPTION_FILTER_ALARMS](https://developer.android.com/reference/android/app/NotificationManager.html#INTERRUPTION_FILTER_ALARMS)过滤级别，它对应于新增的“仅闹铃”免打扰模式。
* 新增了[CATEGORY_REMINDER](https://developer.android.com/reference/android/app/Notification.html#CATEGORY_REMINDER)类别值，用于区分用户安排的提醒与其他事件([CATEGORY_EVENT](https://developer.android.com/reference/android/app/Notification.html#CATEGORY_EVENT))和闹铃([CATEGORY_ALARM](https://developer.android.com/reference/android/app/Notification.html#CATEGORY_ALARM))。
* 新增了[Icon](https://developer.android.com/reference/android/graphics/drawable/Icon.html)类，您可以通过[setSmallIcon()](https://developer.android.com/reference/android/app/Notification.Builder.html#setSmallIcon&#40;android.graphics.drawable.Icon&#41;)方法和[setLargeIcon()](https://developer.android.com/reference/android/app/Notification.Builder.html#setLargeIcon&#40;android.graphics.drawable.Icon&#41;)方法将其附加到通知上。同理[addAction()](https://developer.android.com/reference/android/app/Notification.Builder.html#addAction&#40;int, java.lang.CharSequence, android.app.PendingIntent&#41;)方法现在接受[Icon](https://developer.android.com/reference/android/graphics/drawable/Icon.html)对象，而不接受可绘制资源ID。
* 新增了[getActiveNotifications()](https://developer.android.com/reference/android/app/NotificationManager.html#getActiveNotifications())方法，让您的应用能够了解哪些通知目前处于活动状态。要查看使用此功能的应用实现，请参阅[ActiveNotifications示例](https://developer.android.com/samples/ActiveNotifications/index.html)。

### 蓝牙触控笔支持
此版本改善了对用户使用蓝牙触控笔进行输入的支持。用户可将兼容的蓝牙触控笔与其手机或平板电脑配对并建立连接。连接后，来自触摸屏的位置信息将与来自触控笔的压力和按键信息融合，从而实现比单纯使用触摸屏更丰富的表达。您的应用可以通过在Activity中注册[View.OnContextClickListener](https://developer.android.com/reference/android/view/View.OnContextClickListener.html)对象和[GestureDetector.OnContextClickListener](https://developer.android.com/reference/android/view/GestureDetector.OnContextClickListener.html)对象，侦听触控笔按键动作并执行辅助操作。

可使用[MotionEvent](https://developer.android.com/reference/android/view/MotionEvent.html)方法和常量来检测触控笔按键交互：
* 如果用户使用带按键的触控笔触按应用屏幕，[getTooltype()](https://developer.android.com/reference/android/view/MotionEvent.html#getToolType&#40;int&#41;)方法会返回[TOOL_TYPE_STYLUS](https://developer.android.com/reference/android/view/MotionEvent.html#TOOL_TYPE_STYLUS)。
* 对于以Android 6.0（API 级别 23）为目标平台的应用，当用户按触控笔的主按键时，[getButtonState()](https://developer.android.com/reference/android/view/MotionEvent.html#getButtonState&#40;&#41;)方法会返回 [BUTTON_STYLUS_PRIMARY](https://developer.android.com/reference/android/view/MotionEvent.html#BUTTON_STYLUS_PRIMARY)。如果触控笔有辅助按键，当用户按下它时，该方法会返回[BUTTON_STYLUS_SECONDARY](https://developer.android.com/reference/android/view/MotionEvent.html#BUTTON_STYLUS_SECONDARY)。如果用户同时按下两个按键，该方法会同时返回通过OR运算符连接起来的两个值 ([BUTTON_STYLUS_PRIMARY](https://developer.android.com/reference/android/view/MotionEvent.html#BUTTON_STYLUS_PRIMARY)|[BUTTON_STYLUS_SECONDARY](https://developer.android.com/reference/android/view/MotionEvent.html#BUTTON_STYLUS_SECONDARY))。
* 对于以较低平台版本为目标的应用，[getButtonState()](https://developer.android.com/reference/android/view/MotionEvent.html#getButtonState&#40;int&#41;)方法返回[BUTTON_SECONDARY](https://developer.android.com/reference/android/view/MotionEvent.html#BUTTON_SECONDARY)（按下触控笔主按键时）、[BUTTON_TERTIARY](https://developer.android.com/reference/android/view/MotionEvent.html#BUTTON_TERTIARY)（按下触控笔辅助按键时）之一或同时返回这两者。

### 改进的蓝牙低功耗扫描
如果您的应用执行蓝牙低功耗扫描，可以使用新增的[setCallbackType()](https://developer.android.com/reference/android/bluetooth/le/ScanSettings.Builder.html#setCallbackType&#40;int&#41;)方法指定您只希望在下列条件下通知回调：首次找到与设置的[ScanFilter](https://developer.android.com/reference/android/bluetooth/le/ScanFilter.html)匹配的播发数据包，或者已过很长时间后才再次看到该数据包。这种扫描方法与旧平台版本中提供的方法相比更加节能。

### Hotspot 2.0第1版支持
此版本在Nexus 6和Nexus 9设备上添加了对Hotspot 2.0第1版规范的支持。要在您的应用中配置Hotspot 2.0凭据，请使用[WifiEnterpriseConfig](https://developer.android.com/reference/android/net/wifi/WifiEnterpriseConfig.html)类的新方法，如[setPlmn()](https://developer.android.com/reference/android/net/wifi/WifiEnterpriseConfig.html#setPlmn&#40;java.lang.String&#41;)方法和[setRealm()](https://developer.android.com/reference/android/net/wifi/WifiEnterpriseConfig.html#setRealm&#40;java.lang.String&#41;)方法。在[WifiConfiguration](https://developer.android.com/reference/android/net/wifi/WifiConfiguration.html)对象中，您可以设置[FQDN](https://developer.android.com/reference/android/net/wifi/WifiConfiguration.html#FQDN)字段和[providerFriendlyName](https://developer.android.com/reference/android/net/wifi/WifiConfiguration.html#providerFriendlyName)字段。新增的[isPasspointNetwork()](https://developer.android.com/reference/android/net/wifi/ScanResult.html#isPasspointNetwork&#40;&#41;)方法可指示检测到的网络是否为Hotspot 2.0接入点。

### 4K显示模式
现在，平台允许应用在兼容硬件上请求将显示分辨率升级到4K渲染。要查询当前物理分辨率，请使用新增的[Display.Mode](https://developer.android.com/reference/android/view/Display.Mode.html) API。请注意，如果UI是以较低逻辑分辨率绘制并通过放大达到更高的物理分辨率，则[getPhysicalWidth()](https://developer.android.com/reference/android/view/Display.Mode.html#getPhysicalWidth&#40;&#41;)方法返回的物理分辨率可能不同于[getSize()](https://developer.android.com/reference/android/view/Display.html#getSize&#40;android.graphics.Point&#40;)所报告的逻辑分辨率。

您可以通过设置应用窗口的[preferredDisplayModeId](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#preferredDisplayModeId)属性请求系统更改应用运行时的物理分辨率。如果您想切换到4K显示分辨率，此功能会很有帮助。在4K显示模式下，UI仍然以原始分辨率（如 1080p）渲染，通过放大达到4K，但[SurfaceView](https://developer.android.com/reference/android/view/SurfaceView.html)对象可能会以原生分辨率显示内容。

### 主题化ColorStateList
对于运行Android 6.0（API 级别 23）的设备，现在支持在[ColorStateList](https://developer.android.com/reference/android/content/res/ColorStateList.html)中使用主题属性。[Resources.getColorStateList()](https://developer.android.com/reference/android/content/res/Resources.html#getColorStateList&#40;int&#41;)方法和[Resources.getColor()](https://developer.android.com/reference/android/content/res/Resources.html#getColor&#40;int&#41;)方法已弃用。如果您要调用这些API，请改为调用新增的[Context.getColorStateList()](https://developer.android.com/reference/android/content/Context.html#getColorStateList&#40;int&#41;)方法或[Context.getColor()](https://developer.android.com/reference/android/content/Context.html#getColor&#40;int&#41;)方法。还可在v4 appcompat库中通过[ContextCompat](https://developer.android.com/reference/android/support/v4/content/ContextCompat.html)使用这些方法。

### 音频功能
此版本增强了Android上的音频处理功能，包括：
* 通过新增的[android.media.midi](https://developer.android.com/reference/android/media/midi/package-summary.html) API提供了对[MIDI](http://en.wikipedia.org/wiki/MIDI)协议的支持。使用这些API可发送和接收MIDI事件。
* 新增了[AudioRecord.Builder](https://developer.android.com/reference/android/media/AudioRecord.Builder.html)类和[AudioTrack.Builder](https://developer.android.com/reference/android/media/AudioTrack.Builder.html)类，分别用于创建数字音频采集和回放对象，还可用于配置音频源和接收器属性来替换系统默认值。
* 用于关联音频和输入设备的API钩子。如果您的应用允许用户通过与Android TV相连的游戏控制器或遥控器启动语音搜索，此功能尤为有用。系统会在用户启动搜索时调用新增的[onSearchRequested()](https://developer.android.com/reference/android/app/Activity.html#onSearchRequested&#40;android.view.SearchEvent&#41;)回调。要确定用户的输入设备是否内置麦克风，请从该回调检索[InputDevice](https://developer.android.com/reference/android/view/InputDevice.html)对象，然后调用新的[hasMicrophone()](https://developer.android.com/reference/android/view/InputDevice.html#hasMicrophone&#40;&#41;)方法。
* 新增了[getDevices()](https://developer.android.com/reference/android/media/AudioManager.html#getDevices&#40;int&#41;)方法，让您可以检索系统当前连接的所有音频设备的列表。如果您想让系统在音频设备连接或断开时通知应用，还可以注册一个[AudioDeviceCallback](https://developer.android.com/reference/android/media/AudioDeviceCallback.html)对象。

### 视频功能
此版本为视频处理API添加了新功能，包括：
* 新增了[MediaSync](https://developer.android.com/reference/android/media/MediaSync.html)类，可帮助应用同步渲染音频流和视频流。音频缓冲区以非锁定方式提交，并通过回调返回。此外，它还支持动态回放速率。
* 新增了[EVENT_SESSION_RECLAIMED](https://developer.android.com/reference/android/media/MediaDrm.html#EVENT_SESSION_RECLAIMED)事件，它表示应用打开的会话已被资源管理器收回。如果您的应用使用 DRM 会话，则应处理此事件，并确保不使用收回的会话。
* 新增了[ERROR_RECLAIMED](https://developer.android.com/reference/android/media/MediaCodec.CodecException.html#ERROR_RECLAIMED)错误代码，它表示资源管理器收回了编解码器使用的媒体资源。出现此异常时，必须释放编解码器，因为它已转入终止状态。
* 新增了[getMaxSupportedInstances()](https://developer.android.com/reference/android/media/MediaCodecInfo.CodecCapabilities.html#getMaxSupportedInstances&#40;&#41;)接口，用于获取有关支持的编解码器实例最大并发数量的提示。
* 新增了[setPlaybackParams()](https://developer.android.com/reference/android/media/MediaPlayer.html#setPlaybackParams&#40android.media.PlaybackParams&#41;)方法，用于设置快动作回放或慢动作回放的媒体回放速率。此外，它还会随视频一起自动拉长或加速音频回放。

### 相机功能
此版本提供了下列用于访问相机闪光灯和相机图像再处理的新API：

#### Flashlight API
如果相机设备带有闪光灯，您可以通过调用[setTorchMode()](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode&#40;java.lang.String, boolean&#41;)方法，在不打开相机设备的情况下打开或关闭闪光灯的火炬模式。应用对闪光灯或相机设备不享有独占所有权。每当相机设备不可用，或者开启火炬的其他相机资源不可用时，火炬模式即会被关闭并变为不可用状态。其他应用也可调用[setTorchMode()](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode&#40;java.lang.String, boolean&#41;)来关闭火炬模式。当最后一个开启火炬模式的应用关闭时，火炬模式就会被关闭。

您可以注册一个回调，通过调用[registerTorchCallback()](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#registerTorchCallback&#40;android.hardware.camera2.CameraManager.TorchCallback, android.os.Handler&#41;)方法接收有关火炬模式状态的通知。第一次注册回调时，系统会立即调用它，并返回所有当前已知配备闪光灯的相机设备的火炬模式状态。如果成功开启或关闭火炬模式，系统会调用[onTorchModeChanged()](https://developer.android.com/reference/android/hardware/camera2/CameraManager.TorchCallback.html#onTorchModeChanged&#40;java.lang.String, boolean&#41;)方法。

#### Reprocessing API
[Camera2](https://developer.android.com/reference/android/hardware/camera2/package-summary.html) API进行了扩展，以支持YUV和专用不透明格式图像再处理。要确定这些再处理功能是否可用，请调用[getCameraCharacteristics()](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#getCameraCharacteristics&#40;java.lang.String&#41;)并检查有无[REPROCESS_MAX_CAPTURE_STALL](https://developer.android.com/reference/android/hardware/camera2/CameraCharacteristics.html#REPROCESS_MAX_CAPTURE_STALL)密钥。如果设备支持再处理，您可以通过调用[createReprocessableCaptureSession()](https://developer.android.com/reference/android/hardware/camera2/CameraDevice.html#createReprocessableCaptureSession&#40;android.hardware.camera2.params.InputConfiguration, java.util.List<android.view.Surface>, android.hardware.camera2.CameraCaptureSession.StateCallback, android.os.Handler&#41;)创建一个可再处理的相机采集会话并创建输入缓冲区再处理请求。

使用[ImageWriter](https://developer.android.com/reference/android/media/ImageWriter.html)类可将输入缓冲区流与相机再处理输入相连。要获得空白缓冲区，请遵循以下编程模型：
1. 调用[dequeueInputImage()](https://developer.android.com/reference/android/media/ImageWriter.html#dequeueInputImage&#40;&#41;)方法。
2. 在输入缓冲区中填充数据。
3. 通过调用[queueInputImage()](https://developer.android.com/reference/android/media/ImageWriter.html#queueInputImage&#40;android.media.Image&#41;)方法将缓冲区发送至相机。

如果您将[ImageWriter](https://developer.android.com/reference/android/media/ImageWriter.html)对象与[PRIVATE](https://developer.android.com/reference/android/graphics/ImageFormat.html#PRIVATE)图像一起使用，您的应用并不能直接访问图像数据。请改为调用[queueInputImage()](https://developer.android.com/reference/android/media/ImageWriter.html#queueInputImage&#40;android.media.Image&#41;)方法，将[PRIVATE](https://developer.android.com/reference/android/graphics/ImageFormat.html#PRIVATE)图像直接传递给[ImageWriter](https://developer.android.com/reference/android/media/ImageWriter.html)，而不进行任何缓冲区复制。

[ImageReader](https://developer.android.com/reference/android/media/ImageReader.html)类现在支持 PRIVATE 格式图像流。凭借此支持特性，您的应用可使[ImageReader](https://developer.android.com/reference/android/media/ImageReader.html)输出图像保持为循环图像队列，还可选择一个或多个图像并将其发送给[ImageWriter](https://developer.android.com/reference/android/media/ImageWriter.html)进行相机再处理。

### Android for Work功能
此版本提供了下列用于Android for Work的新API：
* **用于企业所有、单一用途设备的增强型控件**：现在，设备所有者可以通过控制以下设置来改善企业所有、单一用途 (COSU) 设备的管理：
    * 通过[setKeyguardDisabled()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#setKeyguardDisablede&#40;android.content.ComponentName, booleane&#41;)方法停用或重新启用键盘锁。
    * 通过[setStatusBarDisabled()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#setStatusBarDisabled&#41;android.content.ComponentName, boolean&#41;)方法停用或重新启用状态栏（包括快速设置、通知以及启动Google Now的向上划动导航手势）。
    * 通过[UserManager](https://developer.android.com/reference/android/os/UserManager.html)常量[DISALLOW_SAFE_BOOT](https://developer.android.com/reference/android/os/UserManager.html#DISALLOW_SAFE_BOOT)停用或重新启用安全启动。
    * 通过[STAY_ON_WHILE_PLUGGED_IN](https://developer.android.com/reference/android/provider/Settings.Global.html#STAY_ON_WHILE_PLUGGED_IN)常量防止屏幕在插入电源的情况下关闭。
* **设备所有者静默式安装和卸载应用**：现在，设备所有者可使用[PackageInstaller](https://developer.android.com/reference/android/content/pm/PackageInstaller.html) API在不依赖Google Play for Work的情况下静默式安装和卸载应用。现在，您可以通过设备所有者配置设备，从而无需用户干预即可获取并安装应用。此功能可用于在不激活Google帐户的情况下实现信息亭或其他此类设备的一键式配置。
* **静默式企业证书访问**： 现在，当应用调用[choosePrivateKeyAlias()](https://developer.android.com/reference/android/security/KeyChain.html#choosePrivateKeyAlias&#40;android.app.Activity, android.security.KeyChainAliasCallback, java.lang.String[], java.security.Principal[], java.lang.String, int, java.lang.String&#41;)时，配置文件所有者或设备所有者可以在系统提示用户选择证书前调用 [onChoosePrivateKeyAlias()](https://developer.android.com/reference/android/app/admin/DeviceAdminReceiver.html#onChoosePrivateKeyAlias&#40;android.content.Context, android.content.Intent, int, android.net.Uri, java.lang.String&#41;)方法，静默式向发出请求的应用提供别名。此功能让您可以在无需用户交互的情况下授予托管应用访问证书的权限。
* **自动接受系统更新**。现在，设备所有者可以通过[setSystemUpdatePolicy()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#setSystemUpdatePolicy&#40;android.content.ComponentName, android.app.admin.SystemUpdatePolicy&#41;)设置一个系统更新政策来自动接受系统更新（例如对于信息亭设备），或者推迟更新并在至多30天的时间内防止用户获取更新。此外，管理员还可设置每日必须获取更新的时间窗口，例如在信息亭设备无人使用的时段。有可用的系统更新时，系统会检查设备规范控制器应用是否设置了系统更新政策，并相应地执行操作。
* **授权证书安装**：配置文件所有者或设备所有者现在可以授权第三方应用调用以下[DevicePolicyManager](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html)证书管理API：
    * [getInstalledCaCerts()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#getInstalledCaCerts&#40;android.content.ComponentName&#41;)
    * [hasCaCertInstalled()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#hasCaCertInstalled&#40;android.content.ComponentName, byte[]&#41;)
    * [installCaCert()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#installCaCert&#40;android.content.ComponentName, byte[]&#41;)
    * [uninstallCaCert()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#uninstallCaCert&#40;android.content.ComponentName, byte[]&#41;)
    * [uninstallAllUserCaCerts()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#uninstallAllUserCaCerts&#40;android.content.ComponentName&#41;)
    * [installKeyPair()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#installKeyPair&#40;android.content.ComponentName, java.security.PrivateKey, java.security.cert.Certificate, java.lang.String&#41;)
* **流量消耗情况跟踪**。现在，配置文件所有者或设备所有者可以利用新增的[NetworkStatsManager](https://developer.android.com/reference/android/app/usage/NetworkStatsManager.html)方法查询**Settings > Data Usage**中显示的流量使用情况统计信息。配置文件所有者会被自动授予查询其管理的配置文件相关数据的权限，而设备所有者则被授予对其管理的主要用户使用情况数据的访问权。
* **运行时权限管理**：配置文件所有者或设备所有者可以利用[setPermissionPolicy()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#setPermissionPolicy&#40;android.content.ComponentName, int&#41;)设置适用于所有应用全部运行时请求的权限政策，以提示用户授予权限，或自动以静默方式授予或拒绝权限。如果设置后一种政策，则用户将无法修改配置文件所有者或设备所有者在应用权限屏幕的**Settings**内所做的选择。
* **Settings**中的**VPN**：现在，**Settings > More > VPN**中会显示VPN应用。此外，现在，关于VPN使用情况的通知取决于该VPN的配置方式。对于配置文件所有者，通知取决于该VPN是针对托管配置文件、个人配置文件还是同时针对这两者进行配置。对于设备所有者，通知取决于VPN是否针对整个设备进行配置。
* **工作状态通知**：现在，每当来自托管配置文件的应用具有前台Activity时，状态栏就会出现一个公文包图标。此外，如果设备直接解锁到托管配置文件中某个应用的Activity，则会显示一个Toast，通知用户他们位于托管配置文件内。

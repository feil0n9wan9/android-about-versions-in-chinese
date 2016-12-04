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

要通过指纹扫描对用户进行身份验证，请获取新的[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)类的实例并调用[authenticate()](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#authenticate(android.hardware.fingerprint.FingerprintManager.CryptoObject, android.os.CancellationSignal, int, android.hardware.fingerprint.FingerprintManager.AuthenticationCallback, android.os.Handler))方法。您的应用程式必须在配有指纹传感器的相容装置上执行。您必须为应用程式实施指纹验证流程的使用者介面，并在使用者介面中使用标准的Android指纹图示。 Android指纹图标（`c_fp_40px.png`）包含在指纹对话框示例中。如果您正在开发使用指纹身份验证的多个应用，请注意每个应用都必须独立验证用户的指纹。

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

要设置在用户成功验证后可以重复使用相同密钥的超时持续时间，请在设置[KeyGenerator](https://developer.android.com/reference/javax/crypto/KeyGenerator.html)或[KeyPairGenerator](https://developer.android.com/reference/java/security/KeyPairGenerator.html)时调用新的[setUserAuthenticationValidityDurationSeconds()](https://developer.android.com/reference/android/security/keystore/KeyGenParameterSpec.Builder.html#setUserAuthenticationValidityDurationSeconds(int))方法。

避免过度显示重新身份验证对话框 - 您的应用应首先尝试使用加密对象，如果超时过期，请使用[createConfirmDeviceCredentialIntent()](https://developer.android.com/reference/android/app/KeyguardManager.html#createConfirmDeviceCredentialIntent(java.lang.CharSequence, java.lang.CharSequence))方法重新验证应用中的用户。

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
此版本提供了一个新的语音交互API，与[语音操作](https://developers.google.com/voice-actions/)一起，允许您在应用程序中构建对话语音体验。调用[isVoiceInteraction()](https://developer.android.com/reference/android/app/Activity.html#isVoiceInteraction())方法来确定语音操作是否触发了您的活动。如果是这样，您的应用程序可以使用[VoiceInteractor](https://developer.android.com/reference/android/app/VoiceInteractor.html)类来请求用户的语音确认，从选项列表中选择等等。

大多数语音交互起源于用户语音动作。然而，语音交互活动也可以在没有用户输入的情况下开始。例如，通过语音交互启动的另一个应用程序也可以发送启动语音交互的意图。要确定您的活动是从用户语音查询启动还是从其他语音交互应用启动，请调用[isVoiceInteractionRoot()](https://developer.android.com/reference/android/app/Activity.html#isVoiceInteractionRoot())方法。如果另一个应用程式启动您的活动，该方法会传回false。然后，您的应用可能会提示用户确认他们打算执行此操作。

要详细了解实施语音操作，请参阅[语音操作开发者网站](https://developers.google.com/voice-actions/interaction/)。

### 协助API
此版本为用户提供了一种通过助理与您的应用互动的新方式。 要使用此功能，用户必须启用助理才能使用当前上下文。一旦启用，用户可以通过长按**主屏幕**按钮在任何应用程序中召唤助理。

通过设置[FLAG_SECURE](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#FLAG_SECURE)标志，您的应用程序可以选择不与助理共享当前上下文。除了平台传递给助理的标准信息集之外，您的应用程序还可以通过使用新的[AssistContent](https://developer.android.com/reference/android/app/assist/AssistContent.html)类来共享其他信息。

要向助理提供您应用的其他上下文，请按照以下步骤操作：
* 实现[Application.OnProvideAssistDataListener](https://developer.android.com/reference/android/app/Application.OnProvideAssistDataListener.html)接口。
* 使用[registerOnProvideAssistDataListener()](https://developer.android.com/reference/android/app/Application.html#registerOnProvideAssistDataListener(android.app.Application.OnProvideAssistDataListener))注册此侦听器。
* 为了提供特定于活动的上下文信息，覆盖[onProvideAssistData()](https://developer.android.com/reference/android/app/Activity.html#onProvideAssistData(android.os.Bundle))回调和可选的新的[onProvideAssistContent()](https://developer.android.com/reference/android/app/Activity.html#onProvideAssistContent(android.app.assist.AssistContent))回调。

### 可用的存储设备
在此版本中，用户可以使用SD卡等外部存储设备。采用外部存储设备对设备进行加密和格式化，就像内部存储一样。此功能允许用户在存储设备之间移动这些应用程序的应用程序和私人数据。当移动应用程序时，系统遵循清单中的[android：installLocation](https://developer.android.com/guide/topics/manifest/manifest-element.html#install)首选项。

如果您的应用程序访问以下API或字段，请注意，当应用程序在内部和外部存储设备之间移动时，它们返回的文件路径将动态更改。在构建文件路径时，强烈建议您始终动态调用这些API。不要使用硬编码文件路径或保留之前构建的完全限定文件路径。

[Context](https://developer.android.com/reference/android/content/Context.html)方法：
* getFilesDir（）
* getCacheDir（）
* getCodeCacheDir（）
* getDatabasePath（）
* getDir（）
* getNoBackupFilesDir（）
* getFileStreamPath（）
* getPackageCodePath（）
* getPackageResourcePath（）
[ApplicationInfo](https://developer.android.com/reference/android/content/pm/ApplicationInfo.html)字段：
* dataDir
* sourceDir
* nativeLibraryDir
* publicSourceDir
* splitSourceDir
* splitPublicSourceDirs
要调试此功能，您可以通过运行以下命令启用通过USB On-The-Go（OTG）电缆连接到Android设备的USB驱动器：
```
$ adb shell sm set-force-adoptable true
```

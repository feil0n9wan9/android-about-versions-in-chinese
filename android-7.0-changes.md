# Android 7.0行为变更
除了新功能和新特性，Android 7.0还包含了各种系统和API行为更改。本文档介绍了一些您应该理解，并在您的应用程序中需要考虑的重要更改。

如果您的Android应用程序已发布，请注意，您的应用程式可能会受到平台中这些变更的影响。

### 电量和内存
Android 7.0包含了提高设备的电池寿命和减少RAM使用的系统行为更改。这些更改会影响您的应用对系统资源的访问权限，以及您的应用通过某些隐式intent与其他应用互动的方式。

#### Doze
在Android 6.0（API级别23）中引入的Doze通过在用户将设备拔出、固定且屏幕关闭的情况下延迟CPU和网络活动来延长电池寿命。Android 7.0通过在用户将设备拔出、屏幕关闭但不一定是固定（例如手机在用户口袋中晃动）的情况下应用CPU和网络限制的一部分子集来进一步增强Doze。

当设备处于电池供电状态并且屏幕已关闭一段时间后，设备将进入Doze并应用第一个限制子集：它会关闭应用程序网络访问权限，并延迟作业和同步。如果设备在进入Doze模式后静止一段时间，系统会将其他的Doze限制应用于[PowerManager.WakeLock](https://developer.android.com/reference/android/os/PowerManager.WakeLock.html)，[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)警报，GPS和Wi-Fi扫描。无论是应用某些还是所有Doze限制，系统都会将设备唤醒以获得短暂的维护窗口期，在此维护窗口期间允许应用程序进行网络访问，并且可以执行任何延迟的作业/同步。

请注意，激活屏幕或插入设备会退出Doze并移除这些处理限制。此行为不影响针对于先前在Android 6.0（API级别23）中引入的的Doze适配应用程序的建议和最佳做法，如在[针对Doze和App Standby进行优化](https://developer.android.com/training/monitoring-device-state/doze-standby.html)中所述。您仍应遵循这些建议，例如使用Google云消息（GCM）发送和接收消息，并开始规划更新以适应其他Doze行为。

#### Svelte计划：后台优化
Android 7.0删除了三个隐式广播，以帮助优化内存使用和功耗。此变更是必要的，因为隐式广播会频繁启动注册并在后台监听它们的应用程序。移除这些广播可以显著地改善设备性能和用户体验。

移动设备经历频繁的连接改变，例如当在Wi-Fi和移动数据之间移动时。目前，应用程序可以通过在清单中注册隐式[CONNECTIVITY_ACTION](https://developer.android.com/reference/android/net/ConnectivityManager.html#CONNECTIVITY_ACTION)广播的接收器来监视连接的更改。由于许多应用程序注册接收此广播，单个网络交换机可以使其全部唤醒并立即处理广播。

同样，在以前的Android版本中，应用可以注册接收来自其他应用（例如相机）的隐含[ACTION_NEW_PICTURE](https://developer.android.com/reference/android/hardware/Camera.html#ACTION_NEW_PICTURE)和[ACTION_NEW_VIDEO](https://developer.android.com/reference/android/hardware/Camera.html#ACTION_NEW_VIDEO)广播。当用户使用相机应用拍摄照片时，这些应用会唤醒以处理广播。

为了缓解这些问题，Android 7.0应用以下优化：

以Android 7.0为目标的应用程式不会收到[CONNECTIVITY_ACTION](https://developer.android.com/reference/android/net/ConnectivityManager.html#CONNECTIVITY_ACTION)广播，即使他们有要求收到这些活动通知的清单项目。正在运行的应用程序仍然可以在其主线程上侦听`CONNECTIVITY_CHANGE`，如果他们使用BroadcastReceiver请求通知。
应用无法发送或接收[ACTION_NEW_PICTURE](https://developer.android.com/reference/android/hardware/Camera.html#ACTION_NEW_PICTURE)或[ACTION_NEW_VIDEO](https://developer.android.com/reference/android/hardware/Camera.html#ACTION_NEW_VIDEO)广播。此优化会影响所有应用，而不仅仅是针对Android 7.0的应用。
如果您的应用使用任何这些意图，您应该尽快移除它们的依赖关系，以便您可以正确定位Android 7.0设备。 Android框架提供了几种解决方案来减少对这些隐式广播的需求。例如，当满足指定的条件（例如到非计量网络的连接）时，[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) API提供了稳定的机制来调度网络操作。您甚至可以使用[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)对内容提供商的更改做出反应。

有关Android 7.0（API级别24）中的后台优化以及如何调整应用程序的详细信息，请参阅[后台优化](https://developer.android.com/topic/performance/background-optimization.html)。

### 权限变更
Android 7.0包含可能会影响您应用程序的权限变更。

#### 文件系统权限更改
为了提高私人文件的安全性，以Android 7.0或更高版本为目标的应用的私人目录限制访问权限（0700）。此设置可防止私有文件的元数据泄露，例如其大小或存在。此权限更改具有多个副作用：

* 私有文件的文件权限不应该被所有者放松，并且尝试使用[MODE_WORLD_READABLE](https://developer.android.com/reference/android/content/Context.html#MODE_WORLD_READABLE)和/或[MODE_WORLD_WRITEABLE](https://developer.android.com/reference/android/content/Context.html#MODE_WORLD_WRITEABLE)这样做会触发[SecurityException](https://developer.android.com/reference/java/lang/SecurityException.html)。

> **注意**：迄今为止，此限制未完全实施。应用程序仍然可以使用本机API或[File](https://developer.android.com/reference/java/io/File.html) API修改其私有目录的权限。但是，我们强烈不鼓励放松私有目录的权限。

* 传递`file：//`在包域之外的URI可能使接收者留下不可访问的路径。因此，尝试传递`file：//` URI触发`FileUriExposedException`。推荐的共享私有文件内容的方法是使用`FileProvider`。
* [DownloadManager](https://developer.android.com/reference/android/app/DownloadManager.html)不能再按文件名共享私有存储的文件。在访问[COLUMN_LOCAL_FILENAME](https://developer.android.com/reference/android/app/DownloadManager.html#COLUMN_LOCAL_FILENAME)时，旧应用程序可能会遇到不可访问的路径。如果应用程式指定Android 7.0以上版本，当您尝试存取[COLUMN_LOCAL_FILENAME](https://developer.android.com/reference/android/app/DownloadManager.html#COLUMN_LOCAL_FILENAME)时，会触发[SecurityException](https://developer.android.com/reference/java/lang/SecurityException.html)。通过使用[DownloadManager.Request.setDestinationInExternalFilesDir()](https://developer.android.com/reference/android/app/DownloadManager.Request.html#setDestinationInExternalFilesDir(android.content.Context, java.lang.String, java.lang.String))或[DownloadManager.Request.setDestinationInExternalPublicDir()](https://developer.android.com/reference/android/app/DownloadManager.Request.html#setDestinationInExternalPublicDir(java.lang.String, java.lang.String))将下载位置设置为公共位置的旧应用程序仍然可以访问COLUMN_LOCAL_FILENAME中的路径，但是，强烈建议不要使用此方法。访问由[DownloadManager](https://developer.android.com/reference/android/app/DownloadManager.html)公开的文件的首选方法是使用[ContentResolver.openFileDescriptor()](https://developer.android.com/reference/android/content/ContentResolver.html#openFileDescriptor(android.net.Uri, java.lang.String))。

### 应用程序间文件共享
对于以Android 7.0为目标的应用程式，Android架构会强制执行[StrictMode](https://developer.android.com/reference/android/os/StrictMode.html) API政策，禁止在应用程式外显示`file：//` URI。如果包含文件URI的意图离开您的应用程序，则该应用程序将失败，并显示`FileUriExposedException`异常。

要在应用程序之间共享文件，您应该发送一个`content：//` URI并授予对URI的临时访问权限。授予此权限的最简单方法是使用[FileProvider](https://developer.android.com/reference/android/support/v4/content/FileProvider.html)类。有关权限和共享文件的详细信息，请参阅[共享文件](https://developer.android.com/training/secure-file-sharing/index.html)。

### 辅助功能改进
Android 7.0包含旨在改善视力低或视力受损的用户的平台的可用性的更改。这些更改通常不需要在应用中更改代码，但您应该查看这些功能并使用您的应用对其进行测试，以评估对用户体验的潜在影响。

#### 屏幕缩放
Android 7.0允许用户设置**显示大小**，放大或缩小屏幕上的所有元素，从而提高低视力用户的设备可访问性。用户无法将屏幕缩放到最小屏幕宽度[sw320dp](https://developer.android.com/guide/topics/resources/providing-resources.html)，这是一个通用中型手机Nexus 4的宽度。

当设备密度变化时，系统以以下方式通知正在运行的应用程序：

* 如果应用程序指定的API级别为23或更低，系统会自动终止其所有后台进程。这意味着，如果用户切换离开这样的应用程序打开设置屏幕并更改**显示大小**设置，系统杀死该应用程序相同的方式，在低内存情况下。如果应用程序有任何前台进程，系统会按照处理运行时更改中所述通知这些进程配置更改，就像设备的方向已更改一样。
* 如果某个应用定位到Android 7.0，则会按照处理运行时更改中所述通知其所有进程（前台和后台）的配置更改。

大多数应用程式不需要进行任何变更即可支援这项功能，前提是应用程式遵循Android最佳做法。具体事项检查：
* 在屏幕宽度为sw320dp的设备上测试您的应用程序，并确保它能够正常工作。
* 当设备配置更改时，更新任何依赖于密度的缓存信息，例如缓存的位图或从网络加载的资源。当应用程序从暂停状态恢复时，检查配置更改。
> **注意**：如果您缓存与配置相关的数据，最好添加相关元数据，例如适当的屏幕大小或像素密度。保存此元数据允许您决定是否需要在配置更改后刷新缓存的数据。
* 避免使用px单位指定尺寸，因为它们不会随屏幕密度而缩放。相反，请使用与[密度无关的像素](https://developer.android.com/guide/practices/screens_support.html)（`dp`）单位指定尺寸。

#### 设置向导中的视觉设置
Android 7.0在欢迎屏幕上包含视觉设置，用户可以在新设备上设置以下辅助功能设置：**放大手势**，**字体大小**，**显示大小**和**话语提示**。此更改增加了与不同屏幕设置相关的错误的可见性。要评估此功能的影响，您应该在启用这些设置的情况下测试您的应用。您可以在**设置** > **辅助功能**下找到设置。

### NDK应用程序链接到平台库
从Android 7.0开始，系统会阻止应用程序与非NDK库动态链接，这可能会导致应用程序崩溃。行为的这种变化旨在为跨平台更新和不同设备创建一致的应用体验。即使你的代码可能不是链接到私人库，可能是你的应用程序中的第三方静态库可能这样做。因此，所有开发人员应检查以确保其应用程式在执行Android 7.0的装置上不会发生当机。如果您的应用程序使用本机代码，您应该只使用[公共NDK API](https://developer.android.com/ndk/guides/stable_apis.html)。

您的应用可能尝试访问私有平台API的方法有三种：
* 您的应用程序直接访问私人平台库。您应该更新应用程序，以包含其自己的那些库的副本或使用[公共NDK API](https://developer.android.com/ndk/guides/stable_apis.html)。
* 您的应用程序使用访问私有平台库的第三方库。即使你确定你的应用程序不直接访问私人库，你仍然应该测试你的应用程序这种情况。
* 您的应用程式参照的是未包含在APK中的图书库。例如，如果您尝试使用自己的OpenSSL副本，但忘记将其与您的应用程序的APK捆绑在一起，则可能会发生这种情况。该应用可能在包含libcrypto.so的Android平台版本上正常运行。但是，该应用可能会在不包含此库的Android版本（例如Android 6.0及更高版本）上崩溃。要解决这个问题，请确保您将所有非NDK库与您的APK捆绑在一起。
应用程序不应使用未包含在NDK中的本机库，因为它们可能会在不同版本的Android之间更改或删除。从OpenSSL切换到BoringSSL就是这种更改的一个示例。此外，因为没有包括在NDK中的平台库没有兼容性要求，所以不同的设备可以提供不同级别的兼容性。

为了减少此限制可能对当前发布的应用程序产生的影响，一组看到显着用途的库（如`libandroid_runtime.so`，`libcutils.so`，`libcrypto.so`和`libssl.so`）在Android 7.0上可以临时访问（API级别24），适用于指定API级别为23或更低的应用。如果您的应用加载这些库之一，logcat会生成警告，并且目标设备上会显示Toast以通知您。如果您看到这些警告，您应该更新您的应用程序以包含其自己的那些库的副本或仅使用公共NDK API。未来的Android平台版本可能会限制私人图书馆的使用，并导致您的应用程序崩溃。

所有应用程序在调用既不公开也不可临时访问的API时会生成运行时错误。结果是`System.loadLibrary`和`dlopen(3)`都返回`NULL`，并可能导致您的应用程序崩溃。您应该查看应用代码，以删除对私有平台API的使用，并使用运行Android 7.0（API级别24）的设备或模拟器彻底测试您的应用。如果不确定您的应用程序是否使用私有库，可以[检查logcat](#检查您的应用程序是否使用专用库)以识别运行时错误。

#### 检查您的应用程序是否使用专用库
为了帮助您识别加载专用库的问题，logcat可能会生成警告或运行时错误。 例如，如果您的应用的目标API级别为23或更低，并尝试访问运行Android 7.0的设备上的私人库，则可能会看到类似以下内容的警告：
```
03-21 17:07:51.502 31234 31234 W linker  : library "libandroid_runtime.so"
("/system/lib/libandroid_runtime.so") needed or dlopened by
"/data/app/com.popular-app.android-2/lib/arm/libapplib.so" is not accessible
for the namespace "classloader-namespace" - the access is temporarily granted
as a workaround for http://b/26394120
```
这些logcat警告告诉您哪个库尝试访问私有平台API，但不会导致您的应用程序崩溃。 但是，如果应用程序目标API级别为24或更高，logcat会生成以下运行时错误，您的应用程序可能会崩溃：
```
java.lang.UnsatisfiedLinkError: dlopen failed: library "libcutils.so"
("/system/lib/libcutils.so") needed or dlopened by
"/system/lib/libnativeloader.so" is not accessible for the namespace
"classloader-namespace"
  at java.lang.Runtime.loadLibrary0(Runtime.java:977)
  at java.lang.System.loadLibrary(System.java:1602)
```
如果您的应用程序使用动态链接到私有平台API的第三方库，那么您也可能会看到这些logcat输出。 Android 7.0DK中的readelf工具允许您通过运行以下命令来生成给定.so文件的所有动态链接共享库的列表：
```
aarch64-linux-android-readelf -dW libMyLibrary.so
```

#### 更新您的应用程序
以下是您可以采取的一些步骤，以修复这些类型的错误，并确保您的应用程序不会在未来的平台更新崩溃：
* 如果您的应用程序使用私人平台库，您应该更新它以包含这些库的自己的副本或使用[公共NDK API](https://developer.android.com/ndk/guides/stable_apis.html)。
* 如果您的应用使用访问私有符号的第三方库，请与图书馆作者联系以更新图书馆。
* 确保使用APK打包所有非NDK库。
* 使用标准JNI函数代替`getJavaVM`和`getJNIEnv`从`libandroid_runtime.so`：
```
AndroidRuntime::getJavaVM -> GetJavaVM from <jni.h>
AndroidRuntime::getJNIEnv -> JavaVM::GetEnv or
JavaVM::AttachCurrentThread from <jni.h>.
```
使用`__system_property_get`而不是`libcutils.so`中的私有`property_get`符号。 要做到这一点，使用`__system_property_get`与以下包括：
```
#include <sys/system_properties.h>
```
> 注意：系统属性的可用性和内容不通过CTS测试。 更好的解决方法是避免使用这些属性。
* 使用`libcrypto.so`的本地版本的`SSL_ctrl`符号。 例如，您应该静态链接`libsorpto.a`在您的`.so`文件，或包括动态链接版本的`libcrypto.so`从BoringSSL / OpenSSL和打包它在你的APK。

### Android for Work
Android 7.0包含针对Android for Work的应用的更改，包括对证书安装，密码重置，辅助用户管理和设备标识符访问的更改。如果您正在为Android for Work环境构建应用，则应该查看这些更改并相应地修改应用。
* 您必须先安装委派的证书安装程序，然后DPC才能设置它。对于以Android 7.0（API级别24）为目标的配置文件和设备所有者应用，您应在设备策略控制器（DPC）调用`DevicePolicyManager.setCertInstallerPackage()`之前安装委派的证书安装程序。如果尚未安装安装程序，则系统将抛出`IllegalArgumentException`。
* 重设设备管理员的密码限制现在适用于个人资料所有者。设备管理员不能再使用`DevicePolicyManager.resetPassword()`清除密码或更改已设置的密码。设备管理员仍然可以设置密码，但只有当设备没有密码，PIN或模式时。
* 即使设置了限制，设备和个人资料所有者也可以管理帐户。即使设置了`DISALLOW_MODIFY_ACCOUNTS`用户限制，设备所有者和个人资料所有者也可以调用帐户管理API。
设备所有者可以更容易地管理辅助用户。当设备在设备所有者模式下运行时，将自动设置`DISALLOW_ADD_USER`限制。这将阻止用户创建非托管次要用户。此外，不推荐使用`CreateUser()`和`createAndInitializeUser()`方法;新的`DevicePolicyManager.createAndManageUser()`方法替换它们。
* 设备所有者可以访问设备标识符。设备所有者可以使用`DevicePolicyManagewr.getWifiMacAddress()`访问设备的Wi-Fi MAC地址。如果设备上从未启用Wi-Fi，此方法将返`null`值。
* 工作模式设置控制对工作应用的访问。当工作模式关闭时，系统启动器会指示工作应用程式无法显示为灰色。再次启用工作模式将恢复正常行为。
* 当从设置UI安装包含客户端证书链和相应私钥的PKCS＃12文件时，链中的CA证书不再安装到受信任证书存储器。当应用程序尝试稍后检索客户端证书链时，这不会影响[KeyChain.getCertificateChain()](https://developer.android.com/reference/android/security/KeyChain.html#getCertificateChain(android.content.Context, java.lang.String))的结果。如果需要，CA证书应该分别通过Settings UI安装到受信任的凭据存储，并在.crt或.cer文件扩展名下使用DER编码格式。
* 从Android 7.0开始，每位用户都会管理指纹注册和存储。如果个人资料所有者的设备策略客户端（DPC）在运行Android 7.0（API级别24）的设备上定位API级别23（或更低），则用户仍然可以在设备上设置指纹，但是工作应用程序无法访问设备指纹。当DPC目标为24级及以上的API级别时，用户可以通过转到**设置** > **安全** >**工作文件安全**设置，为工作资料专门设置指纹。
* `DevicePolicyManager.getStorageEncryptionStatus()`返回新的加密状态`ENCRYPTION_STATUS_ACTIVE_PER_USER`，以指示加密处于活动状态，并且加密密钥与用户绑定。仅当DPC针对API Level 24及更高版本时，才会返回新状态。对于定位到较早API级别的应用，返回`ENCRYPTION_STATUS_ACTIVE`，即使加密密钥特定于用户或配置文件。
* 在Android 7.0中，如果设备具有安装了单独的工作挑战的工作配置文件，通常影响整个设备的几种方法的行为会有所不同。这些方法不仅影响整个设备，而且仅适用于工作资料。（这些方法的完整列表在[DevicePolicyManager.getParentProfileInstance()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#getParentProfileInstance(android.content.ComponentName))文档中。）例如，[DevicePolicyManager.lockNow()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#lockNow())只锁定工作配置文件，而不是锁定整个设备。对于每个这些方法，您可以通过调用[DevicePolicyManager](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html)的父实例上的方法来获取旧的行为;您可以通过调用[DevicePolicyManager.getParentProfileInstance()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#getParentProfileInstance(android.content.ComponentName))获取此父对象。因此，例如，如果调用父实例的[lockNow()](https://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#lockNow())方法，整个设备将被锁定。

有关Android 7.0中Android for Work变更的详细信息，请参阅[Android for Work更新](https://developers.google.com/android/work/overview)。

### 注解保留
Android 7.0修复了注释的可见性被忽略的错误。 此问题使运行时访问它不应该能够访问的注释。 这些注释包括：
* `VISIBILITY_BUILD`：仅在构建时可见。
* `VISIBILITY_SYSTEM`：在运行时可见，但只能对底层系统。
如果您的应用依赖此行为，请为运行时必须可用的注释添加保留策略。可以使用`@Retention(RetentionPolicy.RUNTIME)`。

### 其他要点
* 当应用程式在Android 7.0上执行，但指定较低的API层级，且使用者变更显示大小时，应用程式流程就会遭到停用。应用程序必须能够正常处理此场景。否则，当用户从“最近”还原时，它会崩溃。

您应该测试您的应用程序，以确保不会发生此行为。您可以通过在通过DDMS手动杀死应用程序时导致相同的崩溃。

定位到Android 7.0（API级别24）及以上的应用在密度更改时不会自动终止;然而，他们仍然可能对配置更改做出不良反应。

* Android 7.0上的应用程序应该能够正常处理配置更改，并且不应在后续启动时崩溃。您可以通过更改字体大小（**设置** > **显示** > **字体大小**），然后从“最近”还原应用程序来验证应用程序行为。
* 由于以前版本的Android中的错误，系统没有标记写入主线程上的TCP套接字作为严格模式违例。 Android 7.0修复了这个bug。展示此行为的应用程序现在会抛出`android.os.NetworkOnMainThreadException`。一般来说，在主线程上执行网络操作是一个坏主意，因为这些操作通常具有高的尾延迟，导致ANR和jank。
* `Debug.startMethodTracing()`系列方法现在默认将输出存储在共享存储上的特定于软件包的目录中，而不是存储在SD卡的顶层。这意味着应用程序不再需要使用这些API的`WRITE_EXTERNAL_STORAGE`权限。
* 许多平台API现在已经开始检查通过[Binder](https://developer.android.com/reference/android/os/Binder.html)事务发送的大量有效负载，并且系统现在将`TransactionTooLargeExceptions`重新抛出为`RuntimeExceptions`，而不是静默地记录或抑制它们。一个常见的例子是在[Activity.onSaveInstanceState()中](https://developer.android.com/reference/android/app/Activity.html#onSaveInstanceState(android.os.Bundle))存储过多的数据，这会导致`ActivityThread.StopInfo`在您的应用程序针对Android 7.0时抛出一个`RuntimeException`。
* 如果应用程序向[视图](https://developer.android.com/reference/android/view/View.html发布了[Runnable](https://developer.android.com/reference/java/lang/Runnable.html)任务，并且[视图](https://developer.android.com/reference/android/view/View.html)未附加到窗口，则系统将[Runnable](https://developer.android.com/reference/java/lang/Runnable.html)任务与[视图](https://developer.android.com/reference/android/view/View.html)排队;在[视图](https://developer.android.com/reference/android/view/View.html)附加到窗口之前，[Runnable](https://developer.android.com/reference/java/lang/Runnable.html)任务才会执行。此行为修复了以下错误：
    * 如果一个应用程序从一个线程而不是想要的窗口的UI线程发布到一个[视图](https://developer.android.com/reference/android/view/View.html)，[Runnable](https://developer.android.com/reference/java/lang/Runnable.html)可能会运行在错误的线程作为结果。
    * 如果[Runnable](https://developer.android.com/reference/java/lang/Runnable.html)任务从线程而不是looper线程发布，则应用程序可能会暴露[Runnable](https://developer.android.com/reference/java/lang/Runnable.html)任务。
* 如果Android 7.0版（含[DELETE_PACKAGES](https://developer.android.com/reference/android/Manifest.permission.html#DELETE_PACKAGES)）权限的应用程式尝试删除套件，但其他应用程式已安装该套件，系统需要使用者确认。在这种情况下，应用程序应该期望[STATUS_PENDING_USER_ACTION](https://developer.android.com/reference/android/content/pm/PackageInstaller.html#STATUS_PENDING_USER_ACTION)作为返回状态时，他们调用[PackageInstaller.uninstall()](https://developer.android.com/reference/android/content/pm/PackageInstaller.html#uninstall(java.lang.String, android.content.IntentSender))。
不推荐使用名为Crypto的JCA提供程序，因为其唯一的算法SHA1PRNG的密码弱。应用程序不能再使用SHA1PRNG（不安全地）导出密钥，因为此提供程序已不可用。有关详细信息，请参阅博客文章[安全“Crypto”提供程序在Android N中已弃用](http://android-developers.blogspot.hk/2016/06/security-crypto-provider-deprecated-in.html)。
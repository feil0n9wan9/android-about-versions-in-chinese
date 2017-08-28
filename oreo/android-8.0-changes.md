# Android 8.0行为变更
Android 8.0除了提供诸多新特性和功能外，还对系统和API行为做出了各种变更。本文重点介绍您应该了解并在开发应用时加以考虑的一些主要变更。

其中大部分变更会影响所有应用，而不论应用针对的是何种版本的Android。不过，有几项变更仅影响针对Android 8.0的应用。为清楚起见，本页面分为两个部分：[针对所有API级别的应用](#atap)和[针对Android 8.0的应用](#o-apps)。

### <a id="atap"></a>针对所有API级别的应用
这些行为变更适用于在Android 8.0平台上运行的所有应用，无论这些应用是针对哪个API级别构建。所有开发者都应查看这些变更，并修改其应用以正确支持这些变更（如果适用）。

#### 后台执行限制
Android 8.0为提高电池寿命而引入的变更之一是，当您的应用进入[已缓存](https://developer.android.google.cn/guide/topics/processes/process-lifecycle.html)状态时，如果没有活动的[组件](https://developer.android.google.cn/guide/components/fundamentals.html#Components)，系统将解除应用具有的所有唤醒锁。

此外，为提高设备性能，系统会限制未在前台运行的应用的某些行为。具体而言：
* 现在，在后台运行的应用对后台服务的访问受到限制。
* 应用无法使用其清单注册大部分隐式广播（即，并非专门针对此应用的广播）。

默认情况下，这些限制仅适用于针对O的应用。不过，用户可以从**Settings**屏幕为任意应用启用这些限制，即使应用并不是以O为目标平台。

Android 8.0还对特定函数做出了以下变更：
* 如果针对Android 8.0的应用尝试在不允许其创建后台服务的情况下使用[startService()](https://developer.android.google.cn/reference/android/content/Context.html#startService&#40;android.content.Intent&#41;)函数，则该函数将引发一个[IllegalStateException](https://developer.android.google.cn/reference/java/lang/IllegalStateException.html)。
* 新的`Context.startForegroundService()`函数将启动一个前台服务。现在，即使应用在后台运行，系统也允许其调用`Context.startForegroundService()`。不过，应用必须在创建服务后的五秒内调用该服务的[startForeground()](https://developer.android.google.cn/reference/android/app/Service.html#startForeground&#40;int, android.app.Notification&#41;)函数。

如需了解详细信息，请参阅后[台执行限制](background.html)。

#### Android后台位置限制
为节约电池电量，保持良好的用户体验和确保系统健康运行，在运行Android 8.0的设备上使用后台应用时，降低了后台应用接收位置更新的频率。此行为变更会影响包括Google Play服务在内的所有接收位置更新的应用。

此类变更会影响以下API：
* Fused Location Provider(FLP)
* Geofencing
* GNSS Measurements
* Location Manager
* Wi-Fi Manager

为确保您的应用按预期方式运行，请完成以下步骤：
* 查看您的应用的逻辑，并确保您使用的是最新的位置API。
* 测试您的应用是否在每个用例中都表现出预期行为。
* 考虑使用[Fused Location Provider (FLP)](https://developers.google.cn/android/reference/com/google/android/gms/location/FusedLocationProviderApi)或地理围栏来处理依赖于用户当前位置的用例。

如需了解此类变更的详细信息，请参阅[后台位置限制](background-location-limits.html)。

#### 应用快捷键
Android 8.0对应用快捷方式做出了以下变更：
* `com.android.launcher.action.INSTALL_SHORTCUT`广播不再会对您的应用有任何影响，因为它现在是私有的隐式广播。相反，您应使用[ShortcutManager](https://developer.android.google.cn/reference/android/content/pm/ShortcutManager.html)类中的[requestPinShortcut()](https://developer.android.google.cn/reference/android/content/pm/ShortcutManager.html#requestPinShortcut&#40;android.content.pm.ShortcutInfo, android.content.IntentSender&#41;)函数创建应用快捷方式。
* 现在，[ACTION_CREATE_SHORTCUT](https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_CREATE_SHORTCUT) Intent可以创建可使用[ShortcutManager](https://developer.android.google.cn/reference/android/content/pm/ShortcutManager.html)类进行管理的应用快捷方式。此Intent还可以创建不与[ShortcutManager](https://developer.android.google.cn/reference/android/content/pm/ShortcutManager.html)交互的旧版启动器快捷方式。在以前，此Intent只能创建旧版启动器快捷方式。
* 现在，使用[requestPinShortcut()](https://developer.android.google.cn/reference/android/content/pm/ShortcutManager.html#requestPinShortcut&#40;android.content.pm.ShortcutInfo, android.content.IntentSender&#41;)创建的快捷方式和在处理[ACTION_CREATE_SHORTCUT](https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_CREATE_SHORTCUT) Intent的操作组件中创建的快捷方式均已转换为功能齐全的应用快捷方式。因此，应用现在可以使用[ShortcutManager](https://developer.android.google.cn/reference/android/content/pm/ShortcutManager.html)中的函数来更新这些快捷方式。
* 旧版快捷方式仍然保留了它们在旧版Android中的功能，但您必须在应用中手动将它们转换成应用快捷方式。

如需了解有关应用快捷方式变更的更多信息，请参阅[固定快捷方式和小部件预览功能指南](https://developer.android.google.cn/guide/topics/ui/shortcuts.html)。

#### <a id="lai"></a>语言区域和国际化
Android 7.0（API级别24）引入能指定默认类别语言区域的概念，但是某些API在本应使用默认[DISPLAY](https://developer.android.google.cn/reference/java/util/Locale.Category.html#DISPLAY)类别语言区域时，仍然使用不带参数的通用[Locale.getDefault()](https://developer.android.google.cn/reference/java/util/Locale.html#getDefault&#40;&#41;)函数。现在，在Android 8.0中，以下函数使用[Locale.getDefault(Category.DISPLAY)](https://developer.android.google.cn/reference/java/util/Locale.html#getDefault&#41;java.util.Locale.Category&#41;)来代替[Locale.getDefault()](https://developer.android.google.cn/reference/java/util/Locale.html#getDefault&#40;&#41;)：
* [Currency.getDisplayName()](https://developer.android.google.cn/reference/java/util/Currency.html#getDisplayName&#40;&#41;)
* [Currency.getSymbol()](https://developer.android.google.cn/reference/java/util/Currency.html#getSymbol&#40;&#41;)
* [Locale.getDisplayScript()](https://developer.android.google.cn/reference/java/util/Locale.html#getDisplayScript&#40;&#41;)
当为[Locale](https://developer.android.google.cn/reference/java/util/Locale.html)参数指定的***displayScript***值不可用时，[Locale.getDisplayScript(Locale)](https://developer.android.google.cn/reference/java/util/Locale.html#getDisplayScript&#40;&#41;)同样回退到[Locale.getDefault()](https://developer.android.google.cn/reference/java/util/Locale.html#getDefault&#40;&#41;)。

与语言区域和国际化有关的其他变更如下：
* 调用[Currency.getDisplayName(null)](https://developer.android.google.cn/reference/java/util/Currency.html#getDisplayName&#40;&#41;)会引发[NullPointerException](https://developer.android.google.cn/reference/java/lang/NullPointerException.html)，以与文档规定的行为保持一致。
* 时区名称的分析方法发生变化。之前，Android设备使用在启动时取样的系统时钟值，缓存用于分析日期时间的时区名称。因此，如果在启动时或其他较为罕见的情况下系统时钟出错，可能对分析产生负面影响。
现在，一般情况下，在分析时区名称时分析逻辑将使用ICU和当前系统时钟值。此项变更可提供更加准确的结果，如果您的应用使用[SimpleDateFormat](https://developer.android.google.cn/reference/java/text/SimpleDateFormat.html)等类，此结果可能与之前的Android版本不同。
* Android 8.0将ICU的版本更新至版本58。

#### 提醒窗口
如果应用使用[SYSTEM_ALERT_WINDOW](https://developer.android.google.cn/reference/android/Manifest.permission.html#SYSTEM_ALERT_WINDOW)权限并且尝试使用以下窗口类型之一来在其他应用和系统窗口上方显示提醒窗口：
* [TYPE_PHONE](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_PHONE)
* [TYPE_PRIORITY_PHONE](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_PRIORITY_PHONE)
* [TYPE_SYSTEM_ALERT](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_SYSTEM_ALERT)
* [TYPE_SYSTEM_OVERLAY](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_SYSTEM_OVERLAY)
* [TYPE_SYSTEM_ERROR](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_SYSTEM_ERROR)

...那么，这些窗口将始终显示在使用[TYPE_APPLICATION_OVERLAY](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_APPLICATION_OVERLAY)窗口类型的窗口下方。如果应用针对的是Android 8.0，则应用会使用[TYPE_APPLICATION_OVERLAY](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_APPLICATION_OVERLAY)窗口类型来显示提醒窗口。

如需了解详细信息，请参阅[针对Android 8.0的应用](Android-8.0-changes.html#ato)的行为变更内的[提醒窗口的常用窗口类型](Android-8.0-changes.html#cwt)部分。

#### 输入和导航
随着Android应用出现在Chrome操作系统和平板电脑等其他大尺寸设备上，我们看到，用户在Android应用中又重新开始使用键盘导航。在Android 8.0中，我们又再次使用键盘作为导航输入设备，从而为基于箭头和标签的导航构建了一种更可靠并且可预测的模型。

尤其要指出的是，我们对元素焦点行为做出以下变更：
* 现在，如果您没有为[View](https://developer.android.google.cn/reference/android/view/View.html)对象（前景或背景图片）定义任何焦点状态颜色，框架会为[View](https://developer.android.google.cn/reference/android/view/View.html)设置默认的焦点突出显示颜色。此焦点突出显示标志是基于操作组件主题背景的涟漪图片。
* 如果您不希望[View](https://developer.android.google.cn/reference/android/view/View.html)对象在接收焦点时使用此默认突出显示标志，请在包含[View](https://developer.android.google.cn/reference/android/view/View.html)的布局XML文件中将`android:defaultFocusHighlightEnabled`属性设置为`false`，或者将`false`传递至应用界面逻辑中的`setDefaultFocusHighlightEnabled()`。
* 要测试键盘输入对界面元素焦点有何影响，您可以启用**Drawing > Show layout bounds**开发者选项。在Android 8.0中，此选项在当前具有焦点的元素上显示一个“X”图标。

另外，Android 8.0中的所有工具栏元素自动组成[键盘导航键区](android-8.0.html#kbnc)，用户可以更加轻松地导航进入和离开每个作为一个整体的工具栏。

如需详细了解如何在您的应用中改善对键盘导航的支持，请阅读[支持键盘导航](https://developer.android.google.cn/training/keyboard-input/navigation.html)指南。

#### 网页表单自动填充
现在，Android自动填充框架提供对自动填充功能的内置支持，对于安装到运行Android 8.0的设备上的应用，与[WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)对象相关的下列函数已经发生变化：

[WebSettings](https://developer.android.google.cn/reference/android/webkit/WebSettings.html)
* [getSaveFormData()](https://developer.android.google.cn/reference/android/webkit/WebSettings.html#getSaveFormData&#40;&#41;)函数现在返回`false`。之前，此函数返回`true`。
* 调用[setSaveFormData()](https://developer.android.google.cn/reference/android/webkit/WebSettings.html#setSaveFormData&#40;boolean&#41;)不再有任何效果。

[WebViewDatabase](https://developer.android.google.cn/reference/android/webkit/WebViewDatabase.html)
* 调用[clearFormData()](https://developer.android.google.cn/reference/android/webkit/WebViewDatabase.html#clearFormData&#40;&#41;)不再有任何效果。
* [hasFormData()](https://developer.android.google.cn/reference/android/webkit/WebViewDatabase.html#hasFormData&#40;&#41;)函数现在返回`false`。之前，当表单包含数据时，此函数返回`true`。

#### 无障碍功能
现在，无障碍服务可识别应用的[TextView](https://developer.android.google.cn/reference/android/widget/TextView.html)对象内部的所有[ClickableSpan](https://developer.android.google.cn/reference/android/text/style/ClickableSpan.html)实例。

如需了解有关如何让您的应用更便于访问的更多信息，请参阅[无障碍功能](https://developer.android.google.cn/guide/topics/ui/accessibility/index.html)。

#### <a id="networking-all"></a>网络连接和HTTP(S)连接
Android 8.0对网络连接和HTTP(S)连接行为做出了以下变更：
* 无正文的OPTIONS请求具有`Content-Length: 0`标头。之前，这些请求没有`Content-Length`标头。
* HttpURLConnection 在包含斜线的主机或颁发机构名称后面附加一条斜线，使包含空路径的网址规范化。例如，它将`http://example.com`转化为`http://example.com/`。
* 通过ProxySelector.setDefault()设置的自定义代理选择器仅针对所请求的网址（架构、主机和端口）。因此，仅可根据这些值选择代理。传递至自定义代理选择器的网址不包含所请求的网址的路径、查询参数或片段。
* URI不能包含空白标签。

  之前，平台支持一种权宜方法，即允许主机名称中包含空白标签，但这是对URI的非法使用。此权宜方法只是为了确保与旧版libcore兼容。开发者如果对API使用不当，将会看到一条ADB消息：“URI example..com的主机名包含空白标签。此格式不正确，将不被未来的Android版本所接受。”Android 8.0废除了此权宜方法；系统对格式错误的URI会返回null。
* Android 8.0在实现HttpsURLConnection时不会执行不安全的TLS/SSL协议版本回退。
* 对隧道HTTP(S)连接处理进行了如下变更：
  * 在通过连接建立隧道HTTP(S)连接时，系统会在Host行中正确放置端口号(:443)并将此信息发送至中间服务器。之前，端口号仅出现在CONNECT行中。
  * 系统不再将隧道连接请求中的user-agent和和proxy-authorization标头发送至代理服务器。

    在建立隧道时，系统不再将隧道Http(s)URLConnection中的 proxy-authorization标头发送至代理。相反，由系统生成proxy-authorization标头，在代理响应初始请求发送HTTP 407后将其发送至此代理。

    同样地，系统不再将user-agent标头由隧道连接请求复制到建立隧道的代理请求。相反，库为此请求生成user-agent标头。
* 如果之前执行的connect()方法失败，[send(java.net.DatagramPacket)](https://developer.android.google.cn/reference/java/net/DatagramSocket.html#send&#40;java.net.DatagramPacket&#41;)方法将会引发SocketException。
  * 如果存在内部错误，DatagramSocket.connect()会引发 =pendingSocketException。对于Android 8.0之前的版本，即使send()调用成功，后续的recv()调用也会引发SocketException。为确保一致性，现在这两个调用均会引发SocketException。
* 在回退到TCP Echo协议之前，InetAddress.isReachable()会尝试执行ICMP。
  * 对于某些屏蔽端口7(TCP Echo)的主机（例如 google.com），如果它们接受ICMP Echo协议，现在也许能够访问它们。
  * 对于确实无法访问的主机，此项变更意味着调用需要两倍的时间才能返回结果。

#### 蓝牙
Android 8.0对[ScanRecord.getBytes()](https://developer.android.google.cn/reference/android/bluetooth/le/ScanRecord.html#getBytes&#40;&#41;)方法检索的数据长度做出了以下变更：
* [getBytes()](https://developer.android.google.cn/reference/android/bluetooth/le/ScanRecord.html#getBytes&#40;&#41;)方法对于所接收的字节数不作任何假定。因此，应用不应受所返回的任何最小或最大字节数的影响。相反，应用应当计算所返回数组的长度。
* 兼容蓝牙5的设备返回的数据长度可能会超出之前最大60个字节的限制。
* 如果远程设备未提供扫描响应，则也可能返回少于60个字节的数据。

#### 无缝连接
Android 8.0对WLAN设置进行了多项改进，这样可以更轻松地选择能够提供最佳用户体验的WLAN网络。具体变更包括：
* 稳定性和可靠性改进。
* 更加直观的界面。
* 一个合并的WLAN首选项菜单。
* 当附近存在优质的已保存网络时在兼容设备上自动激活WLAN。

#### 安全性
Android 8.0包含以下与安全性有关的变更：
* 此平台不再支持 SSLv3。
* 在与未正确实现TLS协议版本协商的服务器建立HTTPS连接时，[HttpsURLConnection](https://developer.android.google.cn/reference/javax/net/ssl/HttpsURLConnection.html)不再尝试回退到之前的TLS协议版本并重试的权宜方法。
* Android 8.0将使用安全计算(SECCOMP)过滤器来过滤所有应用。允许的系统调用列表仅限于通过bionic公开的系统调用。此外，还提供了其他几个后向兼容的系统调用，但我们不建议使用这些系统调用。
* 现在，您的应用的[WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)对象将在多进程模式下运行。网页内容在独立的进程中处理，此进程与包含应用的进程相隔离，以提高安全性。
* 您无法再假定 APK 驻留在名称以-1或-2结尾的目录中。应用应使用[sourceDir](https://developer.android.google.cn/reference/android/content/pm/ApplicationInfo.html#sourceDir)获取此目录，而不能直接使用目录格式。
* 如需了解与使用原生库有关的安全性增强的信息，请参阅[原生库](#nl)。

有关提升应用安全性的其他准则，请参阅[面向Android开发者的安全性](https://developer.android.google.cn/topic/security/index.html)。

#### 隐私性
Android 8.0对平台做出了以下与隐私性有关的变更。
* 现在，平台改变了标识符的处理方式。
    * 对于在OTA之前安装到某个版本Android 8.0（API级别26）的应用，除非在OTA后卸载并重新安装，否则[ANDROID_ID](https://developer.android.google.cn/reference/android/provider/Settings.Secure.html#ANDROID_ID)的值将保持不变。要在OTA后在卸载期间保留值，开发者可以使用密钥/值备份关联旧值和新值。
    * 对于安装在运行Android 8.0的设备上的应用，[ANDROID_ID](https://developer.android.google.cn/reference/android/provider/Settings.Secure.html#ANDROID_ID)的值现在将根据应用签署密钥和用户确定作用域。应用签署密钥、用户和设备的每个组合都具有唯一的[ANDROID_ID](https://developer.android.google.cn/reference/android/provider/Settings.Secure.html#ANDROID_ID)值。因此，在相同设备上运行但具有不同签署密钥的应用将不会再看到相同的[ANDROID_ID](https://developer.android.google.cn/reference/android/provider/Settings.Secure.html#ANDROID_ID)（即使对于同一用户来说，也是如此）。
    * 现在，根据应用（而不是根据用户）来确定[ANDROID_ID](https://developer.android.google.cn/reference/android/provider/Settings.Secure.html#ANDROID_ID)的值范围。应用软件包名称、签名、用户和设备的每个组合都具有唯一的[ANDROID_ID](https://developer.android.google.cn/reference/android/provider/Settings.Secure.html#ANDROID_ID)值。同一个设备上运行的两个应用不再出现Android ID相同的情况，因此不可能建立关联。
    * 只要签署密钥相同（并且应用未在OTA之前安装到某个版本的O），[ANDROID_ID](https://developer.android.google.cn/reference/android/provider/Settings.Secure.html#ANDROID_ID)的值在软件包卸载或重新安装时就不会发生变化。
    * 即使系统更新导致软件包签署密钥发生变化，[ANDROID_ID](https://developer.android.google.cn/reference/android/provider/Settings.Secure.html#ANDROID_ID)的值也不会变化。

      要借助一个简单的标准系统实现应用获利，请使用广告ID。广告ID是Google Play服务针对广告服务提供的唯一ID，此ID可由用户重置。

  * 查询`net.hostname`系统属性返回的结果为空。

#### 记录未捕获的异常
如果某个应用安装的[Thread.UncaughtExceptionHandler](https://developer.android.google.cn/reference/java/lang/Thread.UncaughtExceptionHandler.html)未移交给默认的[Thread.UncaughtExceptionHandler](https://developer.android.google.cn/reference/java/lang/Thread.UncaughtExceptionHandler.html)，则当出现未捕获的异常时，系统不会终止应用。从Android 8.0开始，在此情况下系统将记录异常堆栈跟踪情况；在之前的平台版本中，系统不会记录异常堆栈跟踪情况。

我们建议，自定义[Thread.UncaughtExceptionHandler](https://developer.android.google.cn/reference/java/lang/Thread.UncaughtExceptionHandler.html)实现始终移交给默认处理程序处理；遵循此建议的应用不受Android 8.0此项变更的影响。

#### 联系人提供程序使用情况统计方法的变更
在之前版本的Android 中，联系人提供程序组件允许开发者获取每个联系人的使用情况数据。此使用情况数据揭示了与某个联系人相关联的每个电子邮件地址和每个电话号码的信息，包括与该联系人联系的次数以及上次联系该联系人的时间。请求[READ_CONTACTS](https://developer.android.google.cn/reference/android/Manifest.permission.html#READ_CONTACTS)权限的应用可以读取此数据。

如果应用请求[READ_CONTACTS](https://developer.android.google.cn/reference/android/Manifest.permission.html#READ_CONTACTS)权限，它们仍可以读取此数据。从Android 8.0开始，使用情况数据查询会返回近似值，而不是精确值。不过，Android系统内部仍然会保留精确值，因此，此变更不会影响auto-complete API。

此行为变更会影响以下查询参数：
* [TIMES_CONTACTED](https://developer.android.google.cn/reference/android/provider/ContactsContract.ContactOptionsColumns.html#TIMES_CONTACTED)
* [TIMES_USED](https://developer.android.google.cn/reference/android/provider/ContactsContract.DataUsageStatColumns.html#TIMES_USED)
* [LAST_TIME_CONTACTED](https://developer.android.google.cn/reference/android/provider/ContactsContract.ContactOptionsColumns.html#LAST_TIME_CONTACTED)
* [LAST_TIME_USED](https://developer.android.google.cn/reference/android/provider/ContactsContract.DataUsageStatColumns.html#LAST_TIME_USED)

#### <a id="ch-all"></a>集合的处理
现在，[AbstractCollection.removeAll()](https://developer.android.google.cn/reference/java/util/AbstractCollection.html#removeAll&#40;java.util.Collection<?>&#41;)和[AbstractCollection.retainAll()](https://developer.android.google.cn/reference/java/util/AbstractCollection.html#retainAll&#40;java.util.Collection<?>&#41;)始终引发[NullPointerException](https://developer.android.google.cn/reference/java/lang/NullPointerException.html)；之前，当集合为空时不会引发[NullPointerException](https://developer.android.google.cn/reference/java/lang/NullPointerException.html)。此项变更使行为符合文档要求。

#### Android企业版
Android 8.0[更改](https://developer.android.com/work/versions/Android-8.0.html#behavior-changes)了企业应用（包括设备规范控制器 (DPC)）的某些API和功能的行为。这些变更包括：
* 新增多种行为，帮助应用支持完全托管设备中的工作资料。
* 变更系统更新处理、应用验证和身份验证方式，以提高设备和系统的完整性。
* 改进用户在配置、通知、“最近使用的应用”屏幕和Always on VPN方面的体验。

如需查看Android 8.0中的所有企业版变更和了解它们可能给您的应用带来的影响，请阅读[企业中的 Android](https://developer.android.com/work/versions/Android-8.0.html#behavior-changes)。

### <a id="o-apps"></a>针对Android 8.0的应用
这些行为变更专门应用于针对O平台或更高平台版本的应用。针对Android 8.0或更高平台版本进行编译，或将`targetSdkVersion`设为Android 8.0或更高版本的应用开发者必须修改其应用以正确支持这些行为（如果适用）。

#### 提醒窗口
使用[SYSTEM_ALERT_WINDOW](https://developer.android.google.cn/reference/android/Manifest.permission.html#SYSTEM_ALERT_WINDOW)权限的应用无法再使用以下窗口类型来在其他应用和系统窗口上方显示提醒窗口：
* [TYPE_PHONE](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_PHONE)
* [TYPE_PRIORITY_PHONE](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_PRIORITY_PHONE)
* [TYPE_SYSTEM_ALERT](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_SYSTEM_ALERT)
* [TYPE_SYSTEM_OVERLAY](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_SYSTEM_OVERLAY)
* [TYPE_SYSTEM_ERROR](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_SYSTEM_ERROR)

相反，应用必须使用名为[TYPE_APPLICATION_OVERLAY](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_APPLICATION_OVERLAY)的新窗口类型。

使用[TYPE_APPLICATION_OVERLAY](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_APPLICATION_OVERLAY)窗口类型显示应用的提醒窗口时，请记住新窗口类型的以下特性：
* 应用的提醒窗口始终显示在状态栏和输入法等关键系统窗口的下面。
* 系统可以移动使用[TYPE_APPLICATION_OVERLAY](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_APPLICATION_OVERLAY)窗口类型的窗口或调整其大小，以改善屏幕显示效果。
* 通过打开通知栏，用户可以访问设置来阻止应用显示使用[TYPE_APPLICATION_OVERLAY](https://developer.android.google.cn/reference/android/view/WindowManager.LayoutParams.html#TYPE_APPLICATION_OVERLAY)窗口类型显示的提醒窗口。

#### 内容变更通知
Android 8.0更改了[ContentResolver.notifyChange()](https://developer.android.google.cn/reference/android/content/ContentResolver.html#notifyChange&#40;android.net.Uri, android.database.ContentObserver&#41;)和[registerContentObserver(Uri, boolean, ContentObserver)](https://developer.android.google.cn/reference/android/content/ContentResolver.html#registerContentObserver&#40;android.net.Uri, boolean, android.database.ContentObserver&#41;)在面向针对Android 8.0的应用中的行为方式。

现在，这些API需要在所有URI中为颁发机构定义一个有效的[ContentProvider](https://developer.android.google.cn/reference/android/content/ContentProvider.html)。使用相关权限定义一个有效的[ContentProvider](https://developer.android.google.cn/reference/android/content/ContentProvider.html)可帮助您的应用防范来自恶意应用的内容变更，并防止将可能的私密数据泄露给恶意应用。

#### 视图焦点
可点击的[View](https://developer.android.google.cn/reference/android/view/View.html)对象现在默认也可以成为焦点。如果您希望[View](https://developer.android.google.cn/reference/android/view/View.html)对象可点击但不可成为焦点，请在包含[View](https://developer.android.google.cn/reference/android/view/View.html)的布局XML文件中将[android:focusable](https://developer.android.google.cn/reference/android/view/View.html#attr_android:focusable)属性设置为`false`，或者将`false`传递至应用界面逻辑中的[setFocusable()](https://developer.android.google.cn/reference/android/view/View.html#setFocusable&#40;boolean&#41;)。

#### 安全性
如果您的应用的网络安全性配置[选择退出](https://developer.android.google.cn/training/articles/security-config.html#CleartextTrafficPermitted)对明文流量的支持，那么，您的应用的[WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)对象无法通过HTTP访问网站。每个[WebView](https://developer.android.google.cn/reference/android/webkit/WebView.html)对象必须转而使用HTTPS。

有关提升应用安全性的其他准则，请参阅[面向Android开发者的安全性](https://developer.android.google.cn/topic/security/index.html)。

#### 帐号访问和可检测性
除非身份验证器拥有用户帐号或用户授予访问权限，否则，应用将无法再访问用户帐号。仅拥有[GET_ACCOUNTS](https://developer.android.google.cn/reference/android/Manifest.permission.html#GET_ACCOUNTS)权限尚不足以访问用户帐号。要获得帐号访问权限，应用应使用[AccountManager.newChooseAccountIntent()](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#newChooseAccountIntent&#40;android.accounts.Account, java.util.List<android.accounts.Account>, java.lang.String[], java.lang.String, java.lang.String, java.lang.String[], android.os.Bundle&#41;)或特定于身份验证器的方法。获得帐号访问权限后，应用可以调用[AccountManager.getAccounts()](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#getAccounts&#40;&#41;)来访问帐号。

Android 8.0已弃用[LOGIN_ACCOUNTS_CHANGED_ACTION](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#LOGIN_ACCOUNTS_CHANGED_ACTION)。相反，应用在运行时应使用[addOnAccountsUpdatedListener()](https://developer.android.google.cn/reference/android/accounts/AccountManager.html#addOnAccountsUpdatedListener&#40;android.accounts.OnAccountsUpdateListener, android.os.Handler, boolean&#41;)获取帐号更新信息。

有关新增API和增加的帐号访问和可检测性方法的信息，请参阅此文档的“新增 API”部分中的[帐号访问和可检测性](android-8.0.html#naa)。

#### <a id="pr"></a>隐私性
以下变更影响Android 8.0的隐私性。
* 系统属性`net.dns1`、`net.dns2`、`net.dns3`和`net.dns4`不再可用，此项变更可加强平台的隐私性。
* 要获取DNS服务器之类的网络连接信息，具有[ACCESS_NETWORK_STATE](https://developer.android.google.cn/reference/android/Manifest.permission.html#ACCESS_NETWORK_STATE)权限的应用可以注册[NetworkRequest](https://developer.android.google.cn/reference/android/net/NetworkRequest.html)或[NetworkCallback](https://developer.android.google.cn/reference/android/net/ConnectivityManager.NetworkCallback.html)对象。这些类在Android 5.0（API 级别 21）及更高版本中提供。
* 从 Android 8.0开始，不再支持Build.SERIAL，此字段将返回一个未定义的值。需要知道硬件序列号的应用应改为使用新的[Build.getSerial()](https://developer.android.google.cn/reference/android/os/Build.html#getSeriale#40;&#41;)方法，该方法要求具有[READ_PHONE_STATE](https://developer.android.google.cn/reference/android/Manifest.permission.html#READ_PHONE_STATE)权限。
* [LauncherApps](https://developer.android.google.cn/reference/android/content/pm/LauncherApps.html) API不再允许托管配置文件应用获取有关主配置文件的信息。当某个用户在托管配置文件中时，[LauncherApps](https://developer.android.google.cn/reference/android/content/pm/LauncherApps.html) API的行为就像同一配置文件组的其他配置文件中未安装任何应用一样。和之前一样，尝试访问无关联的配置文件会引发SecurityExceptions。

#### 权限
在Android 8.0之前，如果应用在运行时请求权限并且被授予该权限，系统会错误地将属于同一权限组并且在清单中注册的其他权限也一起授予应用。

对于针对Android 8.0的应用，此行为已被纠正。系统只会授予应用明确请求的权限。然而，一旦用户为应用授予某个权限，则所有后续对该权限组中权限的请求都将被自动批准。

例如，假设某个应用在其清单中列出[READ_EXTERNAL_STORAGE](https://developer.android.google.cn/reference/android/Manifest.permission.html#READ_EXTERNAL_STORAGE)和[WRITE_EXTERNAL_STORAGE](https://developer.android.google.cn/reference/android/Manifest.permission.html#WRITE_EXTERNAL_STORAGE)。应用请求[READ_EXTERNAL_STORAGE](https://developer.android.google.cn/reference/android/Manifest.permission.html#READ_EXTERNAL_STORAGE)，并且用户授予了该权限。如果该应用针对的是API级别24或更低级别，系统还会同时授予[WRITE_EXTERNAL_STORAGE](https://developer.android.google.cn/reference/android/Manifest.permission.html#WRITE_EXTERNAL_STORAGE)，因为该权限也属于同一[STORAGE](https://developer.android.google.cn/reference/android/Manifest.permission_group.html#STORAGE)权限组并且也在清单中注册过。如果该应用针对的是Android 8.0，则系统此时仅会授予[READ_EXTERNAL_STORAGE](https://developer.android.google.cn/reference/android/Manifest.permission.html#READ_EXTERNAL_STORAGE)；不过，如果该应用后来又请求[WRITE_EXTERNAL_STORAGE](https://developer.android.google.cn/reference/android/Manifest.permission.html#WRITE_EXTERNAL_STORAGE)，则系统会立即授予该权限，而不会提示用户。

#### 媒体
* 框架会执行音频闪避。进行[AUDIOFOCUS_GAIN_TRANSIENT_MAY_DUCK](https://developer.android.google.cn/reference/android/media/AudioManager.html#AUDIOFOCUS_GAIN_TRANSIENT_MAY_DUCK)时，应用不会失去焦点。新的API适用于需要暂停而不是闪避的应用。请注意，此行为无法在Android 8.0 1版本中实现。
* 当用户打电话时，活动的媒体流将在通话期间静音。
* 音频流类型应仅用于音量控制；所有其他流类型的使用（例如[AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html)构造函数）仍有效，但系统会将其作为错误记录下来。
* 所有与音频相关的 API 均使用[AudioAttributes](https://developer.android.google.cn/reference/android/media/AudioAttributes.html)来描述音频播放用例。
* 使用[AudioTrack](https://developer.android.google.cn/reference/android/media/AudioTrack.html)时，如果应用请求了足够大的音频缓冲区，则框架将尝试使用深度缓冲区输出（如果可用）。
* 在Android 8.0中，媒体按钮事件的处理有所不同：
  1. 在界面操作组件中处理媒体按钮未发生变化：前台操作组件在处理媒体按钮时仍然优先。
  2. 如果前台操作组件不处理媒体按钮，系统会将媒体按钮路由到最近在本地播放音频的应用。在确定哪些应用接收媒体按钮事件时，不再考虑活动状态、标志和媒体会话的播放状态。即使在应用调用 setActive(false) 后，媒体会话仍然可以接收媒体按钮事件。
  3. 如果应用的媒体会话已经释放，系统会将媒体按钮事件发送到应用的 MediaButtonReceiver（如果有）。
  4. 对于任何其他情况，系统都会舍弃媒体按钮事件。与其开始播放错误的应用，不如不播放任何东西。

  下图汇总了新的媒体按钮路由逻辑。

  媒体按钮路由

#### <a id="nl"></a>原生库
在针对Android 8.0的应用中，如果原生库包含任何可写且可执行的加载代码段，则不会再加载原生库。倘若某些应用的原生库包含不正确的加载代码段，则此变更可能会导致这些应用停止工作。这是一种安全加强措施。

如需了解详细信息，请参阅[可写且可执行的代码段](https://android.googlesource.com/platform/bionic/+/master/android-changes-for-ndk-developers.md#Writable-and-Executable-Segments-Enforced-for-API-level-26)。

与早期的开发者预览版相同，Android 8.0还有助于更轻松地发现所有与链接器有关的问题。链接器的变更绑定到应用的目标API级别。如果应用的目标API级别发生链接器变更，则该应用无法加载该库。如果您的目标API级别低于发生链接器变更的API级别，则logcat会显示一条警告消息。在预览版期间，与链接器有关的问题不仅会显示在logcat中，也会以toast的形式显示。对于特定的API级别，警告可能会变成错误，此变更有助于提前发现此类问题。

#### <a id="o-ch"></a>集合的处理
在Android 8.0中，[Collections.sort()](https://developer.android.google.cn/reference/java/util/Collections.html#sort&#40;java.util.List<T>#41;)是在[List.sort()](https://developer.android.google.cn/reference/java/util/List.html#sort&#40;java.util.Comparator<? super E>&#41;)的基础上实现的。在Android 7.x（API 级别24和25）中，则恰恰相反。在过去，[List.sort()](https://developer.android.google.cn/reference/java/util/List.html#sort&#40;java.util.Comparator<? super E>&#41;)的默认实现会调用[Collections.sort()](https://developer.android.google.cn/reference/java/util/Collections.html#sort&#40;java.util.List<T>&#41;)。

此项变更使[Collections.sort()](https://developer.android.google.cn/reference/java/util/Collections.html#sort&#40;java.util.List<T>&#41;)可以利用优化的[List.sort()](https://developer.android.google.cn/reference/java/util/List.html#sort&#40;java.util.Comparator<? super E>&#41;)实现，但具有以下限制：

* [List.sort()](https://developer.android.google.cn/reference/java/util/List.html#sort&#40;java.util.Comparator<? super E>&#41;)的实现不能调用[Collections.sort()](https://developer.android.google.cn/reference/java/util/Collections.html#sort&#40;java.util.List<T>&#41;)，因为这会导致堆栈因无限递归而溢出。相反，如果您需要[List](https://developer.android.google.cn/reference/java/util/List.html)实现的默认行为，应避免重写`sort()`。
如果父类以不适当的方法实现`sort()`，通常最好使用在[List.toArray()](https://developer.android.google.cn/reference/java/util/List.html#toArray&#40;&#41;)、[Arrays.sort()](https://developer.android.google.cn/reference/java/util/Arrays.html#sort&#40;java.lang.Object[]&#41;) 和[ListIterator.set()](https://developer.android.google.cn/reference/java/util/ListIterator.html#set&#40;E&#41;)的基础上构建的实现重写[List.sort()](https://developer.android.google.cn/reference/java/util/List.html#sort&#40;java.util.Comparator<? super E>&#41;)。例如：
```
@Override
public void sort(Comparator<? super E> c) {
  Object[] elements = toArray();
  Arrays.sort(elements, c);
  ListIterator<E> iterator = (ListIterator<Object>) listIterator();
  for (Object element : elements) {
    iterator.next();
    iterator.set((E) element);
  }
}
```
在大多数情况下，您也可以使用根据API级别委托给其他默认实现的实现重写[List.sort()](https://developer.android.google.cn/reference/java/util/List.html#sort&#40;java.util.Comparator<? super E>&#41;)。例如：
```
@Override
public void sort(Comparator<? super E> comparator) {
  if (Build.VERSION.SDK_INT <= 25) {
    Collections.sort(this);
  } else {
    super.sort(comparator);
  }
}
```
如果您选择后者只是因为您希望开发一种适用于所有API级别的`sort()`方法，可以考虑赋予其一个唯一的名称，例如`sortCompat()`，而不是重写 sort()。
* 现在，[Collections.sort()](https://developer.android.google.cn/reference/java/util/Collections.html#sort&#40;java.util.List<T>&#41;)只是对调用`sort()`的List实现进行的一项结构性修改。例如，在Android 8.0之前的平台版本中，如果通过调用[List.sort()](https://developer.android.google.cn/reference/java/util/List.html#sort&#40;java.util.Comparator<? super E>&#41;)进行排序，则当迭代处理[ArrayList](https://developer.android.google.cn/reference/java/util/ArrayList.html)以及在迭代过程中调用`sort()`时，会引发[ConcurrentModificationException](https://developer.android.google.cn/reference/java/util/ConcurrentModificationException.html)。而[Collections.sort()](https://developer.android.google.cn/reference/java/util/Collections.html#sort&#40;java.util.List<T>&#41;)则不会引发异常。
此项变更使平台行为更加一致：现在，两种方法都会引发[ConcurrentModificationException](https://developer.android.google.cn/reference/java/util/ConcurrentModificationException.html)。

#### 类加载行为
Android 8.0检查确保类加载器在加载新类时不会违反运行时假设条件。不论类引用自Java（来自[forName()](https://developer.android.google.cn/reference/java/lang/Class.html#forName&#40;java.lang.String, boolean, java.lang.ClassLoader&#41;)）、Dalvik字节码还是JNI，都会执行这些检查。平台不会拦截Java对[loadClass()](https://developer.android.google.cn/reference/java/lang/ClassLoader.html#loadClass&#40;java.lang.String&#41;)函数的直接调用，也不会检查此类调用的结果。此行为不应影响运行良好的类加载器的正常运行。

平台将检查类加载器返回的类描述符是否与预期的描述符一致。如果返回的描述符与预期不符，平台会引发 NoClassDefFoundError 错误，并在异常日志中存储一条注明不一致之处的详细错误消息。

平台还检查请求的类描述符是否有效。此检查捕获间接加载诸如`GetFieldID()`等类的JNI调用，向这些类传递无效的描述符。例如，找不到包含`java/lang/String`签名的字段，是因为此签名无效；它应为`Ljava/lang/String;`。

这与JNI对`FindClass()`的调用不同，其中`java/lang/String`是一个有效的完全限定名称。

Android 8.0不支持多个类加载器同时尝试使用相同的DexFile对象来定义类。尝试进行此操作，会导致Android运行时引发[InternalError](https://developer.android.google.cn/reference/java/lang/InternalError.html)错误，同时显示消息“Attempt to register dex file `<filename>` with multiple class loaders”。

DexFile API现已弃用，强烈建议您改为使用此平台的类加载器之一，包括`PathClassLoader`或`BaseDexClassLoader`。

> **注**： 您可以创建多个引用文件系统中同一个APK或JAR文件容器的类加载器。这样做通常不会占用大量内存：如果存储而不压缩容器中的DEX文件，平台可以对此类文件执行`mmap`操作，而不直接提取它们。但是，如果平台必须从容器中提取DEX文件，以这种方式引用DEX文件可能占用大量内存。

在Android中，所有类加载器都被视为支持并行运行。当多个线程争用同一个类加载器加载相同的类时，第一个完成此操作的线程胜出，而操作结果将用于其他线程。无论类加载器是返回同一个类、返回不同的类还是引发异常，都将发生此行为。该平台静默忽略此类异常。

> **注意**： 在低于Android 8.0的平台版本中，违反这些假设条件可能导致多次定义同一个类、由于类混淆造成堆损坏和其他不良影响。

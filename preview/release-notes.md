# 发布说明
本页提供Android O开发者预览版各程碑版本的详细信息。请在安装设备镜像或[提交bug](https://developer.android.google.cn/preview/feedback.html)之前回顾此发布说明，避免重复劳动。

查看[反馈页面](feedback.html)了解如何及在哪里报告平台问题，应用问题和第三方SDK问题。

要Android O讨论开发人员讨论问题和想法，加入[开发者预览Google+社区](https://developer.android.google.cn/preview/dev-community)。

### 开发者预览3
*日期：2017/06*

*编译: OPP3.170518.006 (Nexus 5X, Nexus 6P, Nexus Player, Pixel C, Pixel, Pixel XL)*

*模拟器支持：x86 & ARM (32/64-bit)*

*Google Play服务：10*

*API更新：[DP3/25](https://developer.android.google.cn/sdk/api_diff/26/changes.html)，[DP3/DP2](https://developer.android.google.cn/sdk/api_diff/26-incr/changes.html)*

#### 通用说明
此公开发布的beta版本主要提供给**早期采用者**，适合日常使用、开发和兼容性测试。请注意关于此版本的这些注意事项：
* 此公开发布的beta版本可能在支持的设备上有各种**稳定性、电池和性能方面的问题**。
* 一些应用程序在此公开发布的beta版本上**可能不会按照预期工作**，包括Google的应用和其他应用。
* 此公开发布beta版本在支持的设备上是**通过兼容性测试套件（CTS）认证的**。所有依赖兼容性认证构建的应用都能在这些设备上正常工作（如Android Pay）。
* 此公开发布的beta版本**支持以下设备：**Nexus 5X，Nexus 6P，Nexus Player，Pixel C，Pixel，and Pixel XL**。
* Android模拟器系统镜像可用于手机，Android TV和Android Wear。

#### 已知问题
##### 性能和电量
* 已知系统和应用程序的性能会间歇性变慢/卡顿，设备可能会偶尔无响应。这些问题可能会随使用时间变得更加严重。
* 此早期版本就屏幕开、关使用方面，电池寿命可能会回退。

##### 相机
* 相机可能启动很慢。

##### 蓝牙
* 偶尔会有音频卡顿问题。

##### Wi-Fi
* Wi-Fi驱动问题可能导致无法找到可用的Wi-Fi网络。

##### 多媒体
* 音乐音频被路由到设备扬声器而不是USB头戴设备。
* 用户在通话期间连接或断开有线耳机，通话音频被路由到有线耳机而不是蓝牙设备。

##### 备份与恢复
* 当新账户通过云端恢复流程创建时会显示“未发现备份”消息。

##### 多窗口
* 在解锁的设备上，停靠的应用程序在水平模式下可能会变得不可见。
* 热座上的图标可能会在滑动的应用程序上出现波动。
* 在启动系统设置程序时，画中画窗口将进入全屏幕。

##### 启动器
* 在Pixel设备上，当在非常短的屏幕配置中（例如，当设备处于水平方向分屏多窗口模式时），启动器可能会渲染一些主屏幕的UI元素脱离屏幕，如快速搜索框。

##### GCM/FCM
* 使用Android O中FCM/GCM的开发者应该将他们的Google Play服务SDK升级到10.2.1及以上，可以在Android Studio的SDK管理器操作。

##### Android Wear
* 当前只支持x86模拟器。
* 如果你的应用支持依赖[穿戴支持库](https://developer.android.google.cn/training/wearables/apps/layouts.html#UiLibrary)，那它将无法使用穿戴UI库中的[WearableNavigationDrawerView](https://developer.android.google.cn/reference/android/support/wear/widget/drawer/WearableNavigationDrawerView.html)类。如果你试图将该类与穿戴支持库一起使用，可能会发生类强转异常。更多关于`WearableNavigationDrawerView`的信息，请参阅[穿戴UI库中的操作与导航抽屉](https://developer.android.google.cn/training/wearables/ui/wear-ui-library.html#action-and-nav)。

##### 输入法
* 设备可能对配对的蓝牙键盘输入无响应。

##### 企业版
* 从**快速设置**面板关闭工作模式将导致设备重启并运行工作应用程序。
* 在设置了工作模式的设备，即使解锁，一些敏感的通知内容也会被隐藏。这影响主用户及工作模式的用户。
* 当打开一个工作模式下的应用程序，解锁工作挑战可能会显示一个空白绿色面板。可以通过点击**主页**按钮，然后通过**最近**界面返回应用的方式绕过。

##### 预置应用
* 当结束从锁屏界面发起的紧急通话时，拨号程序可能奔溃。
* 当电话权限被撤销时，拨号程序可能奔溃。
* 当权限被撤销后重新授权，发送短信可能导致短信应用奔溃。

#### API变更
本节列出Android O从开发者预览2升级到开发者预览3值得关注的特性、行为和API变更。完整的API变更列表，请查看API变更更新报告：
* [API级别25到Android O开发者预览3](https://developer.android.google.cn/sdk/api_diff/26/changes.html)
* [Android O开发者预览2到开发者预览3](https://developer.android.google.cn/sdk/api_diff/26-incr/changes.html)

##### 画中画模式
`PictureInPictureArgs`类被重命名为[PictureInPictureParams](https://developer.android.google.cn/reference/android/app/PictureInPictureParams.html)，相关的类和方法也做了相应的更新。

### 开发者预览2
*日期：2017/05*

*编译: OPP2.170420.019 (Nexus 5X, Nexus 6P, Nexus Player, Pixel, Pixel C, Pixel XL)*

*模拟器支持：x86 & ARM (32/64-bit)*

*Google Play服务：10*

*API更新：[DP2/25][https://developer.android.google.cn/sdk/api_diff/o-dp2/changes.html]，[DP2/DP1](https://developer.android.google.cn/sdk/api_diff/o-dp2-incr/changes.html)*

#### 通用说明
此公开发布的beta版本主要提供给**早期采用者**，适合日常使用、开发和兼容性测试。请注意关于此版本的这些注意事项：
* 此公开发布的beta版本可能在支持的设备上有各种**稳定性、电池和性能方面的问题**。
* 一些应用程序在此公开发布的beta版本上**可能不会按照预期工作**，包括Google的应用和其他应用。
* 此公开发布beta版本在支持的设备上是**通过兼容性测试套件（CTS）认证的**。所有依赖兼容性认证构建的应用都能在这些设备上正常工作（如Android Pay）。
* 此公开发布的beta版本**支持以下设备：**Nexus 5X，Nexus 6P，Nexus Player，Pixel C，Pixel，and Pixel XL**。
* Android模拟器系统镜像可用于手机，Android TV和Android Wear。

#### 已知问题
##### 性能和电量
* 已知系统和应用程序的性能会间歇性变慢/卡顿，设备可能会偶尔无响应。这些问题可能会随使用时间变得更加严重。
* 此早期版本就屏幕开、关使用方面，电池寿命可能会回退。

##### System UI
* 设备更换壁纸将导致最大字体和大号显示字体。

##### 相机
* 相机可能启动很慢。

##### 蓝牙
* 偶尔会有音频卡顿问题。

##### Wi-Fi
* Wi-Fi驱动问题可能导致无法找到可用的Wi-Fi网络。
* 通过Cast播放YouTobe视频时，通知可能有声音。

##### 外部存储器
* 在Nexus Player上，用户可能不能浏览和探索可移动设备的内容。

##### 初始化向导
* 初始化向导存在NFC服务奔溃。

##### 备份和恢复
* 执行从另一台设备恢复/从云端恢复，Wi-Fi数据不会被恢复。

##### 多用户
* 当添加新用户，设备可能卡在**触摸结束**界面。

##### 设置
* 从语言选择列表导航返回的欢迎界面无法访问视觉设置。

##### GCM/FCM
* 使用Android O中FCM/GCM的开发者应该将他们的Google Play服务SDK升级到10.2.1及以上，可以在Android Studio的SDK管理器操作。

##### Android Wear
* 当前只支持x86模拟器。

##### Android TV
* 如果在初始设置过程中对讲系统被打开，那些横跨多屏的文本字段不会显示键盘字符。
* 在Google Play电影和电视应用上播放视频，画中画窗口不会消失。
* 当任何不可选择的**设置**子选项被说出同时**OK**按钮被选择，对讲系统停止工作。

##### 企业版
* [使用Google账户下载设备策略控制器](https://developers.google.cn/android/work/overview#google_accounts)未完成。配置用户可以通过从Google Play手动下载DPC，并将账户手动添加到工作配置中的方式绕过。对于设备所有者，使用二维码或NFC代替。
* 在快速设置中点击管理的设备文字会显示一个空白的设备监视对话框。
* 当试图设置一个新的重置密码标识，TestDPC应用奔溃。
* 一旦从快速设置面板关闭工作模式，设备重启。

##### 预置应用
* 拨号应用在构建加载启动后会奔溃。
* 通话应用的短信回复不会唤起短信应用。
* YouTobe应用从画中画模式返回后卡死。
* 视频播放结束，画中画窗口消失后，Chrome应用奔溃。

##### 输入法
* Google印度键盘点击**&#63;**按钮后点击**隐藏键盘**按钮会奔溃。

#### API变更
本节列出Android O从开发者预览2升级到开发者预览3值得关注的特性、行为和API变更。完整的API变更列表，请查看API变更更新报告：
* [API级别25到Android O开发者预览2](https://developer.android.google.cn/sdk/api_diff/o-dp2/changes.html)
* [Android O开发者预览1到开发者预览2](https://developer.android.google.cn/sdk/api_diff/o-dp2-incr/changes.html)

##### 框架
`findViewById()`方法的所有版本现在返回`<T extends View> T`而不是`View`。更新信息请参阅[Android O特性与API](https://developer.android.google.cn/preview/api-overview.html#fvbi-signature)。

##### 后台执行限制
开发者预览2版本中对后台执行限制做了如下更变：
* 即使应用程序目标设备不是Android O，用户也可在**设置**屏幕内开启后台执行限制。
* `NotificationManager.startServiceInForeground()`（开发者预览1中引入）方法现已经被弃用。要启动一个前台服务，使用[Context.startForegroundService()](https://developer.android.google.cn/reference/android/content/Context.html#startForegroundService&#40;android.content.Intent&#41;)。
* 一些广播已经被添加或删除以限制隐式广播。[隐式广播异常](https://developer.android.google.cn/preview/features/background-broadcasts.html)已被添加以反映这些变更。

更多信息，请参阅[后台执行限制](https://developer.android.google.cn/preview/features/background.html)。

##### Wi-Fi感知
在开发者预览1版本中，创建一个方法说明符的方法是`createNetworkSpecifier()`。从开发者预览2开始，创建方法说明符的方法对于非加密连接是`createNetworkSpecifierOpen()`，对于加密连接是`createNetworkSpecifierPassphrase`。更多信息，请参阅[WifiAwareSession](https://developer.android.google.cn/reference/android/net/wifi/aware/WifiAwareSession.html)和[DiscoverySession](https://developer.android.google.cn/reference/android/net/wifi/aware/DiscoverySession.html)。更多信息，请参阅[Wi-Fi感知](https://developer.android.google.cn/preview/features/wifi-aware.html)。

##### WebView
考虑到支持新的Android自动填充框架，一些类和方法在开发者预览2中有不同的功能。更多详细信息请参阅[Web表单自动填充](https://developer.android.google.cn/preview/behavior-changes.html#wfa)。
##### 运行时APP分析
在Android O开发者预览2版本中，[java.lang.reflect](https://developer.android.google.cn/reference/java/lang/reflect/package-summary.html)包中新增一些类，使得在应用程序逻辑中发现一些有关参数，方法和构造函数运行时信息变得更加容易。

更多关于Android O中可用的Java 8特性的信息，请参阅[使用Java 8语言特性](https://developer.android.google.cn/studio/preview/features/java8-support.html)。

##### ICU4J支持
Android O更新了如下[ICU4J Android框架API](https://developer.android.google.cn/guide/topics/resources/icu4j-framework.html)：
* [android.icu.lang](https://developer.android.google.cn/reference/android/icu/lang/package-summary.html)
* [android.icu.text](https://developer.android.google.cn/reference/android/icu/text/package-summary.html)
* [android.icu.util](https://developer.android.google.cn/reference/android/icu/util/package-summary.html)

##### 自动填充框架
在Android O开发者预览2版本中，自动填充框架提供了[View.setAutofillHints()](https://developer.android.google.cn/reference/android/android/view/View.html#setAutofillHints(java.lang.String...)方法和[android:autofillHints](https://developer.android.google.cn/reference/android/view/View.html#attr_android:autofillHints)属性，你可以用来提供每一个字段期望的输入类型系统提示。

更多自动填充框架中此功能和其他功能信息，咨询[自动填充框架](https://developer.android.google.cn/preview/features/autofill.html)页面。

##### 可下载字体
Android O开发者预览版2添加了一些类和方法使应用可以从字体提供者下载字体。此功能在Android O中可用，并且在支持26中以支持运行Android API 14及更高版本的设备。

关于此特性的最新信息，请参阅[可下载字体](https://developer.android.google.cn/preview/features/downloadable-fonts.html)。

##### XML字体
XML字体功能可以通过支持库26来支持运行Android API 14及更高版本的设备。

关于此特性的最新信息，请参阅[XML字体](https://developer.android.google.cn/preview/features/fonts-in-xml.html)。

##### 自动缩放TextViews
自动缩放TextViews功能可以通过支持库26来支持运行Android API 14及更高版本的设备。

关于此特性更多信息，请参阅[自动缩放TextViews](https://developer.android.google.cn/preview/features/autosizing-textview.html)。

##### 输入和导航
开发者预览版2的Android输入和导航兼容做了如下变更：
* [View](https://developer.android.google.cn/reference/android/view/View.html)类添加了一个新的属性和方法以便更细粒度控制UI元素的高亮。

更新信息请参阅[输入域导航](https://developer.android.google.cn/preview/behavior-changes.html#ian)。

##### 电话权限
一些与电话相关的已经被添加到开发者预览版2。

更多信息请参阅[权限](https://developer.android.google.cn/preview/api-overview.html#perms)。

#### 开发者预览1
*日期：2017/03*

*编译：OPP1.170223.012 (Nexus 5X, Nexus 6P, Nexus Player, Pixel, Pixel XL), OPP1.170223.013 (Pixel C)*

*模拟器支持：x86 & ARM (32/64-bit)*

*Google Play服务：10*

*API更新：[DP1/25](https://developer.android.google.cn/sdk/api_diff/o-dp1/changes.html)*

#### 通用说明
此开发者预览版本仅适用于**应用程序开发人员**进行兼容性测试和开发。此版本不适用于日常使用。请注意这些关于发行版的通用注意事项：
* 此版本在所有设备上都有各种稳定性和性能问题，使其**不适合在手机或平板电脑上使用**，特别是非开发人员。
* 已知系统和应用程序的**性能会间歇性的慢/卡顿**，设备可能会偶尔无响应。这些问题可能会随使用时间变得更加严重。
* 此早期版本就屏幕开、关使用方面，电池寿命可能会回退。
* 某些应用程序在开发者预览1上可能无法正常运行。这包括Google的应用程序以及其他应用程序。
* 此早期版本不是兼容性测试套件（CTS）认证的版本。依赖于CTS认证而构建的应用程序（例如Android Pay）将无法正常工作。
* 此预览版本支持以下设备：Nexus 5X，Nexus 6P，Nexus Player，Pixel C，Pixel和Pixel XL。
* Android模拟器系统镜像可用于手机，Android TV和Android Wear。
* O开发者预览的初始版本只能通过手动下载。此版本不支持通过Android Beta计划进行的OTA更新。当前，Android Beta仍然专注于牛轧糖，但Android Beta渠道的Android O将于今年晚些时候推出。

#### 已知问题
##### 性能和电量
* 已知系统和应用程序的性能会间歇性变慢/卡顿，设备可能会偶尔无响应。这些问题可能会随使用时间变得更加严重。
* 此早期版本就屏幕开、关使用方面，电池寿命可能会回退。

##### System UI
* 当调整屏幕亮度，System UI会奔溃，然后导航到概览（最近）并选择以个新应用。
* 偶现Home导航问题，伴随着未完成场景转换导致设备不可用。
* 在Quick Settings中点击可能会在另一个地方注册。
* 当使用自动填充，有时会出现“Android系统显示其上”消息，但是可以忽略（如果关闭，自动将不可用）。

##### GCM/FCM
* 除非设置了FCM主配置，否则FCM第二配置不能生效。如果应用没有设置主配置，则无法在第二配置中生成FCM标识。
* 使用Android O中FCM/GCM的开发者应该将他们的Google Play服务SDK升级到10.2.1，可以在Android Studio的SDK管理器操作。

##### Android TV
* 配置好限制设置，然后重启设备可能导致设备无法使用。
* 在搜索框中输入引起奔溃。
* 画中画图像不会填充窗口。
* 通过语音命令启动应用程序功能不工作。
* 应用程序设置中不会显示应用大小。
* 远程控制的通话按钮不工作。
* Fugu上的恢复出厂设置不工作。
* 已连接的远程控制设备会间歇性断开。
* 通过USB连接上的设备无法格式化。**设置为存储驱动器**格式化选项对于通过USB连接的驱动器无法正常工作。
* 设备可能在插电时白屏卡死。
* 设置中位置状态可能被关闭，并且用户无法重新打开。
* 不能在应用程序和游戏行中重新排序游戏。
* 在重新运行媒体应用后，内容可能不会播放。

##### Wi-Fi
* Wi-Fi网络无法拨打611电话。

##### 语音搜索和热门词汇
* 热门词汇检测后偶尔会有重复的动作建议。

##### Android for Work
* 开启工作设置可能引起设置应用程序奔溃。要解决此问题，移除然后重新添加工作设置。

##### Android设备管理
* 选择**设置 > 安全与锁屏 > Android设备管理**菜单选项可能引起奔溃。更新到最新版Google Play服务将解决此问题。

##### Android Wear
* 来自应用商店的内嵌应用安装可能会奔溃，可以使用Side-loading来解决此问题。
* 当前只支持x86模拟器。
* 在切换表盘、触摸表盘设置或重启设备之前，默认表盘复杂功能可能不会加载。

##### 其他
* 使用Google Drive应用的PDF查看器加载PDF文档可能引起设备冻结。
* 应用和游戏可能在GLThread中遇到ANR，如`AssetManager::open`引起奔溃或者加载失败。
* 使用Google Play游戏服务的游戏的可能会有登录问题。
* 最新发布的Android Auto可能在开发者预览1上不能正常工作。
* NDK不能编译目标为Android O开发者预览1的应用（即将发布的r15将修复此问题）。

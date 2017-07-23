# 后台执行限制

每次在后台运行时，应用都会消耗一部分有限的设备资源，例如RAM。 这可能会影响用户体验，如果用户正在使用占用大量资源的应用（例如玩游戏或观看视频），影响尤为明显。

为了提升用户体验，Android O对应用在后台运行时可以执行的操作施加了限制。本文档说明了操作系统的一些变更，以及如何更新应用以便在新限制下正常运行。

### 概览
多个Android应用和服务可以同时运行。 例如，用户可以在一个窗口中玩游戏，同时在另一个窗口中浏览网页，并使用第三个应用播放音乐。

同时运行的应用越多，对系统造成的负担越大。 如果还有应用或服务在后台运行，这会对系统造成更大负担，进而可能导致用户体验下降；例如，音乐应用可能会突然关闭。

为了降低发生这些问题的几率，Android O对应用在用户不与其直接交互时可以执行的操作施加了限制。

应用在两个方面受到限制：
* [后台服务限制](https://developer.android.google.cn/preview/features/background.html#services)：处于空闲状态时，应用可以使用的后台服务存在限制。 这些限制不适用于前台服务，因为前台服务更容易引起用户注意。
* [广播限制](https://developer.android.google.cn/preview/features/background.html#broadcasts)：除了有限的例外情况，应用无法使用清单注册隐式广播。 它们仍然可以在运行时注册这些广播，并且可以使用清单注册专门针对它们的显式广播。

注：默认情况下，这些限制仅适用于针对O的应用。 不过，用户可以从**Settings**屏幕为任意应用启用这些限制，即使应用并不是以O为目标平台。

在大多数情况下，应用都可以使用[JobScheduler](https://developer.android.google.cn/reference/android/app/job/JobScheduler.html)作业克服这些限制。 这种方式让应用安排为在未活跃运行时执行工作，不过仍能够使系统可以在不影响用户体验的情况下安排这些作业。

Android O提供针对[JobScheduler](https://developer.android.google.cn/reference/android/app/job/JobScheduler.html)的多个改进，让您可以更轻松地使用计划作业取代服务和广播接收器；如需了解详细信息，请参阅[JobScheduler改进](https://developer.android.google.cn/preview/api-overview.html#jobscheduler)。

### 后台服务限制
在后台中运行的服务会消耗设备资源，这可能降低用户体验。 为了缓解这一问题，系统对这些服务施加了一些限制。

系统可以区分*前台*和*后台*应用。（用于服务限制目的的后台定义与内存管理使用的定义不同；一个应用按照内存管理的定义可能处于后台，但按照能够启动服务的定义又处于前台。）如果满足以下任意条件，应用将被视为处于前台：
* 具有可见 Activity（不管该 Activity 已启动还是已暂停）。
* 具有前台服务。
* 另一个前台应用已关联到该应用（不管是通过绑定到其中一个服务，还是通过使用其中一个内容提供程序）。例如，如果另一个应用绑定到该应用的服务，那么该应用处于前台：
  * [IME](https://developer.android.google.cn/guide/topics/text/creating-input-method.html)
  * 壁纸服务
  * 通知侦听器
  * 语音或文本服务

如果以上条件均不满足，应用将被视为处于后台。

处于前台时，应用可以自由创建和运行前台服务与后台服务。进入后台时，在一个持续数分钟的时间窗内，应用仍可以创建和使用服务。

在该时间窗结束后，应用将被视为处于*空闲*状态。此时，系统将停止应用的后台服务，就像应用已经调用服务的“[Service.stopSelf()](https://developer.android.google.cn/reference/android/app/Service.html#stopSelf())”方法。

在这些情况下，后台应用将被置于一个临时白名单中并持续数分钟。位于白名单中时，应用可以无限制地启动服务，并且其后台服务也可以运行。

处理对用户可见的任务时，应用将被置于白名单中，例如：
* 处理一条高优先级[Firebase云消息传递(FCM)](https://firebase.google.cn/docs/cloud-messaging/)消息。
* 接收广播，例如短信/彩信消息。
* 从通知执行[PendingIntent](https://developer.android.google.cn/reference/android/app/PendingIntent.html)。

在很多情况下，您的应用都可以使用[JobScheduler](https://developer.android.google.cn/reference/android/app/job/JobScheduler.html)作业替换后台服务。例如，CoolPhotoApp需要检查用户是否已经从朋友那里收到共享的照片，即使该应用未在前台运行。

之前，应用使用一种会检查其云存储的后台服务。为了迁移到Android O，开发者使用一个计划作业替换了这种后台服务，该作业将按一定周期启动，查询服务器，然后退出。

在Android O之前，创建前台服务的方式通常是先创建一个后台服务，然后将该服务推到前台。

Android O有一项复杂功能；系统不允许后台应用创建后台服务。因此，Android O引入了一种全新的方法，即`Context.startForegroundService()`，以在前台启动新服务。

在系统创建服务后，应用有五秒的时间来调用该服务的[startForeground()](https://developer.android.google.cn/reference/android/app/Service.html#startForeground&#40;int, android.app.Notification&#41;)方法以显示新服务的用户可见通知。

如果应用在此时间限制内未调用[startForeground()](https://developer.android.google.cn/reference/android/app/Service.html#startForeground&#40;int, android.app.Notification&#41;)，则系统将停止服务并声明此应用为[ANR](https://developer.android.google.cn/training/articles/perf-anr.html)。

### 广播限制
如果应用注册为接收广播，则在每次发送广播时，应用的接收器都会消耗资源。 如果多个应用注册为接收基于系统事件的广播，这会引发问题；触发广播的系统事件会导致所有应用快速地连续消耗资源，从而降低用户体验。

为了缓解这一问题，Android 7.0（API 级别 25）对广播施加了一些限制，如[后台优化](https://developer.android.google.cn/topic/performance/background-optimization.html)中所述。

Android O让这些限制更为严格。
* 针对Android O的应用无法继续在其清单中为隐式广播注册广播接收器。*隐式广播*是一种不专门针对该应用的广播。例如，[ACTION_PACKAGE_REPLACED](https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_PACKAGE_REPLACED)就是一种隐式广播，因为它将发送到注册的所有侦听器，让后者知道设备上的某些软件包已被替换。

不过，[ACTION_MY_PACKAGE_REPLACED](https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_MY_PACKAGE_REPLACED)不是隐式广播，因为不管已为该广播注册侦听器的其他应用有多少，它都会只发送到软件包已被替换的应用。
* 应用可以继续在它们的清单中注册显式广播。
* 应用可以在运行时使用[Context.registerReceiver()](https://developer.android.google.cn/reference/android/content/Context.html#registerReceiver&#40;android.content.BroadcastReceiver, android.content.IntentFilter&#41;)为任意广播（不管是隐式还是显式）注册接收器。
* 需要[签名权限](https://developer.android.google.cn/guide/topics/manifest/permission-element.html#plevel)的广播不受此限制所限，因为这些广播只会发送到使用相同证书签名的应用，而不是发送到设备上的所有应用。

在许多情况下，之前注册隐式广播的应用使用[JobScheduler](https://developer.android.google.cn/reference/android/app/job/JobScheduler.html)作业可以获得类似的功能。

例如，一款社交照片应用可能需要不时地执行数据清理，并且倾向于在设备连接到充电器时执行此操作。

之前，应用已经在清单中为[ACTION_POWER_CONNECTED](https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_POWER_CONNECTED)注册了一个接收器；当应用接收到该广播时，它会检查清理是否必要。 为了迁移到Android O，应用将该接收器从其清单中移除。

应用将清理作业安排在设备处于空闲状态和充电时运行。

**注：**很多隐式广播当前均已不受此限制所限。 应用可以继续在其清单中为这些广播注册接收器，不管应用针对哪个 API 级别。 有关已豁免广播的列表，请参阅[隐式广播](https://developer.android.google.cn/preview/features/background-broadcasts.html)例外。

{: .note}

### 迁移指南
默认情况下，这些更改仅影响针对O的应用。不过，用户可以从**Settings**屏幕为任意应用启用这些限制，即使应用并不是以O为目标平台。

您可能需要更新应用，使其符合新限制。

了解您的应用如何使用服务。 如果您的应用依赖某些在它处于空闲时于后台运行的服务，您需要替换这些服务。

可能的解决方法包括：
* 如果处于后台时您的应用需要创建一个前台服务，请使用新的`NotificationManager.startServiceInForeground()`
方法，而不是创建一个后台服务，然后尝试将其推到前台。
* 如果服务容易被用户注意，请将其设为前台服务。例如，播放音频的服务始终应为前台服务。
使用`NotificationManager.startServiceInForeground()`而不是[startService()](https://developer.android.google.cn/reference/android/content/Context.html#startService&#40;android.content.Intent&#41;)创建服务。
* 寻找一种使用计划作业实现服务功能的方式。 如果服务未在执行容易立即被用户注意到的操作，一般情况下，您都能够使用计划作业。
* 发生网络事件时，请使用[FCM](https://firebase.google.cn/docs/cloud-messaging/)选择性地唤醒您的应用，而不是在后台轮询。
* 在应用正常处于前台之前，请推迟后台工作。

检查在您应用的清单中定义的广播接收器。 如果您的清单为显式广播声明了接收器，您必须予以替换。 可能的解决方法包括：
* 通过调用[Context.registerReceiver()](https://developer.android.google.cn/reference/android/content/Context.html#registerReceiver&#40;android.content.BroadcastReceiver, android.content.IntentFilter&#41;)而不是在清单中声明接收器的方式在运行时创建接收器。
* 使用计划作业检查条件是否会触发隐式广播。

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

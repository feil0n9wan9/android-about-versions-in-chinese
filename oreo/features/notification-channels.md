# 通知渠道
Android 8.0引入了通知渠道，以提供统一的系统来帮助用户管理通知。当您以Android 8.0为目标平台时，必须实现一个或多个通知渠道，以便向用户显示通知。如果不以Android 8.0为目标平台，当应用运行在Android 8.0设备上时，其行为将与运行在Android 7.0上时相同。

您可以为需要发送的每个不同的通知类型创建一个通知渠道。还可以创建通知渠道来反映应用的用户做出的选择。例如，您可以为一款消息传递应用的用户创建的每个对话组建立单独的通知渠道。

用户现在可以使用一致的系统UI管理大多数与通知有关的设置。所有发布至通知渠道的通知都具有相同的行为。当用户修改任何下列特性的行为时，修改将作用于通知渠道：
* 重要性
* 声音
* 光
* 振动
* 在锁屏上显示
* 替换免打扰模式
* 用户可以访问**设置**，或长按通知来更改这些行为，甚至可以随时屏蔽通知渠道。在创建某个通知渠道并将其提交到通知管理器后，便无法通过编程方式修改通知渠道的行为；这些设置由用户掌控。

### 通知优先级和重要性
Android 8.0弃用了为单个通知设置优先级的功能。现在，创建通知渠道时您可以设置建议重要性级别。您为通知渠道指定的重要性级别适用于您发布至该渠道的所有通知消息。您可以为渠道配置五个重要性级别中的一个，这些级别配置的是渠道可以打断用户的程度，范围是[IMPORTANCE_NONE(0)](https://developer.android.google.cn/reference/android/app/NotificationManager.html#IMPORTANCE_NONE)至[IMPORTANCE_HIGH(4)](https://developer.android.google.cn/reference/android/app/NotificationManager.html#IMPORTANCE_HIGH)。默认重要性级别为3：在所有位置显示，发出提示音，但不会对用户产生视觉干扰。创建通知渠道后，只有系统可以修改其重要性。

### 创建通知渠道
要创建通知渠道，请执行下列操作：
1. 构建一个在软件包内具有唯一ID的通知渠道对象。
2. 为该通知渠道对象配置所需的任何初始设置（例如提示音以及对用户可见的可选说明）。
3. 将通知渠道对象提交到通知管理器。
4. 如果试图使用初始值创建的通知渠道已存在，不会执行任何操作，因此启动应用时可以放心地执行以上步骤序列。以下示例代码演示的是如何创建具有低重要性级别和自定义振动模式的通知渠道。
```java
NotificationManager mNotificationManager =
        (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
// The id of the channel.
String id = "my_channel_01";
// The user-visible name of the channel.
CharSequence name = getString(R.string.channel_name);
// The user-visible description of the channel.
String description = getString(R.string.channel_description);
int importance = NotificationManager.IMPORTANCE_LOW;
NotificationChannel mChannel = new NotificationChannel(id, name, importance);
// Configure the notification channel.
mChannel.setDescription(description);
mChannel.enableLights(true);
// Sets the notification light color for notifications posted to this
// channel, if the device supports this feature.
mChannel.setLightColor(Color.RED);
mChannel.enableVibration(true);
mChannel.setVibrationPattern(new long[]{100, 200, 300, 400, 500, 400, 300, 200, 400});
mNotificationManager.createNotificationChannel(mChannel);
```
您还可以通过调用[createNotificationChannels()](https://developer.android.google.cn/reference/android/app/NotificationManager.html#createNotificationChannels&#40;java.util.List<android.app.NotificationChannel>&#41;)一次性创建多个通知渠道。

### 创建通知渠道组
如果您的应用支持多个用户帐户，则可为每个帐户创建一个通知渠道组。通知渠道组用于对一款应用内的多个同名通知渠道进行管理。例如，一款社交网络应用可能提供面向个人用户帐户以及企业用户帐户的支持。在此情境下，每个用户帐户可能都需要多个功能和名称相同的通知渠道。
* 一个包括2个通知渠道的个人用户帐户：
  * 帖子新增评论的通知。
  * 联系人推荐帖子的通知。
* 一个包括2个通知渠道的企业用户帐户：
  * 帖子新增评论的通知。
  * 联系人推荐帖子的通知。
在本例中，将与每个用户帐户相关的通知渠道组织成专用组可确保用户能在**设置**中轻松地进行区分。每个通知渠道组都必须在软件包内具有唯一ID，并具有用户可见的名称。下面这段代码演示了如何创建通知渠道组。
```java
// The id of the group.
String group = "my_group_01";
// The user-visible name of the group.
CharSequence name = getString(R.string.group_name);;
NotificationManager mNotificationManager =
        (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
mNotificationManager.createNotificationChannelGroup(new NotificationChannelGroup(group, name));
```
新建渠道组后，便可调用[setGroup()](https://developer.android.google.cn/reference/android/app/NotificationChannel.html#setGroup&#40;java.lang.String&#41;)将某个新渠道关联到该组。您只能在将渠道提交给通知管理器之前修改通知渠道与组之间的关联。

### 创建通知
要创建通知，请调用[Notification.Builder.build()](https://developer.android.google.cn/reference/android/app/Notification.Builder.html#build&#40;&#41;)，它返回的[Notification](https://developer.android.google.cn/reference/android/app/Notification.html)对象包含您的指定值。要发出通知，请通过调用[notify()](https://developer.android.google.cn/reference/android/app/NotificationManager.html#notify&#40;int, android.app.Notification&#41;)将[Notification](https://developer.android.google.cn/reference/android/app/Notification.html)对象传递给系统。

### 必需的通知内容
[Notification](https://developer.android.google.cn/reference/android/app/Notification.html)对象必须包含以下内容：
* 小图标，由[setSmallIcon()](https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setSmallIcon&#40;android.graphics.drawable.Icon&#41;)设置
* 标题，由[setContentTitle()](https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setContentTitle&#40;java.lang.CharSequence&#41;)设置
* 详细文本，由[setContentText()](https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setContentText&#40;java.lang.CharSequence&#41;)设置
* 有效的通知渠道ID，由[setChannelId()](https://developer.android.google.cn/reference/android/app/Notification.Builder.html#setChannelId&#40;java.lang.String&#41;)设置
如果您以Android 8.0为目标平台并且在不指定有效通知渠道的情况下发布通知，那么通知将无法发布，系统会记录错误。
> **注**：您可以在Android 8.0中启用一个新设置，当针对Android 8.0的应用试图在没有通知渠道的情况下发布时，以[toast](https://developer.android.google.cn/guide/topics/ui/notifiers/toasts.html)形式显示屏幕警告。要为运行Android 8.0的开发设备启用该设置，请转到**设置 > 开发者选项**，然后打开**显示通知渠道警告**。

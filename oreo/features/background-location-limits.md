# 后台位置限制
为降低功耗，**无论应用的目标SDK版本为何**，Android 8.0开发者预览版都会对后台应用检索用户当前位置的频率进行限制。

如果您的应用在后台运行时依赖实时提醒或运动检测，这一位置检索行为就显得特别重要，必须紧记。
> **重要说明**：作为起点，我们只允许后台应用每小时接收几次位置更新。我们将在整个预览版阶段继续根据系统影响和开发者的反馈优化位置更新间隔。

系统会对前台应用和后台应用进行区分。应用满足以下任一条件即视为前台应用：
* 它具有可见的Activity，无论Activity处于启动还是暂停状态。
* 它具有前台服务。
* 另一个前台应用通过绑定到应用的其中一个服务或使用应用的其中一个内容提供程序与应用相连。

如果以上所有条件均不满足，应用即视为后台应用。

### 前台应用行为得到保留
如果应用在运行Android 8.0的设备上处于前台，其位置更新行为将与Android 7.1.1（API级别25）及更低版本上相同。
> **警告**：如果您的应用长时间进行近乎实时的位置更新检索，将大幅度缩短设备的电池寿命。

### 优化应用的位置行为
考虑在您的应用接收位置更新不频繁的情况下其后台运行用例是否根本无法成功。如果属于这种情况，您可以通过执行下列操作之一提高位置更新的检索频率：
* 将您的应用转至前台。
* 使用应用中的某个[前台服务](https://developer.android.google.cn/guide/components/services.html#Foreground)。激活此服务时，您的应用必须在[通知区](https://developer.android.google.cn/guide/topics/ui/notifiers/notifications.html)显示一个持续性的通知。
* 使用Geofencing API的元素（例如[GeofencingApi](https://developers.google.cn/android/reference/com/google/android/gms/location/GeofencingApi)接口），这些元素针对最大限度减少耗电进行了专门优化。
* 使用被动位置侦听器，它可以在后台应用加快位置请求频率时提高位置更新的接收频率。
> **注**：如果您的应用需要访问的位置历史记录包含时间频繁更新，请使用批处理版本的Fused Location Provider API元素，例如[FusedLocationProviderApi](https://developers.google.cn/android/reference/com/google/android/gms/location/FusedLocationProviderApi)接口。当您的应用运行于后台时，此API会以高于非批处理版本API的频率接收用户的位置。但切记，您的应用批量接收更新的频率仍仅为每小时几次。

### 受影响的API
对后台应用位置检索行为的更改影响下列API：
[Fused Location Provider(FLP)](https://developers.google.cn/android/reference/com/google/android/gms/location/FusedLocationProviderApi)
  * 如果您的应用运行在后台，位置系统服务只会根据Android 8.0行为变更中定义的间隔，按每小时几次的频率为其计算新位置。即使您的应用请求进行更频繁的位置更新，也仍是如此。
  * 如果您的应用运行在前台，与Android 7.1.1（API级别25）相比，在位置采样率上不会有任何变化。

Geofencing
  * 后台应用可以高于接收Fused Location Provider更新的频率接收地理围栏转换事件。
  * 地理围栏事件的平均响应时间是大约每两分钟一次。

GNSS Measurements和GNSS Navigation Messages
  * 当您的应用位于后台时，注册用于接收[GnssMeasurement](https://developer.android.google.cn/reference/android/location/GnssMeasurement.html)和[GnssNavigationMessage](https://developer.android.google.cn/reference/android/location/GnssNavigationMessage.html)输出的回调会停止执行。

Location Manager
  * 提供给后台应用的位置更新只会根据Android 8.0行为变更中定义的间隔，按每小时几次的频率提供。
  > **注**：如果运行您的应用的设备安装了Google Play服务，强烈建议您改用[Fused Location Provider(FLP)](https://developers.google.cn/android/reference/com/google/android/gms/location/FusedLocationProviderApi)。

WLAN 管理器
  * [startScan()](https://developer.android.google.cn/reference/android/net/wifi/WifiManager.html#startScan&#40;&#41;方法对后台应用执行完整扫描的频率仅为每小时数次。如果不久之后后台应用再次调用此方法，[WifiManager](https://developer.android.google.cn/reference/android/net/wifi/WifiManager.html)类将提供上次扫描所缓存的结果。

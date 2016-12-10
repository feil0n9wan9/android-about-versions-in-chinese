# 数据表

本页面提供包含某一共同特征（如Android版本或屏幕大小）的设备相对数量的相关信息。此信息通过揭示哪些设备在Android和Google Play生态系统中处于活动状态，可以帮助您确定在支持不同设备方面优先级。

此数据反映的是运行最新版Google Play商店应用的设备，该应用与Android 2.2及更高版本兼容。每个数据快照表示之前7天内访问过Google Play商店的所有设备。


### 平台版本
本节提供有关运行给定Android平台版本的设备相对数量的数据。

有关如何根据平台版本设定应用程序目标设备的信息，请阅读[支持不同平台版本](https://developer.android.com/training/basics/supporting-devices/platforms.html)。

![](http://oartlnou9.bkt.clouddn.com/android-about-versions/images/dashboards-platform-versions.png)


### 屏幕尺寸和密度
本节提供有关具有特定屏幕配置（由屏幕大小和密度的组合定义）的设备相对数量的数据。为了简化您为不同屏幕配置设计用户界面的方式，Android将实际屏幕大小和密度的范围划分为几个类别，如下表所示。

有关如何在应用程序中支持多种屏幕配置的信息，请阅读[支持多种屏幕](https://developer.android.com/guide/practices/screens_support.html)。

![](http://oartlnou9.bkt.clouddn.com/android-about-versions/images/dashboards-screen-sizes-and-densities.png)


### Open GL版本
本节提供有关支持特定版本的OpenGL ES的设备相对数量的数据。请注意，对一个特定版本的OpenGL ES的支持也意味着对其任何较低版本的支持（如对2.0版本的支持也意味着对1.1的支持）。

要声明应用程序需要哪个版本的OpenGL ES，您应该使用[&lt;uses-feature&gt;](https://developer.android.com/guide/topics/manifest/uses-feature-element.html)元素的`android：glEsVersion`属性。您还可以使用[&lt;supports-gl-texture&gt;](https://developer.android.com/guide/topics/manifest/supports-gl-texture-element.html)元素声明应用程序使用的GL压缩格式。

![](http://oartlnou9.bkt.clouddn.com/android-about-versions/images/dashboards-opengl-version-table.png)![](http://oartlnou9.bkt.clouddn.com/android-about-versions/images/dashboards-opengl-version-chart.png)
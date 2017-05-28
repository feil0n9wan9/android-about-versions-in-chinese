# 计划概述
欢迎加入Android O开发者计划，此计划将为您提供针对下一个版本的Android系统提升您应用的兼容性和开发应用所需的所有功能。它是一款免费软件。您只需下载预览版工具即可立即使用。

|||
|:-----|:-----|
|**硬件和模拟器系统映像**|**最新的平台代码**|
|在Nexus 5X、6P、Player、Pixel C、Pixel、Pixel XL以及模拟器中运行并测试应用。|我们将在预览版期间提供多次更新，因此您将可以针对最新的平台变更测试您的应用。|
|**新行为和新功能**|**反馈和支持**|
|尽早做好支持新平台行为（例如新运行时权限模型和省电功能）的准备工作。|使用[Issue Tracker](https://developer.android.google.cn/preview/bugs)向我们报告问题并提供反馈。与[开发者社群](https://developer.android.google.cn/preview/dev-community)中的其他开发者建立联系。|

### 时间表和更新
![](images/preview-overview/o-preview-updates.svg)

O开发者预览从2017年3月21日开放下载，到向AOSP和OEM提供最终的Android O公开版本时停止使用，最终版本预计将于2017年第三季度发布。

在开发阶段的各个关键里程碑，我们将为您的开发和测试环境提供更新。每个更新都包括SDK工具、预览版系统映像、模拟器、API参考和API差异。里程碑列表如下。
* [预览 1](preview-release-notes.html)（初始版本，alpha）
* [预览 2](preview-release-notes.html)（增量更新，beta）
* [预览 3](preview-release-notes.html)（最终 API 和官方 SDK，在 Play 中发布）
* [预览 4](preview-release-notes.html)（接近最终版本系统映像，用于最终测试）
* 向AOSP和生态系统发布**最终版本**

对开发者而言，预览版早期的焦点是确保您当前的应用与新平台兼容，并提供早期反馈。在预览版的后期（其实贯穿整个预览版期间及之后），您的工作是调整自己应用中的功能，并锁定新平台。

> [**请参阅迁移指南**](preview-migration.html)，了解让应用与新平台兼容的简单步骤，然后在您准备就绪后锁定新平台。

**前两个预览版里程碑**提供**早期测试和开发环境**，帮助您发现当前应用中的兼容性问题，并针对新平台计划必要的迁移或功能工作。这是向我们提供功能和API以及文件兼容性问题反馈的优先期 — 请使用使用[Issue Tracker](https://developer.android.google.cn/preview/bugs)提交反馈。在更新期间，API可能会有变更。

在**预览 3**和**预览 4**中，您将可以使用**最终版的O API和SDK**进行开发，以及获取接近最终版的系统映像，用于测试系统行为和功能。此时，Android O会提供标准的API级别。您可以开始对旧版应用进行最终的兼容性测试，并优化使用O API或功能的新代码。

此外，从 预览 4开始，您将可以面向运行正式API级别的Android O系统的设备**发布应用**，例如选择加入Android Beta计划的消费者设备。您可以先在Google Play的alpha和beta渠道发布应用，通过Android Beta消费者对应用进行测试，然后在商店大范围推广。

如果您在Android O中进行测试和开发，我们强烈建议您随着预览版更新的发布，**将开发环境保持为相应的最新版本**。

当有预览版更新可用时，我们将通过[Android开发者博客](http://android-developers.blogspot.com/)、此网站以及[Android O](https://developer.android.google.cn/preview/dev-community)开发者社群通知您。

### O开发者预览包含的内容
O开发者预览包括您在各种使用不同屏幕尺寸、网络技术、CPU/GPU芯片组和硬件架构的设备中测试现有应用所需的所有功能。

#### SDK工具
您可通过[Android Studio](https://developer.android.google.cn/studio/intro/update.html)中的SDK管理器下载这些组件：
* O开发者预览**SDK和工具**
* O开发者预览**模拟器系统映像**（32 位和 64 位）
* 适用于Android TV的O开发者预览**模拟器系统映像**（32 位）
* O开发者预览支持库

我们将根据需要在每个里程碑为这些开发工具提供更新。

### 硬件系统映像
O开发者预览包含可用于在物理设备上进行测试和开发的硬件系统映像。

您可以从[下载页面](preview-download.html)为这些设备下载系统映像：
* **Nexus 5X** (GSM/LTE) "bullhead"设备系统映像
* **Nexus 6P** (GSM/LTE) "angler"设备系统映像
* **Nexus Player** (Android TV) "fugu"设备系统映像
* **Pixel** "marlin"设备系统映像
* **Pixel XL** "sailfish"设备系统映像
* **Pixel C** "ryu"设备系统映像

我们将在每个里程碑为这些设备提供更新的系统映像。您可以手动下载更新的系统映像，并刷入测试设备（如需要，可多次刷入）。这尤其适合需要多次重刷设备的自动化测试环境。

> **注**：初始版本的O开发者预览(DP1)仅面向开发者，不适合日常使用或消费者使用，因此，我们仅通过手动下载方式提供此版本。开发者预览 1不支持通过Android Beta计划获取更新。

### 通过Android Beta计划获得OTA更新
开发者预览 1无法通过Android Beta计划以OTA更新形式获得。我们将在之后的版本中提供这种试用Android O的便捷方式，敬请关注。

### 文档和示例代码
开发者预览网站上提供的以下文档资源有助于您了解Android O：
* [迁移到Android O](preview-migration.html)，提供入门指南的分步说明。
* [行为变更](preview-behavior-changes.html)，带您了解主要测试领域。
* 新API文档，包括[API概览](preview-api-overview.html)以及有关通知渠道和自动填充等主要功能的详细开发者指南。
* [示例代码](preview-samples.html)，演示如何支持通知渠道和其他新功能。
* O开发者当前版本的[版本说明](preview-release-notes.html)，包括变更说明。

#### API参考和差异报告
预览版的完整[API参考](https://developer.android.google.cn/reference/index.html)可在线获取。新API添加有水印并显示“Android O开发者预览”。请注意，只有使用O预览SDK开发应用时才可以使用这些API。

> **注**：要显示O API，请务必在任何参考页面的左侧导航中将API级别选择器设为“O”。

要详细了解每个预览版中新增、修改和移除的API，请参阅[API差异报告](https://developer.android.google.cn/sdk/api_diff/o-dp1/changes.html)，点击相应的链接将重定向至相关的API参考文档。

### 支持资源
在O开发者预览中测试和开发时，请使用以下渠道报告问题和提供反馈。
* [开发者预览Issue Tracker](https://issuetracker.google.com/issues?q=componentid:190602%20status:open&s=modified_time:desc)是您的**主要反馈渠道**。您可通过Issue Tracker报告错误、性能问题和一般反馈。您还可检查已知问题并找出解决方法步骤。我们将对您的问题进行分类并发送到Android工程团队以供审查，且会为您提供进度更新通知。
* 有关如何报告各种问题的详情，请参阅“报告预览版问题”。
* [Android O开发者社群](https://developer.android.google.cn/dev-community)是一个Google+社群。在此社群中，您可**与其他使用Android O的开发者建立联系**。大家可以分享观察结果或想法，或查找Android O问题的解决方法。

### 锁定目标、预览API和发布
O开发者预览提供的系统和Android库仅面向开发，**不具备标准的API级别**。如果您想锁定新平台和使用新的Android O API开发应用，可以将应用的 [targetSdkVersion](https://developer.android.google.cn/guide/topics/manifest/uses-sdk-element.html)、[minSdkVersion](https://developer.android.google.cn/guide/topics/manifest/uses-sdk-element.html)和gradle `compileSdkVersion` 设为`“N”`，从而锁定预览版的Android O。

Android O开发者提供**预览版 API** — 在最终的SDK发布之前，这些API都不是正式的API。目前，最终的SDK计划于2017年第三季度发布。这意味着一段时期内，特别是该计划的最初几周内，**API可能会出现变化**。我们会通过Android O开发者预览的每次更新为您提供一份变更摘要。

> **注**：虽然预览版API可能会更改，但基本系统行为仍保持稳定，可以立即用于测试。

Google Play**禁止发布针对O开发者预览的应用**。当Android O最终版本SDK可用时，您可以锁定官方Android O API级别，并通过alpha、beta和生产发布渠道将应用发布至Google Play。与此同时，如果您需要将针对Android O的应用分发给测试者，则可随时通过电子邮件或直接从您的网站下载实现这一点。

### 如何上手
要开始使用Android O，请[**下载系统映像**](preview-downloads.html)并将其安装在受支持的设备上，然后[**阅读迁移指南**](preview-migration.html)，了解针对O进行兼容性测试和开发的大致步骤。

感谢您加入Android O开发者预览计划！

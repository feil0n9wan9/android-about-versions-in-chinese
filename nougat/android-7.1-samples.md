# Android 7.1示例
以下代码示例适用于Android 7.1（API 25）。要在Android Studio中下载示例，请选择**File > Import Samples**菜单选项。

### 应用程序快捷方式示例
此示例演示如何使用Android 7.1（API级别25）中引入的[应用程序快捷方式API](android-7.1.html#shortcuts)。该API允许应用程序定义一组intent，并当用户长按应用程序的启动器图标时显示。示例给出了在XML中静态地注册链接以及在运行时动态地注册链接的方式。

[App shortcuts sample](https://developer.android.com/samples/AppShortcuts/index.html)

### 图像键盘应用程序示例
此示例演示如何使用[Android Support Library](https://developer.android.com/topic/libraries/support-library/index.html)实现[Commit Content API](https://developer.android.com/reference/android/view/inputmethod/InputConnection.html#commitContent(android.view.inputmethod.InputContentInfo,%20int,%20android.os.Bundle)。该API为输入法将图像和其他富内容直接发送到应用程序中的文本编辑器提供了一种通用方式，同时允许用户使用自定义表情符号、贴纸和其他应用程序提供的富内容来构建内容。

[Image keyboard app sample](https://developer.android.com/samples/CommitContentSampleApp/index.html)

### 图像键盘输入法示例
此示例演示如何使用[Commit Content API](https://developer.android.com/reference/android/view/inputmethod/InputConnection.html#commitContent(android.view.inputmethod.InputContentInfo,%20int,%20android.os.Bundle)和[Android Support Library](https://developer.android.com/topic/libraries/support-library/index.html)编写[自定义图像键盘](https://developer.android.com/guide/topics/text/image-keyboard.html)。此键盘将显示在兼容的应用程序（也使用Commit Content API）内，允许用户将emojis表情、贴纸和其他富内容插入文本编辑器。

[Image keyboard IME sample](https://developer.android.com/samples/CommitContentSampleIME/index.html)

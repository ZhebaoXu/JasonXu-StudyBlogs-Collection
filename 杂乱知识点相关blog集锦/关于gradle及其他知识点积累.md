#### 一、Android targetSdkVersion 原理

其中，compileSdkVersion 和 minSdkVersion 都非常好理解，前者表示编译的 SDK 版本，后者表示应用兼容的最低 SDK 版本。

targetSdkVersion 是 Android 系统提供前向兼容的主要手段。这是什么意思呢？随着 Android 系统的升级，某个系统的 API 或者模块的行为可能会发生改变，但是为了保证老 APK 的行为还是和以前兼容。只要 APK 的 targetSdkVersion 不变，即使这个 APK 安装在新 Android 系统上，其行为还是保持老的系统上的行为，这样就保证了系统对老应用的前向兼容性。

所以，我们可以猜测到，如果 Android 系统升级，发生这种兼容行为的变化时，一般都会在原来的保存新旧两种逻辑，并通过 `if-else` 方法来判断执行哪种逻辑。

最后，我们也可以理解原文中说的那句话的含义，明白了为什么修改了 APK 的 targetSdkVersion 行为会发生变化，也明白了为什么修改 targetSdkVersion 需要做完整的测试了。



#### 二、对fitsSystemWindow属性的理解：

 这里的fitsSystemWindow具体的作用就是你的contentview是否忽略actionbar,title,屏幕的底部虚拟按键，将整个屏幕当作可用的空间。 
​       正常情况，contentview可用的空间是去除了actionbar,title,底部按键的空间后剩余的可用区域；这个属性设置为true,则忽略，false则不忽略
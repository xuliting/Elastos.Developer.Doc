# Carrier for Android开发文档

## 1 环境准备

1. 两台API21以上的 Android 手机。
2. 安装有 AndroidStudio 的PC机。

## 2 建立工程

1. 在网址 <https://github.com/elastos/Elastos.NET.Carrier.Android.SDK/releases> 中下载 android.sdk-debug.tar.gz。
2. 在 AndroidStudio 中新建 android 工程如 CarrierDemo，minimum SDK 选择 API21 或以上。
3. 将 android.sdk-debug.tar.gz 中的 elacarrier.jar 和 libs/* 文件夹拷贝到 CarrierDemo/app/libs/ 下。
4. 修改 CarrierDemo/app/build.gradle，在其中添加：
    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }
5. 在 AndroidManifest.xml 中添加 internet 权限。

## 3 启动Carrier

1. 新建 DefaultCarrierOptions.java，并继承于 Carrier.Options。设置 BootstrapNodes，可参照 CarrierDemo/app/src/main/java/org/elastos/carrier/demo/DefaultCarrierOptions.java。
2. 新建 DefaultCarrierHandler.java，并继承于 AbstractCarrierHandler。可参照 CarrierDemo/app/src/main/java/org/elastos/carrier/demo/DefaultCarrierHandler.java。
3. 新建 CarrierHelper.java，用于对外提供简单的 API。新建 startCarrier函数用于启动 Carrier。在这个函数中添加 DefaultCarrierHandler 和 DefaultCarrierHandler 的实例，最后调用 Carrier.start()。
实现可参照 CarrierDemo/app/src/main/java/org/elastos/carrier/demo/CarrierHelper.java。
4. 在 MainActivity.java 中调用 CarrierHelper.startCarrier()。
5. 在 DefaultCarrierHandler.java 中重载 onConnection() 函数，监听 Carrier 和 BootstrapNode 的连接状态（Online/Offline）。
6. 运行 CarrierDemo， Carrier 就启动起来了。

## 4 显示和扫描地址（可选）

为了快速添加好友，在 CarrierDemo 中添加了扫描二维码功能，这个功能与 Carrier 使用无关，可忽略。

1. 显示地址。添加 MyAddr 按钮，并实现点击显示二维码，具体实现可参照 MainActivity.java 的 showAddress() 函数。
2. 扫描好友地址。添加 CAMERA， VIBRATE 等权限，添加 ScanAddr 按钮，并实现点击扫描二维码，具体实现可参照 MainActivity.java 的 scanAddress() 函数。

## 5 添加好友

1. 两个手机分别称为A和B。均安装有 CarrierDemo。
2. 在AB双方都处于 Online 状态时，A获取B到好友地址，并调用 addFriend() 函数添加好友B，该函数使用的是B的 Address。可参照 CarrierHelper.java 的 addFriend()。
3. 当A调用 addFriend() 后，被添加的一方会收到 onFriendRequest() 回调，在该回调中， Carrier 会将A的 Address 转换为 UserId，从此处开始， Carrier 将全部使用 UserId 进行身份辨识。可以在 DefaultCarrierHandler.java 重载此函数进行好友认证处理，可参照 CarrierHelper.java 的 acceptFriend()。
4. 通过B的认证后，A会收到 onFriendAdded() 回调，可以在 DefaultCarrierHandler.java 重载此函数进行后续处理。
5. 已经存在的好友不能重复添加，可以通过 getFriends() 函数获取好友列表。

## 6 发送消息

1. AB双方 Online 后，对方均会收到 onFriendConnection() 回调，可以在 DefaultCarrierHandler.java 重载此函数进行后续处理。
2. 在AB双方都处于 Online 状态时，可以通过 sendFriendMessage() 函数向对方发送消息，可参照 CarrierHelper.java 的 sendMessage()。
3. 当A发送消息给B后，B会收到 onFriendMessage() 回调，可以在 DefaultCarrierHandler.java 重载此函数进行消息处理。

## 7 建立Session

1. Carrier 可以通过 session 建立 P2P 连接。
2. 首先需要初始化 Session 的 manager， manager 的回调在当有另一方发出连接请求时触发。可参照 CarrierSessionHelper.java 的 initSessionManager() 函数实现。
3. session 状态可以通过 StreamHandler.onStateChanged() 回调获得，可以在 DefaultSessionHandler.java 重载此函数进行状态处理。

4. A创建 session，可参照 CarrierSessionHelper.java 的 newSessionAndStream() 函数实现。
   * 初始化 SessionManager ，调用 Manager.getInstance(Carrier.getInstance(), sessionHandler) 实现初始化。
   * Manager.newSession() 函数创建一个 session。
   * 通过 Session.addStream 函数添加一个 Stream。
5. A创建 session 并初始化完成后，调用 Session.request() 函数后，B会收到 Manager 的 onSessionRequest() 回调，在回调中，同样调用 newSessionAndStream() 函数建立B端的 session。
6. B创建 session 并初始化完成后，调用 Session.replyRequest() ，A会收到 SessionRequestCompleteHandler.onCompletion() 回调。
7. 当B的 Stream 状态均变成 TransportReady 时，调用 Session.start()。
8. A在等待 onCompletion() 后，调用 Session.start()。
9. 当AB的 Stream 状态均变成 Connected 时，说明 Session 连接创建成功。

## 8 通过Session发送数据

1. AB双方的 Session 均处于 Connected 状态时，可以通过 Stream.writeData() 函数向对方发送数据，可参照 CarrierSessionHelper.java 的 sendData()。
2. 当A发送数据给B后，B会收到 onStreamData() 回调，可以在 DefaultSessionHandler.java 重载此函数进行数据处理。

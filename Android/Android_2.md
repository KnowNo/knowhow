# 应用进程被杀掉后无法收到推送消息

#### 问题详情
使用 LeanCloud 的 Push 来推送一些业务紧迫型消息，但是在 Android 端使用过程中发现只要 App 被内存回收或者强制 kill 掉，那么将不在会收到推送消息，而将手机再次打开时会收到这条已经**过时**的消息。

#### 解决方案
iOS 能做到这点是因为进程关闭后，Apple 和设备的系统之间还会存在连接，这条连接跟应用无关，所以无论应用是否被杀掉，消息都能发送到 APNs 再通过这条连接发送到设备。

但对 Android 来说，如果是国内用户，因为众所周知的原因，Google 和设备之间的这条连接是无法使用的，所以应用只能自己去保持连接并在后台持续运行，一旦后台进程被杀掉，就无法收到推送消息了。

如果应用的服务对象主要是国外用户，就能通过 GCM   （Google Cloud Messaging）来克服上述问题。 LeanCloud 的美国节点以对外发布，将来会提供 GCM 支持，但目前 GCM 在国内确实无法使用。

对应用被杀导致推送收不到的情况确实很难避免，因为毕竟无法建立系统级别的长连接去收消息。不过 LeanCloud SDK 已经采取了各种办法保持应用在后台运行，能保证在大部分情况下能收到消息的。 

如果刚打开手机，应用也并没有打开，这时能收到推送的消息吗？ 后台保持运行的并非应用，而是 LeanCloud 负责接收消息的服务，这个服务是通过你们的 App 引入的。

它启用后就可以收消息。上面也提了，这个服务会尽量保持运行的。LeanCloud 会在后台开启一个服务来持续监听，就算应用关闭了，这个服务也会在后台也不会导致这个服务失效。
# webrtc-001
  iOS下音视频通信-基于WebRTC
   参考 https://www.jianshu.com/p/c49da1d93df4

https://blog.csdn.net/vM199zkg3Y7150u5/article/details/78399270?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.nonecaseWebRTC就是这样一个基于P2P的音视频通信技术	4
WebRTC的服务器与信令	4
webrtc中的JSEP	4
NAT技术	5
ICE协议框架	6
iOS下WebRTC环境的搭建：	6
WebRTC的API	7
WebRTC建立点对点连接的流程	7
性能	8
WebRTC就是这样一个基于P2P的音视频通信技术

客户端A⇋客户端B ... 客户端A⇋客户端C ...（可以无数个客户端之间互联）

WebRTC的服务器与信令
WebRTC就不需要服务端了么？这是显然是错误的认识，严格来说它仅仅是不需要服务端来进行数据中转而已
WebRTC提供了浏览器到浏览器（点对点）之间的通信，但并不意味着WebRTC不需要服务器。暂且不说基于服务器的一些扩展业务，WebRTC至少有两件事必须要用到服务器：
浏览器之间交换建立通信的元数据（信令）必须通过服务器。
为了穿越NAT和防火墙。
webrtc中的JSEP
JSEP（JavaScript Session Establishment Protocol，JavaScript会话建立协议），是一个信令控制协议。

从JSEP完成的功能来看，JSEP相当于一个软化的信令控制协议，只能完成媒体链接的功能，在比较简单的web应用中完全可以胜任，在比较复杂的应用中需要和其他的模块或者信令相结合才能完成完整的应用。像用户查找，位置服务这些功能还需要一个完整的信令控制层。



SIP是应用层的协议，用来建立改变和终结多媒体连接，如语音呼叫。 SIP也可以在已经存在的呼叫上新增加新的呼叫，实现多方会议。SIP的重点是通信双方连接的建立以及呼叫的控制，但是对其他需求来说扩展性不好
NAT技术

NAT技术的出现，其实就是为了解决IPV4下的IP地址匮乏
一个路由器的公网地址对应了n个内网的地址

思路：
我们借助一个公网IP服务器,a,b都往公网IP/PORT发包,公网服务器就可以获知a,b的IP/PORT，又由于a,b主动给公网IP服务器发包，所以公网服务器可以穿透NAT A,NAT B送包给a,b。
所以只要公网IP将b的IP/PORT发给a,a的IP/PORT发给b。这样下次a和b互相消息，就不会被NAT阻拦了。

在RTCPeeConnection中，使用ICE框架来保证RTCPeerConnection能实现NAT穿越











ICE协议框架
ICE协议框架，它大约是由以下几个技术和协议组成的：STUN、NAT、TURN、SDP，这些协议技术，帮助ICE共同实现了NAT/防火墙穿越。

iOS下WebRTC环境的搭建：

WebRTC已经在我们的浏览器中了。如果我们用浏览器，则可以直接使用js调用对应的WebRTC的API，实现音视频通信。


然而我们是在iOS平台，所以我们需要去官网下载指定版本的源码，并且对其进行编译，大概一下，其中源码大小10个多G，编译过程会遇到一系列坑，而我们编译完成最终形成的webrtc的.a库大概有300多m。


WebRTC的API

MediaStream：通过MediaStream的API能够通过设备的摄像头及话筒获得视频、音频的同步流
RTCPeerConnection：RTCPeerConnection是WebRTC用于构建点对点之间稳定、高效的流传输的组件
RTCDataChannel：RTCDataChannel使得浏览器之间（点对点）建立一个高吞吐量、低延时的信道，用于传输任意数据。
其中RTCPeerConnection是我们WebRTC的核心组件。


WebRTC建立点对点连接的流程

[stʌn]



Apple Safari
苹果刚刚加入 WebRTC 阵营中，宣布 iOS 11 和 Safari 11 中支持 WebRTC。
但是苹果并不是全部支持，DataChannel 现在并不能使用，视频编解码是 H.264 ，而不是 VP8。而且这点可能不会发生改变。
总的来说，WebRTC 现在已经覆盖所有的现代浏览器了。


当 WebRTC 不能用或者不能够满足需求时，你总是可以在封闭的应用中使用 WebRTC 技术。
对于 iOS 和 Android 来说，你可以下载 WebRTC 源代码，然后在它的上面编写自己的应用，或者使用 WebView 这样的操作系统。
对于电脑端来说，最通常的做法是使用 Electron，一个围绕 Chromium 搭建的开源应用容器。它可以让你的网页应用编程一个电脑应用，并且可以跨 Windows，Mac，和 Linux 系统使用。而且不管你用的是 IE 或者其他任何浏览器都可以，没有问题

性能
诚然 webRTC 在回声消除，图像编解码等方面已经做得十分出色，但它在性能上的问题还是不可忽视的：
	由于需要进行视频编解码，所以 CPU 占用率非常高，尤其是在移动设备上； 
	在移动设备上获取的视频分辨率有限，最高只能达到 640 * 480； 
	带宽有限时，音视频质量较差，延时明显； 
综上所述，虽然 webRTC 具有不需安装插件或者客户端，开源免费，强大的网络穿透能力，出色的音视频处理技术等等优点，但由于兼容性及性能上的问题，要投入到生产中还需要时间，主要是 IOS11 的普及以及 CPU 占用率和延时的问题。


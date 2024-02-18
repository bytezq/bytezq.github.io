WebRTC是Google开源的在线实时通信的方案，简单来讲就是互联网音视频会议，由于是RFC标准协议，并且浏览器支持， 因此也不断的在拓展边界，应用在低延迟的音视频场景，比如在线会议、直播连麦、低延迟直播、远程机器人控制、智能门铃、 直播的网页推流等。

WebRTC实际上是两个Web浏览器之间直接通信的标准，主要包含了信令(Signaling)和媒体(Media)两个部分的协议。 信令解决两个设备之间的能力的协商的问题，比如支持的编解码能力。媒体解决两个设备之间加密和低延迟媒体包传输的能力。 除此之外，WebRTC本身还实现了语言处理技术比如3A，网络拥塞控制比如NACK、FEC和GCC，音视频编解码，平滑和低延迟播放技术。

```
+----------------+                        +----------------+
+    Browser     +----<--Signaling----->--+    Browser     +
+ (like Chrome)  +----<----Media----->----+ (like Chrome)  +
+----------------+                        +----------------+
```

复制

> Note: WebRTC已经是RFC正式标准，因此各种浏览器都已经支持，而开源的实现也很多，因此不限于浏览器，移动端的浏览器和 Native库也有很多，因此为了沟通的简单起见，本文一般以浏览器指代所有支持WebRTC协议的客户端或设备。

实际上，在互联网上，两个浏览器几乎无法直接通信，特别是不在一个局域网，而且是在远距离跨城市甚至跨国家时，两个浏览器之间传输数据会经过非常多的网络路由器和防火墙，因此传输质量无法保障。因此，实际应用是需要经过服务器中转，而WebRTC服务器有几种类型：

- Signaling Server: 信令服务，两个浏览器之间交换SDP的服务。如果是多人会议，则需要提供房间服务，本质上都是为各个浏览器交换SDP。而在流媒体领域，为了可以使用WebRTC推流和播放，像推送和播放RTMP/SRT/HLS流一样，WHIP/WHEP协议被设计出来了。
- TURN Server: 转发服务，帮助两个浏览器之间转发媒体数据的服务。这是一种透明转发服务，并不会实现数据缓存，因此当多人会议时，浏览器之间需要传输`N*N + N*(N-2)`份数据。一般只应用在非常少的通信场景中，比如一对一。
- SFU Server: 选择性转发服务，服务器上有缓存数据，因此浏览器只需要上传一份数据，服务器会复制给其他参会者。SRS就是SFU，关于SFU的作用可以参考[这里](https://stackoverflow.com/a/75491178/17679565)。目前主要的WebRTC服务器都是SFU服务器，会有`N*N`份流传输，比TURN少`N*(N-2)`份上行数据传输，能解决大部分的传输问题。
- MCU Server: 多点控制服务，服务器将会议中的流合并成一路，这样浏览器只需要传输`N*2`份数据，上传一路下载一路数据。但由于需要编解码，服务器支持的流的数量比SFU要少一个量级，只有在某些特定场景才会采用，具体参考[#3625](https://github.com/ossrs/srs/discussions/3625)

我们重点介绍SFU的工作流，因为SFU是在WebTC服务器中使用最多的，它本质上就是一个浏览器：

```
+----------------+                        +---------+
+    Browser     +----<--Signaling----->--+   SFU   +
+ (like Chrome)  +----<----Media----->----+  Server +
+----------------+                        +---------+
```

复制

> Note: SFU一般都会有Signaling的能力，其实可以把RTMP地址也可以看成是一种非常简化的信令协议，只是WebRTC的信令需要协商 媒体和传输能力，所以比较复杂。在复杂的WebRTC系统中，可能有独立的Signaling和Room集群，但是SFU同样也会有简化的Signaling能力， 只是可能是用于和其他服务通信。

SRS是一个媒体服务器，提供了Signaling和SFU Server的能力。和其他SFU比如Janus不同的是，SRS是基于Stream的，尽管房间中可以有多个参与者，本质上都是有人在推流，其他人在订阅这个流。这样可以避免将房间中的所有流，都耦合到一个SFU传输，可以分散到 多个SFU传输，这样可以支持更多人的会议。

SRS支持的Signaling就是WHIP和WHEP，具体请参考[HTTP API](https://ossrs.net/lts/zh-cn/docs/v5/doc/webrtc#http-api)部分。和直播很不一样的是，由于Signaling和Media分离， 因此需要设置Candidate，详细参考[Candidate](https://ossrs.net/lts/zh-cn/docs/v5/doc/webrtc#config-candidate)。Media默认使用UDP传输，若UDP不可用也可以用TCP参考 [TCP](https://ossrs.net/lts/zh-cn/docs/v5/doc/webrtc#webrtc-over-tcp)。若遇到不可用的情况，很有可能是Candidate设置不对，也有可能是防火墙或端口不通，请参考 [Connectivity](https://ossrs.net/lts/zh-cn/docs/v5/doc/webrtc#connection-failures)使用工具检查。SRS还支持了不同协议的转换，比如推流RTMP后用WebRTC观看参考 [RTMP to WebRTC](https://ossrs.net/lts/zh-cn/docs/v5/doc/webrtc#rtmp-to-rtc)，或者用WebRTC推流后用HLS观看参考[RTC to RTMP](https://ossrs.net/lts/zh-cn/docs/v5/doc/webrtc#rtc-to-rtmp)。

SRS是在2020年支持的WebRTC协议，研发的详细过程请参考[#307](https://github.com/ossrs/srs/issues/307)。
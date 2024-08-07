[MRCP在美团语音交互中的实践和应用](https://tech.meituan.com/2023/03/09/practice-and-application-of-mrcp-in-voice-interaction-of-meituan.html)

[MRCP协议学习笔记](http://www.ctiforum.com/news/guonei/534456.html)

[rfc4463](https://datatracker.ietf.org/doc/html/rfc4463)

MRCP可以称为协议上的协议。

MRCP（Media Resource Control Protocol, MRCP）是一种通讯协议，中文定义是：媒体资源控制协议，用于语音服务器向客户端提供各种语音服务（如语音识别和语音合成）。该协议定义了控制媒体处理资源所必需的请求（Request）、应答（Response）和事件（Event）等消息，它需要借助 RTP（Real-Time Transport Protocol, 实时传输协议）创建一个媒体会话、借助 SIP（Session Initiation Protocol, 会话初始化协议） 和 SDP（Session Description Protocol, 会话描述协议） 创建一个控制会话来实现媒体资源服务器端和客户端之间的控制。

SDP（Session Description Protocol, 会话描述协议）
它定义了会话的媒体类型、传输协议、媒体格式等相关信息。SDP 通常被包含在 SIP消息中，以便通信双方可以了解对方的媒体能力和会话描述信息。
SDP 描述信息包括多个部分，每个部分都以一个字母作为标识符，用于标识该部分描述的内容。下面是 SDP 描述信息的主要部分：
- v：协议版本号，通常为 0。
- o：会话拥有者及会话标识符，包括用户名、会话 ID、会话版本号和网络类型等信息。
- s：会话名称，通常为会议主题或标题。
- t：会话时间，包括开始时间和结束时间。
- c：连接信息，包括连接网络类型、地址类型和地址等信息。
- m：媒体类型，包括媒体名称、传输协议、传输端口和媒体格式等信息。
- a：会话属性，包括媒体属性和会话级别属性等信息，如音频编码器、传输速率、带宽等。
通信双方在收到对方的 SDP 描述信息后，可以根据其中包含的媒体能力和会话描述信息协商媒体流的传输方式，以便在 VoIP 通话中传输音频和视频。
```
v=0
o=- 123456789 123456789 IN IP4 192.168.0.1
s=VoIP Session
t=0 0
a=group:BUNDLE audio video
c=IN IP4 192.168.0.1
m=audio 5004 RTP/AVP 0 8 18 101
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:18 G729/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=sendonly
```


SIP（Session Initiation Protocol, 会话初始化协议）
用于建立、修改和终止 VoIP 会话。
- SIP（Session Initiation Protocol）和 SDP（Session Description Protocol）是两种在 VoIP（Voice over Internet Protocol）中广泛使用的协议，它们通常一起使用。
- SIP 是用于建立、修改和终止实时会话的协议，它负责建立 VoIP 通话的呼叫控制部分。而 SDP 则是一种用于描述 VoIP 会话的协议，它负责会话描述和媒体流的传输方式。SDP 可以在 SIP 消息中使用，以便通信双方可以了解对方的媒体能力和会话描述信息。
- 具体来说，SIP 负责处理呼叫控制，包括建立、修改和终止 VoIP 通话。SIP 会发送呼叫请求（INVITE）到对方，对方收到请求后，可以选择接受或拒绝呼叫。如果呼叫被接受，SIP 将会协商媒体流的传输方式，并将 SDP 描述信息包含在 SIP 消息中发送给对方。
- 而 SDP 则负责描述媒体流的传输方式，包括音频编解码器、传输协议（如 RTP）、传输格式（如 MPEG-4）等信息。SDP 描述信息包含在 SIP 消息中，以便通信双方可以了解对方的媒体能力和会话描述信息。通信双方可以根据 SDP 描述信息协商媒体流的传输方式，以便在 VoIP 通话中传输音频和视频。
- 因此，SIP 和 SDP 是两种不同的协议，SIP 负责呼叫控制，而 SDP 则负责描述媒体流的传输方式。两者结合使用，可以实现 VoIP 通话的建立、修改和终止。

- SIP（Session Initiation Protocol）本身并不传输音频流数据，它只是一个会话控制协议，用于建立、修改和终止 VoIP 会话。音频流数据的传输通常使用 RTP（Real-time Transport Protocol）协议。
- 在 SIP 建立会话时，通信双方会通过 SDP（Session Description Protocol）交换媒体能力和会话描述信息，其中包括音频编码器和传输协议等相关信息。通信双方根据 SDP 描述信息协商媒体流的传输方式，并使用 RTP 协议传输音频流数据。
- RTP 是一种用于实时传输音频和视频数据的协议，它可以在 IP 网络上传输数据，并提供序列号、时间戳和校验和等机制，以保证数据的实时性和准确性。RTP 协议通常与 RTCP（Real-time Transport Control Protocol）协议一起使用，用于传输控制信息和统计信息。
- 在 SIP 会话中，通信双方可以使用 RTP 协议传输音频流数据，其中一方作为发送方（sendonly），另一方作为接收方（recvonly），或者两方都同时发送和接收音频流数据（sendrecv）。通过 RTP 协议传输音频流数据，可以实现高质量的 VoIP 通话。
- 总之，SIP 并不传输音频流数据，它只是用于控制 VoIP 会话的协议。音频流数据通常使用 RTP 协议传输，在 SIP 会话中，通信双方可以根据 SDP 描述信息协商媒体流的传输方式，并使用 RTP 协议传输音频流数据，以实现高质量的 VoIP 通话。
```
INVITE sip:alice@example.com SIP/2.0
Via: SIP/2.0/TCP 192.0.2.1:5060;branch=z9hG4bK123456789
From: Bob <sip:bob@example.com>;tag=12345
To: Alice <sip:alice@example.com>
Call-ID: 987654321@192.0.2.1
CSeq: 1 INVITE
Contact: sip:bob@192.0.2.1
Max-Forwards: 70
Content-Type: application/sdp
Content-Length: 297


v=0
o=- 123456789 123456789 IN IP4 192.0.2.1
s=VoIP Session
t=0 0
a=group:BUNDLE audio video
c=IN IP4 192.0.2.1
m=audio 5004 RTP/AVP 0 8 18 101
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:18 G729/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=sendonly
```

#### RTP协议
- RTP（Real-time Transport Protocol）是一种用于实时传输音频和视频数据的协议。它是一种基于 UDP 的协议，用于在 IP 网络上传输数据，并提供序列号、时间戳和校验和等机制，以保证数据的实时性和准确性。
    
      
    RTP 协议包括两个部分：  
      
    

- RTP 数据包：包含音频或视频数据的实际内容，以及用于描述数据的头部信息。  
    
- RTCP 包：用于传输控制信息和统计信息，包括发送者报告、接收者报告和源描述符等信息。  
      
    RTP 头部信息包括以下字段：
    

- 版本号（V）：RTP 版本号，通常为 2。
- 填充位（P）：指示数据包是否包含一些额外的填充字节，以便使数据包的长度达到某个特定值。
- 扩展位（X）：指示 RTP 头部是否包含扩展标记，用于传输一些额外的信息，如帧的时间戳。
- CSRC 计数（CC）：指示 CSRC（Contributing Source）标识符的数量，用于指示贡献数据的源。
- 标记位（M）：指示数据包是否包含一个重要的帧，如关键帧。
- 负载类型（PT）：指示负载的类型，用于表示音频或视频编码器的类型。
- 序列号（SN）：用于指示数据包在数据流中的顺序。
- 时间戳（TS）：用于指示数据包的时间戳，以便接收方可以恢复数据流时正确地播放音频或视频。
- 同步源（SSRC）标识符：指示数据包的源，用于唯一标识数据流中的每个参与者。
    
      
    RTP 协议通常与 RTCP（Real-time Transport Control Protocol）协议一起使用，用于传输控制信息和统计信息。RTCP 协议的目的是提供统计信息和控制功能，以便接收方可以对数据流进行调整，以适应网络状况的变化。  
      
    总之，RTP 是一种用于实时传输音频和视频数据的协议，它提供序列号、时间戳和校验和等机制，以保证数据的实时性和准确性。RTP 协议通常与 RTCP 协议一起使用，用于传输控制信息和统计信息。  
    

- 下面是一个 RTP 数据包的完整例子，用于传输音频数据：
```
80 60 00 01 6f 85 00 06 72 1a 7b 8d 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
```
    
- 该 RTP 数据包包含以下部分：
- 1. 版本号和标志位：80 表示 RTP 版本号为 2，标志位为 10000000。此标志位中的最高位用于指示是否需要填充字节，此处为 0，表示没有填充字节。
- 2. 负载类型（PT）：60 表示负载类型为 96，用于指示音频编码器的类型。
- 3. 序列号（SN）：00 01 表示序列号为 1，用于指示数据包在数据流中的顺序。
- 4. 时间戳（TS）：6f 85 00 06 表示时间戳为 1879048198，用于指示数据包的时间戳，以便接收方可以恢复数据流时正确地播放音频或视频。
- 5. 同步源（SSRC）标识符：72 1a 7b 8d 表示 SSRC 标识符为 1919256597，用于唯一标识数据流中的每个参与者。




- 1. INVITE 请求行：表示该消息是一个呼叫请求，目标地址为 alice@example.com。
- 2. Via 头部：表示消息经过的路由，包括协议、地址和分支 ID 等信息。
- 3. From 头部：表示发起呼叫的用户，包括用户名、地址和标签等信息。
- 4. To 头部：表示被呼叫的用户，包括用户名和地址等信息。
- 5. Call-ID 头部：表示该呼叫的唯一标识符，包括随机数和地址等信息。
- 6. CSeq 头部：表示消息序列号和请求类型，本例中为第一次 INVITE 请求。
- 7. Contact 头部：表示发起呼叫的用户的联系地址。
- 8. Max-Forwards 头部：表示消息转发的最大跳数。
- 9. Content-Type 头部：表示消息正文的类型，本例中为 SDP 描述信息。
- 10. Content-Length 头部：表示消息正文的长度。
- 11. SDP 描述信息：包括会话的媒体类型、传输协议、媒体格式等相关信息。
- 该 SIP 消息的作用是向 Alice 发起一个 VoIP 通话请求，其中包含了 SDP 描述信息，以便通信双方可以了解对方的媒体能力和会话描述信息。Alice 收到该请求后可以选择接受或拒绝该呼叫，并根据 SDP 描述信息协商媒体流的传输方式，以便在 VoIP 通话中传输音频和视频。
![sip-invite](../音视频技术栈/assets/sip_invite.png)　

在简单的应用环境中，一次从媒体资源服务器调用一种单个的媒体资源类型，媒体会话也是单向的。如果同一时间使用多个媒体资源类型时则创建双向的媒体资源流。MRCP和我们常见的IP通信不同，它可以对媒体会话包含一个控制会话连接。此控制会话是通过在SDP描述中添加更多消息，例如包括MRCP客户端请求的媒体资源类型等。这里，媒体会话的传输是使用RTP通过UDP端口来传输；而控制会话则是通过MRCP通过TCP或者SCTP传输。以下示例是一个MRCP客户端连接媒体资源服务器的流程。这里，不要求支持180 响应，但是创建了一个媒体会话和一个控制会话。

![](http://www.ctiforum.com/uploadfile/2018/0507/20180507095817179.jpg)

　　以上流程中，创建了会话以后，MRCP需要控制媒体资源。MRCP协议消息格式类似于HTTP的消息格式，也是一种外部格式。MRCP消息格式包括三种格式：请求消息，响应消息和事件。请求消息是从MRCP客户端发到媒体资源服务器端，而响应消息则是由媒体资源服务器端，根据客户端的请求返回的响应消息，并且响应消息中包含了一个三位数的状态码。另外，响应消息中包含了当前的请求状态，当前请求状态包括：PENDING，IN-PROGRESS 或 COMPLETE的其中一种。

　　PENDING 状态表示当前的请求在等待处理的队列中，等待进行处理，处理方式为先进先出的方式。IN-PROGRESS状态表示当前请求正在被处理。COMPLETE响应则表示完成请求处理，在当前的连接中无更多消息。在以上三种状态中，PENDING和IN-PROGRESS都表示请求未完成处理，需要更多的事件消息。事件消息允许媒体资源服务器端和客户端对某些状态的改变或对客户端发生一个事件来进行通信。事件消息中包括事件名称和请求状态。

　　第一个关于MRCP协议的流程示例是语音合成（speech synthesiser）的示例。这里，我们假设媒体会话和控制会话通过SIP响应已经创建成功，语音流正在传输。MRCP客户端对服务器端发起了一个SPEAK的请求，开始处理此请求，并且要求通过媒体资源服务器合成一个文本。

![](http://www.ctiforum.com/uploadfile/2018/0507/20180507095850446.jpg)

　　具体的请求消息格式如下：

　　MRCP/2.0 380 SPEAK 14321

　　Channel-Identifier: 43b9ae17@speechsynth

　　Content-Type: application/ssml+xml

　　Content-Length: 253

　　Good afternoon Anne.

　　You have one voice message, two e-mails, and three faxes

　　waiting for you.

　　接下来的响应的消息格式为：

　　MRCP/2.0 119 14321 200 IN-PROGRESS

　　// ID是14321，200 表示成功，正在处理中，119消息长度是119 bytes。

　　Channel-Identifier: 43b9ae17@speechsynth

　　Speech-Marker: timestamp=857206027059 // 这里是NTP时间

　　MRCP的状态码包括：200–299（Success）， 400–499（ Client error）和500–599（Server error）。这些状态码和SIP的状态码基本类似。

　　读者已经看到，我们的请求的状态是IN-PROGRESS ，表示正在被媒体资源处理，大部分情况下，媒体数据可能已经返回到MRCP终端。

　　接下来，媒体资源服务器端生成一个SPEAK-COMPLETE事件，表示此请求已经完成，对于此请求来说，没有更多的事件进行处理。

　　MRCP/2.0 157 SPEAK-COMPLETE 14321 COMPLETE

　　Channel-Identifier: 43b9ae17@speechsynth

　　Speech-Marker: timestamp=861500994355

　　Completion-Cause: 000 normal // 表示SPEAK请求的正常处理结束。

　　通过以上几个步骤和响应消息处理，我们可以清晰地看到语音合成的基本处理流程和其相应的返回码。

　　以上是一个语音合成的MRCP处理流程，我们再介绍一个语音识别的MRCP处理流程。这里，我们仍然假设通过SIP协议，会话控制和媒体控制已经创建成功。以下是一个MRCP客户端发出的请求消息，它要求媒体资源服务器对语音进行识别。注意，这里的请求是RECOGNIZE 请求，而不是刚才我们在语音合成时的SPEAK请求。

![](http://www.ctiforum.com/uploadfile/2018/0507/20180507095912583.jpg)

　　RECOGNIZE请求消息如下：

　　MRCP/2.0 461 RECOGNIZE 32121

　　Channel-Identifier: 23af1e13@speechrecog

　　Content-ID:

　　Content-Type: application/srgs+xml

　　Content-Length: 289

　　

　　xml:lang="en-GB">

　　yes

　　no

　　以上消息格式和SPEAK请求格式非常相似，这里的通道是使用的是speechrecog 媒体资源。这里需要识别的是两个选项（Yes/No）。同样，媒体资源服务器对客户端返回一个正在处理的状态消息：

　　MRCP/2.0 79 32121 200 IN-PROGRESS

　　Channel-Identifier: 23af1e13@speechrecog

　　此消息表示媒体资源服务器正在处理客户端请求，也可能语音识别引擎正在采集媒体流数据，马上生成一个识别的语音。当语音识别引擎检测到语音时，它会生成一个START-OF-INPUT消息。

　　MRCP/2.0 111 START-OF-INPUT 32121 IN-PROGRESS

　　Channel-Identifier: 23af1e13@speechrecog

　　Input-Type: speech

　　这里，客户端也可以结束或打断此语音合成。如果正常处理的话，语音识别引擎会生成一个RECOGNITION-COMPLETE事件消息，并且在消息中包含生成的结果。

　　MRCP/2.0 472 RECOGNITION-COMPLETE 32121 COMPLETE

　　Channel-Identifier: 23af1e13@speechrecog

　　Completion-Cause: 000 success

　　// 000 表示成功，001 表示无匹配结果，002 表示输入超时。

　　Content-Type: application/nlsml+xml

　　// W3C发布的NLSML

　　Content-Length: 289

　　

　　xmlns="http://www.ietf.org/xml/ns/mrcpv2">

　　yes

　　yes

　　在MRCP V2版本（RFC6787）中支持了两种结果输出格式，一种是nlsml（通常说的自然语言语义的标记语言或者描述语言）是由W3C发布。另外一种是EMMA标记语言。EMMA全称为Extensible Multimodal Annotation Markup Language （EMMA）。如果读者有兴趣的话，可以查阅相关的参考文档做进一步的研究。

　　6、通过以上完整的介绍，可能读者对MRCP有了一个基本的概念。但是，在部署MRCP客户端或服务器端时，我们这里没有真正关注其安全性的问题。在今天的IP通信中，其实用户已经对SIP协议的使用场景做了很多的设置，但是如果没有对MRCP协议或使用流程做安全设置的话，特别是MRCP协议中使用了多个XML文件和其语法语义解析文件，并且大部分的MRCP客户端都是通过公网访问的第三方的媒体资源服务器，如果没有设置安全措施的话，同样可能给客户带来很多的安全隐患。这些安全问题包括：

- 会话创建时的安全问题。在SIP创建过程中用户一定要注意安全的设置。
- 控制会话的保护。如果对其控制会话没有做保护措施的话，可能导致第三方对其进行安全攻击。
- 媒体会话的保护。如果我们的媒体数据没有经过安全设置，可能导致媒体数据被监听或盗取。
- 非直接的内容访问。因为，我们的合成内容或语音可能经过公网进行处理，如果第三方非法访问我们的最终数据，可能导致安全问题。
- 保护已存储的媒体文件。客户端和媒体资源服务器端需要针对媒体文件存储设置一定的安全措施和安全权限来保证媒体文件不被盗取或非法访问。

　　7、在本分享中，我们首先介绍了关于MRCP的基本结构，然后分别介绍了MRCP响应中多个核心模块和接口，MRCP客户端的处理方式和媒体资源服务器端的处理方式。我们也介绍了MRCP目前支持的媒体资源类型，以及结合语音服务，媒体网关完成对语音识别应用场景。为了进一步帮助读者了解进一步了解MRCP的处理流程，我们对媒体控制和几种请求响应状态和处理流程做了介绍，并且结合语音合成和语音识别的消息处理流程，给读者提供了一个较完整的消息解析。最后，为了部署安全稳定的解决方案，笔者也从几个不同的方面讨论了关于MRCP的安全性问题，希望读者能够引起重视。




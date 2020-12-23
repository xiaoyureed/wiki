---
title: About Browser Communication
categories: java web
---

介绍一些基于浏览器的实时通信技术

<!--more-->

<!-- TOC -->

- [websocket](#websocket)
    - [websocket是什么](#websocket是什么)
    - [为什么要使用](#为什么要使用)
    - [怎么使用](#怎么使用)
        - [client](#client)
        - [server](#server)
            - [和spring集成](#和spring集成)
            - [不和spring集成](#不和spring集成)
- [webrtc](#webrtc)

<!-- /TOC -->

# websocket

## websocket是什么

- WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议

- WebSocket协议实现了 client(浏览器) 和 server 的双向通信, 使得 server 可以主动向 client 推送消息(实时web通信);

- Websocket 其实是一个新协议，跟 HTTP 协议基本没有关系,只是在刚开始建立连接时会通过http发送几个特殊的header, 典型的url类似 ws://example.com:80/some/path

- 没有 websocket 前, 通过 一下两种方式实现实时web通信

  - ajax 轮询: 轮询技术要求客户端以设定的时间间隔周期性地向服务端发送请求，频繁地查询是否有新的数据改动。明显地，这种方法会导致过多不必要的请求，浪费流量和服务器资源

  - 长连接 (long poll): 其实原理跟 ajax轮询 差不多，都是采用轮询的方式，不过采取的是阻塞模型（一直打电话，没收到就不挂电话），也就是说，客户端发起连接后，如果没消息，就一直不返回Response给客户端。直到有消息才返回，返回完之后，客户端再次建立连接，周而复始;在大并发环境下，性能会非常差

- WebSocket 只需要建立一次连接，就可以一直保持连接状态

- Web浏览器和服务器都必须实现 WebSockets 协议来建立和维护连接

## 为什么要使用

传统的通信请求(例如http请求)只能由客户端发起，服务端对请求做出应答处理, 无法实现服务器主动向客户端发起消息, 服务器有连续的状态变化，客户端要获知就非常麻烦;

client 端要实时了解 server端的数据变化, 可通过频繁的轮询 或者 http 长连接, 但是轮询的效率低，长连接非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）

## 怎么使用

### client

`var Socket = new WebSocket(url, [protocol] );`

```js
// 初始化一个 WebSocket 对象, 客户端就会与服务器进行连接
var ws = new WebSocket("ws://localhost:9998/echo");

// 建立 web socket 连接成功触发事件
ws.onopen = function () {
  // 使用 send() 方法发送数据
  ws.send("发送数据");
  alert("数据发送中...");

  if (ws.bufferedAmount === 0) {//bufferedAmount属性，表示还有多少字节的二进制数据没有发送出去。它可以用来判断发送是否结束
  // 发送完毕
} else {
  // 发送还没结束
}
};

// 接收服务端数据时触发事件
ws.onmessage = function (evt) {//服务器数据可能是文本，也可能是二进制数据（blob对象或Arraybuffer对象）
   if(typeof event.data === String) {
    console.log("Received data string");
  }

  if(event.data instanceof ArrayBuffer){
    var buffer = event.data;
    console.log("Received arraybuffer");
  }
};

// 断开 web socket 连接成功触发事件
ws.onclose = function () {
  alert("连接已关闭...");
};

ws.onerror = function(event) {
  // handle error event
};

switch (ws.readyState) {
  case WebSocket.CONNECTING://值为0，表示正在连接。
    // do something
    break;
  case WebSocket.OPEN://值为1，表示连接成功，可以通信了
    // do something
    break;
  case WebSocket.CLOSING://值为2，表示连接正在关闭。
    // do something
    break;
  case WebSocket.CLOSED://值为3，表示连接已经关闭，或者打开连接失败
    // do something
    break;
  default:
    // this never happens
    break;
}

```

### server

有多种实现, 以Java为例: 

#### 和spring集成


```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-websocket</artifactId>
  <version>${spring.version}</version>
</dependency>

```

继承 TextWebSocketHandler 或 BinaryWebSocketHandler 重写需要的方法即可, 看看WebSocketHandler接口提供哪些方法:

```java
public interface WebSocketHandler {

   /**
    * 建立连接后触发的回调
    */
   void afterConnectionEstablished(WebSocketSession session) throws Exception;

   /**
    * 收到消息时触发的回调
    */
   void handleMessage(WebSocketSession session, WebSocketMessage<?> message) throws Exception;

   /**
    * 传输消息出错时触发的回调
    */
   void handleTransportError(WebSocketSession session, Throwable exception) throws Exception;

   /**
    * 断开连接后触发的回调
    */
   void afterConnectionClosed(WebSocketSession session, CloseStatus closeStatus) throws Exception;

   /**
    * 是否处理分片消息
    */
   boolean supportsPartialMessages();

}

```

最后还需要配置, 注解or xml均可, 作用是将自己编写的 handler注册到spring中

```java
@Configuration
@EnableWebSocket
public class WebSocketConfig implements WebSocketConfigurer {

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        registry.addHandler(myHandler(), "/myHandler");
    }

    @Bean
    public WebSocketHandler myHandler() {
        return new MyHandler();
    }

}

```

xml方式:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:websocket="http://www.springframework.org/schema/websocket"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/websocket
        http://www.springframework.org/schema/websocket/spring-websocket.xsd">

    <websocket:handlers>
        <websocket:mapping path="/myHandler" handler="myHandler"/>
    </websocket:handlers>

    <bean id="myHandler" class="org.springframework.samples.MyHandler"/>

</beans>

```

如果你的 web 应用使用了代理服务器 Nginx，那么你还需要为 Nginx 做一些配置，使得它开启 WebSocket 代理功能。

#### 不和spring集成

加入依赖

```xml
 <dependency>
         <groupId>javax</groupId>
         <artifactId>javaee-api</artifactId>
         <version>7.0</version>
         <scope>provided</scope>
 </dependency>

```

```java
package me.gacl.websocket;

import java.io.IOException;
import java.util.concurrent.CopyOnWriteArraySet;

import javax.websocket.*;
import javax.websocket.server.ServerEndpoint;

/**
 * @ServerEndpoint 注解是一个类层次的注解，它的功能主要是将目前的类定义成一个websocket服务器端,
 * 注解的值将被用于监听用户连接的终端访问URL地址,客户端可以通过这个URL来连接到WebSocket服务器端
 */
@ServerEndpoint("/websocket")
public class WebSocketTest {
    //静态变量，用来记录当前在线连接数。应该把它设计成线程安全的。
    private static int onlineCount = 0;

    //concurrent包的线程安全Set，用来存放每个客户端对应的MyWebSocket对象。若要实现服务端与单一客户端通信的话，可以使用Map来存放，其中Key可以为用户标识
    private static CopyOnWriteArraySet<WebSocketTest> webSocketSet = new CopyOnWriteArraySet<WebSocketTest>();

    //与某个客户端的连接会话，需要通过它来给客户端发送数据
    private Session session;

    /**
     * 连接建立成功调用的方法
     * @param session  可选的参数。session为与某个客户端的连接会话，需要通过它来给客户端发送数据
     */
    @OnOpen
    public void onOpen(Session session){
        this.session = session;
        webSocketSet.add(this);     //加入set中
        addOnlineCount();           //在线数加1
        System.out.println("有新连接加入！当前在线人数为" + getOnlineCount());
    }

    /**
     * 连接关闭调用的方法
     */
    @OnClose
    public void onClose(){
        webSocketSet.remove(this);  //从set中删除
        subOnlineCount();           //在线数减1
        System.out.println("有一连接关闭！当前在线人数为" + getOnlineCount());
    }

    /**
     * 收到客户端消息后调用的方法
     * @param message 客户端发送过来的消息
     * @param session 可选的参数
     */
    @OnMessage
    public void onMessage(String message, Session session) {
        System.out.println("来自客户端的消息:" + message);
        //群发消息
        for(WebSocketTest item: webSocketSet){
            try {
                item.sendMessage(message);
            } catch (IOException e) {
                e.printStackTrace();
                continue;
            }
        }
    }

    /**
     * 发生错误时调用
     * @param session
     * @param error
     */
    @OnError
    public void onError(Session session, Throwable error){
        System.out.println("发生错误");
        error.printStackTrace();
    }

    /**
     * 这个方法与上面几个方法不一样。没有用注解，是根据自己需要添加的方法。
     * @param message
     * @throws IOException
     */
    public void sendMessage(String message) throws IOException{
        this.session.getBasicRemote().sendText(message);
        //this.session.getAsyncRemote().sendText(message);
    }

    public static synchronized int getOnlineCount() {
        return onlineCount;
    }

    public static synchronized void addOnlineCount() {
        WebSocketTest.onlineCount++;
    }

    public static synchronized void subOnlineCount() {
        WebSocketTest.onlineCount--;
    }
}

```


# webrtc

Web Real-Time Communication 网页实时通信, 是一个支持网页浏览器进行实时语音对话或视频对话的技术. 基于P2P的音视频通信技术, 建立连接后的通信过程无需中间 server (创建连接和断开连接的过程还是需要的)

webrtc 和 websocket 区别:

-   WebSockets允许浏览器和Web服务器之间的全双工通信
-   WebRTC的PeerConnection允许两个浏览器之间的全双工通信

建立连接的过程:

1. A和B连接上服务端，建立一个TCP长连接（任意协议都可以，WebSocket/MQTT/Socket原生/XMPP），为了省事，直接采用WebSocket，这样一个信令通道就有了。

2. A从服务器获得ice server同时生成包含session description（SDP）的offer，发送给Socket服务端。

3. Socket服务端把A的offer和candidate转发给B，B会保存下A这些信息。

4. 然后B发送包含自己session description的answer(因为它收到的是offer，所以返回的是answer，但是内容都是SDP)和ice candidate给Socket服务端。

5. Socket服务端把B的answer和ice candidate给A，A保存下B的这些信息。

WebRTC至少有两件事必须要用到服务器:

-   客户端之间交换建立通信的元数据（信令）必须通过服务器。断开连接需要服务器来告知另一端P2P连接已断开

-   穿越NAT和防火墙.

    如果客户端A想给客户端B发送数据，则数据来到客户端B所在的路由器下，会被NAT阻拦，这样B就无法收到A的数据了 。webrtc是通过 ICE 这套框架来处理复杂的网络环境,

信令的类型:

-   Offer：建立点对点的连接时，发起端（A客户端）需要发送的信令
-   Answer：建立点对点的连接时，被叫端（B客户端）需要发送的信令
-   Bye：点对点的连接断开时，发送的信令
 


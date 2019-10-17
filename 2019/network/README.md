# [network](https://juejin.im/entry/5a93dd9b5188257a5911e9be)

### 基础知识
1. ip 
2. dns

### [socket 强烈推荐文章](https://www.jianshu.com/p/cde27461c226)

### 异步socket
1. channel
2. buffer
3. selector

### netty
1. netty 架构 
 ```
Netty是一个NIO客户端服务器框架;
 ```
2. netty channel
 ```
SimpleChannelInboundHandler 
ChannelOutboundHandlerAdapter
ChannelHandlerContext
 ```

3. netty 线程模型
 ```
EventLoopGroup extends EventExecutorGroup 
EventLoop extends OrderedEventExecutor, EventLoopGroup

 DefaultEventExecutorGroup extends MultithreadEventExecutorGroup 
 DefaultEventExecutor extends SingleThreadEventExecutor 
 SingleThreadEventExecutor 它是一个单线程的执行器

 NioEventLoop extends SingleThreadEventLoop  extends SingleThreadEventExecutor
 NioEventLoopGroup extends MultithreadEventLoopGroup 

 ChannelPromise extends ChannelFuture, Promise<Void>

SelectorProvider创建Selector选择器


 ```

 4. netty websocket
  ```
HttpServerCodec
ChuckedWriteHandler
HttpObjectAggregator
WebSocketServerProtocolHandler
  ```
Netty启动引导类Bootstrap
===
```java
ServerBootstrap serverBootstrap = new ServerBootstrap();
            serverBootstrap
                    .group(bossGroup, workGroup)
                    .channel(NioServerSocketChannel.class)
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                @Override
                protected void initChannel(SocketChannel socketChannel) throws Exception {
                    socketChannel.pipeline().addLast(new EchoHandler());
                }
            });

Bootstrap bootstrap = new Bootstrap();
            bootstrap.group(group)
                    .channel(NioSocketChannel.class)
                    .remoteAddress(new InetSocketAddress(host, port))
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) throws Exception {
                            ch.pipeline().addLast(new EchoClientHandler());
                        }
                    });
```
## group:设置线程组模型
```java
 public ServerBootstrap group(EventLoopGroup parentGroup, EventLoopGroup childGroup) {
        super.group(parentGroup);
        if (this.childGroup != null) {
            throw new IllegalStateException("childGroup set already");
        }
        this.childGroup = ObjectUtil.checkNotNull(childGroup, "childGroup");
        return this;
    }
public ServerBootstrap group(EventLoopGroup group) {
    return group(group, group);
}
```
### Reactor线程模型
* 单线程
    ```java
    EventLoopGroup eventLoopGroup = new NioEventLoopGroup(1);
     serverBootstrap
                    .group(eventLoopGroup)
    ```
* 多线程
    ```java
    EventLoopGroup eventLoopGroup1 = new NioEventLoopGroup(1);
    EventLoopGroup eventLoopGroup2 = new NioEventLoopGroup();
     serverBootstrap
                    .group(eventLoopGroup1,eventLoopGroup2)
    ```
* 主从线程
    ```java
    EventLoopGroup eventLoopGroup1 = new NioEventLoopGroup();
    EventLoopGroup eventLoopGroup2 = new NioEventLoopGroup();
     serverBootstrap
                    .group(eventLoopGroup1,eventLoopGroup2)
    ```

## channel 设置channel通道类型
## option: 作用于每个新建立的channel
* ChannelOption.SO_BACKLOG: 存放已完成三次握手的请求的等待队列的最大长度;
* ChannelOption.TCP_NODELAY: 为了解决Nagle的算法问题，默认是false, 要求高实时性，有数据时马上发送，就将该选项设置为true关闭Nagle算法；如果要减少发送次数，就设置为false，会累积一定大小后再发送
......
## childOption: 作用于被accept之后的连接
## childHandler: 用于对每个通道里面的数据处理
## remoteAddress： 服务端地址
## handler：和服务端通信的处理器


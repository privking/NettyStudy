ChannelHandler和ChannelPipeline

## 
* handlerAdded : 当 ChannelHandler 添加到 ChannelPipeline 调用 
* handlerRemoved : 当 ChannelHandler 从 ChannelPipeline 移除时调用 
* exceptionCaught : 执行抛出异常时调用

## 

* ChannelHandler下主要是两个子接口
    * ChannelInboundHandler：(入站) 处理输入数据和Channel状态类型改变， 适配器 ChannelInboundHandlerAdapter（适配器设计模式） 常用的：SimpleChannelInboundHandler
    * ChannelOutboundHandler：(出站) 处理输出数据，适配器 ChannelOutboundHandlerAdapter 

## 

* ChannelPipeline： 好比厂里的流水线一样，可以在上面添加多个ChannelHanler，也可看成是一串 ChannelHandler 实例，拦截穿过 Channel 的输入输出 event, ChannelPipeline 实现了拦截器的一种高级形式，使得用户可以对事件的处理以及ChannelHanler之间交互获得完全的控制权

## 

* 多个入站出站ChannelHandler的执行顺序
    * InboundHandler顺序执行，OutboundHandler逆序执行
    * InboundHandler之间传递数据，通过ctx.fireChannelRead(msg)
    * InboundHandler通过ctx.write(msg)，则会传递到outboundHandler
    * 使用ctx.write(msg)传递消息，Inbound需要放在结尾，在Outbound之后，不然outboundhandler会不执行；但是使用channel.write(msg)、pipline.write(msg)情况会不一致，都会执行
    * outBound和Inbound谁先执行，针对客户端和服务端而言，客户端是发起请求再接受数据，先outbound再inbound，服务端则相反
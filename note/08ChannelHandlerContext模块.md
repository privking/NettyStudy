ChannelHandlerContext模块
===
* ChannelHandlerContext是连接ChannelHandler和ChannelPipeline的桥梁，ChannelHandlerContext部分方法和Channel及ChannelPipeline重合,好比调用write方法,Channel、ChannelPipeline、ChannelHandlerContext 都可以调用此方法，前两者都会在整个管道流里传播，而ChannelHandlerContext就只会在后续的Handler里面传播
* AbstractChannelHandlerContext类双向链表结构，next/prev分别是后继节点，和前驱节点
* DefaultChannelHandlerContext 是实现类，但是大部分都是父类那边完成，这个只是简单的实现一些方法 主要就是判断Handler的类型
* ChannelInboundHandler之间的传递，主要通过调用ctx里面的FireXXX()方法来实现下个handler的调用
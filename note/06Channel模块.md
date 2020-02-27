Channel模块
===
* 什么是Channel: 客户端和服务端建立的一个连接通道
* 什么是ChannelHandler： 负责Channel的逻辑处理
* 什么是ChannelPipeline: 负责管理ChannelHandler的有序容器
* 他们是什么关系:
    * 一个Channel包含一个ChannelPipeline，所有ChannelHandler都会顺序加入到ChannelPipeline中 创建Channel时会自动创建一个ChannelPipeline，每个Channel都有一个管理它的pipeline，这关联是永久性的
* Channel当状态出现变化，就会触发对应的事件
    * 状态：
        * channelRegistered: channel注册到一个EventLoop
        * channelActive: 变为活跃状态（连接到了远程主机），可以接受和发送数据
        * channelInactive: channel处于非活跃状态，没有连接到远程主机
        * channelUnregistered: channel已经创建，但是未注册到一个EventLoop里面，也就是没有和Selector绑定
    * 先后顺序
        * channelRegistered->channelActive->channelInactive->channelUnregistered
ChannelFuture
===
* Netty中的所有I/O操作都是异步的,这意味着任何I/O调用都会立即返回，而ChannelFuture会提供有关的信息I/O操作的结果或状态。
* ChannelFuture状态
    * 未完成：当I/O操作开始时，将创建一个新的对象，新的最初是未完成的 - 它既没有成功，也没有成功，也没有被取消，因为I/O操作尚未完成。
    * 已完成：当I/O操作完成，不管是成功、失败还是取消，Future都是标记为已完成的, 失败的时候也有具体的信息，例如原因失败，但请注意，即使失败和取消属于完成状态
    * 注意：不要在IO线程内调用future对象的sync或者await方法。不能在channelHandler中调用sync或者await方法
* ChannelPromise：继承于ChannelFuture，进一步拓展用于设置IO操作的结果

 
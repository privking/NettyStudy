TCP粘包拆包
===

## 什么是TCP粘包拆包
* TCP拆包: 一个完整的包可能会被TCP拆分为多个包进行发送
* TCP粘包: 把多个小的包封装成一个大的数据包发送, client发送的若干数据包 Server接收时粘成一包  
* 发送方和接收方都可能出现这个原因   
    * 发送方的原因：TCP默认会使用Nagle算法     
    * 接收方的原因: TCP接收到数据放置缓存中，应用程序从缓存中读取 
* UDP: 是没有粘包和拆包的问题，有边界协议
## TCP半包读写常见解决方案
* 发送方：可以关闭Nagle算法
* 接受方: TCP是无界的数据流，并没有处理粘包现象的机制, 且协议本身无法避免粘包，半包读写的发生需要在应用层进行处理
* 应用层解决半包读写的办法
    * 设置定长消息 (10字符)
        * `xdclass000xdclass000xdclass000xdclass000`   
        * FixedLengthFrameDecoder：固定长度解码器             
    * 设置消息的边界 ($$ 切割)
        * `sdfafwefqwefwe$$dsafadfadsfwqehidwuehfiw$$879329832r89qweew$$`
        * DelimiterBasedFrameDecoder： 指定消息分隔符的解码器
            * maxLength：表示一行最大的长度，如果超过这个长度依然没有检测自定义分隔符，将会抛出TooLongFrameException
            * failFast：如果为true，则超出maxLength后立即抛出TooLongFrameException，不进行继续解码.如果为false，则等到完整的消息被解码后，再抛出TooLongFrameException异常
            * stripDelimiter：解码后的消息是否去除掉分隔符
            * delimiters：分隔符，ByteBuf类型
        * LineBasedFrameDecoder: 以换行符为结束标志的解码器 
    * 使用带消息头的协议，消息头存储消息开始标识及消息的长度信息
        * `Header+Body`
        * LengthFieldBasedFrameDecoder：message = header+body, 基于长度解码的通用解码器
            * maxFrameLength:数据包的最大长度
            * lengthFieldOffset:长度字段的偏移位，长度字段开始的地方，意思是跳过指定长度个字节之后的才是消息体字段
            * lengthFieldLength:长度字段占的字节数, 帧数据长度的字段本身的长度
            * lengthAdjustment:一般 Header + Body，添加到长度字段的补偿值,如果为负数，开发人员认为这个 Header的长度字段是整个消息包的长度，则Netty应该减去对应的数字
            * initialBytesToStrip:从解码帧中第一次去除的字节数, 获取完一个完整的数据包之后，忽略前面的指定位数的长度字节，应用解码器拿到的就是不带长度域的数据包
            * failFast:是否快速失败

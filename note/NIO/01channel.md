channel
===
* Java NIO的通道类似流，但又有些不同：
    * 既可以从通道中读取数据，又可以写数据到通道。但流的读写通常是单向的。
    * 通道可以异步地读写。
    * 通道中的数据总是要先读到一个Buffer，或者总是要从一个Buffer中写入。


## Channel的实现
* 这些是Java NIO中最重要的通道的实现：
    * FileChannel 从文件中读写数据。
    * DatagramChannel 能通过UDP读写网络中的数据。
    * SocketChannel 能通过TCP读写网络中的数据。
    * ServerSocketChannel可以监听新进来的TCP连接，像Web服务器那样。对每一个新进来的连接都会创建一个SocketChannel。

## Demo
```java
RandomAccessFile aFile = new RandomAccessFile("data/nio-data.txt", "rw");
FileChannel inChannel = aFile.getChannel();

ByteBuffer buf = ByteBuffer.allocate(48);

int bytesRead = inChannel.read(buf);
while (bytesRead != -1) {

System.out.println("Read " + bytesRead);
buf.flip();

while(buf.hasRemaining()){
System.out.print((char) buf.get());
}

buf.clear();
bytesRead = inChannel.read(buf);
}
aFile.close();
```
### RandomAccessFile
```
Java除了File类之外，还提供了专门处理文件的类，即RandomAccessFile（随机访问文件）类。该类是Java语言中功能最为丰富的文件访问类，它提供了众多的文件访问方法。RandomAccessFile类支持“随机访问”方式，这里“随机”是指可以跳转到文件的任意位置处读写数据。在访问一个文件的时候，不必把文件从头读到尾，而是希望像访问一个数据库一样“随心所欲”地访问一个文件的某个部分，这时使用RandomAccessFile类就是最佳选择。


RandomAccessFile(File file ,  String mode)
//创建随机存储文件流，文件属性由参数File对象指定
RandomAccessFile(String name ,  String mode)
//创建随机存储文件流，文件名由参数name指定

其中mode值及对应的含义如下：
    “r”：以只读的方式打开，调用该对象的任何write（写）方法都会导致IOException异常
    “rw”：以读、写方式打开，支持文件的读取或写入。若文件不存在，则创建之。
    “rws”：以读、写方式打开，与“rw”不同的是，还要对文件内容的每次更新都同步更新到潜在的存储设备中去。这里的“s”表示synchronous（同步）的意思
    “rwd”：以读、写方式打开，与“rw”不同的是，还要对文件内容的每次更新都同步更新到潜在的存储设备中去。使用“rwd”模式仅要求将文件的内容更新到存储设备中，而使用“rws”模式除了更新文件的内容，还要更新文件的元数据（metadata），因此至少要求1次低级别的I/O操作

```

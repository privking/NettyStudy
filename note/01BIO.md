BIO
===
## BIO编写Client/Server通信优缺点
* 优点：
    * 模型简单
    * 编码简单
* 缺点:
    * 性能瓶颈，请求数和线程数 N：N关系
    * 高并发情况下，CPU切换线程上下文损耗大
* 案例：web服务器Tomcat7之前，都是使用BIO，7之后就使用NIO
* 改进：伪NIO,使用线程池去处理业务逻辑

## Demo
```java
package priv.king;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class BIOClient {
    public static final int PORT=8080;
    public static final String HOST="localhost";
    public static void main(String[] args) throws IOException {
        Socket socket = null;
        BufferedReader in = null;
        PrintWriter out = null;
        try {
            socket = new Socket(HOST,PORT);
            System.out.println("Server is started in port:"+PORT);
            in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            out = new PrintWriter(socket.getOutputStream(),true);
            out.println("I am client");
            System.out.println("ServerTime:  "+in.readLine().toString());
        }catch (Exception e){

        }finally {
            if(in !=null){
                try {
                    in.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(out!=null){
                try {
                    out.close();
                }catch (Exception e ){
                    e.printStackTrace();
                }

            }
            if(socket!=null){
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }
}

```
```java
package priv.king;

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class BIOServer {
    public static final int PORT=8080;
    public static void main(String[] args) throws IOException {
        ServerSocket server = null;
        try {
            server = new ServerSocket(PORT);
            System.out.println("Server is started in port:"+PORT);
            Socket socket = null;
            while (true){
                 socket = server.accept();
                 new Thread(new ServerHandler(socket)).start();
            }

        }catch (Exception e){

        }finally {
            if(server!=null){
                System.out.println("server is stop");
                server.close();
            }

        }
    }
}

```
```java
package priv.king;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
import java.time.LocalDateTime;

public class ServerHandler implements Runnable {
    private Socket socket;

    public ServerHandler(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        BufferedReader in = null;
        PrintWriter out = null;
        try {
           in = new BufferedReader(new InputStreamReader(this.socket.getInputStream()));
           out = new PrintWriter(this.socket.getOutputStream(),true);
           String body = null;
           while ((body=in.readLine())!=null&& body.length()!=0){
               System.out.println("msg: "+body);
               out.println(LocalDateTime.now());

           }
        }catch (Exception e ){
            e.printStackTrace();
        }finally {
            if(in !=null){
                try {
                    in.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
           if(out!=null){
               try {
                   out.close();
               }catch (Exception e ){
                   e.printStackTrace();
               }

           }
           if(socket!=null){
               try {
                   socket.close();
               } catch (IOException e) {
                   e.printStackTrace();
               }
           }

        }
    }
}

```
JavaNIOPath
===
* Path接口是java NIO2的一部分。首次在java 7中引入。Path接口在java.nio.file包下，所以全称是java.nio.file.Path。 java中的Path表示文件系统的路径。可以指向文件或文件夹。也有相对路径和绝对路径之分。绝对路径表示从文件系统的根路径到文件或是文件夹的路径。相对路径表示从特定路径下访问指定文件或文件夹的路径。
## 创建Path实例
* 为了使用java.nio.file.Path实例，必须首先创建它。可以使用Paths 类的静态方法Paths.get()来产生一个实例。以下是示例：
```java
import java.nio.file.Path;
import java.nio.file.Paths;

public class PathExample {

public static void main(String[] args) {

Path path = Paths.get("c:\\data\\myfile.txt");

}
}
```
### 创建绝对路径Path
```java
Path path = Paths.get("c:\\data\\myfile.txt");

Path path = Paths.get("/home/jakobjenkov/myfile.txt");
```
### 创建相对路径Path
* 相对路径指从一个已确定的路径开始到某一文件或文件夹的路径。将确定路径和相对路径拼接起来就是相对路径对应的绝对路径地址。
* java NIO Path类也能使用相对路径。可以通过Paths.get(basePath, relativePath)创建一个相对路径Path。示例如下：
```java
Path projects = Paths.get("d:\\data", "projects");

Path file = Paths.get("d:\\data", "projects\\a-project\\myfile.txt");
```






# 前言

在丰富多样的技术框架和复杂的技术选型过程中，我们通常会对中间件的产品类别以及版本等方面进行统一规划与管理。然而，对于中间件的配置，目前是否存在统一的标准呢？

以等级保护要求较高的系统中所使用的 Tomcat 为例，这会引出一系列关于统一规范的思考：

1. 其一，Tomcat 的管理页面是否应当予以禁用？
2. 其二，在 Tomcat 的 404 页面中，是否需要将版本号进行隐藏处理？
3. 其三，Tomcat 的堆内存最大值与最小值应设置为多少才最为适宜？
4. ...

诸如此类的问题，促使我们必须进行深入且全面的思考，进而针对各类中间件、操作系统、数据库等制定出一套统一的、开发人员必须严格遵守的标准，我将其定义为基线，此亦为【**中间件标准计划**】的起草初衷。

- 在基线之前我会设立相应问题来回顾相关知识，称为【**醒脑**】



# 1、Tomcat











Tomcat是一个Web服务器，例如，假设有一个在线购物网站，用户想查看商品列表。当他们点击“商品列表”链接时，浏览器会向Tomcat服务器发送一个请求。Tomcat接收到这个请求后，会找到对应处理这个请求的Servlet或JSP，执行它们来查询数据库中的商品信息，然后生成包含这些信息的HTML页面，并将这个页面返回给浏览器显示。这个过程就是一个典型的Tomcat处理请求的例子。



## 1.1、Tomcat的缺省端口是多少？怎么修改

默认端口是8080。如果需要修改这个端口号，可以通过编辑配置文件`/conf/server.xml`。(搜索port字段)

- 例如，如果你的Tomcat安装在Linux系统上，修改完`server.xml`后，可以通过Tomcat的bin目录下的`shutdown.sh`脚本来停止Tomcat,然后使用`startup.sh`脚本来重新启动它。
- 在Windows上，你可以通过服务面板或使用bin目录下的`startup.bat`和`shutdown.bat`脚本来启动和停止Tomcat。更改Toct的默认端口可以帮助解决端口冲突的问题，或者是出于安全考虑，避免使用常见的默认端口。





## 1.2、Tomcat有哪几种Connector运行模式

1. BIO（Blocking I/O）模式：这是Tomcat的默认运行模式。在BIO模式下，Tomcat为每个客户端连接启动一个线程来处理请求。然而，当客户端数量较多时，会创建大量的处理线程，导致资源浪费，尤其是在高并发场景下。
   - Tomcat7以下版本在默认情况下是以bio模式运行的。一般而言，bio模式是三种运行模式中性能最低的一种。
2. NIO（Non-blocking I/O）模式：NIO模式采用基于缓冲区并能提供**非阻塞I/O**操作的Java API，因此具有比BIO更好的并发运行性能。想运行在该模式下，直接修改`server.xml`的`Connector`节点，修改`protocol="org.apache.coyote.http11.Http11NioProtocol"`。
   - Tomcat8以上版本，默认采用的模式就是NIO模式。无需做额外配置。

```xml
    <Connector port="8080" protocol="org.apache.coyote.http11.Http11NioProtocol"
               connectionTimeout="20000"
               redirectPort="8443"
               maxParameterCount="1000"
               />
```

3. AIO（Asynchronous I/O）模式：AIO模式是基于时间和回调机制实现的异步IO。在这种模式下，应用操作完成后会直接返回，不会阻塞线程。当后台处理完成后，操作系统会通知相应的线程进行后续操作。

```xml
    <Connector port="8080" protocol="org.apache.coyote.http11.Http11AprProtocol"
               connectionTimeout="20000"
               redirectPort="8443"
               maxParameterCount="1000"
               />
```

> **高并发**应用首选此模式，就是还需要其他依赖库。

Tomcat8.5及以后版本：进一步增加了对NIO2(Non-Blocking I/O2)的支持，NIO2是Java7中引入的一个新的I/O库,
提供了更强大的异步非阻塞/O处理能力。虽然NIO是默认设置，但Tomcat也支持NIO2和APR(Apache Portable Runtime,一种基于本地代码的高性能I/O模型)作为可配置选项。





## 1.3、Tomcat优化方案归纳

Tomcat优化方案主要可以从配置优化、性能调优和硬件资源优化三个方面进行归纳。

1. **配置优化**
   - 调整JVM参数：通过调整Tomcat启动脚本中的Java虚拟机(JVM)参数，可以优化内存使用和垃圾回收策略。增加堆内存和非堆内存的大小可以减少内存溢出的风险，并提高处理能力。
   - 使用NIO连接器(Tomcat8以上版本，默认采用的模式就是NIO模式。无需做额外配置。)
2. **优化线程池设置**：调整Tomcat的线程池配置，包括最大线程数、最小空闲线程数等，可以提高对并发请求的处理能力。例如在SpringBoot中对Tomcat的一些配置如下：

```yaml
# 开发环境配置
server:
  # 服务器的HTTP端口，默认为8080
  port: 8080
  # 启用压缩可以减少通过网络发送的数据量，从而缩短响应时间
  compression:
    enabled: true
    mime-types: application/json,application/xml,text/html,text/xml,text/plain
    min-response-size: 1024
  connection-timeout:  20000           # 客户端连接超时时间（以毫秒为单位）
  servlet:
    # 应用的访问路径
    context-path: /
  tomcat:
    # 配置 tomcat 的访问日志
    basedir: D:/KuangStudy_RuoYiFast/logs/tomcat
    keep-alive-timeout:  10000         # 保持连接超时时间（以毫秒为单位）
    max-keep-alive-requests:  100      # 可通过保持连接发送的最大请求
    # tomcat的URI编码
    uri-encoding: UTF-8
    # 连接数满后的排队数，默认为100
    accept-count: 1000
    threads:
      # tomcat最大线程数，默认为200
      max: 800
      # Tomcat启动初始化的线程数，默认值10
      min-spare: 100
      max-connections:  10000      # 可处理的最大连接数
      accept-count:  1000          # 传入连接请求的最大队列长度
```

3. 使用压缩：启用HTTP响应压缩功能可以减少网络传输的数据量，提高响应速度。这对于文本类数据（如HTML、CSS和
   JavaScript文件)尤其有效。(在上面的yaml文件中也可以配置)





## 1.4、Tomcat主配置文件server.xml

`server.xml`是Tomcat主要的配置文件之一，位于`$CATALINA HOME/conf/`目录下。这个XML格式的文件负责配置Tomcat服务器的核心组件，包括服务、连接器(Connector)、引擎(Engine)、虚拟主机(Host)和上下文(Context)。通过编辑`server.xml`文件，可以对Tomcat服务器进行细致的配置，以满足不同的部署需求。以下是`server.xml`文件中一些关键配置元素的作用：

1. Server:顶层元素，代表Tomcat服务器实例本身。可以配置一些全局属性，如端口号(用于关闭Tomcat的端口，不是用于服务请求)、关闭命令等。
2. Connector：连接器元素，负责接收客户端的请求并将其传递给Tomcat处理。可以配置多个Connector,支持不同的协议（如HTTP/1.1、AJP)或监听不同的端口。
3. Executor：执行器元素，允许定义线程池。



## 1.5、Tomcat针对JVM优化参数有哪些及其含义

Tomcat运行在Java虚拟机(JVM)之上，因此通过调整VM的启动参数可以优化Tomcat的性能。JVM参数的调整可以影响Tomcat的响应速度、吞吐量、内存使用以及稳定性。以下是一些针对Tomcat优化的常用V小M参数及其含义：

1. 堆内存设置
   - 初始堆内存`-Xms`：设置JVM启动时堆内存的初始大小。例如，`-Xms512m`表示设置JVM启动时的堆内存大小为512MB
   - 最大堆内存**-Xmx**：设置JVM可以使用的最大堆内存大小。例如，`-Xmx1024m`表示设置JVM最大可用堆内存为1024MB。通过合理调整这两个参数，可以控制Tomcat可用的最大内存，避免内存溢出，同时提高性能。

2. 年轻代(Young Generation)大小设置
   - **-Xmn**:设置年轻代的大小。年轻代的大小直接影响到垃圾收集的频率和时间。合理设置年轻代大小可以减少垃圾收集的次数，提高系统的响应速度。
3. 垃圾回收器设置
   - `-XX:+UseConcMarkSweepGC`:启用CMS(并发标记-清除)垃圾回收器，适用于需要更短回收停顿时间的应用。
   - `-XX:+UseParalleIGC`:启用并行垃圾回收器，提高吞吐量。
   - `-XX:UseG1GC`:启用G1垃圾回收器，适用于大堆内存和需要更可控的停顿时间的应用。
   - 选择合适的垃圾回收器可以优化垃圾收集的效率，减少对应用响应时间的影响。

4. 垃圾回收日志设置
   - `-XIoggc`:设置垃圾回收日志文件的路径。例如，`-xloggc:/path/to/gc.log`
   - `-XX:+PrintGCDetails`:输出详细的垃圾回收日志。
   - `-XX:+PrintGCDateStamps`:在垃圾回收日志中添加时间戳。
   - 启用垃圾回收日志并记录详细信息，对于分析和优化垃圾回收行为非常有用。

5. 直接内存设置
   - `-XX:MaxDirectMemorySize`:设置JVM使用的最大直接内存大小。直接内存常用于NIO操作，通过设置可以避免
     OutOfMemoryError。
6. 元空间 Metaspace 大小设置
   - `-XX:MetaspaceSize`和`-XX:MaxMetaspaceSize`:在Java8及以后版本中，用于替代永久代(PermGen),设置元空间的初始大小和最大大小。





# 2、基线

## 2.1、禁用Tomcat管理页面

为了防止黑客通过Tomcat管理页面对Tomcat发起攻击，例如猜解密码等控制管理页面，攻击服务器。

![](Tomcat.assets/1.png)

```bash
# 1、进入Tomcat文件夹下的 webapps 文件夹
cd D:\Develop\apache-tomcat-9.0.76\webapps
# 2、将docs 、examples、host-manager、manager、ROOT文件夹打包为 webapps_back.tar
tar -cvf webapps_back.tar docs examples host-manager manager ROOT
# 3、删除webapps 文件夹下的所有文件和文件夹
rm -rf docs examples host-manager manager ROOT
# 4、重新创建一个ROOT文件夹
mkdir -p ROOT
# 5、重启tomcat服务
```



## 2.2、隐藏Tomcat版本号

在生产环境中，隐藏Tomcat的版本信息是一个重要的安全措施，可以防止潜在的攻击者获取有关服务器的信息。

![](Tomcat.assets/2.png)

```bash
# 1、进入Tomcat的lib文件
cd D:\Develop\apache-tomcat-9.0.76\lib
# 2、解压 catalina.jar 文件
unzip catalina.jar
# 3、# 进入解压后的文件夹
cd org/apache/catalina/util
# 4、修改
vi ServerInfo.properties
# 5、将 server.info 的值清空
```

![](Tomcat.assets/3.png)







## 2.3、禁止列出目录

禁止通过浏览器来访问目录。

- 修改`conf/web.xml`

![](Tomcat.assets/4.png)



## 2.4、隐藏Tomcat产品信息

- 在`conf/server.xml`的`Connector`项中增加server配置项

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443"
               maxParameterCount="1000"
               server="appsvr"
               />
```

![](Tomcat.assets/5.png)



## 2.5、防止远程恶意关闭Tomcat服务

- 在`conf/server.xml`的`shutdown="SHUTDOWN"`修改为`shutdown="复杂字符串"`

![](Tomcat.assets/6.png)







## 2.6、安装目录

- Unix的安装目录为`/tomcat`
- Windows的安装目录为`D:/tomcat`




















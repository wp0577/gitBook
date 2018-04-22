为什么需要web服务器， web服务器是干什么的？

tomcat就类似于增强版的myWebServer, 通过socket封装了IP和端口号来进行通信

### **Tomcat封装了socket去调用servlet**

---

```
1. Apache是web服务器，Tomcat是应用（java）服务器，它只是一个servlet容器，是Apache的扩展。
```

```




2. Apache和Tomcat都可以做为独立的web服务器来运行，但是Apache不能解释java程序（jsp,serverlet）。




3. Apache是普通服务器，本身只支持html即普通网页。不过可以通过插件支持php,还可以与Tomcat连通(单向Apache连接Tomcat,就是说通过Apache可以访问Tomcat资源。反之不然)




4. 两者都是一种容器，只不过发布的东西不同：Apache是html容器，功能像IIS一样；Tomcat是jsp/servlet容器，用于发布jsp及java的，类似的有IBM的webshere、EBA的Weblogic，sun的JRun等等。




5. Apache和Tomcat是独立的，在通一台服务器上可以集成。




打个比方：Apache是一辆卡车，上面可以装一些东西如html等。但是不能装水，要装水必须要有容器（桶），Tomcat就是一个桶（装像Java这样的水），而这个桶也可以不放在卡车上。




Apache只支持静态网页，但像asp,php,cgi,jsp等动态网页就需要Tomcat来处理。




Apache和Tomcat整合使用：如果客户端请求的是静态页面，则只需要Apache服务器响应请求；如果客户端请求动态页面，则是Tomcat服务器响应请求；因为jsp是服务器端解释代码的，这样整合就可以减少Tomcat的服务开销 。
```

---

Java socket编程教学: [https://www.cnblogs.com/rocomp/p/4790340.html](https://www.cnblogs.com/rocomp/p/4790340.html)

**TCP/IP协议：**

**            是目前世界上应用最为广泛的协议，是以TCP和IP为基础的不同层次上多个协议的集合，也成TCP/IP协议族、或TCP/IP协议栈**

**            TCP：Transmission Control Protocol 传输控制协议**

**            IP：Internet Protocol 互联网协议**

**       3、TCP/IP五层模型**

**            应用层：HTTP、FTP、SMTP、Telnet等**

**            传输层：TCP/IP**

**            网络层：**

**            数据链路层：**

**            物理层：网线、双绞线、网卡等**

**       4、IP地址**

**            为实现网络中不同计算机之间的通信，每台计算机都必须有一个唯一的标识---IP地址。**

**            32位二进制**

**       5、端口**

**            区分一台主机的多个不同应用程序，端口号范围为0-65535，其中0-1023位为系统保留。**

**            如：HTTP：80  FTP：21 Telnet：23**

**IP地址+端口号组成了所谓的Socket，Socket是网络上运行的程序之间双向通信链路的终结点，是TCP和UDP的基础**

**       6、Socket套接字：**

**            网络上具有唯一标识的IP地址和端口组合在一起才能构成唯一能识别的标识符套接字。**

**          Socket原理机制：**

**             通信的两端都有Socket**

**             网络通信其实就是Socket间的通信**

**             数据在两个Socket间通过IO传输**


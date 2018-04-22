tomcat服务器使用反射机制调用Servlet来建立实例，调用一次init方法将实例装载到内存，此时Servlet的生命周期开始。

当init被初始化后，全世界都是调用这个servlet。





![](/assets44/import.png)![](/assets55/import.png)

最后会有三种情况会调用destroy函数

1. reload webApp or 2, shut down tomcat or 3. shutdown service.






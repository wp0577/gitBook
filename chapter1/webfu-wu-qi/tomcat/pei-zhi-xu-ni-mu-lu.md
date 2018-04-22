可以让tomcat管理webapps文件夹外的web应用

在&lt;host&gt;间添加 &lt;Context path="/web" docBase="虚拟目录位置（绝对路劲）"&gt;；

---

---

更改主页，需要在WEB-INF下的web.xml中修改 &lt;welcome first list&gt;

---

---

Tcp/Ip三次握手![](/sda/import.png)**很重要的一点，WEB-INF下的classes文件中保存的是java编译后的文件，也就是.class文件。**


实现Servlet：

1. 实现Servlet接口
2. 通过继承 GenericServlet
3. **通过继承 HttpServlet 重点**

Servlet api并不在java se的api里面

---

根据servlet规范，每次部署servlet都要在web.xml内部署一次。

![](/assets11/import.png)![](/assets22/import.png)

---

实现Servlet接口

实现Servlet接口具体实现方式

![](/assets333/import.png)![](/assets444/import.png)

---

2. 通过generic servlet实现：仅需了解

---

3. 通过HttpServlet 的方法实现

代码实现如下：![](/assets66/import.png)

---

post和get的区别在于post会将用户输入的参数隐藏，而get会将所有信息都放在URL栏中。

![](/assets77/import.png)






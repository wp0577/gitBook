# JSP内置对象

jsp被翻译成servlet之后，service方法中有9个对象定义并初始化完毕，我们在jsp脚本中可以直接使用这9个对象

| **名称** | **类型** | **描述** |
| :--- | :--- | :--- |
| out | javax.servlet.jsp.JspWriter | 用于页面输出 |
| request | javax.servlet.http.HttpServletRequest | 得到用户请求信息， |
| response | javax.servlet.http.HttpServletResponse | 服务器向客户端的回应信息 |
| config | javax.servlet.ServletConfig | 服务器配置，可以取得初始化参数 |
| session | javax.servlet.http.HttpSession | 用来保存用户的信息 |
| application | javax.servlet.ServletContext | 所有用户的共享信息 |
| page | java.lang.Object | 指当前页面转换后的Servlet类的实例 |
| **pageContext** | javax.servlet.jsp.PageContext | JSP的页面容器 |
| exception | java.lang.Throwable | 表示JSP页面所发生的异常，在错误页中才起作用 |

## \(1\)out对象

out的类型：JspWriter

out作用就是想客户端输出内容----out.write\(\)

out缓冲区默认8kb 可以设置成0 代表关闭out缓冲区 内容直接写到respons缓冲器

## \(2\)pageContext对象

jsp页面的上下文对象，作用如下：

page对象与pageContext对象不是一回事

1）pageContext是一个域对象

setAttribute\(String name,Object obj\)

getAttribute\(String name\)

removeAttrbute\(String name\)

pageContext可以向指定的其他域中存取数据

setAttribute\(String name,Object obj,int scope\)

getAttribute\(String name,int scope\)

removeAttrbute\(String name,int scope\)

**findAttribute\(String name\)**

**---依次从pageContext域，request域，session域，application域中获取属性，在某个域中获取后将不在向后寻找**

**四大作用域的总结：**

**page域：当前jsp页面范围**

**request域：一次请求**

**session域：一次会话**

**application域：整个web应用**

2）可以获得其他8大隐式对象

例如：pageContext.getRequest\(\)

pageContext.getSession\(\)


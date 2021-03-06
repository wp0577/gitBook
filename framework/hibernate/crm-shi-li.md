# CRM实例

## 客户数据保存功能：

1：导包

2：Hibernate环境配置，orm文件，主配置文件

3：前端文件导入（各级页面）

4：点击保存按钮发送请求给addCustomerServlet

5：在web.xml中映射servlet地址

6：servlet接受request参数，并通过BeanUtil工具将参数值赋值给Customer类

```text
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        CustomerService customerService = new CustomerServiceImpl();
        Customer customer = new Customer();
        try {
            BeanUtils.populate(customer, request.getParameterMap());
        } catch (Exception e) {
            e.printStackTrace();
        }
        customerService.save(customer);
    }
```

7：servlet调用Service类，service类调用dao类去操作数据。

8：dao类使用HibernateUtil工作获得session并开启事务然后操作数据最后完成操作。

## 客户数据显示功能

#### 页面内重定向：

request.getRequestDispatcher\("你要跳转的页面"\).forward\(request, response\)

#### Servlet在web.xml中的映射可用Servlet注解来实现。

```text
@WebServlet(name = "AddCustomerServlet", urlPatterns = "/AddCustomerServlet")
```

  
实现：

Dao代码

```text
public List<Customer> getAll() {
   //1 获得session
         Session session = HibernateUtils.getCurrentSession();
   //2 创建Criteria对象
         Criteria c = session.createCriteria(Customer.class);
   return c.list();
}
```

 Service代码

```text
public List<Customer> getAll() {
   Session session =  HibernateUtils.getCurrentSession();
   //打开事务
   Transaction tx = session.beginTransaction();
   
   List<Customer> list = customerDao.getAll();
   
   
   //关闭事务
   tx.commit();
   return list;
```

 Servlet代码

```text
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    CustomerServiceImpl customerService = new CustomerServiceImpl();
    List<Customer> all = customerService.getAll();
    request.setAttribute("list", all);
    request.getRequestDispatcher("/jsp/customer/list.jsp").forward(request,response);

}
```

## 新增联系人（多表关系）

{% hint style="info" %}
创建domin对象和云数据ORM时，一定要记得在Hibernate.cfg.xml文件中添加mapping。
{% endhint %}

## 客户列表条件查询

```text
 CustomerServiceImpl customerService = new CustomerServiceImpl();
        String cust_name = request.getParameter("cust_name");
        if (cust_name != null || !"".equals(cust_name)) {
            DetachedCriteria dc = DetachedCriteria.forClass(Customer.class);
            dc.add(Restrictions.like("cust_name","%"+ cust_name + "%"));
            List<Customer> list = customerService.getAll(dc);
            request.setAttribute("list", list);
        }

        request.getRequestDispatcher("/jsp/customer/list.jsp").forward(request,response);

```

用到了Detach离线查询，只需在DAO层关联Detach对象，并返回结果。

```text
	public List<Customer> getAll(DetachedCriteria dc) {
		//1 获得session
				Session session = HibernateUtils.getCurrentSession();
		//2 将离线对象关联到session
				Criteria c = dc.getExecutableCriteria(session);
		//3 执行查询并返回
		return c.list();
```

## 联系人列表查询

{% hint style="info" %}
一对多时，查询多方时报错：

ERROR JavassistProxyFactory HHH000142: Javassist Enhancement failed

1：是因为用了struts2跟Hibernate结合出现的问题，问题的根本原因是在多导了个jar包。javassist-3.11.0.GA.jar跟javassist-3.18.1-GA.jar; 这两个包冲突了;删除一个即可。

[https://bbs.csdn.net/topics/392170747](https://bbs.csdn.net/topics/392170747)

2：
{% endhint %}

跟客户条件列表查询类似


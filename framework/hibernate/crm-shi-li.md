# CRM实例

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


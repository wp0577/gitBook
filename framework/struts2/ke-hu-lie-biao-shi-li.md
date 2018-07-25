# 客户列表实例

##  保存用户：

{% code-tabs %}
{% code-tabs-item title="customerAction" %}
```text
public class CustomerAction extends ActionSupport implements ModelDriven<Customer> {
	private CustomerService cs = new CustomerServiceImpl();
	private Customer customer = new Customer();
	public String add() throws Exception {
		//直接调用Service中的save方法
		cs.save(customer);
		return "toList";
	}
	@Override
	public Customer getModel() {
		return customer;
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Struts.xml" %}
```text
<action name="CustomerAction_*" class="com.wp.example.web.action.CustomerAction" method="{1}" >
            <!-- result元素:结果配置
                    name属性: 标识结果处理的名称.与action方法的返回值对应.
                    type属性: 指定调用哪一个result类来处理结果,默认使用转发.
                    标签体:填写页面的相对路径
            -->
            <result name="success" >/jsp/customer/list.jsp</result>
            <result name="toList" type="redirectAction" >
                <param name="actionName">CustomerAction_list</param>
                <param name="namespace">/</param>
            </result>
            <allowed-methods>list,add</allowed-methods>
        </action>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
用ModelDriven的话，不需要使用get,set方法，直接重写getModel方法即可。
{% endhint %}

## 用OGNL改写客户列表

```text
//用ActionContext进行参数传递
		ActionContext.getContext().put("list", list);
```

```text
<!--用ognl标签取值-->
<!--因为list是放在ActionContext中，属于ValueStack中的context部分所以要用#提取-->
<s:iterator value="#list">
<TR
  style="FONT-WEIGHT: normal; FONT-STYLE: normal; BACKGROUND-COLOR: white; TEXT-DECORATION: none">
<TD><s:property value="cust_name" /> </TD>
<TD><s:property value="cust_level" /></TD>
<TD><s:property value="cust_source" /></TD>
<TD><s:property value="cust_linkman" /></TD>
<TD><s:property value="cust_phone"/></TD>
<TD><s:property value="cust_mobile"/> </TD>
<TD>
<a href="${pageContext.request.contextPath }/customerServlet?method=edit&custId=${cust_id}">修改</a>
&nbsp;&nbsp;
<a href="${pageContext.request.contextPath }/customerServlet?method=delete&custId=${cust_id}">删除</a>
</TD>
</TR>
</s:iterator>
```

## 登陆



{% hint style="info" %}
```text
<!-- 
onsubmit是表单提交事件，通过触发这个事件来提交表单;
action是服务器上处理表单提交数据的程序的路径;
用JS写表单验证函数只能onsubmit有关，onsubmit = "return 函数名()"，一定注意return
-->
<FORM id=form1 name=form1 action="${pageContext.request.contextPath}/UserAction_login" method=post>
```
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="UserAction" %}
```
package com.wp.example.web.action;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;
import com.wp.example.domain.User;
import com.wp.example.service.UserService;
import com.wp.example.service.impl.UserServiceImp;
import org.hibernate.criterion.DetachedCriteria;
import org.hibernate.criterion.Restrictions;

public class UserAction extends ActionSupport implements ModelDriven<User>{

    private User user = new User();
    DetachedCriteria dc = DetachedCriteria.forClass(user.getClass());
    UserService userService = new UserServiceImp();
    public String login() {
        System.out.println(user.getUser_name());
        dc.add(Restrictions.eq("user_name", user.getUser_name()));
        dc.add(Restrictions.eq("user_password", user.getUser_password()));
        boolean flag = userService.check(dc);
        System.out.println(flag);
        if (flag) {
            ActionContext.getContext().getSession().put("user", user);
            return SUCCESS;
        }
        else {
            System.out.println("failded");
            ActionContext.getContext().put("error", "wrong username or password");
            return ERROR;
        }
    }

    @Override
    public User getModel() {
        return user;
    }
}


```
{% endcode-tabs-item %}

{% code-tabs-item title="error code" %}
```text
<SPAN style="color: red">${pageContext.request.getAttribute("error")}</SPAN>

```
{% endcode-tabs-item %}

{% code-tabs-item title="username" %}
```
当前用户：${user.getUser_name()}
```
{% endcode-tabs-item %}
{% endcode-tabs %}


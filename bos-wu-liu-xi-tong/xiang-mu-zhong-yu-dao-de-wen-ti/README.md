# 项目中遇到的问题

## day 01

### Question

* start tomcat, but some bug happened 

![](../../.gitbook/assets/image%20%2813%29.png)

### Answer

*  struts.xml里的&lt;!DOCTYPE struts PUBLIC "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN" "http://struts.apache.org/dtds/struts-2.5.dtd"&gt;  改成&lt;!DOCTYPE struts PUBLIC "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN" "http://struts.apache.org/dtds/struts-2.3.dtd"&gt; 就行了。因为我复制黏贴自己的项目的struts.xml文件时是2.5版本，而视频中给的是2.3。

## day05

### Question

* 通过hibernate插入中文出现？号的问题

首先检查table格式，最开始全是latin编码格式包括字段。

用

```text
<property name="jdbcUrl" value="${jdbc.jdbcUrl}?useUnicode=true&amp;characterEncoding=utf-8"/>
```

 方法后自动生成的表格编码格式变成utf8

并且能够插入中文。

### Question

如果页面获得中文数据后显示？？？号

则

```text
ServletActionContext.getResponse().setContentType("text/json;charset=utf-8");
```

###  Question

hql代码写错导致无执行getHibernateTemplate.find\(hql\);

```text
//此处from后面一定要加空格 不然会粘合在一起
        String hql = "from " + entityClass.getSimpleName();;

```

###  Question

{% hint style="info" %}
如何在表单中通过回车直接submit表单
{% endhint %}

通过在&lt;form&gt;元素上绑定submit事件，开发者可以监听到用户的提交表单的的行为

具体能触发submit事件的行为：

```text
    <input type="submit">
    <input type="image">
    <button type="submit">
```

当某些表单元素获取焦点时，敲击Enter（回车键）

上述这些操作下，都可以截获submit事件。  



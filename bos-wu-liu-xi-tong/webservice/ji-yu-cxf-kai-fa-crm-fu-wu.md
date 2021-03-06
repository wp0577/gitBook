# 基于cxf开发crm服务

## 1.数据库搭建

{% file src="../../.gitbook/assets/t\_customer.sql" caption="t\_customer.sql" %}

## 2.web项目环境搭建 

### 第一步：创建动态web项目 

### 第二步：导入CXF相关jar包

### 第三步：配置web.xml

{% code-tabs %}
{% code-tabs-item title="web.xml" %}
```text
 <context-param>
  	<param-name>contextConfigLocation</param-name>
  	<param-value>classpath:cxf.xml</param-value>
  </context-param>
  
  <listener>
  	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  
  <!-- 配置CXF框架提供的Servlet -->
  <servlet>
  	<servlet-name>cxf</servlet-name>
  	<servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>cxf</servlet-name>
  	<url-pattern>/service/*</url-pattern>
  </servlet-mapping>


```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 第四步：在类路径下提供cxf.xml

```text
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xmlns:jaxws="http://cxf.apache.org/jaxws"
xmlns:soap="http://cxf.apache.org/bindings/soap"
xsi:schemaLocation="http://www.springframework.org/schema/beans 
					http://www.springframework.org/schema/beans/spring-beans.xsd
					http://cxf.apache.org/bindings/soap 
					http://cxf.apache.org/schemas/configuration/soap.xsd
					http://cxf.apache.org/jaxws 
					http://cxf.apache.org/schemas/jaxws.xsd">
	<!-- 引入CXF Bean定义如下,早期的版本中使用 -->
	<import resource="classpath:META-INF/cxf/cxf.xml" />
	<import resource="classpath:META-INF/cxf/cxf-extension-soap.xml" />
	<import resource="classpath:META-INF/cxf/cxf-servlet.xml" />
</beans>

```

### 第五步：针对t\_customer表创建一个Customer客户实体类

![](../../.gitbook/assets/image%20%288%29.png)

{% hint style="info" %}
此处要注意ICustomerService后面不要接&lt;Customer&gt;的范型，因为在后面List&lt;Customer&gt;中的Customer将不会导入Customer类，这样发布出的webservice服务下载后来后将缺失Customer类导致调用getAll\(\)方法后得到null。
{% endhint %}

### 第六步：开发一个接口和实现类

![](../../.gitbook/assets/image%20%2860%29.png)

![](../../.gitbook/assets/image%20%2879%29.png)

### 第七步：配置cxf.xml

```text
<!-- 配置数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"/>
		<property name="url" value="jdbc:mysql:///crm_heima32"/>
		<property name="username" value="root"/>
		<property name="password" value="root"/>
	</bean>
	
	<!-- 事务管理器 -->
	<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>
	</bean>
	
	<!-- 支持事务注解 -->
	<tx:annotation-driven transaction-manager="txManager"/>
	
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="dataSource"/>
	</bean>
	
	<bean id="customerService" class="com.itheima.crm.service.CustomerServiceImpl">
		<property name="jdbcTemplate" ref="jdbcTemplate"/>
	</bean>
	
	<!-- 注册服务 -->
	<jaxws:server id="myService" address="/customer">
		<jaxws:serviceBean>
			<ref bean="customerService"/>
		</jaxws:serviceBean>
	</jaxws:server>

```

[http://localhost:8080/service/customer?wsdl](http://localhost:8080/service/customer?wsdl)查看说明文档

通过wsimport -s . [http://localhost:8080/service/customer?wsdl](http://localhost:8080/service/customer?wsdl) 指令下次access service方法，并调用

```text
ICustomerServiceImplService
```

 接口。


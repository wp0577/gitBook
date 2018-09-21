# 配置

### 1.1. 需要的jar包

1.     spring（包括springmvc）

2.     mybatis

3.     mybatis-spring整合包

4.     数据库驱动

5.     第三方连接池。

6.     Json依赖包Jackson

jar包位置如下图：

![](../../../.gitbook/assets/image%20%28113%29.png)

### 1.2. 整合思路

Dao层：

1、SqlMapConfig.xml，空文件即可，但是需要文件头。

2、applicationContext-dao.xml

a\)      数据库连接Druid

b\)      SqlSessionFactory对象，需要spring和mybatis整合包下的。

c\)      配置mapper文件扫描器。Mapper动态代理开发 增强版

Service层：

1、applicationContext-service.xml包扫描器，扫描@service注解的类。

2、applicationContext-trans.xml配置事务。

Controller层：

1、Springmvc.xml

a\)      包扫描器，扫描@Controller注解的类。

b\)      配置注解驱动

c\)      配置视图解析器

Web.xml文件：

1、配置spring监听器

2、配置前端控制器。

### 1.3. 创建工程

创建动态web工程，步骤如下图：

创建boot-crm，如下图

### 1.4. 加入jar包

加入课前资料中的jar包

### 1.5. 加入配置文件

创建config资源文件夹，在里面创建mybatis和spring文件夹

#### 1.5.1. SqlMapConfig.xml

空文件即可

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;

&lt;configuration&gt;

&lt;/configuration&gt;

#### 1.5.2. applicationContext-dao.xml

需要配置：

加载properties文件，数据源，SqlSessionFactory，Mapper扫描

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"

      xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"

      xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"

      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd

      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd

      http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd"&gt;

      &lt;!-- 配置 读取properties文件 jdbc.properties --&gt;

      &lt;context:property-placeholder location="classpath:jdbc.properties" /&gt;

      &lt;!-- 配置 数据源 --&gt;

      &lt;bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"&gt;

            &lt;property name="driverClassName" value="${jdbc.driver}" /&gt;

            &lt;property name="url" value="${jdbc.url}" /&gt;

            &lt;property name="username" value="${jdbc.username}" /&gt;

            &lt;property name="password" value="${jdbc.password}" /&gt;

      &lt;/bean&gt;

      &lt;!-- 配置SqlSessionFactory --&gt;

      &lt;bean class="org.mybatis.spring.SqlSessionFactoryBean"&gt;

            &lt;!-- 设置MyBatis核心配置文件 --&gt;

            &lt;property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml" /&gt;

            &lt;!-- 设置数据源 --&gt;

            &lt;property name="dataSource" ref="dataSource" /&gt;

      &lt;/bean&gt;

      &lt;!-- 配置Mapper扫描 --&gt;

      &lt;bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"&gt;

            &lt;!-- 设置Mapper扫描包 --&gt;

            &lt;property name="basePackage" value="cn.itcast.crm.mapper" /&gt;

      &lt;/bean&gt;

&lt;/beans&gt;

#### 1.5.3. jdbc.properties

配置数据库信息

jdbc.driver=com.mysql.jdbc.Driver

jdbc.url=jdbc:mysql://localhost:3306/crm?characterEncoding=utf-8

jdbc.username=root

jdbc.password=root

#### 1.5.4. log4j.properties

配置日志信息

\# Global logging configuration

log4j.rootLogger=DEBUG, stdout

\# Console output...

log4j.appender.stdout=org.apache.log4j.ConsoleAppender

log4j.appender.stdout.layout=org.apache.log4j.PatternLayout

log4j.appender.stdout.layout.ConversionPattern=%5p \[%t\] - %m%n

#### 1.5.5. applicationContext-service.xml

配置service扫描

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"

      xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"

      xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"

      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd

      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd

      http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd"&gt;

      &lt;!-- 配置Service扫描 --&gt;

      &lt;context:component-scan base-package="cn.itcast.crm.service" /&gt;

&lt;/beans&gt;

#### 1.5.6. applicationContext-trans.xml

配置事务管理：事务管理器、通知、切面

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"

      xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"

      xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"

      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd

      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd

      http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd"&gt;

      &lt;!-- 事务管理器 --&gt;

      &lt;bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"&gt;

            &lt;!-- 数据源 --&gt;

            &lt;property name="dataSource" ref="dataSource" /&gt;

      &lt;/bean&gt;

      &lt;!-- 通知 --&gt;

      &lt;tx:advice id="txAdvice" transaction-manager="transactionManager"&gt;

            &lt;tx:attributes&gt;

                  &lt;!-- 传播行为 --&gt;

                  &lt;tx:method name="save\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="insert\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="add\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="create\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="delete\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="update\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="find\*" propagation="SUPPORTS" read-only="true" /&gt;

                  &lt;tx:method name="select\*" propagation="SUPPORTS" read-only="true" /&gt;

                  &lt;tx:method name="get\*" propagation="SUPPORTS" read-only="true" /&gt;

                  &lt;tx:method name="query\*" propagation="SUPPORTS" read-only="true" /&gt;

            &lt;/tx:attributes&gt;

      &lt;/tx:advice&gt;

      &lt;!-- 切面 --&gt;

      &lt;aop:config&gt;

            &lt;aop:advisor advice-ref="txAdvice"

                  pointcut="execution\(\* cn.itcast.crm.service.\*.\*\(..\)\)" /&gt;

      &lt;/aop:config&gt;

&lt;/beans&gt;

#### 1.5.7. Springmvc.xml

配置SpringMVC表现层：Controller扫描、注解驱动、视图解析器

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"

      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"

      xmlns:context="http://www.springframework.org/schema/context"

      xmlns:mvc="http://www.springframework.org/schema/mvc"

      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd

        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd"&gt;

      &lt;!-- 配置Controller扫描 --&gt;

      &lt;context:component-scan base-package="cn.itcast.crm.controller" /&gt;

      &lt;!-- 配置注解驱动 --&gt;

      &lt;mvc:annotation-driven /&gt;

      &lt;!-- 配置视图解析器 --&gt;

      &lt;bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"&gt;

            &lt;!-- 前缀 --&gt;

            &lt;property name="prefix" value="/WEB-INF/jsp/" /&gt;

            &lt;!-- 后缀 --&gt;

            &lt;property name="suffix" value=".jsp" /&gt;

      &lt;/bean&gt;

&lt;/beans&gt;

#### 1.5.8. Web.xml

配置Spring、SpringMVC、解决post乱码问题

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xmlns="http://java.sun.com/xml/ns/javaee"

      xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app\_2\_5.xsd"

      id="WebApp\_ID" version="2.5"&gt;

      &lt;display-name&gt;boot-crm&lt;/display-name&gt;

      &lt;welcome-file-list&gt;

            &lt;welcome-file&gt;index.jsp&lt;/welcome-file&gt;

      &lt;/welcome-file-list&gt;

      &lt;!-- 配置spring --&gt;

      &lt;context-param&gt;

            &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;

            &lt;param-value&gt;classpath:spring/applicationContext-\*.xml&lt;/param-value&gt;

      &lt;/context-param&gt;

      &lt;!-- 配置监听器加载spring --&gt;

      &lt;listener&gt;

            &lt;listener-class&gt;org.springframework.web.context.ContextLoaderListener&lt;/listener-class&gt;

      &lt;/listener&gt;

      &lt;!-- 配置过滤器，解决post的乱码问题 --&gt;

      &lt;filter&gt;

            &lt;filter-name&gt;encoding&lt;/filter-name&gt; &lt;filter-class&gt;org.springframework.web.filter.CharacterEncodingFilter&lt;/filter-class&gt;

      &lt;/filter&gt;

      &lt;filter-mapping&gt;

            &lt;filter-name&gt;encoding&lt;/filter-name&gt;

            &lt;url-pattern&gt;/\*&lt;/url-pattern&gt;

      &lt;/filter-mapping&gt;

      &lt;!-- 配置SpringMVC --&gt;

      &lt;servlet&gt;

            &lt;servlet-name&gt;boot-crm&lt;/servlet-name&gt;

            &lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;

            &lt;init-param&gt;

                  &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;

                  &lt;param-value&gt;classpath:spring/springmvc.xml&lt;/param-value&gt;

            &lt;/init-param&gt;

            &lt;!-- 配置springmvc什么时候启动，参数必须为整数 --&gt;

            &lt;!-- 如果为0或者大于0，则springMVC随着容器启动而启动 --&gt;

            &lt;!-- 如果小于0，则在第一次请求进来的时候启动 --&gt;

            &lt;load-on-startup&gt;1&lt;/load-on-startup&gt;

      &lt;/servlet&gt;

      &lt;servlet-mapping&gt;

            &lt;servlet-name&gt;boot-crm&lt;/servlet-name&gt;

            &lt;!-- 所有的请求都进入springMVC --&gt;

            &lt;url-pattern&gt;/&lt;/url-pattern&gt;

      &lt;/servlet-mapping&gt;

&lt;/web-app&gt;

### 1.6. 加入静态资源

最终效果如下图：

![](../../../.gitbook/assets/image%20%2849%29.png)


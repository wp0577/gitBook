# 整合Mybatis

为了更好的学习 springmvc和mybatis整合开发的方法，需要将springmvc和mybatis进行整合。

整合目标：控制层采用springmvc、持久层使用mybatis实现。

### 1.1. 创建数据库表

sql脚本，位置如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

创建数据库表springmvc，导入到数据库中，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

### 1.2. 需要的jar包

1.     spring（包括springmvc）

2.     mybatis

3.     mybatis-spring整合包

4.     数据库驱动

5.     第三方连接池。

jar包位置如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg)

### 1.3. 整合思路

Dao层：

1、SqlMapConfig.xml，空文件即可，但是需要文件头。

2、applicationContext-dao.xml

a\)      数据库连接池

b\)      SqlSessionFactory对象，需要spring和mybatis整合包下的。

c\)      配置mapper文件扫描器。

Service层：

1、applicationContext-service.xml包扫描器，扫描@service注解的类。

2、applicationContext-trans.xml配置事务。

Controller层：

1、Springmvc.xml

a\)      包扫描器，扫描@Controller注解的类。

b\)      配置注解驱动

c\)      配置视图解析器

Web.xml文件：

1、配置spring

2、配置前端控制器。

### 1.4. 创建工程

创建动态web工程springmvc-web，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

### 1.5. 加入jar包

复制jar包到/WEB-INF/lib中

工程自动加载jar包

### 1.6. 加入配置文件

创建资源文件夹config

在其下创建mybatis和spring文件夹，用来存放配置文件，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image012.jpg)

#### 1.6.1. sqlMapConfig.xml

使用逆向工程来生成Mapper相关代码，不需要配置别名。

在config/mybatis下创建SqlMapConfig.xml

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;!DOCTYPE configuration

PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;

&lt;configuration&gt;

&lt;/configuration&gt;

#### 1.6.2. applicationContext-dao.xml

配置数据源、配置SqlSessionFactory、mapper扫描器。

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"

      xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"

      xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"

      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd

      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd

      http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd"&gt;

      &lt;!-- 加载配置文件 --&gt;

      &lt;context:property-placeholder location="classpath:db.properties" /&gt;

      &lt;!-- 数据库连接池 --&gt;

      &lt;bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"

            destroy-method="close"&gt;

            &lt;property name="driverClassName" value="${jdbc.driver}" /&gt;

            &lt;property name="url" value="${jdbc.url}" /&gt;

            &lt;property name="username" value="${jdbc.username}" /&gt;

            &lt;property name="password" value="${jdbc.password}" /&gt;

            &lt;property name="maxActive" value="10" /&gt;

            &lt;property name="maxIdle" value="5" /&gt;

      &lt;/bean&gt;

      &lt;!-- 配置SqlSessionFactory --&gt;

      &lt;bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"&gt;

            &lt;!-- 数据库连接池 --&gt;

            &lt;property name="dataSource" ref="dataSource" /&gt;

            &lt;!-- 加载mybatis的全局配置文件 --&gt;

            &lt;property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml" /&gt;

      &lt;/bean&gt;

      &lt;!-- 配置Mapper扫描 --&gt;

      &lt;bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"&gt;

            &lt;!-- 配置Mapper扫描包 --&gt;

            &lt;property name="basePackage" value="cn.itcast.ssm.mapper" /&gt;

      &lt;/bean&gt;

&lt;/beans&gt;

#### 1.6.3. db.properties

配置数据库相关信息

jdbc.driver=com.mysql.jdbc.Driver

jdbc.url=jdbc:mysql://localhost:3306/springmvc?characterEncoding=utf-8

jdbc.username=root

jdbc.password=root

#### 1.6.4. applicationContext-service.xml

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"

      xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"

      xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"

      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd

      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd

      http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd"&gt;

      &lt;!-- 配置service扫描 --&gt;

      &lt;context:component-scan base-package="cn.itcast.ssm.service" /&gt;

&lt;/beans&gt;

#### 1.6.5. applicationContext-trans.xml

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

      &lt;bean id="transactionManager"

            class="org.springframework.jdbc.datasource.DataSourceTransactionManager"&gt;

            &lt;!-- 数据源 --&gt;

            &lt;property name="dataSource" ref="dataSource" /&gt;

      &lt;/bean&gt;

      &lt;!-- 通知 --&gt;

      &lt;tx:advice id="txAdvice" transaction-manager="transactionManager"&gt;

            &lt;tx:attributes&gt;

                  &lt;!-- 传播行为 --&gt;

                  &lt;tx:method name="save\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="insert\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="delete\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="update\*" propagation="REQUIRED" /&gt;

                  &lt;tx:method name="find\*" propagation="SUPPORTS" read-only="true" /&gt;

                  &lt;tx:method name="get\*" propagation="SUPPORTS" read-only="true" /&gt;

                  &lt;tx:method name="query\*" propagation="SUPPORTS" read-only="true" /&gt;

            &lt;/tx:attributes&gt;

      &lt;/tx:advice&gt;

      &lt;!-- 切面 --&gt;

      &lt;aop:config&gt;

            &lt;aop:advisor advice-ref="txAdvice"

                  pointcut="execution\(\* cn.itcast.ssm.service.\*.\*\(..\)\)" /&gt;

      &lt;/aop:config&gt;

&lt;/beans&gt;

#### 1.6.6. springmvc.xml

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"

      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"

      xmlns:context="http://www.springframework.org/schema/context"

      xmlns:mvc="http://www.springframework.org/schema/mvc"

      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd

        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd"&gt;

      &lt;!-- 配置controller扫描包 --&gt;

      &lt;context:component-scan base-package="cn.itcast.ssm.controller" /&gt;

      &lt;!-- 注解驱动 --&gt;

      &lt;mvc:annotation-driven /&gt;

      &lt;!-- Example: prefix="/WEB-INF/jsp/", suffix=".jsp", viewname="test" -&gt;

            "/WEB-INF/jsp/test.jsp" --&gt;

      &lt;!-- 配置视图解析器 --&gt;

      &lt;bean

      class="org.springframework.web.servlet.view.InternalResourceViewResolver"&gt;

            &lt;!-- 配置逻辑视图的前缀 --&gt;

            &lt;property name="prefix" value="/WEB-INF/jsp/" /&gt;

            &lt;!-- 配置逻辑视图的后缀 --&gt;

            &lt;property name="suffix" value=".jsp" /&gt;

      &lt;/bean&gt;

&lt;/beans&gt;

#### 1.6.7. web.xml

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xmlns="http://java.sun.com/xml/ns/javaee"

      xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app\_2\_5.xsd"

      id="WebApp\_ID" version="2.5"&gt;

      &lt;display-name&gt;springmvc-web&lt;/display-name&gt;

      &lt;welcome-file-list&gt;

            &lt;welcome-file&gt;index.html&lt;/welcome-file&gt;

            &lt;welcome-file&gt;index.htm&lt;/welcome-file&gt;

            &lt;welcome-file&gt;index.jsp&lt;/welcome-file&gt;

            &lt;welcome-file&gt;default.html&lt;/welcome-file&gt;

            &lt;welcome-file&gt;default.htm&lt;/welcome-file&gt;

            &lt;welcome-file&gt;default.jsp&lt;/welcome-file&gt;

      &lt;/welcome-file-list&gt;

      &lt;!-- 配置spring --&gt;

      &lt;context-param&gt;

            &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;

            &lt;param-value&gt;classpath:spring/applicationContext\*.xml&lt;/param-value&gt;

      &lt;/context-param&gt;

      &lt;!-- 使用监听器加载Spring配置文件 --&gt;

      &lt;listener&gt;

            &lt;listener-class&gt;org.springframework.web.context.ContextLoaderListener&lt;/listener-class&gt;

      &lt;/listener&gt;

      &lt;!-- 配置SrpingMVC的前端控制器 --&gt;

      &lt;servlet&gt;

            &lt;servlet-name&gt;springmvc-web&lt;/servlet-name&gt;

            &lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;

            &lt;init-param&gt;

                  &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;

                  &lt;param-value&gt;classpath:spring/springmvc.xml&lt;/param-value&gt;

            &lt;/init-param&gt;

      &lt;/servlet&gt;

      &lt;servlet-mapping&gt;

            &lt;servlet-name&gt;springmvc-web&lt;/servlet-name&gt;

            &lt;!-- 配置所有以action结尾的请求进入SpringMVC --&gt;

            &lt;url-pattern&gt;\*.action&lt;/url-pattern&gt;

      &lt;/servlet-mapping&gt;

&lt;/web-app&gt;

### 1.7. 加入jsp页面

复制课前资料的itemList.jsp和itemEdit.jsp到工程中

### 1.8. 效果

配置完效果如下图：


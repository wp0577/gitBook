# 环境搭建

## 导包

![](../../../.gitbook/assets/image%20%28186%29.png)

## 加入配置文件

{% code-tabs %}
{% code-tabs-item title="log4j.properties" %}
```text
在config下创建log4j.properties如下：
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="SqlMapConfig.xml" %}
```text
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 和spring整合后 environments配置将废除 -->
	<environments default="development">
		<environment id="development">
			<!-- 使用jdbc事务管理 -->
			<transactionManager type="JDBC" />
			<!-- 数据库连接池 -->
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url"
					value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8" />
				<property name="username" value="root" />
				<property name="password" value="root" />
			</dataSource>
		</environment>
	</environments>
</configuration>


```
{% endcode-tabs-item %}
{% endcode-tabs %}

SqlMapConfig.xml是mybatis核心配置文件，配置文件内容为数据源、事务管理。

## 创建pojo

![](../../../.gitbook/assets/image%20%2861%29.png)

```text
Public class User {
	private int id;
	private String username;// 用户姓名
	private String sex;// 性别
	private Date birthday;// 生日
	private String address;// 地址


get/set……

```

## sql映射文件

```text
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace：命名空间，用于隔离sql，还有一个很重要的作用，后面会讲 -->
<mapper namespace="test">
</mapper>

```

## 加载映射文件

mybatis框架需要加载Mapper.xml映射文件 将users.xml添加在SqlMapConfig.xml，如下：

![](../../../.gitbook/assets/image%20%2870%29.png)


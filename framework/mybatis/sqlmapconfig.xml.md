# SqlMapConfig.xml



1.1. 配置内容

SqlMapConfig.xml中配置的内容和顺序如下：

**properties**（属性）

settings（全局配置参数）

**typeAliases**（类型别名）

typeHandlers（类型处理器）

objectFactory（对象工厂）

plugins（插件）

environments（环境集合属性对象）

environment（环境子属性对象）

transactionManager（事务管理）

dataSource（数据源）

**mappers**（映射器）

### 1.2. properties（属性）

SqlMapConfig.xml可以引用java属性文件中的配置信息如下：

在config下定义db.properties文件，如下所示：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

db.properties配置文件内容如下：

jdbc.driver=com.mysql.jdbc.Driver

jdbc.url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8

jdbc.username=root

jdbc.password=root

SqlMapConfig.xml引用如下：

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE configuration

PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;

&lt;configuration&gt;

      &lt;!-- 是用resource属性加载外部配置文件 --&gt;

      &lt;properties resource="db.properties"&gt;

            &lt;!-- 在properties内部用property定义属性 --&gt;

            &lt;!-- 如果外部配置文件有该属性，则内部定义属性被外部属性覆盖 --&gt;

            &lt;property name="jdbc.username" value="root123" /&gt;

            &lt;property name="jdbc.password" value="root123" /&gt;

      &lt;/properties&gt;

      &lt;!-- 和spring整合后 environments配置将废除 --&gt;

      &lt;environments default="development"&gt;

            &lt;environment id="development"&gt;

                  &lt;!-- 使用jdbc事务管理 --&gt;

                  &lt;transactionManager type="JDBC" /&gt;

                  &lt;!-- 数据库连接池 --&gt;

                  &lt;dataSource type="POOLED"&gt;

                        &lt;property name="driver" value="${jdbc.driver}" /&gt;

                        &lt;property name="url" value="${jdbc.url}" /&gt;

                        &lt;property name="username" value="${jdbc.username}" /&gt;

                        &lt;property name="password" value="${jdbc.password}" /&gt;

                  &lt;/dataSource&gt;

            &lt;/environment&gt;

      &lt;/environments&gt;

      &lt;!-- 加载映射文件 --&gt;

      &lt;mappers&gt;

            &lt;mapper resource="sqlmap/User.xml" /&gt;

            &lt;mapper resource="mapper/UserMapper.xml" /&gt;

      &lt;/mappers&gt;

&lt;/configuration&gt;

注意： MyBatis 将按照下面的顺序来加载属性：

u  在 properties 元素体内定义的属性首先被读取。

u  然后会读取properties 元素中resource或 url 加载的属性，它会覆盖已读取的同名属性。

### 1.3. typeAliases（类型别名）

#### 1.3.1. mybatis支持别名：

| 别名 | 映射的类型 |
| :--- | :--- |
| \_byte | byte |
| \_long | long |
| \_short | short |
| \_int | int |
| \_integer | int |
| \_double | double |
| \_float | float |
| \_boolean | boolean |
| string | String |
| byte | Byte |
| long | Long |
| short | Short |
| int | Integer |
| integer | Integer |
| double | Double |
| float | Float |
| boolean | Boolean |
| date | Date |
| decimal | BigDecimal |
| bigdecimal | BigDecimal |
| map | Map |

#### 1.3.2. 自定义别名：

在SqlMapConfig.xml中配置如下：

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE configuration

PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;

&lt;configuration&gt;

      &lt;!-- 是用resource属性加载外部配置文件 --&gt;

      &lt;properties resource="db.properties"&gt;

            &lt;!-- 在properties内部用property定义属性 --&gt;

            &lt;property name="jdbc.username" value="root123" /&gt;

            &lt;property name="jdbc.password" value="root123" /&gt;

      &lt;/properties&gt;

      &lt;typeAliases&gt;

            &lt;!-- 单个别名定义 --&gt;

            &lt;typeAlias alias="user" type="cn.itcast.mybatis.pojo.User" /&gt;

            &lt;!-- 批量别名定义，扫描整个包下的类，别名为类名（大小写不敏感） --&gt;

            &lt;package name="cn.itcast.mybatis.pojo" /&gt;

            &lt;package name="其它包" /&gt;

      &lt;/typeAliases&gt;

      &lt;!-- 和spring整合后 environments配置将废除 --&gt;

      &lt;environments default="development"&gt;

            &lt;environment id="development"&gt;

                  &lt;!-- 使用jdbc事务管理 --&gt;

                  &lt;transactionManager type="JDBC" /&gt;

                  &lt;!-- 数据库连接池 --&gt;

                  &lt;dataSource type="POOLED"&gt;

                        &lt;property name="driver" value="${jdbc.driver}" /&gt;

                        &lt;property name="url" value="${jdbc.url}" /&gt;

                        &lt;property name="username" value="${jdbc.username}" /&gt;

                        &lt;property name="password" value="${jdbc.password}" /&gt;

                  &lt;/dataSource&gt;

            &lt;/environment&gt;

      &lt;/environments&gt;

      &lt;!-- 加载映射文件 --&gt;

      &lt;mappers&gt;

            &lt;mapper resource="sqlmap/User.xml" /&gt;

            &lt;mapper resource="mapper/UserMapper.xml" /&gt;

      &lt;/mappers&gt;

&lt;/configuration&gt;

在mapper.xml配置文件中，就可以使用设置的别名了

别名大小写不敏感

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

### 1.4. mappers（映射器）

Mapper配置的几种方法：

#### 1.4.1. &lt;mapper resource=" " /&gt;

使用相对于类路径的资源（现在的使用方式）

如：&lt;mapper resource="sqlmap/User.xml" /&gt;

#### 1.4.2. &lt;mapper class=" " /&gt;

使用mapper接口类路径

如：&lt;mapper class="cn.itcast.mybatis.mapper.UserMapper"/&gt;

注意：此种方法要求mapper接口名称和mapper映射文件名称相同，且放在同一个目录中。

#### 1.4.3. &lt;package name=""/&gt;

注册指定包下的所有mapper接口

如：&lt;package name="cn.itcast.mybatis.mapper"/&gt;

注意：此种方法要求mapper接口名称和mapper映射文件名称相同，且放在同一个目录中。


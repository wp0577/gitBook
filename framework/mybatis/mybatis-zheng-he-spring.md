# Mybatis整合spring

## 1.  Mybatis整合spring

### 1.1. 整合思路

1、SqlSessionFactory对象应该放到spring容器中作为单例存在。

2、传统dao的开发方式中，应该从spring容器中获得sqlsession对象。

3、Mapper代理形式中，应该从spring容器中直接获得mapper的代理对象。

4、数据库的连接以及数据库连接池事务管理都交给spring容器来完成。

### 1.2. 整合需要的jar包

1、spring的jar包

2、Mybatis的jar包

3、Spring+mybatis的整合包。

4、Mysql的数据库驱动jar包。

5、数据库连接池的jar包。

jar包位置如下所示：

![](../../.gitbook/assets/image%20%286%29.png)

### 1.3. 整合的步骤

#### 1.3.1. 创建工程

如下图创建一个java工程：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

#### 1.3.2. 导入jar包

前面提到的jar包需要导入，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg)

#### 1.3.3. 加入配置文件

1. mybatisSpring的配置文件

2. 的配置文件sqlmapConfig.xml

a\)      数据库连接及连接池

b\)      事务管理（暂时可以不配置）

c\)      sqlsessionFactory对象，配置到spring容器中

d\)      mapeer代理对象或者是dao实现类配置到spring容器中。

创建资源文件夹config拷贝加入配置文件，如下图

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)

**1.3.3.1. SqlMapConfig.xml**

配置文件是SqlMapConfig.xml，如下：

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE configuration

PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-config.dtd"&gt;

&lt;configuration&gt;

      &lt;!-- 设置别名 --&gt;

      &lt;typeAliases&gt;

            &lt;!-- 2. 指定扫描包，会把包内所有的类都设置别名，别名的名称就是类名，大小写不敏感 --&gt;

            &lt;package name="cn.itcast.mybatis.pojo" /&gt;

      &lt;/typeAliases&gt;

&lt;/configuration&gt;

**1.3.3.2. applicationContext.xml**

SqlSessionFactoryBean属于mybatis-spring这个jar包

对于spring来说，mybatis是另外一个架构，需要整合jar包。

在项目中加入mybatis-spring-1.2.2.jar的源码，如下图

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image012.jpg)

效果，如下图所示，图标变化，表示源码加载成功：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image014.jpg)

整合Mybatis需要的是SqlSessionFactoryBean，位置如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image016.jpg)

applicationContext.xml，配置内容如下

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

            &lt;!-- 配置mybatis核心配置文件 --&gt;

            &lt;property name="configLocation" value="classpath:SqlMapConfig.xml" /&gt;

            &lt;!-- 配置数据源 --&gt;

            &lt;property name="dataSource" ref="dataSource" /&gt;

      &lt;/bean&gt;

&lt;/beans&gt;

**1.3.3.3. db.properties**

jdbc.driver=com.mysql.jdbc.Driver

jdbc.url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8

jdbc.username=root

jdbc.password=root

**1.3.3.4. log4j.properties**

\# Global logging configuration

log4j.rootLogger=DEBUG, stdout

\# Console output...

log4j.appender.stdout=org.apache.log4j.ConsoleAppender

log4j.appender.stdout.layout=org.apache.log4j.PatternLayout

log4j.appender.stdout.layout.ConversionPattern=%5p \[%t\] - %m%n

**1.3.3.5. 效果：**

加入的配置文件最终效果如下：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image018.jpg)

### 1.4. Dao的开发

两种dao的实现方式：

1、原始dao的开发方式

2、使用Mapper代理形式开发方式

a\)      直接配置Mapper代理

b\)      使用扫描包配置Mapper代理

需求：

1. 实现根据用户id查询

2. 实现根据用户名模糊查询

3. 添加用户

#### 1.4.1. 创建pojo

**public** **class** User {

      **private** **int** id;

      **private** String username;// 用户姓名

      **private** String sex;// 性别

      **private** Date birthday;// 生日

      **private** String address;// 地址

get/set。。。

}

#### 1.4.2. 传统dao的开发方式

原始的DAO开发接口+实现类来完成。

需要dao实现类需要继承SqlsessionDaoSupport类

**1.4.2.1. 实现Mapper.xml**

编写User.xml配置文件，如下：

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;mapper namespace="test"&gt;

      &lt;!-- 根据用户id查询 --&gt;

      &lt;select id="queryUserById" parameterType="int" resultType="user"&gt;

            select \* from user where id = \#{id}

      &lt;/select&gt;

      &lt;!-- 根据用户名模糊查询用户 --&gt;

      &lt;select id="queryUserByUsername" parameterType="string"

            resultType="user"&gt;

            select \* from user where username like '%${value}%'

      &lt;/select&gt;

      &lt;!-- 添加用户 --&gt;

      &lt;insert id="saveUser" parameterType="user"&gt;

            &lt;selectKey keyProperty="id" keyColumn="id" order="AFTER"

                  resultType="int"&gt;

                  select last\_insert\_id\(\)

            &lt;/selectKey&gt;

            insert into user

            \(username,birthday,sex,address\)

            values

            \(\#{username},\#{birthday},\#{sex},\#{address}\)

      &lt;/insert&gt;

&lt;/mapper&gt;

**1.4.2.2. 加载Mapper.xml**

在SqlMapConfig如下图进行配置:

![](../../.gitbook/assets/image%20%2884%29.png)

**1.4.2.3. 实现UserDao接口**

**public** **interface** UserDao {

      /\*\*

       \* 根据id查询用户

       \*

       \* **@param** id

       \* **@return**

       \*/

      User queryUserById\(**int** id\);

      /\*\*

       \* 根据用户名模糊查询用户列表

       \*

       \* **@param** username

       \* **@return**

       \*/

      List&lt;User&gt; queryUserByUsername\(String username\);

      /\*\*

       \* 保存

       \*

       \* **@param** user

       \*/

      **void** saveUser\(User user\);

}

**1.4.2.4. 实现UserDaoImpl实现类**

编写DAO实现类，实现类必须集成SqlSessionDaoSupport

SqlSessionDaoSupport提供getSqlSession\(\)方法来获取SqlSession

**public** **class** UserDaoImpl **extends** SqlSessionDaoSupport **implements** UserDao {

      @Override

      **public** User queryUserById\(**int** id\) {

            // 获取SqlSession

            SqlSession sqlSession = **super**.getSqlSession\(\);

            // 使用SqlSession执行操作

            User user = sqlSession.selectOne\("queryUserById", id\);

            // 不要关闭sqlSession

            **return** user;

      }

      @Override

      **public** List&lt;User&gt; queryUserByUsername\(String username\) {

            // 获取SqlSession

            SqlSession sqlSession = **super**.getSqlSession\(\);

            // 使用SqlSession执行操作

            List&lt;User&gt; list = sqlSession.selectList\("queryUserByUsername", username\);

            // 不要关闭sqlSession

            **return** list;

      }

      @Override

      **public** **void** saveUser\(User user\) {

            // 获取SqlSession

            SqlSession sqlSession = **super**.getSqlSession\(\);

            // 使用SqlSession执行操作

            sqlSession.insert\("saveUser", user\);

            // 不用提交,事务由spring进行管理

            // 不要关闭sqlSession

      }

}

**1.4.2.5. 配置dao**

把dao实现类配置到spring容器中，如下图

![](../../.gitbook/assets/image%20%28229%29.png)

**1.4.2.6. 测试方法**

创建测试方法，可以直接创建测试Junit用例。

如下图所示进行创建。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image024.jpg)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image026.jpg)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image028.jpg)

编写测试方法如下：

**public** **class** UserDaoTest {

      **private** ApplicationContext context;

      @Before

      **public** **void** setUp\(\) **throws** Exception {

            **this**.context = **new** ClassPathXmlApplicationContext\("classpath:applicationContext.xml"\);

      }

      @Test

      **public** **void** testQueryUserById\(\) {

            // 获取userDao

            UserDao userDao = **this**.context.getBean\(UserDao.**class**\);

            User user = userDao.queryUserById\(1\);

            System.**out**.println\(user\);

      }

      @Test

      **public** **void** testQueryUserByUsername\(\) {

            // 获取userDao

            UserDao userDao = **this**.context.getBean\(UserDao.**class**\);

            List&lt;User&gt; list = userDao.queryUserByUsername\("张"\);

            **for** \(User user : list\) {

                  System.**out**.println\(user\);

            }

      }

      @Test

      **public** **void** testSaveUser\(\) {

            // 获取userDao

            UserDao userDao = **this**.context.getBean\(UserDao.**class**\);

            User user = **new** User\(\);

            user.setUsername\("曹操"\);

            user.setSex\("1"\);

            user.setBirthday\(**new** Date\(\)\);

            user.setAddress\("三国"\);

            userDao.saveUser\(user\);

            System.**out**.println\(user\);

      }

}

#### 1.4.3. Mapper代理形式开发dao

**1.4.3.1. 实现Mapper.xml**

编写UserMapper.xml配置文件，如下：

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;mapper namespace="cn.itcast.mybatis.mapper.UserMapper"&gt;

      &lt;!-- 根据用户id查询 --&gt;

      &lt;select id="queryUserById" parameterType="int" resultType="user"&gt;

            select \* from user where id = \#{id}

      &lt;/select&gt;

      &lt;!-- 根据用户名模糊查询用户 --&gt;

      &lt;select id="queryUserByUsername" parameterType="string"

            resultType="user"&gt;

            select \* from user where username like '%${value}%'

      &lt;/select&gt;

      &lt;!-- 添加用户 --&gt;

      &lt;insert id="saveUser" parameterType="user"&gt;

            &lt;selectKey keyProperty="id" keyColumn="id" order="AFTER"

                  resultType="int"&gt;

                  select last\_insert\_id\(\)

            &lt;/selectKey&gt;

            insert into user

            \(username,birthday,sex,address\) values

            \(\#{username},\#{birthday},\#{sex},\#{address}\)

      &lt;/insert&gt;

&lt;/mapper&gt;

**1.4.3.2. 实现UserMapper接口**

**public** **interface** UserMapper {

      /\*\*

       \* 根据用户id查询

       \*

       \* **@param** id

       \* **@return**

       \*/

      User queryUserById\(**int** id\);

      /\*\*

       \* 根据用户名模糊查询用户

       \*

       \* **@param** username

       \* **@return**

       \*/

      List&lt;User&gt; queryUserByUsername\(String username\);

      /\*\*

       \* 添加用户

       \*

       \* **@param** user

       \*/

      **void** saveUser\(User user\);

}

**1.4.3.3. 方式一：配置mapper代理**

在applicationContext.xml添加配置

MapperFactoryBean也是属于mybatis-spring整合包

&lt;!-- Mapper代理的方式开发方式一，配置Mapper代理对象 --&gt;

&lt;bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean"&gt;

      &lt;!-- 配置Mapper接口 --&gt;

      &lt;property name="mapperInterface" value="cn.itcast.mybatis.mapper.UserMapper" /&gt;

      &lt;!-- 配置sqlSessionFactory --&gt;

      &lt;property name="sqlSessionFactory" ref="sqlSessionFactory" /&gt;

&lt;/bean&gt;

**1.4.3.4. 测试方法**

**public** **class** UserMapperTest {

      **private** ApplicationContext context;

      @Before

      **public** **void** setUp\(\) **throws** Exception {

            **this**.context = **new** ClassPathXmlApplicationContext\("classpath:applicationContext.xml"\);

      }

      @Test

      **public** **void** testQueryUserById\(\) {

            // 获取Mapper

            UserMapper userMapper = **this**.context.getBean\(UserMapper.**class**\);

            User user = userMapper.queryUserById\(1\);

            System.**out**.println\(user\);

      }

      @Test

      **public** **void** testQueryUserByUsername\(\) {

            // 获取Mapper

            UserMapper userMapper = **this**.context.getBean\(UserMapper.**class**\);

            List&lt;User&gt; list = userMapper.queryUserByUsername\("张"\);

            **for** \(User user : list\) {

                  System.**out**.println\(user\);

            }

      }

      @Test

      **public** **void** testSaveUser\(\) {

            // 获取Mapper

            UserMapper userMapper = **this**.context.getBean\(UserMapper.**class**\);

            User user = **new** User\(\);

            user.setUsername\("曹操"\);

            user.setSex\("1"\);

            user.setBirthday\(**new** Date\(\)\);

            user.setAddress\("三国"\);

            userMapper.saveUser\(user\);

            System.**out**.println\(user\);

      }

}

**1.4.3.5. 方式二：扫描包形式配置mapper**

&lt;!-- Mapper代理的方式开发方式二，扫描包方式配置代理 --&gt;

&lt;bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"&gt;

      &lt;!-- 配置Mapper接口 --&gt;

      &lt;property name="basePackage" value="cn.itcast.mybatis.mapper" /&gt;

&lt;/bean&gt;

每个mapper代理对象的id就是类名，首字母小写


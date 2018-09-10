---
description: >-
  Mapper接口开发方法只需要程序员编写Mapper接口（相当于Dao接口），由Mybatis框架根据接口定义创建接口的动态代理对象，代理对象的方法体同上边Dao接口实现类方法。
---

# Mapper动态代理方式

Mapper接口开发需要遵循以下规范：

1、         Mapper.xml文件中的namespace与mapper接口的类路径相同。

2、         Mapper接口方法名和Mapper.xml中定义的每个statement的id相同

3、         Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同

4、         Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同

#### 1.1.1. Mapper.xml\(映射文件\)

            定义mapper映射文件UserMapper.xml

将UserMapper.xml放在config下mapper目录下，效果如下：

![](../../../.gitbook/assets/image%20%2849%29.png)

UserMapper.xml配置文件内容：

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;!-- namespace：命名空间，用于隔离sql --&gt;

&lt;!-- 还有一个很重要的作用，使用动态代理开发DAO，1. namespace必须和Mapper接口类路径一致 --&gt;

&lt;mapper namespace="cn.itcast.mybatis.mapper.UserMapper"&gt;

      &lt;!-- 根据用户id查询用户 --&gt;

      &lt;!-- 2. id必须和Mapper接口方法名一致 --&gt;

      &lt;!-- 3. parameterType必须和接口方法参数类型一致 --&gt;

      &lt;!-- 4. resultType必须和接口方法返回值类型一致 --&gt;

      &lt;select id="queryUserById" parameterType="int"

            resultType="cn.itcast.mybatis.pojo.User"&gt;

            select \* from user where id = \#{id}

      &lt;/select&gt;

      &lt;!-- 根据用户名查询用户 --&gt;

      &lt;select id="queryUserByUsername" parameterType="string"

            resultType="cn.itcast.mybatis.pojo.User"&gt;

            select \* from user where username like '%${value}%'

      &lt;/select&gt;

      &lt;!-- 保存用户 --&gt;

      &lt;insert id="saveUser" parameterType="cn.itcast.mybatis.pojo.User"&gt;

            &lt;selectKey keyProperty="id" keyColumn="id" order="AFTER"

                  resultType="int"&gt;

                  select last\_insert\_id\(\)

            &lt;/selectKey&gt;

            insert into user\(username,birthday,sex,address\) values

            \(\#{username},\#{birthday},\#{sex},\#{address}\);

      &lt;/insert&gt;

&lt;/mapper&gt;

#### 1.1.2. UserMapper\(接口文件\)

创建UserMapper接口代码如下：

**public** **interface** UserMapper {

      /\*\*

       \* 根据id查询

       \*

       \* **@param** id

       \* **@return**

       \*/

      User queryUserById\(**int** id\);

      /\*\*

       \* 根据用户名查询用户

       \*

       \* **@param** username

       \* **@return**

       \*/

      List&lt;User&gt; queryUserByUsername\(String username\);

      /\*\*

       \* 保存用户

       \*

       \* **@param** user

       \*/

      **void** saveUser\(User user\);

}

#### 1.1.3. 加载UserMapper.xml文件

修改SqlMapConfig.xml文件，添加以下所示的内容：

      &lt;!-- 加载映射文件 --&gt;

      &lt;mappers&gt;

            &lt;mapper resource="sqlmap/User.xml" /&gt;

            &lt;mapper resource="mapper/UserMapper.xml" /&gt;

      &lt;/mappers&gt;

#### 1.1.4. 测试

编写的测试方法如下：

**public** **class** UserMapperTest {

      **private** SqlSessionFactory sqlSessionFactory;

      @Before

      **public** **void** init\(\) **throws** Exception {

            // 创建SqlSessionFactoryBuilder

            SqlSessionFactoryBuilder sqlSessionFactoryBuilder = **new** SqlSessionFactoryBuilder\(\);

            // 加载SqlMapConfig.xml配置文件

            InputStream inputStream = Resources.getResourceAsStream\("SqlMapConfig.xml"\);

            // 创建SqlsessionFactory

            **this**.sqlSessionFactory = sqlSessionFactoryBuilder.build\(inputStream\);

      }

      @Test

      **public** **void** testQueryUserById\(\) {

            // 获取sqlSession，和spring整合后由spring管理

            SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

            // 从sqlSession中获取Mapper接口的代理对象

            UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

            // 执行查询方法

            User user = userMapper.queryUserById\(1\);

            System.**out**.println\(user\);

            // 和spring整合后由spring管理

            sqlSession.close\(\);

      }

      @Test

      **public** **void** testQueryUserByUsername\(\) {

            // 获取sqlSession，和spring整合后由spring管理

            SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

            // 从sqlSession中获取Mapper接口的代理对象

            UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

            // 执行查询方法

            List&lt;User&gt; list = userMapper.queryUserByUsername\("张"\);

            **for** \(User user : list\) {

                  System.**out**.println\(user\);

            }

            // 和spring整合后由spring管理

            sqlSession.close\(\);

      }

      @Test

      **public** **void** testSaveUser\(\) {

            // 获取sqlSession，和spring整合后由spring管理

            SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

            // 从sqlSession中获取Mapper接口的代理对象

            UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

            // 创建保存对象

            User user = **new** User\(\);

            user.setUsername\("刘备"\);

            user.setBirthday\(**new** Date\(\)\);

            user.setSex\("1"\);

            user.setAddress\("蜀国"\);

            // 执行查询方法

            userMapper.saveUser\(user\);

            System.**out**.println\(user\);

            // 和spring整合后由spring管理

            sqlSession.commit\(\);

            sqlSession.close\(\);

      }

}

#### 1.1.5. 小结

u  selectOne和selectList

动态代理对象调用sqlSession.selectOne\(\)和sqlSession.selectList\(\)是根据mapper接口方法的返回值决定，如果返回list则调用selectList方法，如果返回单个对象则调用selectOne方法。

u  namespace

mybatis官方推荐使用mapper代理方法开发mapper接口，程序员不用编写mapper接口实现类，使用mapper代理方法时，输入参数可以使用pojo包装对象或map对象，保证dao的通用性。


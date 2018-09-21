# 输入映射和输出映射



Mapper.xml映射文件中定义了操作数据库的sql，每个sql是一个statement，映射文件是mybatis的核心。

### 1.1. 环境准备

1. 复制昨天的工程，按照下图进行

2. 如下图粘贴，并更名

3. 只保留Mapper接口开发相关的文件，其他的删除

4. 如下图修改SqlMapConfig.xml配置文件。Mapper映射器只保留包扫描的方式

![](../../.gitbook/assets/image%20%28245%29.png)

### 1.2. parameterType\(输入类型\)

#### 1.2.1. 传递简单类型

参考第一天内容。

使用\#{}占位符，或者${}进行sql拼接。

#### 1.2.2. 传递pojo对象

参考第一天的内容。

Mybatis使用ognl表达式解析对象字段的值，\#{}或者${}括号中的值为pojo属性名称。

#### 1.2.3. 传递pojo包装对象

            开发中通过可以使用pojo传递查询条件。

查询条件可能是综合的查询条件，不仅包括用户查询条件还包括其它的查询条件（比如查询用户信息的时候，将用户购买商品信息也作为查询条件），这时可以使用包装对象传递输入参数。

包装对象：Pojo类中的一个属性是另外一个pojo。

需求：根据用户名模糊查询用户信息，查询条件放到QueryVo的user属性中。

**1.2.3.1. 编写QueryVo**

**public** **class** QueryVo {

      // 包含其他的pojo

      **private** User user;

      **public** User getUser\(\) {

            **return** user;

      }

      **public** **void** setUser\(User user\) {

            **this**.user = user;

      }

}

**1.2.3.2. Sql语句**

SELECT \* FROM user WHERE username LIKE '%张%'

**1.2.3.3. Mapper.xml文件**

在UserMapper.xml中配置sql，如下图。

![](../../.gitbook/assets/image%20%2859%29.png)

**1.2.3.4. Mapper接口**

在UserMapper接口中添加方法，如下图：

![](../../.gitbook/assets/image%20%2831%29.png)

**1.2.3.5. 测试方法**

在UserMapeprTest增加测试方法，如下：

@Test

**public** **void** testQueryUserByQueryVo\(\) {

      // mybatis和spring整合，整合之后，交给spring管理

      SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

      // 创建Mapper接口的动态代理对象，整合之后，交给spring管理

      UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

      // 使用userMapper执行查询，使用包装对象

      QueryVo queryVo = **new** QueryVo\(\);

      // 设置user条件

      User user = **new** User\(\);

      user.setUsername\("张"\);

      // 设置到包装对象中

      queryVo.setUser\(user\);

      // 执行查询

      List&lt;User&gt; list = userMapper.queryUserByQueryVo\(queryVo\);

      **for** \(User u : list\) {

            System.**out**.println\(u\);

      }

      // mybatis和spring整合，整合之后，交给spring管理

      sqlSession.close\(\);

}

**1.2.3.6. 效果**

测试结果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image014.jpg)

### 1.3. resultType\(输出类型\)

#### 1.3.1. 输出简单类型

需求:查询用户表数据条数

sql：SELECT count\(\*\) FROM \`user\`

**1.3.1.1. Mapper.xml文件**

在UserMapper.xml中配置sql，如下图：

![](../../.gitbook/assets/image%20%2813%29.png)

**1.3.1.2. Mapper接口**

在UserMapper添加方法，如下图：

![](../../.gitbook/assets/image%20%2832%29.png)

**1.3.1.3. 测试方法**

在UserMapeprTest增加测试方法，如下：

@Test

**public** **void** testQueryUserCount\(\) {

      // mybatis和spring整合，整合之后，交给spring管理

      SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

      // 创建Mapper接口的动态代理对象，整合之后，交给spring管理

      UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

      // 使用userMapper执行查询用户数据条数

      **int** count = userMapper.queryUserCount\(\);

      System.**out**.println\(count\);

      // mybatis和spring整合，整合之后，交给spring管理

      sqlSession.close\(\);

}

**1.3.1.4. 效果**

测试结果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image020.jpg)

注意：输出简单类型必须查询出来的结果集有一条记录，最终将第一个字段的值转换为输出类型。

#### 1.3.2. 输出pojo对象

参考第一天内容

#### 1.3.3. 输出pojo列表

参考第一天内容。

### 1.4. resultMap

            resultType可以指定将查询结果映射为pojo，但需要pojo的属性名和sql查询的列名一致方可映射成功。

            如果sql查询字段名和pojo的属性名不一致，可以通过resultMap将字段名和属性名作一个对应关系 ，resultMap实质上还需要将查询结果映射到pojo对象中。

            resultMap可以实现将查询结果映射为复杂类型的pojo，比如在查询结果映射对象中包括pojo和list实现一对一查询和一对多查询。

需求：查询订单表order的所有数据

sql：SELECT id, user\_id, number, createtime, note FROM \`order\`

#### 1.4.1. 声明pojo对象

数据库表如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image022.jpg)

Order对象：

**public** **class** Order {

      // 订单id

      **private** **int** id;

      // 用户id

      **private** Integer userId;

      // 订单号

      **private** String number;

      // 订单创建时间

      **private** Date createtime;

      // 备注

      **private** String note;

get/set。。。

}

#### 1.4.2. Mapper.xml文件

创建OrderMapper.xml配置文件，如下：

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;!-- namespace：命名空间，用于隔离sql，还有一个很重要的作用，Mapper动态代理开发的时候使用，需要指定Mapper的类路径 --&gt;

&lt;mapper namespace="cn.itcast.mybatis.mapper.OrderMapper"&gt;

      &lt;!-- 查询所有的订单数据 --&gt;

      &lt;select id="queryOrderAll" resultType="order"&gt;

            SELECT id, user\_id,

            number,

            createtime, note FROM \`order\`

      &lt;/select&gt;

&lt;/mapper&gt;

#### 1.4.3. Mapper接口

编写接口如下：

**public** **interface** OrderMapper {

      /\*\*

       \* 查询所有订单

       \*

       \* **@return**

       \*/

      List&lt;Order&gt; queryOrderAll\(\);

}

#### 1.4.4. 测试方法

编写测试方法OrderMapperTest如下：

**public** **class** OrderMapperTest {

      **private** SqlSessionFactory sqlSessionFactory;

      @Before

      **public** **void** init\(\) **throws** Exception {

            InputStream inputStream = Resources.getResourceAsStream\("SqlMapConfig.xml"\);

            **this**.sqlSessionFactory = **new** SqlSessionFactoryBuilder\(\).build\(inputStream\);

      }

      @Test

      **public** **void** testQueryAll\(\) {

            // 获取sqlSession

            SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

            // 获取OrderMapper

            OrderMapper orderMapper = sqlSession.getMapper\(OrderMapper.**class**\);

            // 执行查询

            List&lt;Order&gt; list = orderMapper.queryOrderAll\(\);

            **for** \(Order order : list\) {

                  System.**out**.println\(order\);

            }

      }

}

#### 1.4.5. 效果

测试效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image024.jpg)

发现userId为null

解决方案：使用resultMap

#### 1.4.6. 使用resultMap

由于上边的mapper.xml中sql查询列\(user\_id\)和Order类属性\(userId\)不一致，所以查询结果不能映射到pojo中。

需要定义resultMap，把orderResultMap将sql查询列\(user\_id\)和Order类属性\(userId\)对应起来

改造OrderMapper.xml，如下：

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;!-- namespace：命名空间，用于隔离sql，还有一个很重要的作用，Mapper动态代理开发的时候使用，需要指定Mapper的类路径 --&gt;

&lt;mapper namespace="cn.itcast.mybatis.mapper.OrderMapper"&gt;

      &lt;!-- resultMap最终还是要将结果映射到pojo上，type就是指定映射到哪一个pojo --&gt;

      &lt;!-- id：设置ResultMap的id --&gt;

      &lt;resultMap type="order" id="orderResultMap"&gt;

            &lt;!-- 定义主键 ,非常重要。如果是多个字段,则定义多个id --&gt;

            &lt;!-- property：主键在pojo中的属性名 --&gt;

            &lt;!-- column：主键在数据库中的列名 --&gt;

            &lt;id property="id" column="id" /&gt;

            &lt;!-- 定义普通属性 --&gt;

            &lt;result property="userId" column="user\_id" /&gt;

            &lt;result property="number" column="number" /&gt;

            &lt;result property="createtime" column="createtime" /&gt;

            &lt;result property="note" column="note" /&gt;

      &lt;/resultMap&gt;

      &lt;!-- 查询所有的订单数据 --&gt;

      &lt;select id="queryOrderAll" resultMap="orderResultMap"&gt;

            SELECT id, user\_id,

            number,

            createtime, note FROM \`order\`

      &lt;/select&gt;

&lt;/mapper&gt;

#### 1.4.7. 效果

只需要修改Mapper.xml就可以了，再次测试结果如下：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image026.jpg)


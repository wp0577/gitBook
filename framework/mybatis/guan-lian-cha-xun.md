# 关联查询



1.1. 商品订单数据模型

### 1.2. 一对一查询

需求：查询所有订单信息，关联查询下单用户信息。

注意：因为一个订单信息只会是一个人下的订单，所以从查询订单信息出发关联查询用户信息为一对一查询。如果从用户信息出发查询用户下的订单信息则为一对多查询，因为一个用户可以下多个订单。

sql语句：

SELECT

            o.id,

            o.user\_id userId,

            o.number,

            o.createtime,

            o.note,

            u.username,

            u.address

FROM

            \`order\` o

LEFT JOIN \`user\` u ON o.user\_id = u.id

#### 1.2.1. 方法一：使用resultType

使用resultType，改造订单pojo类，此pojo类中包括了订单信息和用户信息

这样返回对象的时候，mybatis自动把用户信息也注入进来了

**1.2.1.1. 改造pojo类**

OrderUser类继承Order类后OrderUser类包括了Order类的所有字段，只需要定义用户的信息字段即可，如下图：

![](../../.gitbook/assets/image%20%2812%29.png)

**1.2.1.2. Mapper.xml**

在UserMapper.xml添加sql，如下

&lt;!-- 查询订单，同时包含用户数据 --&gt;

&lt;select id="queryOrderUser" resultType="orderUser"&gt;

      SELECT

      o.id,

      o.user\_id

      userId,

      o.number,

      o.createtime,

      o.note,

      u.username,

      u.address

      FROM

      \`order\` o

      LEFT JOIN \`user\` u ON o.user\_id = u.id

&lt;/select&gt;

**1.2.1.3. Mapper接口**

在UserMapper接口添加方法，如下图：

![](../../.gitbook/assets/image%20%2896%29.png)

**1.2.1.4. 测试方法：**

在UserMapperTest添加测试方法，如下：

@Test

**public** **void** testQueryOrderUser\(\) {

      // mybatis和spring整合，整合之后，交给spring管理

      SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

      // 创建Mapper接口的动态代理对象，整合之后，交给spring管理

      UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

      // 使用userMapper执行根据条件查询用户

      List&lt;OrderUser&gt; list = userMapper.queryOrderUser\(\);

      **for** \(OrderUser ou : list\) {

            System.**out**.println\(ou\);

      }

      // mybatis和spring整合，整合之后，交给spring管理

      sqlSession.close\(\);

}

**1.2.1.5. 效果**

测试结果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg)

**1.2.1.6. 小结**

            定义专门的pojo类作为输出类型，其中定义了sql查询结果集所有的字段。此方法较为简单，企业中使用普遍。

#### 1.2.2. 方法二：使用resultMap

使用resultMap，定义专门的resultMap用于映射一对一查询结果。

**1.2.2.1. 改造pojo类**

            在Order类中加入User属性，user属性中用于存储关联查询的用户信息，因为订单关联查询用户是一对一关系，所以这里使用单个User对象存储关联查询的用户信息。

改造Order如下图：

![](../../.gitbook/assets/image%20%2873%29.png)

**1.2.2.2. Mapper.xml**

这里resultMap指定orderUserResultMap，如下：

&lt;resultMap type="order" id="orderUserResultMap"&gt;

      &lt;id property="id" column="id" /&gt;

      &lt;result property="userId" column="user\_id" /&gt;

      &lt;result property="number" column="number" /&gt;

      &lt;result property="createtime" column="createtime" /&gt;

      &lt;result property="note" column="note" /&gt;

      &lt;!-- association ：配置一对一属性 --&gt;

      &lt;!-- property:order里面的User属性名 --&gt;

      &lt;!-- javaType:属性类型 --&gt;

      &lt;association property="user" javaType="user"&gt;

            &lt;!-- id:声明主键，表示user\_id是关联查询对象的唯一标识--&gt;

            &lt;id property="id" column="user\_id" /&gt;

            &lt;result property="username" column="username" /&gt;

            &lt;result property="address" column="address" /&gt;

      &lt;/association&gt;

&lt;/resultMap&gt;

&lt;!-- 一对一关联，查询订单，订单内部包含用户属性 --&gt;

&lt;select id="queryOrderUserResultMap" resultMap="orderUserResultMap"&gt;

      SELECT

      o.id,

      o.user\_id,

      o.number,

      o.createtime,

      o.note,

      u.username,

      u.address

      FROM

      \`order\` o

      LEFT JOIN \`user\` u ON o.user\_id = u.id

&lt;/select&gt;

**1.2.2.3. Mapper接口**

编写UserMapper如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

**1.2.2.4. 测试方法**

在UserMapperTest增加测试方法，如下：

@Test

**public** **void** testQueryOrderUserResultMap\(\) {

      // mybatis和spring整合，整合之后，交给spring管理

      SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

      // 创建Mapper接口的动态代理对象，整合之后，交给spring管理

      UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

      // 使用userMapper执行根据条件查询用户

      List&lt;Order&gt; list = userMapper.queryOrderUserResultMap\(\);

      **for** \(Order o : list\) {

            System.**out**.println\(o\);

      }

      // mybatis和spring整合，整合之后，交给spring管理

      sqlSession.close\(\);

}

**1.2.2.5. 效果**

测试效果如下图：

![](../../.gitbook/assets/image.png)

### 1.3. 一对多查询

案例：查询所有用户信息及用户关联的订单信息。

用户信息和订单信息为一对多关系。

sql语句：

SELECT

            u.id,

            u.username,

            u.birthday,

            u.sex,

            u.address,

            o.id oid,

            o.number,

            o.createtime,

            o.note

FROM

            \`user\` u

LEFT JOIN \`order\` o ON u.id = o.user\_id

#### 1.3.1. 修改pojo类

在User类中加入List&lt;Order&gt; orders属性,如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image014.jpg)

#### 1.3.2. Mapper.xml

在UserMapper.xml添加sql，如下：

&lt;resultMap type="user" id="userOrderResultMap"&gt;

      &lt;id property="id" column="id" /&gt;

      &lt;result property="username" column="username" /&gt;

      &lt;result property="birthday" column="birthday" /&gt;

      &lt;result property="sex" column="sex" /&gt;

      &lt;result property="address" column="address" /&gt;

      &lt;!-- 配置一对多的关系 --&gt;

      &lt;collection property="orders" javaType="list" ofType="order"&gt;

            &lt;!-- 配置主键，是关联Order的唯一标识 --&gt;

            &lt;id property="id" column="oid" /&gt;

            &lt;result property="number" column="number" /&gt;

            &lt;result property="createtime" column="createtime" /&gt;

            &lt;result property="note" column="note" /&gt;

      &lt;/collection&gt;

&lt;/resultMap&gt;

&lt;!-- 一对多关联，查询订单同时查询该用户下的订单 --&gt;

&lt;select id="queryUserOrder" resultMap="userOrderResultMap"&gt;

      SELECT

      u.id,

      u.username,

      u.birthday,

      u.sex,

      u.address,

      o.id oid,

      o.number,

      o.createtime,

      o.note

      FROM

      \`user\` u

      LEFT JOIN \`order\` o ON u.id = o.user\_id

&lt;/select&gt;

#### 1.3.3. Mapper接口

编写UserMapper接口，如下图：

![](../../.gitbook/assets/image%20%2855%29.png)

#### 1.3.4. 测试方法

在UserMapperTest增加测试方法，如下

@Test

**public** **void** testQueryUserOrder\(\) {

      // mybatis和spring整合，整合之后，交给spring管理

      SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

      // 创建Mapper接口的动态代理对象，整合之后，交给spring管理

      UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

      // 使用userMapper执行根据条件查询用户

      List&lt;User&gt; list = userMapper.queryUserOrder\(\);

      **for** \(User u : list\) {

            System.**out**.println\(u\);

      }

      // mybatis和spring整合，整合之后，交给spring管理

      sqlSession.close\(\);

}

#### 1.3.5. 效果

测试效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image018.jpg)


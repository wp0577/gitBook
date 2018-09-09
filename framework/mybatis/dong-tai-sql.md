# 动态Sql

  
通过Mybatis提供的各种标签方法实现动态拼接sql。

需求：根据性别和名字查询用户

查询sql：

SELECT id, username, birthday, sex, address FROM \`user\` WHERE sex = 1 AND username LIKE '%张%'

### 1.1. If标签

#### 1.1.1. Mapper.xml文件

UserMapper.xml配置sql，如下：

&lt;!-- 根据条件查询用户 --&gt;

&lt;select id="queryUserByWhere" parameterType="user" resultType="user"&gt;

      SELECT id, username, birthday, sex, address FROM \`user\`

      WHERE sex = \#{sex} AND username LIKE

      '%${username}%'

&lt;/select&gt;

#### 1.1.2. Mapper接口

编写Mapper接口，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

#### 1.1.3. 测试方法

在UserMapperTest添加测试方法，如下：

@Test

**public** **void** testQueryUserByWhere\(\) {

      // mybatis和spring整合，整合之后，交给spring管理

      SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

      // 创建Mapper接口的动态代理对象，整合之后，交给spring管理

      UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

      // 使用userMapper执行根据条件查询用户

      User user = **new** User\(\);

      user.setSex\("1"\);

      user.setUsername\("张"\);

      List&lt;User&gt; list = userMapper.queryUserByWhere\(user\);

      **for** \(User u : list\) {

            System.**out**.println\(u\);

      }

      // mybatis和spring整合，整合之后，交给spring管理

      sqlSession.close\(\);

}

#### 1.1.4. 效果

测试效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

如果注释掉  user.setSex\("1"\)，测试结果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg)

测试结果二很显然不合理。

按照之前所学的，要解决这个问题，需要编写多个sql，查询条件越多，需要编写的sql就更多了，显然这样是不靠谱的。

解决方案，使用动态sql的if标签

#### 1.1.5. 使用if标签

改造UserMapper.xml，如下：

&lt;!-- 根据条件查询用户 --&gt;

&lt;select id="queryUserByWhere" parameterType="user" resultType="user"&gt;

      SELECT id, username, birthday, sex, address FROM \`user\`

      WHERE 1=1

      &lt;if test="sex != null and sex != ''"&gt;

            AND sex = \#{sex}

      &lt;/if&gt;

      &lt;if test="username != null and username != ''"&gt;

            AND username LIKE

            '%${username}%'

      &lt;/if&gt;

&lt;/select&gt;

注意字符串类型的数据需要要做不等于空字符串校验。

#### 1.1.6. 效果

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)

如上图所示，测试OK

### 1.2. Where标签

上面的sql还有where 1=1 这样的语句，很麻烦

可以使用where标签进行改造

改造UserMapper.xml，如下

&lt;!-- 根据条件查询用户 --&gt;

&lt;select id="queryUserByWhere" parameterType="user" resultType="user"&gt;

      SELECT id, username, birthday, sex, address FROM \`user\`

&lt;!-- where标签可以自动添加where，同时处理sql语句中第一个and关键字 --&gt;

      &lt;where&gt;

            &lt;if test="sex != null"&gt;

                  AND sex = \#{sex}

            &lt;/if&gt;

            &lt;if test="username != null and username != ''"&gt;

                  AND username LIKE

                  '%${username}%'

            &lt;/if&gt;

      &lt;/where&gt;

&lt;/select&gt;

#### 1.2.1. 效果

测试效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

### 1.3. Sql片段

Sql中可将重复的sql提取出来，使用时用include引用即可，最终达到sql重用的目的。

把上面例子中的id, username, birthday, sex, address提取出来，作为sql片段，如下：

&lt;!-- 根据条件查询用户 --&gt;

&lt;select id="queryUserByWhere" parameterType="user" resultType="user"&gt;

      &lt;!-- SELECT id, username, birthday, sex, address FROM \`user\` --&gt;

      &lt;!-- 使用include标签加载sql片段；refid是sql片段id --&gt;

      SELECT &lt;include refid="userFields" /&gt; FROM \`user\`

      &lt;!-- where标签可以自动添加where关键字，同时处理sql语句中第一个and关键字 --&gt;

      &lt;where&gt;

            &lt;if test="sex != null"&gt;

                  AND sex = \#{sex}

            &lt;/if&gt;

            &lt;if test="username != null and username != ''"&gt;

                  AND username LIKE

                  '%${username}%'

            &lt;/if&gt;

      &lt;/where&gt;

&lt;/select&gt;

&lt;!-- 声明sql片段 --&gt;

&lt;sql id="userFields"&gt;

      id, username, birthday, sex, address

&lt;/sql&gt;

如果要使用别的Mapper.xml配置的sql片段，可以在refid前面加上对应的Mapper.xml的namespace

例如下图

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image012.jpg)

### 1.4. foreach标签

向sql传递数组或List，mybatis使用foreach解析，如下：

根据多个id查询用户信息

查询sql：

SELECT \* FROM user WHERE id IN \(1,10,24\)

#### 1.4.1. 改造QueryVo

如下图在pojo中定义list属性ids存储多个用户id，并添加getter/setter方法

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image014.jpg)

#### 1.4.2. Mapper.xml文件

UserMapper.xml添加sql，如下：

&lt;!-- 根据ids查询用户 --&gt;

&lt;select id="queryUserByIds" parameterType="queryVo" resultType="user"&gt;

      SELECT \* FROM \`user\`

      &lt;where&gt;

            &lt;!-- foreach标签，进行遍历 --&gt;

            &lt;!-- collection：遍历的集合，这里是QueryVo的ids属性 --&gt;

            &lt;!-- item：遍历的项目，可以随便写，，但是和后面的\#{}里面要一致 --&gt;

            &lt;!-- open：在前面添加的sql片段 --&gt;

            &lt;!-- close：在结尾处添加的sql片段 --&gt;

            &lt;!-- separator：指定遍历的元素之间使用的分隔符 --&gt;

            &lt;foreach collection="ids" item="item" open="id IN \(" close="\)"

                  separator=","&gt;

                  \#{item}

            &lt;/foreach&gt;

      &lt;/where&gt;

&lt;/select&gt;

测试方法如下图：

@Test

**public** **void** testQueryUserByIds\(\) {

      // mybatis和spring整合，整合之后，交给spring管理

      SqlSession sqlSession = **this**.sqlSessionFactory.openSession\(\);

      // 创建Mapper接口的动态代理对象，整合之后，交给spring管理

      UserMapper userMapper = sqlSession.getMapper\(UserMapper.**class**\);

      // 使用userMapper执行根据条件查询用户

      QueryVo queryVo = **new** QueryVo\(\);

      List&lt;Integer&gt; ids = **new** ArrayList&lt;&gt;\(\);

      ids.add\(1\);

      ids.add\(10\);

      ids.add\(24\);

      queryVo.setIds\(ids\);

      List&lt;User&gt; list = userMapper.queryUserByIds\(queryVo\);

      **for** \(User u : list\) {

            System.**out**.println\(u\);

      }

      // mybatis和spring整合，整合之后，交给spring管理

      sqlSession.close\(\);

}

#### 1.4.3. 效果

测试效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image016.jpg)


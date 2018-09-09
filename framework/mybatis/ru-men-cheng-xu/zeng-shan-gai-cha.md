# 增删改查

## 实现根据id查询用户

```text
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace：命名空间，用于隔离sql，还有一个很重要的作用，后面会讲 -->
<mapper namespace="test">

	<!-- id:statement的id 或者叫做sql的id-->
	<!-- parameterType:声明输入参数的类型 -->
	<!-- resultType:声明输出结果的类型，应该填写pojo的全路径 -->
	<!-- #{}：输入参数的占位符，相当于jdbc的？ -->
	<select id="queryUserById" parameterType="int"
		resultType="cn.itcast.mybatis.pojo.User">
		SELECT * FROM `user` WHERE id  = #{id}
	</select>

</mapper>

```

测试程序：

```text
public class MybatisTest {
	private SqlSessionFactory sqlSessionFactory = null;

	@Before
	public void init() throws Exception {
		// 1. 创建SqlSessionFactoryBuilder对象
		SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();

		// 2. 加载SqlMapConfig.xml配置文件
		InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");

		// 3. 创建SqlSessionFactory对象
		this.sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
	}

	@Test
	public void testQueryUserById() throws Exception {
		// 4. 创建SqlSession对象
		SqlSession sqlSession = sqlSessionFactory.openSession();

		// 5. 执行SqlSession对象执行查询，获取结果User
		// 第一个参数是User.xml的statement的id，第二个参数是执行sql需要的参数；
		Object user = sqlSession.selectOne("queryUserById", 1);

		// 6. 打印结果
		System.out.println(user);

		// 7. 释放资源
		sqlSession.close();
	}
}

```

## 实现根据用户名模糊查询用户

{% code-tabs %}
{% code-tabs-item title="方法一" %}
```text

5.5.1.1. 映射文件
在User.xml配置文件中添加如下内容：
	<!-- 如果返回多个结果，mybatis会自动把返回的结果放在list容器中 -->
	<!-- resultType的配置和返回一个结果的配置一样 -->
	<select id="queryUserByUsername1" parameterType="string"
		resultType="cn.itcast.mybatis.pojo.User">
		SELECT * FROM `user` WHERE username LIKE #{username}
	</select>

5.5.1.2. 测试程序
MybatisTest中添加测试方法如下：
	@Test
	public void testQueryUserByUsername1() throws Exception {
		// 4. 创建SqlSession对象
		SqlSession sqlSession = sqlSessionFactory.openSession();

		// 5. 执行SqlSession对象执行查询，获取结果User
		// 查询多条数据使用selectList方法
		List<Object> list = sqlSession.selectList("queryUserByUsername1", "%王%");

		// 6. 打印结果
		for (Object user : list) {
			System.out.println(user);
		}

		// 7. 释放资源
		sqlSession.close();
	}

```
{% endcode-tabs-item %}

{% code-tabs-item title="方法二" %}
    5.5.2.1. 映射文件：
    在User.xml配置文件中添加如下内容：
    	<!-- 如果传入的参数是简单数据类型，${}里面必须写value -->
    	<select id="queryUserByUsername2" parameterType="string"
    		resultType="cn.itcast.mybatis.pojo.User">
    		SELECT * FROM `user` WHERE username LIKE '%${value}%'
    	</select>

    5.5.2.2. 测试程序：
    MybatisTest中添加测试方法如下：
    @Test
    public void testQueryUserByUsername2() throws Exception {
    	// 4. 创建SqlSession对象
    	SqlSession sqlSession = sqlSessionFactory.openSession();

    	// 5. 执行SqlSession对象执行查询，获取结果User
    	// 查询多条数据使用selectList方法
    	List<Object> list = sqlSession.selectList("queryUserByUsername2", "王");

    	// 6. 打印结果
    	for (Object user : list) {
    		System.out.println(user);
    	}

    	// 7. 释放资源
    	sqlSession.close();
    }
{% endcode-tabs-item %}
{% endcode-tabs %}

## 小结 

### \#{}和${}

{}表示一个占位符号，通过\#{}可以实现preparedStatement向占位符中设置值，自动进行java类型和jdbc类型转换。\#{}可以有效防止sql注入。 \#{}可以接收简单类型值或pojo属性值。 如果parameterType传输单个简单类型值，\#{}括号中可以是value或其它名称。

${}表示拼接sql串，通过${}可以将parameterType 传入的内容拼接在sql中且不进行jdbc类型转换， ${}可以接收简单类型值或pojo属性值，如果parameterType传输单个简单类型值，${}括号中只能是value。

### parameterType和resultType 

parameterType：指定输入参数类型，mybatis通过ognl从输入对象中获取参数值拼接在sql中。 resultType：指定输出结果类型，mybatis将sql查询结果的一行记录数据映射为resultType指定类型的对象。如果有多条数据，则分别进行映射，并把对象放到容器List中

### SelectOne和selectList 

selectOne查询一条记录，如果使用selectOne查询多条记录则抛出异常： org.apache.ibatis.exceptions.TooManyResultsException: Expected one result \(or null\) to be returned by selectOne\(\), but found: 3 at org.apache.ibatis.session.defaults.DefaultSqlSession.selectOne\(DefaultSqlSession.java:70\) selectList可以查询一条或多条记录。

## 实现添加用户

```text
在User.xml配置文件中添加如下内容：
	<!-- 保存用户 -->
	<insert id="saveUser" parameterType="cn.itcast.mybatis.pojo.User">
		INSERT INTO `user`
		(username,birthday,sex,address) VALUES
		(#{username},#{birthday},#{sex},#{address})
	</insert>

5.7.2. 测试程序
MybatisTest中添加测试方法如下：
@Test
public void testSaveUser() {
	// 4. 创建SqlSession对象
	SqlSession sqlSession = sqlSessionFactory.openSession();

	// 5. 执行SqlSession对象执行保存
	// 创建需要保存的User
	User user = new User();
	user.setUsername("张飞");
	user.setSex("1");
	user.setBirthday(new Date());
	user.setAddress("蜀国");

	sqlSession.insert("saveUser", user);
	System.out.println(user);

	// 需要进行事务提交
	sqlSession.commit();

	// 7. 释放资源
	sqlSession.close();
}

```

## mysql自增主键返回

```text
查询id的sql
SELECT LAST_INSERT_ID()

通过修改User.xml映射文件，可以将mysql自增主键返回：
如下添加selectKey 标签
<!-- 保存用户 -->
<insert id="saveUser" parameterType="cn.itcast.mybatis.pojo.User">
	<!-- selectKey 标签实现主键返回 -->
	<!-- keyColumn:主键对应的表中的哪一列 -->
	<!-- keyProperty：主键对应的pojo中的哪一个属性 -->
	<!-- order：设置在执行insert语句前执行查询id的sql，孩纸在执行insert语句之后执行查询id的sql -->
	<!-- resultType：设置返回的id的类型 -->
	<selectKey keyColumn="id" keyProperty="id" order="AFTER"
		resultType="int">
		SELECT LAST_INSERT_ID()
	</selectKey>
	INSERT INTO `user`
	(username,birthday,sex,address) VALUES
	(#{username},#{birthday},#{sex},#{address})
</insert>

LAST_INSERT_ID():是mysql的函数，返回auto_increment自增列新记录id值。

```

## Mysql使用 uuid实现主键

```text
需要增加通过select uuid()得到uuid值
<!-- 保存用户 -->
<insert id="saveUser" parameterType="cn.itcast.mybatis.pojo.User">
	<!-- selectKey 标签实现主键返回 -->
	<!-- keyColumn:主键对应的表中的哪一列 -->
	<!-- keyProperty：主键对应的pojo中的哪一个属性 -->
	<!-- order：设置在执行insert语句前执行查询id的sql，孩纸在执行insert语句之后执行查询id的sql -->
	<!-- resultType：设置返回的id的类型 -->
	<selectKey keyColumn="id" keyProperty="id" order="BEFORE"
		resultType="string">
		SELECT LAST_INSERT_ID()
	</selectKey>
	INSERT INTO `user`
	(username,birthday,sex,address) VALUES
	(#{username},#{birthday},#{sex},#{address})
</insert>

注意这里使用的order是“BEFORE”

```

## 修改用户

```text
根据用户id修改用户名

使用的sql：
UPDATE `user` SET username = '赵云' WHERE id = 26

5.8.1. 映射文件
在User.xml配置文件中添加如下内容：
<!-- 更新用户 -->
<update id="updateUserById" parameterType="cn.itcast.mybatis.pojo.User">
	UPDATE `user` SET
	username = #{username} WHERE id = #{id}
</update>

5.8.2. 测试程序
MybatisTest中添加测试方法如下：
@Test
public void testUpdateUserById() {
	// 4. 创建SqlSession对象
	SqlSession sqlSession = sqlSessionFactory.openSession();

	// 5. 执行SqlSession对象执行更新
	// 创建需要更新的User
	User user = new User();
	user.setId(26);
	user.setUsername("关羽");
	user.setSex("1");
	user.setBirthday(new Date());
	user.setAddress("蜀国");

	sqlSession.update("updateUserById", user);

	// 需要进行事务提交
	sqlSession.commit();

	// 7. 释放资源
	sqlSession.close();
}

```

## 删除用户

```text
根据用户id删除用户

使用的sql
DELETE FROM `user` WHERE id = 47
5.9.1. 映射文件：
在User.xml配置文件中添加如下内容：
	<!-- 删除用户 -->
	<delete id="deleteUserById" parameterType="int">
		delete from user where
		id=#{id}
	</delete>

5.9.2. 测试程序：
MybatisTest中添加测试方法如下：
	@Test
	public void testDeleteUserById() {
		// 4. 创建SqlSession对象
		SqlSession sqlSession = sqlSessionFactory.openSession();

		// 5. 执行SqlSession对象执行删除
		sqlSession.delete("deleteUserById", 48);

		// 需要进行事务提交
		sqlSession.commit();

		// 7. 释放资源
		sqlSession.close();
	}

```




# 配置

 1：导包，创建对象

2：orm元数据配置文件Customer.hbm.xml

```text
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
   <!-- 配置表与实体对象的关系 -->
   <!-- package属性:填写一个包名.在元素内部凡是需要书写完整类名的属性,可以直接写简答类名了. -->
<hibernate-mapping package="com.wp.example.domain" >
	<!-- 
		class元素: 配置实体与表的对应关系的
			name: 完整类名
			table:数据库表名
	 -->
	<class name="Customer" table="cst_customer" >
		<!-- id元素:配置主键映射的属性
				name: 填写主键对应属性名
				column(可选): 填写表中的主键列名.默认值:列名会默认使用属性名
				type(可选):填写列(属性)的类型.hibernate会自动检测实体的属性类型.
						每个类型有三种填法: java类型|hibernate类型|数据库类型
				not-null(可选):配置该属性(列)是否不能为空. 默认值:false
				length(可选):配置数据库中列的长度. 默认值:使用数据库类型的最大长度
		 -->
		<id name="cust_id"  >
			<!-- generator:主键生成策略
			(identity : 主键自增.由数据库来维护主键值.录入时不需要指定主键.
有线程安全问题): 主键hibrnate来维护.每次插入前会先查询表中id最大值.+1作为新主键值.			
            native:hilo+sequence+identity 自动三选一策略.
			) -->
			<generator class="native"></generator>
		</id>
		<!-- property元素:除id之外的普通属性映射
				name: 填写属性名
				column(可选): 填写列名
				type(可选):填写列(属性)的类型.hibernate会自动检测实体的属性类型.
						每个类型有三种填法: java类型|hibernate类型|数据库类型
				not-null(可选):配置该属性(列)是否不能为空. 默认值:false
				length(可选):配置数据库中列的长度. 默认值:使用数据库类型的最大长度
		 -->
		<property name="cust_name" column="cust_name" >
			<!--  <column name="cust_name" sql-type="varchar" ></column> -->
		</property>
		<property name="cust_source" column="cust_source" ></property>
		<property name="cust_industry" column="cust_industry" ></property>
		<property name="cust_level" column="cust_level" ></property>
		<property name="cust_linkman" column="cust_linkman" ></property>
		<property name="cust_phone" column="cust_phone" ></property>
		<property name="cust_mobile" column="cust_mobile" ></property>
	</class>
</hibernate-mapping>
```

3：主配置文件 hibernate.cfg.xml

```text
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<session-factory>
	
		<!-- 
		#hibernate.dialect org.hibernate.dialect.MySQLDialect
		#hibernate.dialect org.hibernate.dialect.MySQLInnoDBDialect
		#hibernate.dialect org.hibernate.dialect.MySQLMyISAMDialect
		#hibernate.connection.driver_class com.mysql.jdbc.Driver
		#hibernate.connection.url jdbc:mysql:///test
		#hibernate.connection.username gavin
		#hibernate.connection.password
		 -->
		 <!-- 数据库驱动 -->
		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
		 <!-- 数据库url -->
		<property name="hibernate.connection.url">jdbc:mysql:///Hibernate</property>
		 <!-- 数据库连接用户名 -->
		<property name="hibernate.connection.username">root</property>
		 <!-- 数据库连接密码 -->
		<property name="hibernate.connection.password">wupan0577</property>
		<!-- 数据库方言
			不同的数据库中,sql语法略有区别. 指定方言可以让hibernate框架在生成sql语句时.针对数据库的方言生成.
			sql99标准: DDL 定义语言  库表的增删改查
					  DCL 控制语言  事务 权限
					  DML 操纵语言  增删改查
			注意: MYSQL在选择方言时,请选择最短的方言.
		 -->
		<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
		
		
		<!-- #hibernate.show_sql true 
			 #hibernate.format_sql true
		-->
		<!-- 将hibernate生成的sql语句打印到控制台 -->
		<property name="hibernate.show_sql">true</property>
		<!-- 将hibernate生成的sql语句格式化(语法缩进) -->
		<property name="hibernate.format_sql">true</property>
		<!-- 
		## auto schema export  自动导出表结构. 自动建表
		#hibernate.hbm2ddl.auto create		自动建表.每次框架运行都会创建新的表.以前表将会被覆盖,表数据会丢失.(开发环境中测试使用)
		#hibernate.hbm2ddl.auto create-drop 自动建表.每次框架运行结束都会将所有表删除.(开发环境中测试使用)
		#hibernate.hbm2ddl.auto update(推荐使用) 自动生成表.如果已经存在不会再生成.如果表有变动.自动更新表(不会删除任何数据).
		#hibernate.hbm2ddl.auto validate	校验.不自动生成表.每次启动会校验数据库中表是否正确.校验失败.
		 -->
		<property name="hibernate.hbm2ddl.auto">update</property>
		
		 <!-- 指定hibernate操作数据库时的隔离级别 
			#hibernate.connection.isolation 1|2|4|8		
			0001	1	读未提交
			0010	2	读已提交
			0100	4	可重复读
			1000	8	串行化
		 -->
		 <property name="hibernate.connection.isolation">4</property>
		 
		 <!-- 指定session与当前线程绑定 -->
		 <property name="hibernate.current_session_context_class">thread</property>
		 
		<!-- 引入orm元数据
			路径书写: 填写src下的路径
		 -->
		<mapping resource="com/wp/example/domain/Customer.hbm.xml" />

		
	</session-factory>
</hibernate-configuration>
```

4：API详解

```text
//学习Session对象
//session对象功能: 表达hibernate框架与数据库之间的连接(会话).session类似于
//				JDBC年代的connection对象. 还可以完成对数据库中数据的增删改查操作.
//				session是hibernate操作数据库的核心对象
@Test
	//session的新增
	public void fun2(){
		//1 创建,调用空参构造
		Configuration conf = new Configuration().configure();
		//2 根据配置信息,创建 SessionFactory对象
		SessionFactory sf = conf.buildSessionFactory();
		//3 获得session
		Session session = sf.openSession();
		//4 session获得操作事务的Transaction对象
		//获得操作事务的tx对象
		//Transaction tx = session.getTransaction();
		//开启事务并获得操作事务的tx对象(建议使用)
		Transaction tx2 = session.beginTransaction();
		//----------------------------------------------
		Customer c = new Customer();
		c.setCust_name("传智播客");
		
		session.save(c);
		//----------------------------------------------
		tx2.commit();//提交事务
		session.close();//释放资源
		sf.close();//释放资源
	}
```


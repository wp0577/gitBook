# Mybatis解决jdbc编程的问题

1、 数据库连接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库连接池可解决此问题。 解决：在SqlMapConfig.xml中配置数据连接池，使用连接池管理数据库链接。 

2、 Sql语句写在代码中造成代码不易维护，实际应用sql变化的可能较大，sql变动需要改变java代码。 解决：将Sql语句配置在XXXXmapper.xml文件中与java代码分离。 

3、 向sql语句传参数麻烦，因为sql语句的where条件不一定，可能多也可能少，占位符需要和参数一一对应。 解决：Mybatis自动将java对象映射至sql语句，通过statement中的parameterType定义输入参数的类型。 

4、 对结果集解析麻烦，sql变化导致解析代码变化，且解析前需要遍历，如果能将数据库记录封装成pojo对象解析比较方便。 解决：Mybatis自动将sql执行结果映射至java对象，通过statement中的resultType定义输出结果的类型。


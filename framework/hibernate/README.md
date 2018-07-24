# Hibernate

![](http://www.xmind.net/m/JqEX)

## Unknown initial character set index '255' received from server. Initial client character 解决方法

2018年06月23日 23:02:26阅读数：782

Unknown initial character set index '255' received from server. Initial client character set can be forced via the 'characterEncoding' property.从错误的提示信息中发现字符集设置出现问题

mysql连接数据库时报此错误：//String url = "jdbc:mysql://localhost:3306/db\_cjky" 如果使用这句就会报错。  
//Unknown initial character set index '255' received from server. Initial client character set can be forced via the 'characterEncoding' property.  
String url = "jdbc:mysql://localhost:3306/db\_cjky?useUnicode=true&amp;characterEncoding=utf8";//改成这句，就可以了  
最终解决方法：删除 \WebContent\WEB-INF\lib目录下的。mysql-connector的jar文件。原因是：MySQL驱动和数据库字符集设置不搭配




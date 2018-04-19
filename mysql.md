### 一. The operation of database.

```
一：对数据库的操作
1.	创建一个库
create database 库名
create database 库名 character set 编码
2.	删除一个库
drop database 库名
3.	使用库
use 库名
4.      查看当前正在操作的库
select database<>;
*********************************************
二：对数据库表的操作
1.创建一张表
create table 表名(
	字段名 类型(长度) [约束],
	字段名 类型(长度) [约束],
	字段名 类型(长度) [约束]
);
2.查看数据库表
创建完成后，我们可以查看数据库表
show tables;
查看表的结构
desc 表名
3.删除一张表
drop table 表名
4.修改表
4.1 添加一列
alter table 表名 add 字段名 类型(长度) [约束]
4.2 修改列的类型(长度、约束)
alter table 表名 modify 要修改的字段名 类型(长度) [约束]
4.3 修改列的列名
alter table 表名 change 旧列名 新列名 类型(长度) [约束]
4.4 删除表的列
alter table 表名 drop 列名
4.5 修改表名
rename table 表名 to 新表名
4.6 修改表的字符集
alter table 表名 character set 编码
************************************************
三、对数据库表记录进行操作(修改)
1.插入记录
insert into 表名(列名1,列名2,列名3……) values(值1,值2,值3……)
insert into 表名 values(值1,值2,值3……)
1.1 插入数据中文乱码问题解决办法
方式一：【不建议！】
直接修改数据库安装目录里面的my.ini文件的第57行

方式二：
	set names gbk;

2.修改表记录
2.1 不带条件的
update 表名 set 字段名=值, 字段名=值, 字段名=值……
 
2.2 带条件的
update 表名 set字段名=值, 字段名=值, 字段名=值…… where 条件

3.删除表记录
3.1 带条件的
delete from 表名 where 条件
注意，删除后，uid不会重置！

3.2.不带条件的
先准备数据
insert into tbl_user values(null,’老王’,’666’);

删除操作
	delete from 表名;

3.3 面试题
说说delete与truncate的区别？
delete删除的时候是一条一条的删除记录，它配合事务，可以将删除的数据找回。
truncate删除，它是将整个表摧毁，然后再创建一张一模一样的表。它删除的数据无法找回。

Delete操作演示：
注意：delete删除，uid不会重置！而使用truncate操作，uid会重置

4.查询操作
语法：
	select [distinct] *| 列名，列名 from 表名 [where条件]
4.1 简单查询
1.查询所有商品
select * from product；
2. 查询商品名和商品价格
select pname,price from product;
3.查询所有商品信息使用表别名
select * from product as p;
4.查询商品名，使用列别名
select pname as p from product
5.去掉重复值(按照价格)
select distinct(price) from product;

先准备数据：
insert into product values (null,'李士雪',38,null);
6.将所有的商品的价格+10进行显示
select pname,price+10 from product;
4.2 条件查询
1.查询商品名称为"左慈"的商品信息
。。。。。
```

![](/as2/import.png)

2.查询价格&gt;60元的所有商品信息

![](/ass2/import.png)

4.查询商品id在\(3,6,9\)范围内的所有商品信息

![](/as4/import.png)

#### 4.3 排序

1.查询所有的商品，按价格进行排序\(升序、降序\)

![](/ass5/import.png)

#### 4.4 聚合函数

1.获得所有商品的价格的总和

![](/as7/import.png)



2.获得所有商品的平均价格

select avg&lt;price&gt; from product;



3.获得所有商品的个数

select count&lt;\*&gt; from product;

#### 4.5 分组操作

1.添加分类id \(alter table product add cid varchar\(32\);\)

2.初始化数据

update product set cid='1';

update product set cid='2' where pid in \(5,6,7\);

1.根据cid字段分组，分组后统计商品的个数。

select cid,avg&lt;price&gt; from product group by cid having avg&lt;price&gt; &gt;2000;

2.根据cid分组，分组统计每组商品的平均价格，并且平均价格大于20000元。

select cid,avg&lt;price&gt; from product group by cid having avg&lt;price&gt; &gt;2000;



#### 4.6 查询总结

select 一般在的后面的内容都是要查询的字段

from 要查询到表

where

group by

having分组后带有条件只能使用having

order by它必须放到最后面




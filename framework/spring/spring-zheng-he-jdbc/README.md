# Spring整合JDBC

{% embed url="https://wp0577.gitbook.io/test1/mysql/lian-jie-chi/dbutil" %}

## spring提供了很多模板整合Dao技术

![](../../../.gitbook/assets/image%20%2819%29.png)

## spring中提供了一个可以操作数据库的对象.对象封装了jdbc技术

JDBCTemplate =&gt; JDBC模板对象

与DBUtils中的QueryRunner非常相似.

![](../../../.gitbook/assets/image%20%28162%29.png)

## 步骤

### 导包

![](../../../.gitbook/assets/image%20%28110%29.png)

### 准备数据库

### 书写Dao

![&#x8BE5;&#x56FE;&#x7247;&#x5185;&#x7684;Dao&#x5BF9;&#x8C61;&#x5DF2;&#x7ECF;&#x7EE7;&#x627F;&#x4E86;DaoSupport&#x5BF9;&#x8C61;&#xFF0C;&#x6240;&#x4EE5;&#x53EF;&#x4EE5;&#x76F4;&#x63A5;&#x8C03;&#x7528;&#x7236;&#x7C7B;&#x7684;&#x65B9;&#x6CD5;&#x5F97;&#x5230;jdbcTemplate](../../../.gitbook/assets/image%20%28199%29.png)

### Srping配置

#### 依赖关系 

![](../../../.gitbook/assets/image%20%28215%29.png)

![](../../../.gitbook/assets/image%20%2865%29.png)

#### JDBCDaoSupport

![&#x53EF;&#x4EE5;&#x4E0D;&#x7528;&#x6CE8;&#x5165;JDBCTemplate&#x5BF9;&#x8C61;](../../../.gitbook/assets/image%20%28176%29.png)

#### 读取外部的properties配置

![](../../../.gitbook/assets/image%20%28226%29.png)


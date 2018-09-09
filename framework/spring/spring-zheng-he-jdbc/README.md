# Spring整合JDBC

{% embed data="{\"url\":\"https://wp0577.gitbook.io/test1/mysql/lian-jie-chi/dbutil\",\"type\":\"link\",\"title\":\"DButil - JavaEE\",\"icon\":{\"type\":\"icon\",\"url\":\"https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/spaces%2F-LFcIEEpyyi7v-mjfE-X%2Favatar.png?generation=1529684491520080&alt=media\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://www.gitbook.com/share/space/thumbnail/-LFcIEEpyyi7v-mjfE-X.png\",\"width\":1200,\"height\":630,\"aspectRatio\":0.525}}" %}

## spring提供了很多模板整合Dao技术

![](../../../.gitbook/assets/image%20%2815%29.png)

## spring中提供了一个可以操作数据库的对象.对象封装了jdbc技术

JDBCTemplate =&gt; JDBC模板对象

与DBUtils中的QueryRunner非常相似.

![](../../../.gitbook/assets/image%20%28112%29.png)

## 步骤

### 导包

![](../../../.gitbook/assets/image%20%2882%29.png)

### 准备数据库

### 书写Dao

![&#x8BE5;&#x56FE;&#x7247;&#x5185;&#x7684;Dao&#x5BF9;&#x8C61;&#x5DF2;&#x7ECF;&#x7EE7;&#x627F;&#x4E86;DaoSupport&#x5BF9;&#x8C61;&#xFF0C;&#x6240;&#x4EE5;&#x53EF;&#x4EE5;&#x76F4;&#x63A5;&#x8C03;&#x7528;&#x7236;&#x7C7B;&#x7684;&#x65B9;&#x6CD5;&#x5F97;&#x5230;jdbcTemplate](../../../.gitbook/assets/image%20%28139%29.png)

### Srping配置

#### 依赖关系 

![](../../../.gitbook/assets/image%20%28151%29.png)

![](../../../.gitbook/assets/image%20%2853%29.png)

#### JDBCDaoSupport

![&#x53EF;&#x4EE5;&#x4E0D;&#x7528;&#x6CE8;&#x5165;JDBCTemplate&#x5BF9;&#x8C61;](../../../.gitbook/assets/image%20%28123%29.png)

#### 读取外部的properties配置

![](../../../.gitbook/assets/image%20%28161%29.png)


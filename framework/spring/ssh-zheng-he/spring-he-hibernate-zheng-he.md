# Spring和hibernate整合

## Hibernate单独配置

## Spring整合Hibernate

### 整合原理

将sessionFactory对象交给Spring容器

### 在Spring中配置SessionFactory

#### 1：通过直接读取配置文件

![](../../../.gitbook/assets/image%20%28123%29.png)

#### 2：直接在applicationcontext.xml文件中配置必须属性

![](../../../.gitbook/assets/image%20%2869%29.png)

在该方法中配置要记得配置mapping属性



## Spring整合C3p0连接池

### 1.配置db.properties

![](../../../.gitbook/assets/image%20%28167%29.png)

### 2.引入连接池到spring中

![](../../../.gitbook/assets/image%20%28116%29.png)

### 3.将连接池注入给SessionFactory

![](../../../.gitbook/assets/image%20%28138%29.png)

## Spring整合HibernateTemplate操作数据库

### Dao类创建:继承HibernateDaoSupport

### hibernate模板的操作

#### execute

![](../../../.gitbook/assets/image%20%28131%29.png)

#### findByCriteria

![](../../../.gitbook/assets/image%20%28103%29.png)

### spring中配置dao

![](../../../.gitbook/assets/image%20%28198%29.png)




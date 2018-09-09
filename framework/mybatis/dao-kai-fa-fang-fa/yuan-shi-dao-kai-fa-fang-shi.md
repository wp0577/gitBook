---
description: 原始Dao开发方法需要程序员编写Dao接口和Dao实现类。
---

# 原始Dao开发方式

略



## 使用Dao开发存在的问题

原始Dao开发中存在以下问题： 

 Dao方法体存在重复代码：通过SqlSessionFactory创建SqlSession，调用SqlSession的数据库操作方法

 调用sqlSession的数据库操作方法需要指定statement的id，这里存在硬编码，不得于开发维护。


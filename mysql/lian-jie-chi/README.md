# 连接池

```text
使用连接池改造JDBC的工具类：
1.1.1    需求：
传统JDBC的操作,对连接的对象销毁不是特别好.每次创建和销毁连接都是需要花费时间.可以使用连接池优化的程序.
* 在程序开始的时候,可以创建几个连接,将连接放入到连接池中.用户使用连接的时候,可以从连接池中进行获取.用完之后,可以将连接归还连接池.
1.1.2    分析:
1.1.2.1    技术分析：
```


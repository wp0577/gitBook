# 自定义连接池

```java
【自定义连接池】（了解）
* SUN公司提供了一个连接池的接口.(javax.sql.DataSource).
* 定义一个连接池:实现这个接口.
* 使用List集合存放多个连接的对象.

【自定义连接池的代码】
public class MyDataSource implements DataSource{
    // 创建一个List集合用于存放多个连接对象.
    private List<Connection> list = new ArrayList<Connection>();
    // 在程序开始的时候，初始化几个连接,将连接存放到list中.
    public MyDataSource() {
        // 初始化3个连接:
        for(int i=1;i<=3;i++){
            Connection conn = JDBCUtils.getConnection();
            list.add(conn);
        }
    }

    @Override
    // 获得连接的方法：
    public Connection getConnection() throws SQLException {
        if(list.size() <= 0){
            for(int i=1;i<=3;i++){
                Connection conn = JDBCUtils.getConnection();
                list.add(conn);
    }    
        }
        Connection conn = list.remove(0);
        return conn;
    }

    // 归还连接的方法：
    public void addBack(Connection conn){
        list.add(conn);
    }
...
}
【自定义连接池中问题及如何解决】
    问题?
1.如果使用自定义连接池,那么需要额外记住自定义连接池中的API.
2.能不能使用面向接口的编程方式.
    解决：
不额外提供API方法，就可以解决上述两个问题！！！
能不能还调用Connection的close方法.能不能增强Connection的close方法,原有的销毁变为归还!!!
    如何增强Connection的close方法:
* 增强一个Java类中的某个方法有几种方式???
    * 一种方式：继承的方式.
        * 能够控制这个类的构造的时候,才可以使用继承.

    * 二种方式：装饰者模式方式.
        * 包装对象和被包装的对象都要实现相同的接口.
        * 包装的对象中需要获得到被包装对象的引用.
        ***** 缺点：如果接口的方法比较多,增强其中的某个方法.其他的功能的方法需要原有调用.

    * 三种方式：动态代理的方式.
        * 被增强的对象实现接口就可以.
【继承和装饰者的案例】
/**
 * 继承的方式增强一个类中某个方法:
 */
class Man{
    public void run(){
        System.out.println("跑....");
    }
}

class SuperMan extends Man{
    public void run(){
        // super.run();
        System.out.println("飞....");
    }
}

/**
 * 使用装饰者的方式完成类的方法的增强
 */
interface Waiter{
    public void server();
}

class Waiteress implements Waiter{

    @Override
    public void server() {
        System.out.println("服务...");
    }

}

class WaiteressWrapper implements Waiter{
    private Waiter waiter;

    public WaiteressWrapper(Waiter waiter) {
        this.waiter = waiter;
    }

    @Override
    public void server() {
        System.out.println("微笑...");
        // this.waiter.server();

    }

}
【使用装饰者模式增强Connection的close方法】
public class MyConnection implements Connection{

    private Connection conn;
    private List<Connection> list;

    public MyConnection(Connection conn,List<Connection> list) {
        this.conn = conn;
        this.list = list;
    }


    @Override
    public void close() throws SQLException {
        list.add(conn);
    }
     ...
}

连接池的getConnection方法：
    @Override
    // 获得连接的方法：
    public Connection getConnection() throws SQLException {
        if(list.size() <= 0){
            for(int i=1;i<=3;i++){
                Connection conn = JDBCUtils.getConnection();
                list.add(conn);
            }    
        }
        Connection conn = list.remove(0);
        MyConnection myConn = new MyConnection(conn, list);
        return myConn;
    }
```


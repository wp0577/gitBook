# 实例

DBUtil

```text
public class MyDataSourceUtils {

    //获得Connection ----- 从连接池中获取
    private static ComboPooledDataSource dataSource = new ComboPooledDataSource();

    //创建ThreadLocal
    private static ThreadLocal<Connection> tl = new ThreadLocal<Connection>();

    //开启事务
    public static void startTransaction() throws SQLException{
        Connection conn = getCurrentConnection();
        conn.setAutoCommit(false);
    }

    //获得当前线程上绑定的conn
    public static Connection getCurrentConnection() throws SQLException{
        //从ThreadLocal寻找 当前线程是否有对应Connection
        Connection conn = tl.get();
        if(conn==null){
            //获得新的connection
            conn = getConnection();
            //将conn资源绑定到ThreadLocal（map）上
            tl.set(conn);
        }
        return conn;
    }

    public static Connection getConnection() throws SQLException{
        return dataSource.getConnection();
    }

    //回滚事务
    public static void rollback() throws SQLException {
        getCurrentConnection().rollback();
    }

    //提交事务
    public static void commit() throws SQLException {
        Connection conn = getCurrentConnection();
        conn.commit();
        //将Connection从ThreadLocal中移除
        tl.remove();
        conn.close();

    }

}
```

service

```text
try {

            //开启事务
            //conn = DataSourceUtils.getConnection();
            //conn.setAutoCommit(false);

            //开启事务
            MyDataSourceUtils.startTransaction();

            //转出钱的方法  
            dao.out(out,money);
            //int i = 1/0;
            //转入钱的方法
            dao.in(in,money);

        } catch (Exception e) {
            isTranferSuccess = false;
            //回滚事务
            try {
                MyDataSourceUtils.rollback();
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            e.printStackTrace();
        } finally{
            try {
                MyDataSourceUtils.commit();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
}
```


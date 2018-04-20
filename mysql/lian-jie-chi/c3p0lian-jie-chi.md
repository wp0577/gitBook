```

【C3P0连接池的使用】
第一步：引入C3P0连接池的jar包.
第二步：编写代码：
* 手动设置参数:
* 配置文件设置参数（c3p0-config.xml）：
 
【C3P0改造工具类】
public class JDBCUtils2 {
	private static final ComboPooledDataSource DATA_SOURCE =new ComboPooledDataSource();
	/**
	 * 获得连接的方法
	 */
	public static Connection getConnection(){
		Connection conn = null;
		try {
			conn = DATA_SOURCE.getConnection();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return conn;
	}
...

```




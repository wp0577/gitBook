```
【C3P0连接池的使用】
第一步：引入C3P0连接池的jar包.
第二步：编写代码：
* 手动设置参数:
* 配置文件设置参数（c3p0-config.xml）：

//在导入配置文件后，通过ComboPooledDataSource方法即可调用配置文件内的属性内容
//By default, c3p0 will look for an XML configuration file in its classloader's resource path 
under the name "/c3p0-config.xml". That means the XML file should be placed in a directly or jar
 file directly named in your applications CLASSPATH, in WEB-INF/classes, or some similar location.
  If you prefer not to bundle your configuration with your code, you can specify an ordinary filesystem
   location for c3p0's configuration file via the system property com.mchange.v2.c3p0.cfg.xml. 

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

##  {#blogTitle2}




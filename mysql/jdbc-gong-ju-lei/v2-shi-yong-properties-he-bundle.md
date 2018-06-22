# V2使用properties和bundle

使用properties文件：

![](../../.gitbook/assets/import%20%2850%29.png)使用ResourceBundle去读取properties文件内的value；

```java
package com.neu.wp.util;


import java.sql.*;
import java.util.ResourceBundle;

public class Util1 {

    private static String driver;
    private static String url;
    private static String username;
    private static String password;

    static {
        ResourceBundle rb = ResourceBundle.getBundle("db");
        driver = rb.getString("jdbc.driver");
        url = rb.getString("jdbc.url");
        username = rb.getString("jdbc.username");
        password = rb.getString("jdbc.password");
    }

    public static Connection getConnection() {

        Connection connection = null;
        try {
            Class.forName(driver);
            connection = DriverManager.getConnection(url, username, password);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        catch (SQLException e) {
            e.printStackTrace();
        }
        return connection;

    }

    public static void release(Connection cn, PreparedStatement ps, ResultSet rs) {

        if(rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(ps != null) {
            try {
                ps.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }if(cn != null) {
            try {
                cn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }



    }
}
```

测试:

```java
package com.neu.wp.mysql;

import com.neu.wp.util.Util1;
import org.junit.Test;

import java.sql.*;

public class MysqlTest {

    @Test
    public void login() throws SQLException, ClassNotFoundException {
        loginTest("admin", "admin1");
    }

    public void loginTest(String username, String password) throws ClassNotFoundException, SQLException {
        Connection connection = Util1.getConnection();
        String sql = "select * from admin where username = ? and password = ?";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        preparedStatement.setString(1, username);
        preparedStatement.setString(2, password);
        ResultSet rs = preparedStatement.executeQuery();
        if (rs.next()){
            //System.out.println("id: " + rs.getInt(1) + "name: " + rs.getString(2) + "pass:" +rs.getString(3));
            System.out.println("success");
        }
        else System.out.println("failed");
        Util1.release(connection, preparedStatement, rs);
    }
```


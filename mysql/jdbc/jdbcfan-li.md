```java
package com.neu.wp.mysql;

import org.junit.Test;

import java.sql.*;

public class MysqlTest {

    @Test
    public void login() throws SQLException, ClassNotFoundException {
        loginTest("panwu", "123");
    }

    public void loginTest(String username, String password) throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/wp?autoReconnect=true&useSSL=false", "root", "wupan0577");
        String sql = "select * from user where number = ? and password = ?";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        preparedStatement.setString(1, username);
        preparedStatement.setString(2, password);
        ResultSet rs = preparedStatement.executeQuery();
        if (rs.next()){
            //列索引从1开始，而不是从0开始
            //System.out.println("id: " + rs.getInt(1) + "name: " + rs.getString(2) + "pass:" +rs.getString(3));
            System.out.println("success");
        }
        else System.out.println("failed");

    }
}
```




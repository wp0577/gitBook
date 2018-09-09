# Mybatis逆向工程



使用官方网站的Mapper自动生成工具mybatis-generator-core-1.3.2来生成po类和Mapper映射文件

### 1.1. 导入逆向工程

使用课前资料已有逆向工程，如下图：

![](../../.gitbook/assets/image%20%2847%29.png)

#### 1.1.1. 复制逆向工程到工作空间中

复制的效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

#### 1.1.2. 导入逆向工程到eclipse中

如下图方式进行导入：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

### 1.2. 修改配置文件

在generatorConfig.xml中配置Mapper生成的详细信息，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image012.jpg)

注意修改以下几点:

1.     修改要生成的数据库表

2.     pojo文件所在包路径

3.     Mapper所在的包路径

配置文件如下:

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;!DOCTYPE generatorConfiguration

  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"

  "http://mybatis.org/dtd/mybatis-generator-config\_1\_0.dtd"&gt;

&lt;generatorConfiguration&gt;

      &lt;context id="testTables" targetRuntime="MyBatis3"&gt;

            &lt;commentGenerator&gt;

                  &lt;!-- 是否去除自动生成的注释 true：是 ： false:否 --&gt;

                  &lt;property name="suppressAllComments" value="true" /&gt;

            &lt;/commentGenerator&gt;

            &lt;!--数据库连接的信息：驱动类、连接地址、用户名、密码 --&gt;

            &lt;jdbcConnection driverClass="com.mysql.jdbc.Driver"

                  connectionURL="jdbc:mysql://localhost:3306/mybatis" userId="root" password="root"&gt;

            &lt;/jdbcConnection&gt;

            &lt;!-- &lt;jdbcConnection driverClass="oracle.jdbc.OracleDriver" connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg"

                  userId="yycg" password="yycg"&gt; &lt;/jdbcConnection&gt; --&gt;

            &lt;!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL

                  和 NUMERIC 类型解析为java.math.BigDecimal --&gt;

            &lt;javaTypeResolver&gt;

                  &lt;property name="forceBigDecimals" value="false" /&gt;

            &lt;/javaTypeResolver&gt;

            &lt;!-- targetProject:生成PO类的位置 --&gt;

            &lt;javaModelGenerator targetPackage="cn.itcast.ssm.po"

                  targetProject=".\src"&gt;

                  &lt;!-- enableSubPackages:是否让schema作为包的后缀 --&gt;

                  &lt;property name="enableSubPackages" value="false" /&gt;

                  &lt;!-- 从数据库返回的值被清理前后的空格 --&gt;

                  &lt;property name="trimStrings" value="true" /&gt;

            &lt;/javaModelGenerator&gt;

            &lt;!-- targetProject:mapper映射文件生成的位置 --&gt;

            &lt;sqlMapGenerator targetPackage="cn.itcast.ssm.mapper"

                  targetProject=".\src"&gt;

                  &lt;!-- enableSubPackages:是否让schema作为包的后缀 --&gt;

                  &lt;property name="enableSubPackages" value="false" /&gt;

            &lt;/sqlMapGenerator&gt;

            &lt;!-- targetPackage：mapper接口生成的位置 --&gt;

            &lt;javaClientGenerator type="XMLMAPPER"

                  targetPackage="cn.itcast.ssm.mapper" targetProject=".\src"&gt;

                  &lt;!-- enableSubPackages:是否让schema作为包的后缀 --&gt;

                  &lt;property name="enableSubPackages" value="false" /&gt;

            &lt;/javaClientGenerator&gt;

            &lt;!-- 指定数据库表 --&gt;

            &lt;table schema="" tableName="user"&gt;&lt;/table&gt;

            &lt;table schema="" tableName="order"&gt;&lt;/table&gt;

      &lt;/context&gt;

&lt;/generatorConfiguration&gt;

### 1.3. 生成逆向工程代码

找到下图所示的java文件，执行工程main主函数,

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image014.jpg)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image016.jpg)

刷新工程，发现代码生成，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image018.jpg)

### 1.4. 测试逆向工程代码

1. 复制生成的代码到mybatis-spring工程，如下图

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image020.jpg)

2. 修改spring配置文件

在applicationContext.xml修改

&lt;!-- Mapper代理的方式开发，扫描包方式配置代理 --&gt;

&lt;bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"&gt;

      &lt;!-- 配置Mapper接口，如果需要加载多个包，直接写进来，中间用，分隔 --&gt;

      &lt;!-- &lt;property name="basePackage" value="cn.itcast.mybatis.mapper" /&gt; --&gt;

      &lt;property name="basePackage" value="cn.itcast.ssm.mapper" /&gt;

&lt;/bean&gt;

3. 编写测试方法：

**public** **class** UserMapperTest {

      **private** ApplicationContext context;

      @Before

      **public** **void** setUp\(\) **throws** Exception {

            **this**.context = **new** ClassPathXmlApplicationContext\("classpath:applicationContext.xml"\);

      }

      @Test

      **public** **void** testInsert\(\) {

            // 获取Mapper

            UserMapper userMapper = **this**.context.getBean\(UserMapper.**class**\);

            User user = **new** User\(\);

            user.setUsername\("曹操"\);

            user.setSex\("1"\);

            user.setBirthday\(**new** Date\(\)\);

            user.setAddress\("三国"\);

            userMapper.insert\(user\);

      }

      @Test

      **public** **void** testSelectByExample\(\) {

            // 获取Mapper

            UserMapper userMapper = **this**.context.getBean\(UserMapper.**class**\);

            // 创建User对象扩展类，用户设置查询条件

            UserExample example = **new** UserExample\(\);

            example.createCriteria\(\).andUsernameLike\("%张%"\);

            // 查询数据

            List&lt;User&gt; list = userMapper.selectByExample\(example\);

            System.**out**.println\(list.size\(\)\);

      }

      @Test

      **public** **void** testSelectByPrimaryKey\(\) {

            // 获取Mapper

            UserMapper userMapper = **this**.context.getBean\(UserMapper.**class**\);

            User user = userMapper.selectByPrimaryKey\(1\);

            System.**out**.println\(user\);

      }

}

注意：

1. 逆向工程生成的代码只能做单表查询

2. 不能在生成的代码上进行扩展，因为如果数据库变更，需要重新使用逆向工程生成代码，原来编写的代码就被覆盖了。

3. 一张表会生成4个文件


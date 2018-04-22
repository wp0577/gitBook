1. 因为tomcat下WEB-INF/classes文件下放的是java编译后的文件.class. 所以在IDE中配置TOMCAT项目时,要将SRC中的文件解析输出到classes文件下.
2. 因为javax.servlet的Jar包在Tomcat的lib文件夹下, 所以在dependency中要添加依赖包.
3. TOMCAT接受中文信息乱码问题, 在server.xml中添加...utf-8.
4. 



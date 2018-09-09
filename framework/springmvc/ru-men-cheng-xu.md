# 入门程序

#### 1.1.1. 创建web工程

springMVC是表现层框架，需要搭建web工程开发。

如下图创建动态web工程：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

输入工程名，选择配置Tomcat（如果已有，则直接使用），如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)

配置Tomcat，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg)

选择准备好的Tomcat，这里用的是Tomcat7，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)

选择成功，点击Finish，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

选择刚刚设置成功的Tomcat，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image012.jpg)

如下图选择web的版本是2.5，可以自动生成web.xml配置文件，

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image014.jpg)

创建效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image016.jpg)

#### 1.1.2. 导入jar包

从课前资料中导入springMVC的jar包，位置如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image018.jpg)

复制jar到lib目录，工程直接加载jar包，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image020.jpg)

#### 1.1.3. 加入配置文件

创建config资源文件夹，存放配置文件，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image022.jpg)

**1.1.3.1. 创建springmvc.xml**

创建SpringMVC的核心配置文件

SpringMVC本身就是Spring的子项目，对Spring兼容性很好，不需要做很多配置。

这里只配置一个Controller扫描就可以了，让Spring对页面控制层Controller进行管理。

创建springmvc.xml

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"

      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"

      xmlns:context="http://www.springframework.org/schema/context"

      xmlns:mvc="http://www.springframework.org/schema/mvc"

      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd

        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd"&gt;

      &lt;!-- 配置controller扫描包 --&gt;

      &lt;context:component-scan base-package="cn.itcast.springmvc.controller" /&gt;

&lt;/beans&gt;

配置文件需要的约束文件，位置如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image024.jpg)

创建包cn.itcast.springmvc.controller

效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image026.jpg)

**1.1.3.2. 配置前端控制器**

配置SpringMVC的前端控制器DispatcherServlet

在web.xml中

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

      xmlns="http://java.sun.com/xml/ns/javaee"

      xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app\_2\_5.xsd"

      id="WebApp\_ID" version="2.5"&gt;

      &lt;display-name&gt;springmvc-first&lt;/display-name&gt;

      &lt;welcome-file-list&gt;

            &lt;welcome-file&gt;index.html&lt;/welcome-file&gt;

            &lt;welcome-file&gt;index.htm&lt;/welcome-file&gt;

            &lt;welcome-file&gt;index.jsp&lt;/welcome-file&gt;

            &lt;welcome-file&gt;default.html&lt;/welcome-file&gt;

            &lt;welcome-file&gt;default.htm&lt;/welcome-file&gt;

            &lt;welcome-file&gt;default.jsp&lt;/welcome-file&gt;

      &lt;/welcome-file-list&gt;

      &lt;!-- 配置SpringMVC前端控制器 --&gt;

      &lt;servlet&gt;

            &lt;servlet-name&gt;springmvc-first&lt;/servlet-name&gt;

            &lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;

            &lt;!-- 指定SpringMVC配置文件 --&gt;

            &lt;!-- SpringMVC的配置文件的默认路径是/WEB-INF/${servlet-name}-servlet.xml --&gt;

            &lt;init-param&gt;

                  &lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;

                  &lt;param-value&gt;classpath:springmvc.xml&lt;/param-value&gt;

            &lt;/init-param&gt;

      &lt;/servlet&gt;

      &lt;servlet-mapping&gt;

            &lt;servlet-name&gt;springmvc-first&lt;/servlet-name&gt;

            &lt;!-- 设置所有以action结尾的请求进入SpringMVC --&gt;

            &lt;url-pattern&gt;\*.action&lt;/url-pattern&gt;

      &lt;/servlet-mapping&gt;

&lt;/web-app&gt;

#### 1.1.4. 加入jsp页面

把参考资料中的itemList.jsp复制到工程的/WEB-INF/jsp目录下，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image028.jpg)

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image030.jpg)

#### 1.1.5. 实现显示商品列表页

**1.1.5.1. 创建pojo**

分析页面，查看页面需要的数据，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image032.jpg)

创建商品pojo

**public** **class** Item {

      // 商品id

      **private** **int** id;

      // 商品名称

      **private** String name;

      // 商品价格

      **private** **double** price;

      // 商品创建时间

      **private** Date createtime;

      // 商品描述

      **private** String detail;

创建带参数的构造器

set/get。。。

}

**1.1.5.2. 创建ItemController**

ItemController是一个普通的java类，不需要实现任何接口。

需要在类上添加@Controller注解，把Controller交由Spring管理

在方法上面添加@RequestMapping注解，里面指定请求的url。其中“.action”可以加也可以不加。

@Controller

**public** **class** ItemController {

      // @RequestMapping：里面放的是请求的url，和用户请求的url进行匹配

      // action可以写也可以不写

      @RequestMapping\("/itemList.action"\)

      **public** ModelAndView queryItemList\(\) {

            // 创建页面需要显示的商品数据

            List&lt;Item&gt; list = **new** ArrayList&lt;&gt;\(\);

            list.add\(**new** Item\(1, "1华为 荣耀8", 2399, **new** Date\(\), "质量好！1"\)\);

            list.add\(**new** Item\(2, "2华为 荣耀8", 2399, **new** Date\(\), "质量好！2"\)\);

            list.add\(**new** Item\(3, "3华为 荣耀8", 2399, **new** Date\(\), "质量好！3"\)\);

            list.add\(**new** Item\(4, "4华为 荣耀8", 2399, **new** Date\(\), "质量好！4"\)\);

            list.add\(**new** Item\(5, "5华为 荣耀8", 2399, **new** Date\(\), "质量好！5"\)\);

            list.add\(**new** Item\(6, "6华为 荣耀8", 2399, **new** Date\(\), "质量好！6"\)\);

            // 创建ModelAndView，用来存放数据和视图

            ModelAndView modelAndView = **new** ModelAndView\(\);

            // 设置数据到模型中

            modelAndView.addObject\("list", list\);

            // 设置视图jsp，需要设置视图的物理地址

            modelAndView.setViewName\("/WEB-INF/jsp/itemList.jsp"\);

            **return** modelAndView;

      }

}

#### 1.1.6. 启动项目测试

启动项目，浏览器访问地址

http://127.0.0.1:8080/springmvc-first/itemList.action

效果如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image034.jpg)

为什么可以用呢？我们需要分析一下springMVC的架构图。


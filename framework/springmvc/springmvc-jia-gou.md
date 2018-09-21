# SpringMVC架构

### 1.1. 框架结构

框架结构如下图：

![](../../.gitbook/assets/image%20%2816%29.png)

### 1.2. 架构流程

1、用户发送请求至前端控制器DispatcherServlet

2、DispatcherServlet收到请求调用HandlerMapping处理器映射器。

3、处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器\(如果有则生成\)一并返回给DispatcherServlet。

4、DispatcherServlet通过HandlerAdapter处理器适配器调用处理器

5、执行处理器\(Controller，也叫后端控制器\)。

6、Controller执行完成返回ModelAndView

7、HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet

8、DispatcherServlet将ModelAndView传给ViewReslover视图解析器

9、ViewReslover解析后返回具体View

10、      DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）。

11、      DispatcherServlet响应用户

### 1.3. 组件说明

以下组件通常使用框架提供实现：

u  DispatcherServlet：前端控制器

用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。

u  HandlerMapping：处理器映射器

HandlerMapping负责根据用户请求url找到Handler即处理器，springmvc提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

u  Handler：处理器

Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。

由于Handler涉及到具体的用户业务请求，所以一般情况需要程序员根据业务需求开发Handler。

u  HandlAdapter：处理器适配器

通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

下图是许多不同的适配器，最终都可以使用usb接口连接

![usb&#x8F6C;&#x7F51;&#x53E3;](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image003.jpg)    ![usb&#x8F6C;&#x4E32;&#x53E3;](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg)   ![usb&#x8F6C;ps2](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image005.jpg)

u  ViewResolver：视图解析器

View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。

u  View：视图

springmvc框架提供了很多的View视图类型的支持，包括：jstlView、freemarkerView、pdfView等。我们最常用的视图就是jsp。

一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。

| 说明：在springmvc的各个组件中，处理器映射器、处理器适配器、视图解析器称为springmvc的三大组件。需要用户开发的组件有handler、view |
| :--- |


### 1.4. 默认加载的组件

我们没有做任何配置，就可以使用这些组件

因为框架已经默认加载这些组件了，配置文件位置如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image007.jpg)

\# Default implementation classes for DispatcherServlet's strategy interfaces.

\# Used as fallback when no matching beans are found in the DispatcherServlet context.

\# Not meant to be customized by application developers.

org.springframework.web.servlet.LocaleResolver=org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver

org.springframework.web.servlet.ThemeResolver=org.springframework.web.servlet.theme.FixedThemeResolver

org.springframework.web.servlet.HandlerMapping=org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping,\

      org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping

org.springframework.web.servlet.HandlerAdapter=org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter,\

      org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter,\

      org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter

org.springframework.web.servlet.HandlerExceptionResolver=org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerExceptionResolver,\

      org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolver,\

      org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver

org.springframework.web.servlet.RequestToViewNameTranslator=org.springframework.web.servlet.view.DefaultRequestToViewNameTranslator

org.springframework.web.servlet.ViewResolver=org.springframework.web.servlet.view.InternalResourceViewResolver

org.springframework.web.servlet.FlashMapManager=org.springframework.web.servlet.support.SessionFlashMapManager

### 1.5. 组件扫描器

使用组件扫描器省去在spring容器配置每个Controller类的繁琐。

使用&lt;context:component-scan&gt;自动扫描标记@Controller的控制器类，

在springmvc.xml配置文件中配置如下：

&lt;!-- 配置controller扫描包，多个包之间用,分隔 --&gt;

&lt;context:component-scan base-package="cn.itcast.springmvc.controller" /&gt;

### 1.6. 注解映射器和适配器

#### 1.6.1. 配置处理器映射器

            注解式处理器映射器，对类中标记了@ResquestMapping的方法进行映射。根据@ResquestMapping定义的url匹配@ResquestMapping标记的方法，匹配成功返回HandlerMethod对象给前端控制器。

HandlerMethod对象中封装url对应的方法Method。

从spring3.1版本开始，废除了DefaultAnnotationHandlerMapping的使用，推荐使用RequestMappingHandlerMapping完成注解式处理器映射。

在springmvc.xml配置文件中配置如下：

&lt;!-- 配置处理器映射器 --&gt;

&lt;bean

      class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping" /&gt;

注解描述：

@RequestMapping：定义请求url到处理器功能方法的映射

#### 1.6.2. 配置处理器适配器

注解式处理器适配器，对标记@ResquestMapping的方法进行适配。

从spring3.1版本开始，废除了AnnotationMethodHandlerAdapter的使用，推荐使用RequestMappingHandlerAdapter完成注解式处理器适配。

在springmvc.xml配置文件中配置如下：

&lt;!-- 配置处理器适配器 --&gt;

&lt;bean

      class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter" /&gt;

#### 1.6.3. 注解驱动

直接配置处理器映射器和处理器适配器比较麻烦，可以使用注解驱动来加载。

SpringMVC使用&lt;mvc:annotation-driven&gt;自动加载RequestMappingHandlerMapping和RequestMappingHandlerAdapter

可以在springmvc.xml配置文件中使用&lt;mvc:annotation-driven&gt;替代注解处理器和适配器的配置。

&lt;!-- 注解驱动 --&gt;

&lt;mvc:annotation-driven /&gt;

### 1.7. 视图解析器

视图解析器使用SpringMVC框架默认的InternalResourceViewResolver，这个视图解析器支持JSP视图解析

在springmvc.xml配置文件中配置如下：

      &lt;!-- Example: prefix="/WEB-INF/jsp/", suffix=".jsp", viewname="test" -&gt;

            "/WEB-INF/jsp/test.jsp" --&gt;

      &lt;!-- 配置视图解析器 --&gt;

      &lt;bean

            class="org.springframework.web.servlet.view.InternalResourceViewResolver"&gt;

            &lt;!-- 配置逻辑视图的前缀 --&gt;

            &lt;property name="prefix" value="/WEB-INF/jsp/" /&gt;

            &lt;!-- 配置逻辑视图的后缀 --&gt;

            &lt;property name="suffix" value=".jsp" /&gt;

      &lt;/bean&gt;

逻辑视图名需要在controller中返回ModelAndView指定，比如逻辑视图名为ItemList，则最终返回的jsp视图地址:

“WEB-INF/jsp/itemList.jsp”

最终jsp物理地址：前缀+**逻辑视图名**+后缀

#### 1.7.1. 修改ItemController

修改ItemController中设置视图的代码

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

      modelAndView.addObject\("itemList", list\);

      // 设置视图jsp，需要设置视图的物理地址

      // modelAndView.setViewName\("/WEB-INF/jsp/itemList.jsp"\);

      // 配置好视图解析器前缀和后缀，这里只需要设置逻辑视图就可以了。

      // 视图解析器根据前缀+逻辑视图名+后缀拼接出来物理路径

      modelAndView.setViewName\("itemList"\);

      **return** modelAndView;

}


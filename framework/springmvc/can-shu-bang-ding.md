# 参数绑定

### 1.1. 默认支持的参数类型

#### 1.1.1. 需求

打开商品编辑页面，展示商品信息。

#### 1.1.2. 需求分析

编辑商品信息，首先要显示商品详情

需要根据商品id查询商品信息，然后展示到页面。

请求的url：/itemEdit.action

参数：id（商品id）

响应结果：商品编辑页面，展示商品详细信息。

#### 1.1.3. ItemService接口

编写ItemService接口如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

#### 1.1.4. ItemServiceImpl实现类

@Override

**public** Item queryItemById\(**int** id\) {

      Item item = **this**.itemMapper.selectByPrimaryKey\(id\);

      **return** item;

}

#### 1.1.5. ItemController

页面点击修改按钮，发起请求

http://127.0.0.1:8080/springmvc-web/itemEdit.action?id=1

需要从请求的参数中把请求的id取出来。

Id包含在Request对象中。可以从Request对象中取id。

想获得Request对象只需要在Controller方法的形参中添加一个参数即可。Springmvc框架会自动把Request对象传递给方法。

代码实现

/\*\*

 \* 根据id查询商品

 \*

 \* **@param** request

 \* **@return**

 \*/

@RequestMapping\("/itemEdit"\)

**public** ModelAndView queryItemById\(HttpServletRequest request\) {

      // 从request中获取请求参数

      String strId = request.getParameter\("id"\);

      Integer id = Integer.valueOf\(strId\);

      // 根据id查询商品数据

      Item item = **this**.itemService.queryItemById\(id\);

      // 把结果传递给页面

      ModelAndView modelAndView = **new** ModelAndView\(\);

      // 把商品数据放在模型中

      modelAndView.addObject\("item", item\);

      // 设置逻辑视图

      modelAndView.setViewName\("itemEdit"\);

      **return** modelAndView;

}

#### 1.1.6. 默认支持的参数类型

处理器形参中添加如下类型的参数处理适配器会默认识别并进行赋值。

**1.1.6.1. HttpServletRequest**

通过request对象获取请求信息

**1.1.6.2. HttpServletResponse**

通过response处理响应信息

**1.1.6.3. HttpSession**

通过session对象得到session中存放的对象

#### 1.1.7. Model/ModelMap

**1.1.7.1. Model**

除了ModelAndView以外，还可以使用Model来向页面传递数据，

Model是一个接口，在参数里直接声明model即可。

如果使用Model则可以不使用ModelAndView对象，Model对象可以向页面传递数据，View对象则可以使用String返回值替代。

不管是Model还是ModelAndView，其本质都是使用Request对象向jsp传递数据。

代码实现：

/\*\*

 \* 根据id查询商品,使用Model

 \*

 \* **@param** request

 \* **@param** model

 \* **@return**

 \*/

@RequestMapping\("/itemEdit"\)

**public** String queryItemById\(HttpServletRequest request, Model model\) {

      // 从request中获取请求参数

      String strId = request.getParameter\("id"\);

      Integer id = Integer.valueOf\(strId\);

      // 根据id查询商品数据

      Item item = **this**.itemService.queryItemById\(id\);

      // 把结果传递给页面

      // ModelAndView modelAndView = new ModelAndView\(\);

      // 把商品数据放在模型中

      // modelAndView.addObject\("item", item\);

      // 设置逻辑视图

      // modelAndView.setViewName\("itemEdit"\);

      // 把商品数据放在模型中

      model.addAttribute\("item", item\);

      **return** "itemEdit";

}

**1.1.7.2. ModelMap**

ModelMap是Model接口的实现类，也可以通过ModelMap向页面传递数据

使用Model和ModelMap的效果一样，如果直接使用Model，springmvc会实例化ModelMap。

代码实现：

/\*\*

 \* 根据id查询商品,使用ModelMap

 \*

 \* **@param** request

 \* **@param** model

 \* **@return**

 \*/

@RequestMapping\("/itemEdit"\)

**public** String queryItemById\(HttpServletRequest request, ModelMap model\) {

      // 从request中获取请求参数

      String strId = request.getParameter\("id"\);

      Integer id = Integer.valueOf\(strId\);

      // 根据id查询商品数据

      Item item = **this**.itemService.queryItemById\(id\);

      // 把结果传递给页面

      // ModelAndView modelAndView = new ModelAndView\(\);

      // 把商品数据放在模型中

      // modelAndView.addObject\("item", item\);

      // 设置逻辑视图

      // modelAndView.setViewName\("itemEdit"\);

      // 把商品数据放在模型中

      model.addAttribute\("item", item\);

      **return** "itemEdit";

}

### 1.2. 绑定简单类型

当请求的参数名称和处理器形参名称一致时会将请求参数与形参进行绑定。

这样，从Request取参数的方法就可以进一步简化。

/\*\*

 \* 根据id查询商品,绑定简单数据类型

 \*

 \* **@param** id

 \* **@param** model

 \* **@return**

 \*/

@RequestMapping\("/itemEdit"\)

**public** String queryItemById\(**int** id, ModelMap model\) {

      // 根据id查询商品数据

      Item item = **this**.itemService.queryItemById\(id\);

      // 把商品数据放在模型中

      model.addAttribute\("item", item\);

      **return** "itemEdit";

}

#### 1.2.1. 支持的数据类型

参数类型推荐使用包装数据类型，因为基础数据类型不可以为null

整形：Integer、int

字符串：String

单精度：Float、float

双精度：Double、double

布尔型：Boolean、boolean

说明：对于布尔类型的参数，请求的参数值为true或false。或者1或0

请求url：

http://localhost:8080/xxx.action?id=2&status=false

处理器方法：

public String editItem\(Model model,Integer id,Boolean status\)

#### 1.2.2. @RequestParam

使用@RequestParam常用于处理简单类型的绑定。

value：参数名字，即入参的请求参数名字，如value=“itemId”表示请求的参数      区中的名字为itemId的参数的值将传入

required：是否必须，默认是true，表示请求中一定要有相应的参数，否则将报错

TTP Status 400 - Required Integer parameter 'XXXX' is not present

defaultValue：默认值，表示如果请求中没有同名参数时的默认值

定义如下：

@RequestMapping\("/itemEdit"\)

**public** String queryItemById\(@RequestParam\(value = "itemId", required = **true**, defaultValue = "1"\) Integer id,

            ModelMap modelMap\) {

      // 根据id查询商品数据

      Item item = **this**.itemService.queryItemById\(id\);

      // 把商品数据放在模型中

      modelMap.addAttribute\("item", item\);

      **return** "itemEdit";

}

### 1.3. 绑定pojo类型

#### 1.3.1. 需求 

将页面修改后的商品信息保存到数据库中。

#### 1.3.2. 需求分析

请求的url：/updateItem.action

参数：表单中的数据。

响应内容：更新成功页面

#### 1.3.3. 使用pojo接收表单数据

如果提交的参数很多，或者提交的表单中的内容很多的时候,可以使用简单类型接受数据,也可以使用pojo接收数据。

要求：pojo对象中的属性名和表单中input的name属性一致。

页面定义如下图：

![](../../.gitbook/assets/image%20%2899%29.png)

Pojo\(逆向工程生成\)如下图：

![](../../.gitbook/assets/image%20%28103%29.png)

请求的参数名称和pojo的属性名称一致，会自动将请求参数赋值给pojo的属性。

#### 1.3.4. ItemService接口

ItemService里编写接口方法

/\*\*

 \* 根据id更新商品

 \*

 \* **@param** item

 \*/

**void** updateItemById\(Item item\);

#### 1.3.5. ItemServiceImpl实现类

ItemServiceImpl里实现接口方法

使用updateByPrimaryKeySelective\(item\)方法，忽略空参数

@Override

**public** **void** updateItemById\(Item item\) {

      **this**.itemMapper.updateByPrimaryKeySelective\(item\);

}

#### 1.3.6. ItemController

/\*\*

 \* 更新商品,绑定pojo类型

 \*

 \* **@param** item

 \* **@param** model

 \* **@return**

 \*/

@RequestMapping\("/updateItem"\)

**public** String updateItem\(Item item\) {

      // 调用服务更新商品

      **this**.itemService.updateItemById\(item\);

      // 返回逻辑视图

      **return** "success";

}

注意：

提交的表单中不要有日期类型的数据，否则会报400错误。如果想提交日期类型的数据需要用到后面的自定义参数绑定的内容。

#### 1.3.7. 编写success页面

如下图创建success.jsp页面

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image008.jpg)

页面代码：

&lt;%@ page language="java" contentType="text/html; charset=UTF-8"

    pageEncoding="UTF-8"%&gt;

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"&gt;

&lt;html&gt;

&lt;head&gt;

&lt;meta http-equiv="Content-Type" content="text/html; charset=UTF-8"&gt;

&lt;title&gt;Insert title here&lt;/title&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;h1&gt;商品修改成功！&lt;/h1&gt;

&lt;/body&gt;

&lt;/html&gt;

#### 1.3.8. 解决post乱码问题

提交发现，保存成功，但是保存的是乱码

在web.xml中加入：

      &lt;!-- 解决post乱码问题 --&gt;

      &lt;filter&gt;

            &lt;filter-name&gt;encoding&lt;/filter-name&gt;

            &lt;filter-class&gt;org.springframework.web.filter.CharacterEncodingFilter&lt;/filter-class&gt;

            &lt;!-- 设置编码参是UTF8 --&gt;

            &lt;init-param&gt;

                  &lt;param-name&gt;encoding&lt;/param-name&gt;

                  &lt;param-value&gt;UTF-8&lt;/param-value&gt;

            &lt;/init-param&gt;

      &lt;/filter&gt;

      &lt;filter-mapping&gt;

            &lt;filter-name&gt;encoding&lt;/filter-name&gt;

            &lt;url-pattern&gt;/\*&lt;/url-pattern&gt;

      &lt;/filter-mapping&gt;

以上可以解决post请求乱码问题。

对于get请求中文参数出现乱码解决方法有两个：

修改tomcat配置文件添加编码与工程编码一致，如下：

&lt;Connector URIEncoding="utf-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/&gt;

另外一种方法对参数进行重新编码：

String userName new

String\(request.getParamter\("userName"\).getBytes\("ISO8859-1"\),"utf-8"\)

ISO8859-1是tomcat默认编码，需要将tomcat编码后的内容按utf-8编码

### 1.4. 绑定包装pojo

#### 1.4.1. 需求

使用包装的pojo接收商品信息的查询条件。

#### 1.4.2. 需求分析

包装对象定义如下：

**public** **class** QueryVo {

      **private** Item item;

set/get。。。

}

页面定义如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.jpg)

#### 1.4.3. 接收查询条件

     // 绑定包装数据类型

     @RequestMapping\("/queryItem"\)

     **public** String queryItem\(QueryVo queryVo\) {

          System.**out**.println\(queryVo.getItem\(\).getId\(\)\);

          System.**out**.println\(queryVo.getItem\(\).getName\(\)\);

          **return** "success";

     }

### 1.5. 自定义参数绑定

#### 1.5.1. 需求

在商品修改页面可以修改商品的生产日期，并且根据业务需求自定义日期格式。

#### 1.5.2. 需求分析

由于日期数据有很多种格式，springmvc没办法把字符串转换成日期类型。所以需要自定义参数绑定。

前端控制器接收到请求后，找到注解形式的处理器适配器，对RequestMapping标记的方法进行适配，并对方法中的形参进行参数绑定。可以在springmvc处理器适配器上自定义转换器Converter进行参数绑定。

一般使用&lt;mvc:annotation-driven/&gt;注解驱动加载处理器适配器，可以在此标签上进行配置。

#### 1.5.3. 修改jsp页面

如下图修改itemEdit.jsp页面，显示时间

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image012.jpg)

#### 1.5.4. 自定义Converter

//Converter&lt;S, T&gt;

//S:source,需要转换的源的类型

//T:target,需要转换的目标类型

**public** **class** DateConverter **implements** Converter&lt;String, Date&gt; {

     @Override

     **public** Date convert\(String source\) {

          **try** {

               // 把字符串转换为日期类型

               SimpleDateFormat simpleDateFormat = **new** SimpleDateFormat\("yyy-MM-dd HH:mm:ss"\);

               Date date = simpleDateFormat.parse\(source\);

               **return** date;

          } **catch** \(ParseException e\) {

               // **TODO** Auto-generated catch block

               e.printStackTrace\(\);

          }

          // 如果转换异常则返回空

          **return** **null**;

     }

}

#### 1.5.5. 配置Converter

我们同时可以配置多个的转换器。

类似下图的usb设备，可以接入多个usb设备

![IMG\_256](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image013.jpg)

&lt;!-- 配置注解驱动 --&gt;

&lt;!-- 如果配置此标签,可以不用配置... --&gt;

&lt;mvc:annotation-driven conversion-service="conversionService" /&gt;

&lt;!-- 转换器配置 --&gt;

&lt;bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean"&gt;

     &lt;property name="converters"&gt;

          &lt;set&gt;

               &lt;bean class="cn.itcast.springmvc.converter.DateConverter" /&gt;

          &lt;/set&gt;

     &lt;/property&gt;

&lt;/bean&gt;

#### 1.5.6. 配置方式2（了解）

&lt;!--注解适配器 --&gt;

&lt;bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"&gt;

     &lt;property name="webBindingInitializer" ref="customBinder"&gt;&lt;/property&gt;

&lt;/bean&gt;

&lt;!-- 自定义webBinder --&gt;

&lt;bean id="customBinder" class="org.springframework.web.bind.support.ConfigurableWebBindingInitializer"&gt;

     &lt;property name="conversionService" ref="conversionService" /&gt;

&lt;/bean&gt;

&lt;!-- 转换器配置 --&gt;

&lt;bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean"&gt;

     &lt;property name="converters"&gt;

          &lt;set&gt;

               &lt;bean class="cn.itcast.springmvc.convert.DateConverter" /&gt;

          &lt;/set&gt;

     &lt;/property&gt;

&lt;/bean&gt;

注意：此方法需要独立配置处理器映射器、适配器，

不再使用&lt;mvc:annotation-driven/&gt;

1、         springmvc的入口是一个servlet即前端控制器，而struts2入口是一个filter过滤器。

2、         springmvc是基于方法开发\(一个url对应一个方法\)，请求参数传递到方法的形参，可以设计为单例或多例\(建议单例\)，struts2是基于类开发，传递参数是通过类的属性，只能设计为多例。

3、         Struts采用值栈存储请求和响应的数据，通过OGNL存取数据， springmvc通过参数解析器是将request请求内容解析，并给方法形参赋值，将数据和视图封装成ModelAndView对象，最后又将ModelAndView中的模型数据通过request域传输到页面。Jsp视图解析器默认使用jstl。


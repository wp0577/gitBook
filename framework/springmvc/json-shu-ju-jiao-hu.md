# json数据交互

### 1.1. @RequestBody

作用：

@RequestBody注解用于读取http请求的内容\(字符串\)，通过springmvc提供的HttpMessageConverter接口将读到的内容（json数据）转换为java对象并绑定到Controller方法的参数上。

传统的请求参数：

itemEdit.action?id=1&name=zhangsan&age=12

现在的请求参数：

使用POST请求，在请求体里面加入json数据

{

"id": 1,

"name": "测试商品",

"price": 99.9,

"detail": "测试商品描述",

"pic": "123456.jpg"

}

本例子应用：

@RequestBody注解实现接收http请求的json数据，将json数据转换为java对象进行绑定

### 1.2. @ResponseBody

作用：

@ResponseBody注解用于将Controller的方法返回的对象，通过springmvc提供的HttpMessageConverter接口转换为指定格式的数据如：json,xml等，通过Response响应给客户端

本例子应用：

@ResponseBody注解实现将Controller方法返回java对象转换为json响应给客户端。

### 1.3. 请求json，响应json实现：

#### 1.3.1. 加入jar包

如果需要springMVC支持json，必须加入json的处理jar

我们使用Jackson这个jar，如下图：

![](../../.gitbook/assets/image%20%2841%29.png)

#### 1.3.2. ItemController编写

/\*\*

 \* 测试json的交互

 \* **@param** item

 \* **@return**

 \*/

@RequestMapping\("testJson"\)

// @ResponseBody

**public** @ResponseBody Item testJson\(@RequestBody Item item\) {

      **return** item;

}

#### 1.3.3. 安装谷歌浏览器测试工具

安装程序在课后资料

参考安装文档，如下图：

#### 1.3.4. 测试方法

测试方法，如下图：

#### 1.3.5. 测试结果

如下图：

#### 1.3.6. 配置json转换器

如果不使用注解驱动&lt;mvc:annotation-driven /&gt;，就需要给处理器适配器配置json转换器，参考之前学习的自定义参数绑定。

在springmvc.xml配置文件中，给处理器适配器加入json转换器：

&lt;!--处理器适配器 --&gt;

     &lt;bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"&gt;

          &lt;property name="messageConverters"&gt;

          &lt;list&gt;

          &lt;bean class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter"&gt;&lt;/bean&gt;

          &lt;/list&gt;

          &lt;/property&gt;

     &lt;/bean&gt;


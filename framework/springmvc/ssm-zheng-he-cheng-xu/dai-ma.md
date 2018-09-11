# 代码

## 1.  实现页面展示

### 1.1. 代码实现

编写CustomerController 显示用户列表

@Controller

@RequestMapping\("customer"\)

**public** **class** CustomerController {

     /\*\*

      \* 显示用户列表

      \*

      \* **@return**

      \*/

     @RequestMapping\("list"\)

     **public** String queryCustomerList\(\) {

          **return** "customer";

     }

}

### 1.2. 页面显示问题

访问页面，发现不能正常显示

打开开发者工具，选择Network，

发现css、js等资源文件无法加载

原因：web.xml配置时，是设置所有的请求都进入SpringMVC。但是SpringMVC          无法处理css、js等静态资源，所以无法正常显示

解决方案：

1. 在springmvc.xml中配置

      &lt;!-- 解决静态资源无法被springMVC处理的问题 --&gt;

      &lt;mvc:default-servlet-handler /&gt;

2. 修改web.xml，让所有以action结尾的请求都进入SpringMVC

      &lt;servlet-mapping&gt;

            &lt;servlet-name&gt;boot-crm&lt;/servlet-name&gt;

            &lt;!-- 所有的请求都进入springMVC --&gt;

            &lt;url-pattern&gt;\*.action&lt;/url-pattern&gt;

      &lt;/servlet-mapping&gt;

解决后的效果如下图，可以正常显示页面样式：

我们使用第二种方式解决，因为此项目中的页面的请求都是以action结尾的，所以使用第二种方式，在web.xml里面进行相应的配置

      &lt;servlet-mapping&gt;

            &lt;servlet-name&gt;boot-crm&lt;/servlet-name&gt;

            &lt;!-- 所有以action结尾的请求都进入springMVC --&gt;

            &lt;url-pattern&gt;\*.action&lt;/url-pattern&gt;

      &lt;/servlet-mapping&gt;

## 2.  实现查询条件初始化

### 2.1. 需求分析

页面效果如上图，在查询客户的时候，可以选择客户来源,所属行业,客户级别信息,页面加载时需要初始化查询条件下拉列表。

前端jsp逻辑

&lt;form class="form-inline" action="${pageContext.request.contextPath }/customer/list.action" method="get"&gt;

      &lt;div class="form-group"&gt;

            &lt;label for="customerName"&gt;客户名称&lt;/label&gt;

            &lt;input type="text" class="form-control" id="customerName" value="${custName }" name="custName"&gt;

      &lt;/div&gt;

      &lt;div class="form-group"&gt;

            &lt;label for="customerFrom"&gt;客户来源&lt;/label&gt;

            &lt;select     class="form-control" id="customerFrom" placeholder="客户来源" name="custSource"&gt;

                  &lt;option value=""&gt;--请选择--&lt;/option&gt;

                  &lt;c:forEach items="${fromType}" var="item"&gt;

                        &lt;option value="${item.dict\_id}"&lt;c:if test="${item.dict\_id == custSource}"&gt; selected&lt;/c:if&gt;&gt;${item.dict\_item\_name }&lt;/option&gt;

                  &lt;/c:forEach&gt;

            &lt;/select&gt;

      &lt;/div&gt;

      &lt;div class="form-group"&gt;

            &lt;label for="custIndustry"&gt;所属行业&lt;/label&gt;

            &lt;select     class="form-control" id="custIndustry"  name="custIndustry"&gt;

                  &lt;option value=""&gt;--请选择--&lt;/option&gt;

                  &lt;c:forEach items="${industryType}" var="item"&gt;

                        &lt;option value="${item.dict\_id}"&lt;c:if test="${item.dict\_id == custIndustry}"&gt; selected&lt;/c:if&gt;&gt;${item.dict\_item\_name }&lt;/option&gt;

                  &lt;/c:forEach&gt;

            &lt;/select&gt;

      &lt;/div&gt;

      &lt;div class="form-group"&gt;

            &lt;label for="custLevel"&gt;客户级别&lt;/label&gt;

            &lt;select     class="form-control" id="custLevel" name="custLevel"&gt;

                  &lt;option value=""&gt;--请选择--&lt;/option&gt;

                  &lt;c:forEach items="${levelType}" var="item"&gt;

                        &lt;option value="${item.dict\_id}"&lt;c:if test="${item.dict\_id == custLevel}"&gt; selected&lt;/c:if&gt;&gt;${item.dict\_item\_name }&lt;/option&gt;

                  &lt;/c:forEach&gt;

            &lt;/select&gt;

      &lt;/div&gt;

      &lt;button type="submit" class="btn btn-primary"&gt;查询&lt;/button&gt;

&lt;/form&gt;

按照jsp的要求,把对应的数据查询出来,放到模型中。

数据存放在base\_dict表，可以使用dict\_type\_code类别代码进行查询

使用需要获取的数据如下图：

使用的sql:

SELECT \* FROM base\_dict WHERE dict\_type\_code = '001'

### 2.2. 实现DAO开发

#### 2.2.1. pojo

因为页面显示的名字是下划线方式，和数据库表列名一样，根据页面的样式，编写pojo

**public** **class** BaseDict {

      **private** String dict\_id;

      **private** String dict\_type\_code;

      **private** String dict\_type\_name;

      **private** String dict\_item\_name;

      **private** String dict\_item\_code;

      **private** Integer dict\_sort;

      **private** String dict\_enable;

      **private** String dict\_memo;

get/set。。。。。。

}

#### 2.2.2. Mapper

编写BaseDictMapper

**public** **interface** BaseDictMapper {

      /\*\*

       \* 根据类别代码查询数据

       \*

       \* **@param** dictTypeCode

       \* **@return**

       \*/

      List&lt;BaseDict&gt; queryBaseDictByDictTypeCode\(String dictTypeCode\);

}

#### 2.2.3. Mapper.xml

编写BaseDictMapper.xml

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;mapper namespace="cn.itcast.crm.mapper.BaseDictMapper"&gt;

      &lt;!-- 根据类别代码查询数据 --&gt;

      &lt;select id="queryBaseDictByDictTypeCode" parameterType="String"

            resultType="cn.itcast.crm.pojo.BaseDict"&gt;

            SELECT \* FROM base\_dict WHERE dict\_type\_code =

            \#{dict\_type\_code}

      &lt;/select&gt;

&lt;/mapper&gt;

### 2.3. 实现Service开发

#### 2.3.1. BaseDictService 接口

**public** **interface** BaseDictService {

      /\*\*

       \* 根据类别代码查询

       \*

       \* **@param** dictTypeCode

       \* **@return**

       \*/

      List&lt;BaseDict&gt; queryBaseDictByDictTypeCode\(String dictTypeCode\);

}

#### 2.3.2. BaseDictServiceImpl 实现类

@Service

**public** **class** BaseDictServiceImpl **implements** BaseDictService {

      @Autowired

      **private** BaseDictMapper baseDictMapper;

      @Override

      **public** List&lt;BaseDict&gt; queryBaseDictByDictTypeCode\(String dictTypeCode\) {

            List&lt;BaseDict&gt; list = **this**.baseDictMapper.queryBaseDictByDictTypeCode\(dictTypeCode\);

            **return** list;

      }

}

### 2.4. 实现Controller开发

#### 2.4.1. 修改之前编写的controller

@Controller

@RequestMapping\("customer"\)

**public** **class** CustomerController {

      @Autowired

      **private** BaseDictService baseDictService;

      /\*\*

       \* 显示客户列表

       \*

       \* **@return**

       \*/

      @RequestMapping\("list"\)

      **public** String queryCustomerList\(Model model\) {

            // 客户来源

            List&lt;BaseDict&gt; fromType = **this**.baseDictService.queryBaseDictByDictTypeCode\("002"\);

            // 所属行业

            List&lt;BaseDict&gt; industryType = **this**.baseDictService.queryBaseDictByDictTypeCode\("001"\);

            // 客户级别

            List&lt;BaseDict&gt; levelType = **this**.baseDictService.queryBaseDictByDictTypeCode\("006"\);

            // 把前端页面需要显示的数据放到模型中

            model.addAttribute\("fromType", fromType\);

            model.addAttribute\("industryType", industryType\);

            model.addAttribute\("levelType", levelType\);

            **return** "customer";

      }

}

#### 2.4.2. 效果

实现效果如下图：

#### 2.4.3. 硬编码问题

这里是根据dict\_type\_code类别代码查询数据，这里的查询条件是写死的，有硬编码问题。可以把类别代码提取到配置文件中，再使用@value注解进行加载。

**2.4.3.1. 添加env.properties**

添加env.properties配置文件

\#客户来源

CUSTOMER\_FROM\_TYPE=002

\#客户行业

CUSTOMER\_INDUSTRY\_TYPE=001

\#客户级别

CUSTOMER\_LEVEL\_TYPE=006

**2.4.3.2. 修改springmvc.xml配置文件**

在springmvc.xml中加载env.properties

     &lt;!-- 加载controller需要的配置信息 --&gt;

     &lt;context:property-placeholder location="classpath:env.properties" /&gt;

注意:Controller需要的配置文件信息必须添加到springmvc的配置文件中

**2.4.3.3. 修改Controller方法**

@Controller

@RequestMapping\("customer"\)

**public** **class** CustomerController {

      // 客户来源

      @Value\("${CUSTOMER\_FROM\_TYPE}"\)

      **private** String CUSTOMER\_FROM\_TYPE;

      // 客户行业

      @Value\("${CUSTOMER\_INDUSTRY\_TYPE}"\)

      **private** String CUSTOMER\_INDUSTRY\_TYPE;

      // 客户级别

      @Value\("${CUSTOMER\_LEVEL\_TYPE}"\)

      **private** String CUSTOMER\_LEVEL\_TYPE;

      @Autowired

      **private** BaseDictService baseDictService;

      /\*\*

       \* 显示客户列表

       \*

       \* **@return**

       \*/

      @RequestMapping\("list"\)

      **public** String queryCustomerList\(Model model\) {

            // 客户来源

            List&lt;BaseDict&gt; fromType = **this**.baseDictService.queryBaseDictByDictTypeCode\(**this**.CUSTOMER\_FROM\_TYPE\);

            // 所属行业

            List&lt;BaseDict&gt; industryType = **this**.baseDictService.queryBaseDictByDictTypeCode\(**this**.CUSTOMER\_INDUSTRY\_TYPE\);

            // 客户级别

            List&lt;BaseDict&gt; levelType = **this**.baseDictService.queryBaseDictByDictTypeCode\(**this**.CUSTOMER\_LEVEL\_TYPE\);

            // 把前端页面需要显示的数据放到模型中

            model.addAttribute\("fromType", fromType\);

            model.addAttribute\("industryType", industryType\);

            model.addAttribute\("levelType", levelType\);

            **return** "customer";

      }

}

## 3.  客户列表展示

### 3.1. 需求

展示客户列表，并且可以根据查询条件过滤查询结果，并且实现分页。

效果如下图：

页面代码：

&lt;div class="panel-heading"&gt;客户信息列表&lt;/div&gt;

&lt;!-- /.panel-heading --&gt;

&lt;table class="table table-bordered table-striped"&gt;

      &lt;thead&gt;

            &lt;tr&gt;

                  &lt;th&gt;ID&lt;/th&gt;

                  &lt;th&gt;客户名称&lt;/th&gt;

                  &lt;th&gt;客户来源&lt;/th&gt;

                  &lt;th&gt;客户所属行业&lt;/th&gt;

                  &lt;th&gt;客户级别&lt;/th&gt;

                  &lt;th&gt;固定电话&lt;/th&gt;

                  &lt;th&gt;手机&lt;/th&gt;

                  &lt;th&gt;操作&lt;/th&gt;

            &lt;/tr&gt;

      &lt;/thead&gt;

      &lt;tbody&gt;

            &lt;c:forEach items="${page.rows}" var="row"&gt;

                  &lt;tr&gt;

                        &lt;td&gt;${row.cust\_id}&lt;/td&gt;

                        &lt;td&gt;${row.cust\_name}&lt;/td&gt;

                        &lt;td&gt;${row.cust\_source}&lt;/td&gt;

                        &lt;td&gt;${row.cust\_industry}&lt;/td&gt;

                        &lt;td&gt;${row.cust\_level}&lt;/td&gt;

                        &lt;td&gt;${row.cust\_phone}&lt;/td&gt;

                        &lt;td&gt;${row.cust\_mobile}&lt;/td&gt;

                        &lt;td&gt;

                              &lt;a href="\#" class="btn btn-primary btn-xs" data-toggle="modal" data-target="\#customerEditDialog" onclick="editCustomer\(${row.cust\_id}\)"&gt;修改&lt;/a&gt;

                              &lt;a href="\#" class="btn btn-danger btn-xs" onclick="deleteCustomer\(${row.cust\_id}\)"&gt;删除&lt;/a&gt;

                        &lt;/td&gt;

                  &lt;/tr&gt;

            &lt;/c:forEach&gt;

      &lt;/tbody&gt;

&lt;/table&gt;

分析我们需要根据四个条件进行查询，返回数据是分页对象Page

Sql语句:

SELECT

            a.cust\_id,

            a.cust\_name,

            a.cust\_user\_id,

            a.cust\_create\_id,

            b.dict\_item\_name cust\_source,

            c.dict\_item\_name cust\_industry,

            d.dict\_item\_name cust\_level,

            a.cust\_linkman,

            a.cust\_phone,

            a.cust\_mobile,

            a.cust\_zipcode,

            a.cust\_address,

            a.cust\_createtime

FROM

            customer a

LEFT JOIN base\_dict b ON a.cust\_source = b.dict\_id

LEFT JOIN base\_dict c ON a.cust\_industry = c.dict\_id

LEFT JOIN base\_dict d ON a.cust\_level = d.dict\_id

WHERE

            a.cust\_name LIKE '%马%'

AND a.cust\_source = '6'

AND a.cust\_industry = '2'

AND a.cust\_level = '22'

LIMIT 0, 10

### 3.2. 创建pojo开发

**public** **class** Customer {

      **private** Long cust\_id;

      **private** String cust\_name;

      **private** Long cust\_user\_id;

      **private** Long cust\_create\_id;

      **private** String cust\_source;

      **private** String cust\_industry;

      **private** String cust\_level;

      **private** String cust\_linkman;

      **private** String cust\_phone;

      **private** String cust\_mobile;

      **private** String cust\_zipcode;

      **private** String cust\_address;

      **private** Date cust\_createtime;

get/set。。。。。。

}

### 3.3. 实现DAO

分析：

1. 前台发起请求,需要接收请求过来的查询条件数据，可以使用pojo接收数据。需要依此编写查询逻辑。

2. 前台需要分页显示，根据准备好的分页实现，应该返回分页类Page，而创建Page分页类需要数据总条数，所以也需要查询数据总条数的逻辑。

根据分析，DAO需要编写两个方法:

1. 需要根据条件分页查询客户信息

2. 需要根据条件查询数据总条数

#### 3.3.1. 创建QueryVo

需要编写QueryVo，里面包含查询条件属性和分页数据。

创建接受请求参数的QueryVo：

**public** **class** QueryVo {

      **private** String custName;

      **private** String custSource;

      **private** String custIndustry;

      **private** String custLevel;

      // 当前页码数

      **private** Integer page = 1;

      // 数据库从哪一条数据开始查

      **private** Integer start;

      // 每页显示数据条数

      **private** Integer rows = 10;

get/set。。。。。。

}

#### 3.3.2. Mapper

创建CustomerMapper 接口

**public** **interface** CustomerMapper {

      /\*\*

       \* 根据queryVo分页查询数据

       \*

       \* **@param** queryVo

       \* **@return**

       \*/

      List&lt;Customer&gt; queryCustomerByQueryVo\(QueryVo queryVo\);

      /\*\*

       \* 根据queryVo查询数据条数

       \*

       \* **@param** queryVo

       \* **@return**

       \*/

      **int** queryCountByQueryVo\(QueryVo queryVo\);

}

#### 3.3.3. Mapper.xml

创建CustomerMapper.xml

&lt;?xml version="1.0" encoding="UTF-8" ?&gt;

&lt;!DOCTYPE mapper

PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-mapper.dtd"&gt;

&lt;mapper namespace="cn.itcast.crm.mapper.CustomerMapper"&gt;

      &lt;sql id="customerQueryVo"&gt;

            &lt;where&gt;

                  &lt;if test="custName != null and custName != ''"&gt;

                        AND a.cust\_name LIKE '%${custName}%'

                  &lt;/if&gt;

                  &lt;if test="custSource != null and custSource != ''"&gt;

                        AND a.cust\_source = \#{custSource}

                  &lt;/if&gt;

                  &lt;if test="custIndustry != null and custIndustry != ''"&gt;

                        AND a.cust\_industry = \#{custIndustry}

                  &lt;/if&gt;

                  &lt;if test="custLevel != null and custLevel != ''"&gt;

                        AND a.cust\_level = \#{custLevel}

                  &lt;/if&gt;

            &lt;/where&gt;

      &lt;/sql&gt;

      &lt;!-- 根据queryVo分页查询数据 --&gt;

      &lt;select id="queryCustomerByQueryVo" parameterType="cn.itcast.crm.pojo.QueryVo"

            resultType="cn.itcast.crm.pojo.Customer"&gt;

        SELECT

            a.cust\_id,

            a.cust\_name,

            a.cust\_user\_id,

            a.cust\_create\_id,

            b.dict\_item\_name cust\_source,

            c.dict\_item\_name cust\_industry,

            d.dict\_item\_name cust\_level,

            a.cust\_linkman,

            a.cust\_phone,

            a.cust\_mobile,

            a.cust\_zipcode,

            a.cust\_address,

            a.cust\_createtime

        FROM

            customer a

            LEFT JOIN base\_dict b ON a.cust\_source = b.dict\_id

            LEFT JOIN base\_dict c ON a.cust\_industry = c.dict\_id

            LEFT JOIN base\_dict d ON a.cust\_level = d.dict\_id

            &lt;include refid="customerQueryVo" /&gt;

            &lt;if test="start != null"&gt;

                  LIMIT \#{start}, \#{rows}

            &lt;/if&gt;

      &lt;/select&gt;

      &lt;!-- 根据queryVo查询数据条数 --&gt;

      &lt;select id="queryCountByQueryVo" parameterType="cn.itcast.crm.pojo.QueryVo"

            resultType="int"&gt;

            SELECT count\(1\) FROM customer a

            &lt;include refid="customerQueryVo" /&gt;

      &lt;/select&gt;

&lt;/mapper&gt;

### 3.4. 实现service

#### 3.4.1. 接口

编写接口CustomerService

**public** **interface** CustomerService {

      /\*\*

       \* 根据条件分页查询客户

       \*

       \* **@param** queryVo

       \* **@return**

       \*/

      Page&lt;Customer&gt; queryCustomerByQueryVo\(QueryVo queryVo\);

}

#### 3.4.2. 实现类

编写接口实现类CustomerServiceImpl

@Service

**public** **class** CustomerServiceImpl **implements** CustomerService {

      @Autowired

      **private** CustomerMapper customerMapper;

      @Override

      **public** Page&lt;Customer&gt; queryCustomerByQueryVo\(QueryVo queryVo\) {

            // 设置查询条件,从哪一条数据开始查

            queryVo.setStart\(\(queryVo.getPage\(\) - 1\) \* queryVo.getRows\(\)\);

            // 查询数据结果集

            List&lt;Customer&gt; list = **this**.customerMapper.queryCustomerByQueryVo\(queryVo\);

            // 查询到的数据总条数

            **int** total = **this**.customerMapper.queryCountByQueryVo\(queryVo\);

            // 封装返回的page对象

            Page&lt;Customer&gt; page = **new** Page&lt;&gt;\(total, queryVo.getPage\(\), queryVo.getRows\(\), list\);

            **return** page;

      }

}

### 3.5. 实现Controller

改造Controller的方法

@RequestMapping\("list"\)

**public** String queryCustomerList\(QueryVo queryVo, Model model\) {

      **try** {

            // 解决get请求乱码问题

            **if** \(StringUtils.isNotBlank\(queryVo.getCustName\(\)\)\) {

                  queryVo.setCustName\(**new** String\(queryVo.getCustName\(\).getBytes\("ISO-8859-1"\), "UTF-8"\)\);

            }

      } **catch** \(Exception e\) {

            e.printStackTrace\(\);

      }

      // 客户来源

      List&lt;BaseDict&gt; fromType = **this**.baseDictService.queryBaseDictByDictTypeCode\(**this**.CUSTOMER\_FROM\_TYPE\);

      // 所属行业

      List&lt;BaseDict&gt; industryType = **this**.baseDictService.queryBaseDictByDictTypeCode\(**this**.CUSTOMER\_INDUSTRY\_TYPE\);

      // 客户级别

      List&lt;BaseDict&gt; levelType = **this**.baseDictService.queryBaseDictByDictTypeCode\(**this**.CUSTOMER\_LEVEL\_TYPE\);

      // 把前端页面需要显示的数据放到模型中

      model.addAttribute\("fromType", fromType\);

      model.addAttribute\("industryType", industryType\);

      model.addAttribute\("levelType", levelType\);

      // 分页查询数据

      Page&lt;Customer&gt; page = **this**.customerService.queryCustomerByQueryVo\(queryVo\);

      // 把分页查询的结果放到模型中

      model.addAttribute\("page", page\);

      // 数据回显

      model.addAttribute\("custName", queryVo.getCustName\(\)\);

      model.addAttribute\("custSource", queryVo.getCustSource\(\)\);

      model.addAttribute\("custIndustry", queryVo.getCustIndustry\(\)\);

      model.addAttribute\("custLevel", queryVo.getCustLevel\(\)\);

      **return** "customer";

}

## 4.  修改客户信息

### 4.1. 需求

页面效果如下图：

1、客户列表中点击“修改”按钮弹出客户信息修改窗，并初始化客户信息

2、点击“保存修改”按钮将修改后的结果保存到数据库中

### 4.2. 实现编辑数据回显

在客户列表显示中，可以点击修改按钮，弹出修改界面，打开浏览器的开发者工具，发现当点击修改按钮，会发起一个请求

如下图方式进行查看

分析这里应该是发起请求到后台，获取该用户的详细信息，在页面上可以回显

复制请求路径中的edit.action，在customer.jsp页面中搜索，找到请求逻辑

找到的代码如下图：

发现这里是一个Ajax请求，根据这个请求我们可以开发后台逻辑，提供给前端页面进行调用

### 4.3. 回显功能实现

#### 4.3.1. Mapper接口

在CustomerMapper添加方法

/\*\*

 \* 根据id查询客户

 \*

 \* **@param** id

 \* **@return**

 \*/

Customer queryCustomerById\(Long id\);

#### 4.3.2. Mapper.xml

在CustomerMapper.xml编写sql

&lt;!-- 根据id查询用户 --&gt;

&lt;select id="queryCustomerById" resultType="cn.itcast.crm.pojo.Customer"&gt;

      SELECT \* FROM customer WHERE cust\_id = \#{id}

&lt;/select&gt;

#### 4.3.3. Service接口

编写CustomerService.接口方法

/\*\*

 \* 根据id查询数据

 \*

 \* **@param** id

 \* **@return**

 \*/

Customer queryCustomerById\(Long id\);

#### 4.3.4. Service接口实现类

在CustomerServiceImpl实现接口方法

@Override

**public** Customer queryCustomerById\(Long id\) {

      Customer customer = **this**.customerMapper.queryCustomerById\(id\);

      **return** customer;

}

#### 4.3.5. Controller

在CustomerController编写方法

/\*\*

 \* 根据id查询用户,返回json格式数据

 \*

 \* **@param** id

 \* **@return**

 \*/

@RequestMapping\("edit"\)

@ResponseBody

**public** Customer queryCustomerById\(Long id\) {

      Customer customer = **this**.customerService.queryCustomerById\(id\);

      **return** customer;

}

### 4.4. 实现编辑客户数据

在编辑框，点击保存修改按钮，应该进行数据保存，如下图所示：

发起请求如下图：

在页面找到的请求逻辑是：

**function** updateCustomer\(\) {

      $.post\("&lt;%=basePath%&gt;customer/update.action",$\("\#edit\_customer\_form"\).serialize\(\),function\(data\){

            alert\("客户信息更新成功！"\);

            window.location.reload\(\);

      }\);

}

### 4.5. 编辑功能实现

#### 4.5.1. Mapper接口

在CustomerMapper添加方法

/\*\*

 \* 根据id编辑客户

 \*

 \* **@param** customer

 \*/

**void** updateCustomerById\(Customer customer\);

#### 4.5.2. Mapper.xml

在CustomerMapper.xml编写sql

&lt;select id="updateCustomerById" parameterType="cn.itcast.crm.pojo.Customer"&gt;

      UPDATE \`customer\`

      SET

      &lt;if test="cust\_name !=null and cust\_name != ''"&gt;

            \`cust\_name\` = \#{cust\_name},

      &lt;/if&gt;

      &lt;if test="cust\_user\_id !=null"&gt;

            \`cust\_user\_id\` = \#{cust\_user\_id},

      &lt;/if&gt;

      &lt;if test="cust\_create\_id !=null"&gt;

            \`cust\_create\_id\` = \#{cust\_create\_id},

      &lt;/if&gt;

      &lt;if test="cust\_source !=null and cust\_source != ''"&gt;

            \`cust\_source\` = \#{cust\_source},

      &lt;/if&gt;

      &lt;if test="cust\_industry !=null and cust\_industry != ''"&gt;

            \`cust\_industry\` = \#{cust\_industry},

      &lt;/if&gt;

      &lt;if test="cust\_level !=null and cust\_level != ''"&gt;

            \`cust\_level\` = \#{cust\_level},

      &lt;/if&gt;

      &lt;if test="cust\_linkman !=null and cust\_linkman != ''"&gt;

            \`cust\_linkman\` = \#{cust\_linkman},

      &lt;/if&gt;

      &lt;if test="cust\_phone !=null and cust\_phone != ''"&gt;

            \`cust\_phone\` = \#{cust\_phone},

      &lt;/if&gt;

      &lt;if test="cust\_mobile !=null and cust\_mobile != ''"&gt;

            \`cust\_mobile\` = \#{cust\_mobile},

      &lt;/if&gt;

      &lt;if test="cust\_zipcode !=null and cust\_zipcode != ''"&gt;

            \`cust\_zipcode\` = \#{cust\_zipcode},

      &lt;/if&gt;

      &lt;if test="cust\_address !=null and cust\_address != ''"&gt;

            \`cust\_address\` = \#{cust\_address},

      &lt;/if&gt;

      \`cust\_createtime\` = NOW\(\)

      WHERE

      \(\`cust\_id\` = \#{cust\_id}\);

&lt;/select&gt;

#### 4.5.3. Service接口

编写CustomerService.接口方法

/\*\*

 \* 根据id编辑客户数据

 \*

 \* **@param** customer

 \*/

**void** updateCustomerById\(Customer customer\);

#### 4.5.4. Service接口实现类

在CustomerServiceImpl实现接口方法

@Override

**public** **void** updateCustomerById\(Customer customer\) {

      **this**.customerMapper.updateCustomerById\(customer\);

}

#### 4.5.5. Controller

在CustomerController编写方法

需要正确的响应，要告诉前端更新成功。返回值有没有都可以。

这里需要加@ResponseBody注解，使其不走视图解析器。

/\*\*

 \* 根据id查询用户,返回更新后客户的json格式数据

 \*

 \* **@param** id

 \* **@return**

 \*/

@RequestMapping\("update"\)

@ResponseBody

**public** String updateCustomerById\(Customer customer\) {

      Customer result = **this**.customerService.updateCustomerById\(customer\);

      **return** "OK";

}

## 5.  删除客户

### 5.1. 需求分析

点击客户列表中的删除按钮，提示“警告信息”，如下图

如下图，点击确定后删除用户信息，并刷新页面。

发起的请求如下图：

搜索前端jsp页面逻辑找到如下代码：

**function** deleteCustomer\(id\) {

      **if**\(confirm\('确实要删除该客户吗?'\)\) {

            $.post\("&lt;%=basePath%&gt;customer/**delete**.action",{"id":id},**function**\(data\){

                  alert\("客户删除更新成功！"\);

                  window.location.reload\(\);

            }\);

      }

}

### 5.2. 功能开发

#### 5.2.1. Mapper接口

在CustomerMapper添加方法

/\*\*

 \* 根据id删除用户

 \*

 \* **@param** id

 \*/

**void** deleteCustomerById\(Long id\);

#### 5.2.2. Mapper.xml

在CustomerMapper.xml编写sql

&lt;!-- 根据id删除客户 --&gt;

&lt;delete id="deleteCustomerById" parameterType="long"&gt;

      DELETE FROM

      customer WHERE cust\_id = \#{id}

&lt;/delete&gt;

#### 5.2.3. Service接口

在CustomerService编写接口方法

/\*\*

 \* 根据id删除客户

 \*

 \* **@param** id

 \*/

**void** deleteCustomerById\(Long id\);

#### 5.2.4. Service实现类

在CustomerServiceImpl实现接口方法

@Override

**public** **void** deleteCustomerById\(Long id\) {

      **this**.customerMapper.deleteCustomerById\(id\);

}

#### 5.2.5. Controller

在CustomerController编写方法

/\*\*

 \* 删除用户

 \*

 \* **@param** id

 \* **@return**

 \*/

@RequestMapping\("delete"\)

@ResponseBody

**public** String deleteCustomerById\(Long id\) {

      **this**.customerService.deleteCustomerById\(id\);

      **return** "ok";

}


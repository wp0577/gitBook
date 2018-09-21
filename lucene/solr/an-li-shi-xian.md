# 案例实现



### a\)        原型分析

![](../../.gitbook/assets/image%20%28228%29.png)

### b\)       系统架构

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.png)

![](../../.gitbook/assets/image%20%28106%29.png)

### c\)        工程搭建

创建一个web工程导入jar包

1、springmvc的相关jar包

2、solrJ的jar包

3、Example\lib\ext下的jar包

#### 1.1.1.                  springmvc.xml

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <beans xmlns="http://www.springframework.org/schema/beans" </p>
            <p>xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"</p>
            <p>xmlns:context="http://www.springframework.org/schema/context"</p>
            <p>xmlns:mvc="http://www.springframework.org/schema/mvc"</p>
            <p>xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd</p>
            <p>http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd</p>
            <p>http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd"></p>
            <p>
              <context:component-scan base-package="com.itheima.jd" />
            </p>
            <p>
              <!-- 配置注解驱动，如果配置此标签可以不用配置处理器映射器和适配器 -->
            </p>
            <p>
              <mvc:annotation-driven/>
            </p>
            <p>
              <!-- 配置视图解析器 -->
            </p>
            <p>
              <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            </p>
            <p>
              <property name="prefix" value="/WEB-INF/jsp/" />
            </p>
            <p>
              <property name="suffix" value=".jsp" />
            </p>
            <p>
              </bean>
            </p>
            <p>
              <!-- SolrServer的配置 -->
            </p>
            <p>
              <bean id="httpSolrServer" class="org.apache.solr.client.solrj.impl.HttpSolrServer">
            </p>
            <p>
              <constructor-arg index="0" value="http://localhost:8080/solr" />
            </p>
            <p>
              </bean>
            </p>
            <p>
          </beans>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.2.                  web.xml

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee"
          xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
          id="WebApp_ID" version="2.5">
        </p>
        <p>
          <display-name>solr-jd</display-name>
        </p>
        <p>
          <welcome-file-list>
        </p>
        <p>
          <welcome-file>index.html</welcome-file>
        </p>
        <p>
          <welcome-file>index.htm</welcome-file>
        </p>
        <p>
          <welcome-file>index.jsp</welcome-file>
        </p>
        <p>
          <welcome-file>default.html</welcome-file>
        </p>
        <p>
          <welcome-file>default.htm</welcome-file>
        </p>
        <p>
          <welcome-file>default.jsp</welcome-file>
        </p>
        <p>
          </welcome-file-list>
        </p>
        <p>
          <!-- 配置前段控制器 -->
        </p>
        <p>
          <servlet>
        </p>
        <p>
          <servlet-name>springmvc</servlet-name>
        </p>
        <p>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        </p>
        <p>
          <init-param>
        </p>
        <p>
          <!-- 指定springmvc配置文件的路径</p><p>                           如果不指定默认为：/WEB-INF/${servlet-name}-servlet.xml</p><p>                  --></p>
        <p>
          <param-name>contextConfigLocation</param-name>
        </p>
        <p>
          <param-value>classpath:springmvc.xml</param-value>
        </p>
        <p>
          </init-param>
        </p>
        <p>
          </servlet>
        </p>
        <p>
          <servlet-mapping>
        </p>
        <p>
          <servlet-name>springmvc</servlet-name>
        </p>
        <p>
          <url-pattern>*.action</url-pattern>
        </p>
        <p>
          </servlet-mapping>
        </p>
        <p>
          <!-- 解决post乱码问题 -->
        </p>
        <p>
          <filter>
        </p>
        <p>
          <filter-name>CharacterEncodingFilter</filter-name>
        </p>
        <p>
          <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        </p>
        <p>
          <init-param>
        </p>
        <p>
          <param-name>encoding</param-name>
        </p>
        <p>
          <param-value>utf-8</param-value>
        </p>
        <p>
          </init-param>
        </p>
        <p>
          </filter>
        </p>
        <p>
          <filter-mapping>
        </p>
        <p>
          <filter-name>CharacterEncodingFilter</filter-name>
        </p>
        <p>
          <url-pattern>/*</url-pattern>
        </p>
        <p>
          </filter-mapping>
        </p>
        <p>
          </web-app>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### d\)       Dao

功能：接收service层传递过来的参数，根据参数查询索引库，返回查询结果。

参数：SolrQuery对象

返回值：一个商品列表List&lt;ProductModel&gt;。

方法定义：public List&lt;ProductModel&gt; getResultModelFromSolr\(QueryVo vo\) throws Exception;

商品对象模型：

**public** **class** ProductModel {

      // 商品编号

      **private** String pid;

      // 商品名称

      **private** String name;

      // 商品分类名称

      **private** String catalog\_name;

      // 价格

      **private** **float** price;

      // 商品描述

      **private** String description;

      // 图片名称

      **private** String picture;

}

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Repository</p>
        <p>public class SolrDaoImpl implements SolrDao {</p>
        <p>@Autowired</p>
        <p>private SolrServer solrServer;</p>
        <p>public List
          <ProductModel>getResultModelFromSolr(QueryVo vo) throws Exception{</p>
        <p>SolrQuery solrQuery = new SolrQuery();</p>
        <p>//关键词</p>
        <p>if(null != vo.getQueryString() && !"".equals(vo.getQueryString())){</p>
        <p>solrQuery.setQuery(vo.getQueryString());</p>
        <p>solrQuery.set("df", "product_keywords");</p>
        <p>}</p>
        <p>//商品类型</p>
        <p>if(null != vo.getCatalog_name() && !"".equals(vo.getCatalog_name().trim())){</p>
        <p>solrQuery.addFilterQuery("product_catalog_name:" + vo.getCatalog_name());</p>
        <p>}</p>
        <p>//价格</p>
        <p>if(null != vo.getPrice() && !"".equals(vo.getPrice().trim())){</p>
        <p>String[] split = vo.getPrice().split("-");</p>
        <p>if(split.length == 2){</p>
        <p>solrQuery.addFilterQuery("product_price:[" + split[0] + " TO " + split[1]
          + "]");</p>
        <p>}else{</p>
        <p>solrQuery.addFilterQuery("product_price:[" + split[0] + " TO *]");</p>
        <p>}</p>
        <p>}</p>
        <p>//价格排序</p>
        <p>if("1".equals(vo.getSort())){</p>
        <p>solrQuery.setSort("product_price", ORDER.desc);</p>
        <p>}else{</p>
        <p>solrQuery.setSort("product_price", ORDER.asc);</p>
        <p>}</p>
        <p>//显示的域</p>
        <p>//solrQuery.setFields("id,product_name,product_price");</p>
        <p>//1:设置高亮开关</p>
        <p>solrQuery.setHighlight(true);</p>
        <p>//2：需要高亮的域</p>
        <p>solrQuery.addHighlightField("product_name");</p>
        <p>//3：高亮的简单样式 前缀</p>
        <p>solrQuery.setHighlightSimplePre("<span style='color:red'>");</p><p>                        //4：高亮的简单样式 后缀</p><p>                        solrQuery.setHighlightSimplePost("</span>");</p>
        <p>QueryResponse response = solrServer.query(solrQuery);</p>
        <p>//取出高亮</p>
        <p>Map
          <String, Map<String, List<String>>> highlighting = response.getHighlighting();</p>
        <p>//取出结果集</p>
        <p>SolrDocumentList docs = response.getResults();</p>
        <p>//打印总条数</p>
        <p>System.out.println("总条数：" + docs.getNumFound());</p>
        <p>//结果集</p>
        <p>List
          <ProductModel>productModels = new ArrayList
            <ProductModel>();</p>
        <p>//閬嶅巻</p>
        <p>for (SolrDocument doc : docs) {</p>
        <p>ProductModel productModel = new ProductModel();</p>
        <p>//ID</p>
        <p>String id = (String) doc.get("id");</p>
        <p>productModel.setPid(id);</p>
        <p>//</p>
        <p>Map
          <String, List<String>> map = highlighting.get(id);</p>
        <p>List
          <String>list = map.get("product_name");</p>
        <p>//判断高亮必须不为空</p>
        <p>if(null != list && list.size() > 0){</p>
        <p>String name = list.get(0);</p>
        <p>productModel.setName(name);</p>
        <p>}</p>
        <p>//价格</p>
        <p>productModel.setPrice((float) doc.get("product_price"));</p>
        <p>//商品图片</p>
        <p>productModel.setPicture((String) doc.get("product_picture"));</p>
        <p>//商品类型</p>
        <p>productModel.setCatalog_name((String) doc.get("product_catalog_name"));</p>
        <p>productModels.add(productModel);</p>
        <p>}</p>
        <p>return productModels;</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### e\)        Service

功能：接收action传递过来的参数，根据参数拼装一个查询条件，调用dao层方法，查询商品列表。接收返回的商品列表和商品的总数量，根据每页显示的商品数量计算总页数。

参数：

1、查询条件：字符串

2、商品分类的过滤条件：商品的分类名称，字符串

3、商品价格区间：传递一个字符串，满足格式：“0-100、101-200、201-\*”

4、排序条件：页面传递过来一个升序或者降序就可以，默认是价格排序。0：升序1：降序

5、分页信息：每页显示的记录条数创建一个常量60条。传递一个当前页码就可以了。

业务逻辑

1、根据参数创建查询对象

2、调用dao执行查询。

3、根据总记录数计算总页数。

返回值：ResultModel

方法定义：ResultModel queryProduct\(String queryString, String caltalog\_name, String price, String sort, Integer page\) throws Exception;

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Service</p>
        <p>public class SolrServiceImpl implements SolrService {</p>
        <p>@Autowired</p>
        <p>private SolrDao solrDao;</p>
        <p>public List
          <ProductModel>getResultModelFromSolr(QueryVo vo) throws Exception {</p>
        <p>return solrDao.getResultModelFromSolr(vo);</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### f\)         controller

功能：接收页面传递过来的参数调用service查询商品列表。将查询结果返回给jsp页面，还需要查询参数的回显。

参数：

1、查询条件：字符串

2、商品分类的过滤条件：商品的分类名称，字符串

3、商品价格区间：传递一个字符串，满足格式：“0-100、101-200、201-\*”

4、排序条件：页面传递过来一个升序或者降序就可以，默认是价格排序。0：升序1：降序

5、分页信息：每页显示的记录条数创建一个常量60条。传递一个当前页码就可以了。

6、Model：相当于request。

返回结果：String类型，就是一个jsp的名称。

public class QueryVo {

            //关键词

            private String queryString;

            //过滤条件 商品类型

            //价格区间

            private String catalog\_name;

            private String price;

            //排序  1 正   0倒

            private String sort;

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>/**</p>
        <p>* 商品搜索</p>
        <p>* @author lx</p>
        <p>*</p>
        <p>*/</p>
        <p>@Controller</p>
        <p>public class ProductController {</p>
        <p>@Autowired</p>
        <p>SolrService solrService;</p>
        <p>//搜索</p>
        <p>@RequestMapping("/list.action")</p>
        <p>public String list(QueryVo vo,Model model) throws Exception{</p>
        <p>//结果集</p>
        <p>List
          <ProductModel>productList = solrService.getResultModelFromSolr(vo);</p>
        <p>model.addAttribute("productList", productList);</p>
        <p>model.addAttribute("catalog_name", vo.getCatalog_name());</p>
        <p>model.addAttribute("price", vo.getPrice());</p>
        <p>model.addAttribute("sort", vo.getSort());</p>
        <p>model.addAttribute("queryString", vo.getQueryString());</p>
        <p>return "product_list";</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### g\)        Jsp

参考资料。


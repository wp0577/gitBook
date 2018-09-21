# Solr管理索引库

### a\)        维护索引

### 1.1. 添加/更新文档

添加或更新单个文档

![](../../.gitbook/assets/image%20%2895%29.png)

### 1.2. 批量导入数据

使用dataimport插件批量导入数据。

第一步：把dataimport插件依赖的jar包添加到solrcore（collection1\lib）中

![](../../.gitbook/assets/image%20%28105%29.png)

还需要mysql的数据库驱动。

第二步：配置solrconfig.xml文件，添加一个requestHandler。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <requestHandler name="/dataimport" </p>
            <p>class="org.apache.solr.handler.dataimport.DataImportHandler"></p>
            <p>
              <lst name="defaults">
            </p>
            <p>
              <str name="config">data-config.xml</str>
            </p>
            <p>
              </lst>
            </p>
            <p>
          </requestHandler>
          </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第三步：创建一个data-config.xml，保存到collection1\conf\目录下

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <?xml version="1.0" encoding="UTF-8" ?>
        </p>
        <p>
          <dataConfig>
        </p>
        <p>
          <dataSource type="JdbcDataSource" </p>
            <p>driver="com.mysql.jdbc.Driver"</p>
            <p>url="jdbc:mysql://localhost:3306/lucene"</p>
            <p>user="root"</p>
            <p>password="root"/></p>
            <p>
              <document>
            </p>
            <p>
              <entity name="product" query="SELECT pid,name,catalog_name,price,description,picture FROM products ">
            </p>
            <p>
              <field column="pid" name="id" />
            </p>
            <p>
              <field column="name" name="product_name" />
            </p>
            <p>
              <field column="catalog_name" name="product_catalog_name" />
            </p>
            <p>
              <field column="price" name="product_price" />
            </p>
            <p>
              <field column="description" name="product_description" />
            </p>
            <p>
              <field column="picture" name="product_picture" />
            </p>
            <p>
              </entity>
            </p>
            <p>
              </document>
            </p>
            <p>
              </dataConfig>
            </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第四步：重启tomcat

![](../../.gitbook/assets/image%20%28184%29.png)

第五步：点击“execute”按钮导入数据

**到入数据前会先清空索引库，然后再导入。**

### 1.3. 删除文档

删除索引格式如下：

1） 删除制定ID的索引

&lt;delete&gt;

     &lt;id&gt;8&lt;/id&gt;

&lt;/delete&gt;

&lt;commit/&gt;

2） 删除查询到的索引数据

&lt;delete&gt;

     &lt;query&gt;product\_catalog\_name:幽默杂货&lt;/query&gt;

&lt;/delete&gt;

3） 删除所有索引数据

 &lt;delete&gt;

     &lt;query&gt;\*:\*&lt;/query&gt;

&lt;/delete&gt;

### b\)       查询索引

通过/select搜索索引，Solr制定一些参数完成不同需求的搜索：

1.       q - 查询字符串，必须的，如果查询所有使用\*:\*。

![](../../.gitbook/assets/image%20%28119%29.png)

2.       fq - （filter query）过虑查询，作用：在q查询符合结果中同时是fq查询符合的，例如：：

![](../../.gitbook/assets/image%20%28117%29.png)

过滤查询价格从1到20的记录。

也可以在“q”查询条件中使用product\_price:\[1 TO 20\]，如下：

![](../../.gitbook/assets/image%20%2852%29.png)

也可以使用“\*”表示无限，例如：

20以上：product\_price:\[20 TO \*\]

20以下：product\_price:\[\* TO 20\]

3.       sort - 排序，格式：sort=&lt;field name&gt;+&lt;desc\|asc&gt;\[,&lt;field name&gt;+&lt;desc\|asc&gt;\]… 。示例：

按价格降序

![](../../.gitbook/assets/image%20%28168%29.png)

4.       start - 分页显示使用，开始记录下标，从0开始

5.       rows - 指定返回结果最多有多少条记录，配合start来实现分页。

![](../../.gitbook/assets/image%20%28146%29.png)

显示前10条。

6.       fl - 指定返回那些字段内容，用逗号或空格分隔多个。

![](../../.gitbook/assets/image%20%2868%29.png)

显示商品图片、商品名称、商品价格

7.       df-指定一个搜索Field

也可以在SolrCore目录 中conf/solrconfig.xml文件中指定默认搜索Field，指定后就可以直接在“q”查询条件中输入关键字。

![](../../.gitbook/assets/image%20%28188%29.png)

![](../../.gitbook/assets/image%20%28246%29.png)

8.       wt - \(writer type\)指定输出格式，可以有 xml, json, php, phps, 后面 solr 1.3增加的，要用通知我们，因为默认没有打开。

9.       hl 是否高亮 ,设置高亮Field，设置格式前缀和后缀。

![](../../.gitbook/assets/image%20%28154%29.png)

## 2.  使用SolrJ管理索引库

### a\)        什么是solrJ

solrj是访问Solr服务的java客户端，提供索引和搜索的请求方法，SolrJ通常在嵌入在业务系统中，通过SolrJ的API接口操作Solr服务，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image013.png)

![](../../.gitbook/assets/image%20%2889%29.png)

### b\)       依赖的jar包

![](../../.gitbook/assets/image%20%2898%29.png)

### c\)        添加文档

#### 1.c.1.实现步骤

第一步：创建一个java工程

第二步：导入jar包。包括solrJ的jar包。还需要

![](../../.gitbook/assets/image%20%28219%29.png)

第三步：和Solr服务器建立连接。HttpSolrServer对象建立连接。

第四步：创建一个SolrInputDocument对象，然后添加域。

第五步：将SolrInputDocument添加到索引库。

第六步：提交。

#### 1.c.2.代码实现

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//向索引库中添加索引</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> addDocument() <b>throws</b> Exception {</p>
        <p>//和solr服务器创建连接</p>
        <p>//参数：solr服务器的地址</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://localhost:8080/solr");</p>
        <p>//创建一个文档对象</p>
        <p>SolrInputDocument document = <b>new</b> SolrInputDocument();</p>
        <p>//向文档中添加域</p>
        <p>//第一个参数：域的名称，域的名称必须是在schema.xml中定义的</p>
        <p>//第二个参数：域的值</p>
        <p>document.addField("id", "c0001");</p>
        <p>document.addField("title_ik", "使用solrJ添加的文档");</p>
        <p>document.addField("content_ik", "文档的内容");</p>
        <p>document.addField("product_name", "商品名称");</p>
        <p>//把document对象添加到索引库中</p>
        <p>solrServer.add(document);</p>
        <p>//提交修改</p>
        <p>solrServer.commit();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### d\)       删除文档

#### 1.d.1.                 根据id删除

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//删除文档，根据id删除</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> deleteDocumentByid() <b>throws</b> Exception {</p>
        <p>//创建连接</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://localhost:8080/solr");</p>
        <p>//根据id删除文档</p>
        <p>solrServer.deleteById("c0001");</p>
        <p>//提交修改</p>
        <p>solrServer.commit();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.d.2.                 根据查询删除

查询语法完全支持Lucene的查询语法。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//根据查询条件删除文档</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> deleteDocumentByQuery() <b>throws</b> Exception
          {</p>
        <p>//创建连接</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://localhost:8080/solr");</p>
        <p>//根据查询条件删除文档</p>
        <p>solrServer.deleteByQuery("*:*");</p>
        <p>//提交修改</p>
        <p>solrServer.commit();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### e\)        修改文档

在solrJ中修改没有对应的update方法，只有add方法，只需要添加一条新的文档，和被修改的文档id一致就，可以修改了。本质上就是先删除后添加。

### f\)         查询文档

#### 1.f.1. 简单查询

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//查询索引</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> queryIndex() <b>throws</b> Exception {</p>
        <p>//创建连接</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://localhost:8080/solr");</p>
        <p>//创建一个query对象</p>
        <p>SolrQuery query = <b>new</b> SolrQuery();</p>
        <p>//设置查询条件</p>
        <p>query.setQuery("*:*");</p>
        <p>//执行查询</p>
        <p>QueryResponse queryResponse = solrServer.query(query);</p>
        <p>//取查询结果</p>
        <p>SolrDocumentList solrDocumentList = queryResponse.getResults();</p>
        <p>//共查询到商品数量</p>
        <p>System.out.println("共查询到商品数量:" + solrDocumentList.getNumFound());</p>
        <p>//遍历查询的结果</p>
        <p> <b>for</b> (SolrDocument solrDocument : solrDocumentList) {</p>
        <p>System.out.println(solrDocument.get("id"));</p>
        <p>System.out.println(solrDocument.get("product_name"));</p>
        <p>System.out.println(solrDocument.get("product_price"));</p>
        <p>System.out.println(solrDocument.get("product_catalog_name"));</p>
        <p>System.out.println(solrDocument.get("product_picture"));</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.f.2. 复杂查询

其中包含查询、过滤、分页、排序、高亮显示等处理。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//复杂查询索引</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> queryIndex2() <b>throws</b> Exception {</p>
        <p>//创建连接</p>
        <p>SolrServer solrServer = <b>new</b> HttpSolrServer("http://localhost:8080/solr");</p>
        <p>//创建一个query对象</p>
        <p>SolrQuery query = <b>new</b> SolrQuery();</p>
        <p>//设置查询条件</p>
        <p>query.setQuery("钻石");</p>
        <p>//过滤条件</p>
        <p>query.setFilterQueries("product_catalog_name:幽默杂货");</p>
        <p>//排序条件</p>
        <p>query.setSort("product_price", ORDER.asc);</p>
        <p>//分页处理</p>
        <p>query.setStart(0);</p>
        <p>query.setRows(10);</p>
        <p>//结果中域的列表</p>
        <p>query.setFields("id","product_name","product_price","product_catalog_name","product_picture");</p>
        <p>//设置默认搜索域</p>
        <p>query.set("df", "product_keywords");</p>
        <p>//高亮显示</p>
        <p>query.setHighlight(<b>true</b>);</p>
        <p>//高亮显示的域</p>
        <p>query.addHighlightField("product_name");</p>
        <p>//高亮显示的前缀</p>
        <p>query.setHighlightSimplePre("<em>");</p><p>                  //高亮显示的后缀</p><p>                  query.setHighlightSimplePost("</em>");</p>
        <p>//执行查询</p>
        <p>QueryResponse queryResponse = solrServer.query(query);</p>
        <p>//取查询结果</p>
        <p>SolrDocumentList solrDocumentList = queryResponse.getResults();</p>
        <p>//共查询到商品数量</p>
        <p>System.out.println("共查询到商品数量:" + solrDocumentList.getNumFound());</p>
        <p>//遍历查询的结果</p>
        <p> <b>for</b> (SolrDocument solrDocument : solrDocumentList) {</p>
        <p>System.out.println(solrDocument.get("id"));</p>
        <p>//取高亮显示</p>
        <p>String productName = "";</p>
        <p>Map
          <String, Map<String, List<String>>> highlighting = queryResponse.getHighlighting();</p>
        <p>List
          <String>list = highlighting.get(solrDocument.get("id")).get("product_name");</p>
        <p>//判断是否有高亮内容</p>
        <p> <b>if</b> (<b>null</b> != list) {</p>
        <p>productName = list.get(0);</p>
        <p>} <b>else</b> {</p>
        <p>productName = (String) solrDocument.get("product_name");</p>
        <p>}</p>
        <p>System.out.println(productName);</p>
        <p>System.out.println(solrDocument.get("product_price"));</p>
        <p>System.out.println(solrDocument.get("product_catalog_name"));</p>
        <p>System.out.println(solrDocument.get("product_picture"));</p>
        <p>}</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>
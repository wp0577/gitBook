# 索引库查询



Query查询对象，Lucene会根据Query查询对象生成最终的查询语法，类似关系数据库Sql语法一样Lucene也有自己的查询语法，比如：“name:lucene”表示查询Field的name为“lucene”的文档信息。

            可通过两种方法创建查询对象：

            1）使用Lucene提供Query子类

            Query是一个抽象类，lucene提供了很多查询对象，比如TermQuery项精确查询，NumericRangeQuery数字范围查询等。

            如下代码：

      Query query = **new** TermQuery\(**new** Term\("name", "lucene"\)\);

            2）使用QueryParse解析查询表达式

            QueryParse会将用户输入的查询表达式解析成Query对象实例。

            如下代码：

            QueryParser queryParser = **new** QueryParser\("name", **new** IKAnalyzer\(\)\);

            Query query = queryParser.parse\("name:lucene"\);

### 1.1. 使用query的子类查询

#### 1.1.1.   MatchAllDocsQuery

使用MatchAllDocsQuery查询索引目录中的所有文档

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testMatchAllDocsQuery() <b>throws</b> Exception
          {</p>
        <p>IndexSearcher indexSearcher = getIndexSearcher();</p>
        <p>//创建查询条件</p>
        <p>Query query = <b>new</b> MatchAllDocsQuery();</p>
        <p>//执行查询</p>
        <p>printResult(query, indexSearcher);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.2.   TermQuery

TermQuery，通过项查询，TermQuery不使用分析器所以建议匹配不分词的Field域查询，比如订单号、分类ID号等。

指定要查询的域和要查询的关键词。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//使用Termquery查询</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testTermQuery() <b>throws</b> Exception {</p>
        <p>IndexSearcher indexSearcher = getIndexSearcher();</p>
        <p>//创建查询对象</p>
        <p>Query query = <b>new</b> TermQuery(<b>new</b> Term("content", "lucene"));</p>
        <p>//执行查询</p>
        <p>TopDocs topDocs = indexSearcher.search(query, 10);</p>
        <p>//共查询到的document个数</p>
        <p>System.out.println("查询结果总数量：" + topDocs.totalHits);</p>
        <p>//遍历查询结果</p>
        <p> <b>for</b> (ScoreDoc scoreDoc : topDocs.scoreDocs) {</p>
        <p>Document document = indexSearcher.doc(scoreDoc.doc);</p>
        <p>System.out.println(document.get("filename"));</p>
        <p>//System.out.println(document.get("content"));</p>
        <p>System.out.println(document.get("path"));</p>
        <p>System.out.println(document.get("size"));</p>
        <p>}</p>
        <p>//关闭indexreader</p>
        <p>indexSearcher.getIndexReader().close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.3.   NumericRangeQuery

可以根据数值范围查询。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//数值范围查询</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testNumericRangeQuery() <b>throws</b> Exception
          {</p>
        <p>IndexSearcher indexSearcher = getIndexSearcher();</p>
        <p>//创建查询</p>
        <p>//参数：</p>
        <p>//1.域名</p>
        <p>//2.最小值</p>
        <p>//3.最大值</p>
        <p>//4.是否包含最小值</p>
        <p>//5.是否包含最大值</p>
        <p>Query query = NumericRangeQuery.newLongRange("size", 1l, 1000l, <b>true</b>, <b>true</b>);</p>
        <p>//执行查询</p>
        <p>printResult(query, indexSearcher);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 1.1.4.   BooleanQuery

可以组合查询条件。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//组合条件查询</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testBooleanQuery() <b>throws</b> Exception {</p>
        <p>IndexSearcher indexSearcher = getIndexSearcher();</p>
        <p>//创建一个布尔查询对象</p>
        <p>BooleanQuery query = <b>new</b> BooleanQuery();</p>
        <p>//创建第一个查询条件</p>
        <p>Query query1 = <b>new</b> TermQuery(<b>new</b> Term("filename", "apache"));</p>
        <p>Query query2 = <b>new</b> TermQuery(<b>new</b> Term("content", "apache"));</p>
        <p>//组合查询条件</p>
        <p>query.add(query1, Occur.MUST);</p>
        <p>query.add(query2, Occur.MUST);</p>
        <p>//执行查询</p>
        <p>printResult(query, indexSearcher);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>Occur.MUST：必须满足此条件，相当于and

Occur.SHOULD：应该满足，但是不满足也可以，相当于or

Occur.MUST\_NOT：必须不满足。相当于not

### 1.2. 使用queryparser查询

通过QueryParser也可以创建Query，QueryParser提供一个Parse方法，此方法可以直接根据查询语法来查询。Query对象执行的查询语法可通过System.out.println\(query\);查询。

需要使用到分析器。建议创建索引时使用的分析器和查询索引时使用的分析器要一致。

#### 1.2.1.   QueryParser

需要加入queryParser依赖的jar包。

![](../../.gitbook/assets/image%20%28115%29.png)

**1.1.1.1 程序实现**

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testQueryParser() <b>throws</b> Exception {</p>
        <p>IndexSearcher indexSearcher = getIndexSearcher();</p>
        <p>//创建queryparser对象</p>
        <p>//第一个参数默认搜索的域</p>
        <p>//第二个参数就是分析器对象</p>
        <p>QueryParser queryParser = <b>new</b> QueryParser("content", <b>new</b> IKAnalyzer());</p>
        <p>Query query = queryParser.parse("Lucene是java开发的");</p>
        <p>//执行查询</p>
        <p>printResult(query, indexSearcher);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**1.1.1.2 查询语法**

1、基础的查询语法，关键词查询：

域名+“：”+搜索的关键字

例如：content:java

2、范围查询

域名+“:”+\[最小值 TO 最大值\]

例如：size:\[1 TO 1000\]

范围查询在lucene中支持数值类型，不支持字符串类型。在solr中支持字符串类型。

3、组合条件查询

1）+条件1 +条件2：两个条件之间是并且的关系and

例如：+filename:apache +content:apache

2）+条件1 条件2：必须满足第一个条件，应该满足第二个条件

例如：+filename:apache content:apache

3）条件1 条件2：两个条件满足其一即可。

例如：filename:apache content:apache

4）-条件1 条件2：必须不满足条件1，要满足条件2

例如：-filename:apache content:apache

| Occur.MUST 查询条件必须满足，相当于and | +（加号） |
| :--- | :--- |
| Occur.SHOULD 查询条件可选，相当于or | 空（不用符号） |
| Occur.MUST\_NOT 查询条件不能满足，相当于not非 | -（减号） |

第二种写法：

条件1 AND 条件2

条件1 OR 条件2

条件1 NOT 条件2

#### 1.2.2.   MultiFieldQueryParser

可以指定多个默认搜索域

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> testMultiFiledQueryParser() <b>throws</b> Exception
          {</p>
        <p>IndexSearcher indexSearcher = getIndexSearcher();</p>
        <p>//可以指定默认搜索的域是多个</p>
        <p>String[] fields = {"filename", "content"};</p>
        <p>//创建一个MulitFiledQueryParser对象</p>
        <p>MultiFieldQueryParser queryParser = <b>new</b> MultiFieldQueryParser(fields, <b>new</b> IKAnalyzer());</p>
        <p>Query query = queryParser.parse("java AND apache");</p>
        <p>System.out.println(query);</p>
        <p>//执行查询</p>
        <p>printResult(query, indexSearcher);</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>
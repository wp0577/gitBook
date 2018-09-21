# 配置开发环境及功能实现

### 1.1. Lucene下载

Lucene是开发全文检索功能的工具包，从官方网站下载Lucene4.10.3，并解压。

官方网站：[http://lucene.apache.org/](http://lucene.apache.org/)

版本：lucene4.10.3

Jdk要求：1.7以上

IDE：Eclipse

### 1.2. 使用的jar包

![](../../.gitbook/assets/image%20%2893%29.png)

![](../../.gitbook/assets/image%20%28112%29.png)

Lucene包：

lucene-core-4.10.3.jar

lucene-analyzers-common-4.10.3.jar

lucene-queryparser-4.10.3.jar

其它：

commons-io-2.4.jar

junit-4.9.jar



## 2. 功能实现 索引库

使用indexwriter对象创建索引

### 1.1. 实现步骤

第一步：创建一个java工程，并导入jar包。

第二步：创建一个indexwriter对象。

1）指定索引库的存放位置Directory对象

2）指定一个分析器，对文档内容进行分析。

第二步：创建document对象。

第三步：创建field对象，将field添加到document对象中。

第四步：使用indexwriter对象将document对象写入索引库，此过程进行索引创建。并将索引和document对象写入索引库。

第五步：关闭IndexWriter对象。

### 1.2. Field域的属性

**是否分析**：是否对域的内容进行分词处理。前提是我们要对域的内容进行查询。

**是否索引**：将Field分析后的词或整个Field值进行索引，只有索引方可搜索到。

比如：商品名称、商品简介分析后进行索引，订单号、身份证号不用分析但也要索引，这些将来都要作为查询条件。

**是否存储**：将Field值存储在文档中，存储在文档中的Field才可以从Document中获取

比如：商品名称、订单号，凡是将来要从Document中获取的Field都要存储。

**是否存储的标准：是否要将内容展示给用户**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field类</th>
      <th style="text-align:left">数据类型</th>
      <th style="text-align:left">
        <p>Analyzed</p>
        <p>是否分析</p>
      </th>
      <th style="text-align:left">
        <p>Indexed</p>
        <p>是否索引</p>
      </th>
      <th style="text-align:left">
        <p>Stored</p>
        <p>是否存储</p>
      </th>
      <th style="text-align:left">说明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">StringField(FieldName, FieldValue,Store.YES))</td>
      <td style="text-align:left">字符串</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">Y或N</td>
      <td style="text-align:left">
        <p>这个Field用来构建一个字符串Field，但是不会进行分析，会将整个串存储在索引中，比如(订单号,姓名等)</p>
        <p>是否存储在文档中用Store.YES或Store.NO决定</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">LongField(FieldName, FieldValue,Store.YES)</td>
      <td style="text-align:left">Long型</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">Y或N</td>
      <td style="text-align:left">
        <p>这个Field用来构建一个Long数字型Field，进行分析和索引，比如(价格)</p>
        <p>是否存储在文档中用Store.YES或Store.NO决定</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">StoredField(FieldName, FieldValue)</td>
      <td style="text-align:left">重载方法，支持多种类型</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">
        <p>这个Field用来构建不同类型Field</p>
        <p>不分析，不索引，但要Field存储在文档中</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>TextField(FieldName, FieldValue, Store.NO)</p>
        <p>或</p>
        <p>TextField(FieldName, reader)</p>
      </td>
      <td style="text-align:left">
        <p>字符串</p>
        <p>或</p>
        <p>流</p>
      </td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">Y或N</td>
      <td style="text-align:left">如果是一个Reader, lucene猜测内容比较多,会采用Unstored的策略.</td>
    </tr>
  </tbody>
</table>### 1.3. 代码实现

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//创建索引</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> createIndex() <b>throws</b> Exception {</p>
        <p>//指定索引库存放的路径</p>
        <p>//D:\temp\0108\index</p>
        <p>Directory directory = FSDirectory.open(<b>new</b> File("D:\\temp\\0108\\index"));</p>
        <p>//索引库还可以存放到内存中</p>
        <p>//Directory directory = new RAMDirectory();</p>
        <p>//创建一个标准分析器</p>
        <p>Analyzer analyzer = <b>new</b> StandardAnalyzer();</p>
        <p>//创建indexwriterCofig对象</p>
        <p>//第一个参数： Lucene的版本信息，可以选择对应的lucene版本也可以使用LATEST</p>
        <p>//第二根参数：分析器对象</p>
        <p>IndexWriterConfig config = <b>new</b> IndexWriterConfig(Version.LATEST,
          analyzer);</p>
        <p>//创建indexwriter对象</p>
        <p>IndexWriter indexWriter = <b>new</b> IndexWriter(directory, config);</p>
        <p>//原始文档的路径D:\传智播客\01.课程\04.lucene\01.参考资料\searchsource</p>
        <p>File dir = <b>new</b> File("D:\\传智播客\\01.课程\\04.lucene\\01.参考资料\\searchsource");</p>
        <p> <b>for</b> (File f : dir.listFiles()) {</p>
        <p>//文件名</p>
        <p>String fileName = f.getName();</p>
        <p>//文件内容</p>
        <p>String fileContent = FileUtils.readFileToString(f);</p>
        <p>//文件路径</p>
        <p>String filePath = f.getPath();</p>
        <p>//文件的大小</p>
        <p> <b>long</b> fileSize = FileUtils.sizeOf(f);</p>
        <p>//创建文件名域</p>
        <p>//第一个参数：域的名称</p>
        <p>//第二个参数：域的内容</p>
        <p>//第三个参数：是否存储</p>
        <p>Field fileNameField = <b>new</b> TextField("filename", fileName, Store.YES);</p>
        <p>//文件内容域</p>
        <p>Field fileContentField = <b>new</b> TextField("content", fileContent, Store.YES);</p>
        <p>//文件路径域（不分析、不索引、只存储）</p>
        <p>Field filePathField = <b>new</b> StoredField("path", filePath);</p>
        <p>//文件大小域</p>
        <p>Field fileSizeField = <b>new</b> LongField("size", fileSize, Store.YES);</p>
        <p>//创建document对象</p>
        <p>Document document = <b>new</b> Document();</p>
        <p>document.add(fileNameField);</p>
        <p>document.add(fileContentField);</p>
        <p>document.add(filePathField);</p>
        <p>document.add(fileSizeField);</p>
        <p>//创建索引，并写入索引库</p>
        <p>indexWriter.addDocument(document);</p>
        <p>}</p>
        <p>//关闭indexwriter</p>
        <p>indexWriter.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 1.4. 使用Luke工具查看索引文件

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)

## 2.  功能二：查询索引

### 2.1. 实现步骤

第一步：创建一个Directory对象，也就是索引库存放的位置。

第二步：创建一个indexReader对象，需要指定Directory对象。

第三步：创建一个indexsearcher对象，需要指定IndexReader对象

第四步：创建一个TermQuery对象，指定查询的域和查询的关键词。

第五步：执行查询。

第六步：返回查询结果。遍历查询结果并输出。

第七步：关闭IndexReader对象

### 2.2. IndexSearcher搜索方法

| 方法 | 说明 |
| :--- | :--- |
| indexSearcher.search\(query, n\) | 根据Query搜索，返回评分最高的n条记录 |
| indexSearcher.search\(query, filter, n\) | 根据Query搜索，添加过滤策略，返回评分最高的n条记录 |
| indexSearcher.search\(query, n, sort\) | 根据Query搜索，添加排序策略，返回评分最高的n条记录 |
| indexSearcher.search\(booleanQuery, filter, n, sort\) | 根据Query搜索，添加过滤策略，添加排序策略，返回评分最高的n条记录 |

### 2.3. 代码实现

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//查询索引库</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> searchIndex() <b>throws</b> Exception {</p>
        <p>//指定索引库存放的路径</p>
        <p>//D:\temp\0108\index</p>
        <p>Directory directory = FSDirectory.open(<b>new</b> File("D:\\temp\\0108\\index"));</p>
        <p>//创建indexReader对象</p>
        <p>IndexReader indexReader = DirectoryReader.open(directory);</p>
        <p>//创建indexsearcher对象</p>
        <p>IndexSearcher indexSearcher = <b>new</b> IndexSearcher(indexReader);</p>
        <p>//创建查询</p>
        <p>Query query = <b>new</b> TermQuery(<b>new</b> Term("filename", "apache"));</p>
        <p>//执行查询</p>
        <p>//第一个参数是查询对象，第二个参数是查询结果返回的最大值</p>
        <p>TopDocs topDocs = indexSearcher.search(query, 10);</p>
        <p>//查询结果的总条数</p>
        <p>System.out.println("查询结果的总条数："+ topDocs.totalHits);</p>
        <p>//遍历查询结果</p>
        <p>//topDocs.scoreDocs存储了document对象的id</p>
        <p> <b>for</b> (ScoreDoc scoreDoc : topDocs.scoreDocs) {</p>
        <p>//scoreDoc.doc属性就是document对象的id</p>
        <p>//根据document的id找到document对象</p>
        <p>Document document = indexSearcher.doc(scoreDoc.doc);</p>
        <p>System.out.println(document.get("filename"));</p>
        <p>//System.out.println(document.get("content"));</p>
        <p>System.out.println(document.get("path"));</p>
        <p>System.out.println(document.get("size"));</p>
        <p>}</p>
        <p>//关闭indexreader对象</p>
        <p>indexReader.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 2.4. TopDocs

Lucene搜索结果可通过TopDocs遍历，TopDocs类提供了少量的属性，如下：

| 方法或属性 | 说明 |
| :--- | :--- |
| totalHits | 匹配搜索条件的总记录数 |
| scoreDocs | 顶部匹配记录 |

注意：

Search方法需要指定匹配记录数量n：indexSearcher.search\(query, n\)

TopDocs.totalHits：是匹配索引库中所有记录的数量

TopDocs.scoreDocs：匹配相关度高的前边记录数组，scoreDocs的长度小于等于search方法指定的参数n

## 3.  功能三：支持中文分词

### 3.1. 分析器（Analyzer）的执行过程

如下图是语汇单元的生成过程：

![](../../.gitbook/assets/image%20%28142%29.png)

从一个Reader字符流开始，创建一个基于Reader的Tokenizer分词器，经过三个TokenFilter生成语汇单元Tokens。

要看分析器的分析效果，只需要看Tokenstream中的内容就可以了。每个分析器都有一个方法tokenStream，返回一个tokenStream对象。

### 3.2. 分析器的分词效果

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//查看标准分析器的分词效果</p>
        <p> <b>public</b>  <b>void</b> testTokenStream() <b>throws</b> Exception {</p>
        <p>//创建一个标准分析器对象</p>
        <p>Analyzer analyzer = <b>new</b> StandardAnalyzer();</p>
        <p>//获得tokenStream对象</p>
        <p>//第一个参数：域名，可以随便给一个</p>
        <p>//第二个参数：要分析的文本内容</p>
        <p>TokenStream tokenStream = analyzer.tokenStream("test", "The Spring Framework
          provides a comprehensive programming and configuration model.");</p>
        <p>//添加一个引用，可以获得每个关键词</p>
        <p>CharTermAttribute charTermAttribute = tokenStream.addAttribute(CharTermAttribute.<b>class</b>);</p>
        <p>//添加一个偏移量的引用，记录了关键词的开始位置以及结束位置</p>
        <p>OffsetAttribute offsetAttribute = tokenStream.addAttribute(OffsetAttribute.<b>class</b>);</p>
        <p>//将指针调整到列表的头部</p>
        <p>tokenStream.reset();</p>
        <p>//遍历关键词列表，通过incrementToken方法判断列表是否结束</p>
        <p> <b>while</b>(tokenStream.incrementToken()) {</p>
        <p>//关键词的起始位置</p>
        <p>System.out.println("start->" + offsetAttribute.startOffset());</p>
        <p>//取关键词</p>
        <p>System.out.println(charTermAttribute);</p>
        <p>//结束位置</p>
        <p>System.out.println("end->" + offsetAttribute.endOffset());</p>
        <p>}</p>
        <p>tokenStream.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 3.3. 中文分析器

#### 3.3.1.   Lucene自带中文分词器

l  StandardAnalyzer：

单字分词：就是按照中文一个字一个字地进行分词。如：“我爱中国”，  
 效果：“我”、“爱”、“中”、“国”。

l  CJKAnalyzer

二分法分词：按两个字进行切分。如：“我是中国人”，效果：“我是”、“是中”、“中国”“国人”。

上边两个分词器无法满足需求。

l  SmartChineseAnalyzer

对中文支持较好，但扩展性差，扩展词库，禁用词库和同义词库等不好处理

#### 3.3.2.   第三方中文分析器

·  paoding： 庖丁解牛最新版在 [https://code.google.com/p/paoding/](https://code.google.com/p/paoding/) 中最多支持Lucene 3.0，且最新提交的代码在 2008-06-03，在svn中最新也是2010年提交，已经过时，不予考虑。

·  mmseg4j：最新版已从 [https://code.google.com/p/mmseg4j/](https://code.google.com/p/mmseg4j/) 移至 [https://github.com/chenlb/mmseg4j-solr](https://github.com/chenlb/mmseg4j-solr)，支持Lucene 4.10，且在github中最新提交代码是2014年6月，从09年～14年一共有：18个版本，也就是一年几乎有3个大小版本，有较大的活跃度，用了mmseg算法。

·  IK-analyzer： 最新版在https://code.google.com/p/ik-analyzer/上，支持Lucene 4.10从2006年12月推出1.0版开始， IKAnalyzer已经推出了4个大版本。最初，它是以开源项目Luence为应用主体的，结合词典分词和文法分析算法的中文分词组件。从3.0版本开 始，IK发展为面向Java的公用分词组件，独立于Lucene项目，同时提供了对Lucene的默认优化实现。在2012版本中，IK实现了简单的分词 歧义排除算法，标志着IK分词器从单纯的词典分词向模拟语义分词衍化。 但是也就是2012年12月后没有在更新。

·  ansj\_seg：最新版本在 [https://github.com/NLPchina/ansj\_seg](https://github.com/NLPchina/ansj_seg) tags仅有1.1版本，从2012年到2014年更新了大小6次，但是作者本人在2014年10月10日说明：“可能我以后没有精力来维护ansj\_seg了”，现在由”nlp\_china”管理。2014年11月有更新。并未说明是否支持Lucene，是一个由CRF（条件随机场）算法所做的分词算法。

·  imdict-chinese-analyzer：最新版在 [https://code.google.com/p/imdict-chinese-analyzer/](https://code.google.com/p/imdict-chinese-analyzer/) ， 最新更新也在2009年5月，下载源码，不支持Lucene 4.10 。是利用HMM（隐马尔科夫链）算法。

·  Jcseg：最新版本在git.oschina.net/lionsoul/jcseg，支持Lucene 4.10，作者有较高的活跃度。利用mmseg算法。

#### 3.3.3.   IKAnalyzer

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image003.png)

使用方法：

第一步：把jar包添加到工程中

第二步：把配置文件和扩展词典和停用词词典添加到classpath下

注意：mydict.dic和ext\_stopword.dic文件的格式为UTF-8，注意是无BOM 的UTF-8 编码。

使用EditPlus.exe保存为无BOM 的UTF-8 编码格式，如下图：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.png)

### 3.4. Analyzer使用时机

#### 3.4.1.   索引时使用Analyzer

            输入关键字进行搜索，当需要让该关键字与文档域内容所包含的词进行匹配时需要对文档域内容进行分析，需要经过Analyzer分析器处理生成语汇单元（Token）。分析器分析的对象是文档中的Field域。当Field的属性tokenized（是否分词）为true时会对Field值进行分析，如下图：

![](../../.gitbook/assets/image%20%2899%29.png)

对于一些Field可以不用分析：

1、不作为查询条件的内容，比如文件路径

2、不是匹配内容中的词而匹配Field的整体内容，比如订单号、身份证号等。

#### 3.4.2.   搜索时使用Analyzer

            对搜索关键字进行分析和索引分析一样，使用Analyzer对搜索关键字进行分析、分词处理，使用分析后每个词语进行搜索。比如：搜索关键字：spring web ，经过分析器进行分词，得出：spring  web拿词去索引词典表查找 ，找到索引链接到Document，解析Document内容。

            对于匹配整体Field域的查询可以在搜索时不分析，比如根据订单号、身份证号查询等。

           **** **注意：搜索使用的分析器要和索引使用的分析器一致。**

## 4.  功能四：索引库的维护

### 4.1. 索引库的添加

#### 4.1.1.   步骤

向索引库中添加document对象。

第一步：先创建一个indexwriter对象

第二步：创建一个document对象

第三步：把document对象写入索引库

第四步：关闭indexwriter。

#### 4.1.2.   代码实现

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//添加索引</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> addDocument() <b>throws</b> Exception {</p>
        <p>//索引库存放路径</p>
        <p>Directory directory = FSDirectory.open(<b>new</b> File("D:\\temp\\0108\\index"));</p>
        <p>IndexWriterConfig config = <b>new</b> IndexWriterConfig(Version.LATEST, <b>new</b> IKAnalyzer());</p>
        <p>//创建一个indexwriter对象</p>
        <p>IndexWriter indexWriter = <b>new</b> IndexWriter(directory, config);</p>
        <p>//创建一个Document对象</p>
        <p>Document document = <b>new</b> Document();</p>
        <p>//向document对象中添加域。</p>
        <p>//不同的document可以有不同的域，同一个document可以有相同的域。</p>
        <p>document.add(<b>new</b> TextField("filename", "新添加的文档", Store.YES));</p>
        <p>document.add(<b>new</b> TextField("content", "新添加的文档的内容", Store.NO));</p>
        <p>document.add(<b>new</b> TextField("content", "新添加的文档的内容第二个content", Store.YES));</p>
        <p>document.add(<b>new</b> TextField("content1", "新添加的文档的内容要能看到", Store.YES));</p>
        <p>//添加文档到索引库</p>
        <p>indexWriter.addDocument(document);</p>
        <p>//关闭indexwriter</p>
        <p>indexWriter.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 4.2. 索引库删除

#### 4.2.1.   删除全部

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//删除全部索引</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> deleteAllIndex() <b>throws</b> Exception {</p>
        <p>IndexWriter indexWriter = getIndexWriter();</p>
        <p>//删除全部索引</p>
        <p>indexWriter.deleteAll();</p>
        <p>//关闭indexwriter</p>
        <p>indexWriter.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>说明：将索引目录的索引信息全部删除，直接彻底删除，无法恢复。

**此方法慎用！！**

#### 4.2.2.   指定查询条件删除

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//根据查询条件删除索引</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> deleteIndexByQuery() <b>throws</b> Exception {</p>
        <p>IndexWriter indexWriter = getIndexWriter();</p>
        <p>//创建一个查询条件</p>
        <p>Query query = <b>new</b> TermQuery(<b>new</b> Term("filename", "apache"));</p>
        <p>//根据查询条件删除</p>
        <p>indexWriter.deleteDocuments(query);</p>
        <p>//关闭indexwriter</p>
        <p>indexWriter.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>### 4.3. 索引库的修改

原理就是先删除后添加。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>//修改索引库</p>
        <p>@Test</p>
        <p> <b>public</b>  <b>void</b> updateIndex() <b>throws</b> Exception {</p>
        <p>IndexWriter indexWriter = getIndexWriter();</p>
        <p>//创建一个Document对象</p>
        <p>Document document = <b>new</b> Document();</p>
        <p>//向document对象中添加域。</p>
        <p>//不同的document可以有不同的域，同一个document可以有相同的域。</p>
        <p>document.add(<b>new</b> TextField("filename", "要更新的文档", Store.YES));</p>
        <p>document.add(<b>new</b> TextField("content", "2013年11月18日 - Lucene 简介 Lucene
          是一个基于 Java 的全文信息检索工具包,它不是一个完整的搜索应用程序,而是为你的应用程序提供索引和搜索功能。", Store.YES));</p>
        <p>indexWriter.updateDocument(<b>new</b> Term("content", "java"), document);</p>
        <p>//关闭indexWriter</p>
        <p>indexWriter.close();</p>
        <p>}</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>
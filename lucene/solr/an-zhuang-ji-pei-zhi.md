# 安装及配置



### 1.1. Solr的下载

从Solr官方网站（http://lucene.apache.org/solr/ ）下载Solr4.10.3，根据Solr的运行环境，Linux下需要下载lucene-4.10.3.tgz，windows下需要下载lucene-4.10.3.zip。

Solr使用指南可参考：https://wiki.apache.org/solr/FrontPage。

### 1.2. Solr的文件夹结构

将solr-4.10.3.zip解压：

![](../../.gitbook/assets/image%20%2844%29.png)

bin：solr的运行脚本

contrib：solr的一些贡献软件/插件，用于增强solr的功能。

dist：该目录包含build过程中产生的war和jar文件，以及相关的依赖文件。

docs：solr的API文档

example：solr工程的例子目录：

l  example/solr：

     该目录是一个包含了默认配置信息的Solr的Core目录。

l  example/multicore：

     该目录包含了在Solr的multicore中设置的多个Core目录。

l  example/webapps：

    该目录中包括一个solr.war，该war可作为solr的运行实例工程。

licenses：solr相关的一些许可信息

### 1.3. 运行环境

solr 需要运行在一个Servlet容器中，Solr4.10.3要求jdk使用1.7以上，Solr默认提供Jetty（java写的Servlet容器），本教程使用Tocmat作为Servlet容器，环境如下：

Solr：Solr4.10.3

Jdk：jdk1.7.0\_72

Tomcat：apache-tomcat-7.0.53

### 1.4. Solr整合tomcat

#### 1.4.1.   Solr Home与SolrCore

创建一个Solr home目录，SolrHome是Solr运行的主目录，目录中包括了运行Solr实例所有的配置文件和数据文件，Solr实例就是SolrCore，一个SolrHome可以包括多个SolrCore（Solr实例），每个SolrCore提供单独的搜索和索引服务。

example\solr是一个solr home目录结构，如下：

![](../../.gitbook/assets/image%20%28178%29.png)

上图中“collection1”是一个SolrCore（Solr实例）目录 ，目录内容如下所示：

![](../../.gitbook/assets/image%20%28107%29.png)

说明：

collection1：叫做一个Solr运行实例SolrCore，SolrCore名称不固定，一个solr运行实例对外单独提供索引和搜索接口。

solrHome中可以创建多个solr运行实例SolrCore。

一个solr的运行实例对应一个索引目录。

conf是SolrCore的配置文件目录 。

data目录存放索引文件需要创建

#### 1.4.2.   整合步骤

第一步：安装tomcat。D:\temp\apache-tomcat-7.0.53

第二步：把solr的war包复制到tomcat 的webapp目录下。

把\solr-4.10.3\dist\solr-4.10.3.war复制到D:\temp\apache-tomcat-7.0.53\webapps下。

改名为solr.war  
 第三步：solr.war解压。使用压缩工具解压或者启动tomcat自动解压。解压之后删除solr.war

第四步：把\solr-4.10.3\example\lib\ext目录下的所有的jar包添加到solr工程中

第五步：配置solrHome和solrCore。

1）创建一个solrhome（存放solr所有配置文件的一个文件夹）。\solr-4.10.3\example\solr目录就是一个标准的solrhome。

2）把\solr-4.10.3\example\solr文件夹复制到D:\temp\0108路径下，改名为solrhome，改名不是必须的，是为了便于理解。

3）在solrhome下有一个文件夹叫做collection1这就是一个solrcore。就是一个solr的实例。一个solrcore相当于mysql中一个数据库。Solrcore之间是相互隔离。

i.        在solrcore中有一个文件夹叫做conf，包含了索引solr实例的配置信息。

**ii.**       在conf文件夹下有一个solrconfig.xml。配置实例的相关信息。**如果使用默认配置可以不用做任何修改。**

**Xml的配置信息：**

**Lib**：solr服务依赖的扩展包，默认的路径是collection1\lib文件夹，如果没有                   就创建一个

**dataDir**：配置了索引库的存放路径。默认路径是collection1\data文件夹，如                               果没有data文件夹，会自动创建。

**requestHandler：**

![](../../.gitbook/assets/image%20%28173%29.png)

![](../../.gitbook/assets/image%20%28100%29.png)

第六步：告诉solr服务器配置文件也就是solrHome的位置。修改web.xml使用jndi的方式告诉solr服务器。

**Solr/home名称必须是固定的。**

![](../../.gitbook/assets/image%20%2877%29.png)

![](../../.gitbook/assets/image%20%28172%29.png)

第七步：启动tomcat

第八步：访问http://localhost:8080/solr/

### 1.5. Solr后台管理

#### 1.5.1.   管理界面

![](../../.gitbook/assets/image%20%28182%29.png)

#### 1.5.2.   Dashboard

仪表盘，显示了该Solr实例开始启动运行的时间、版本、系统资源、jvm等信息。

#### 1.5.3.   Logging

Solr运行日志信息

#### 1.5.4.   Cloud

Cloud即SolrCloud，即Solr云（集群），当使用Solr Cloud模式运行时会显示此菜单，如下图是Solr Cloud的管理界面：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image009.png)

#### 1.5.5.   Core Admin

Solr Core的管理界面。Solr Core 是Solr的一个独立运行实例单位，它可以对外提供索引和搜索服务，一个Solr工程可以运行多个SolrCore（Solr实例），一个Core对应一个索引目录。

添加solrcore：

第一步：复制collection1改名为collection2

第二步：修改core.properties。name=collection2

第三步：重启tomcat

#### 1.5.6.   java properties

Solr在JVM 运行环境中的属性信息，包括类路径、文件编码、jvm内存设置等信息。

#### 1.5.7.   Tread Dump

显示Solr Server中当前活跃线程信息，同时也可以跟踪线程运行栈信息。

#### 1.5.8.   Core selector

选择一个SolrCore进行详细操作，如下：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image010.png)

#### 1.5.9.   Analysis

![](../../.gitbook/assets/image%20%28190%29.png)

通过此界面可以测试索引分析器和搜索分析器的执行情况。

#### 1.5.10.              Dataimport

可以定义数据导入处理器，从关系数据库将数据导入 到Solr索引库中。

#### 1.5.11.              Document

通过此菜单可以创建索引、更新索引、删除索引等操作，界面如下：

![](../../.gitbook/assets/image%20%28120%29.png)

/update表示更新索引，solr默认根据id（唯一约束）域来更新Document的内容，如果根据id值搜索不到id域则会执行添加操作，如果找到则更新。

#### 1.5.12.              Query

![](../../.gitbook/assets/image%20%28183%29.png)

通过/select执行搜索索引，必须指定“q”查询条件方可搜索。

### 1.6. 配置中文分析器

#### 1.6.1.   Schema.xml

schema.xml，在SolrCore的conf目录下，它是Solr数据表配置文件，它定义了加入索引的数据的数据类型的。主要包括FieldTypes、Fields和其他的一些缺省设置。

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image014.png)

**1.1.1.1 FieldType域类型定义**

下边“text\_general”是Solr默认提供的FieldType，通过它说明FieldType定义的内容：

![](../../.gitbook/assets/image%20%2869%29.png)

FieldType子结点包括：name,class,positionIncrementGap等一些参数：

name：是这个FieldType的名称

class：是Solr提供的包solr.TextField，solr.TextField 允许用户通过分析器来定制索引和查询，分析器包括一个分词器（tokenizer）和多个过滤器（filter）

positionIncrementGap：可选属性，定义在同一个文档中此类型数据的空白间隔，避免短语匹配错误，此值相当于Lucene的短语查询设置slop值，根据经验设置为100。

在FieldType定义的时候最重要的就是定义这个类型的数据在建立索引和进行查询的时候要使用的分析器analyzer,包括分词和过滤

索引分析器中：使用solr.StandardTokenizerFactory标准分词器，solr.StopFilterFactory停用词过滤器，solr.LowerCaseFilterFactory小写过滤器。

搜索分析器中：使用solr.StandardTokenizerFactory标准分词器，solr.StopFilterFactory停用词过滤器，这里还用到了solr.SynonymFilterFactory同义词过滤器。

**1.1.1.2 Field定义**

在fields结点内定义具体的Field，filed定义包括name,type（为之前定义过的各种FieldType）,indexed（是否被索引）,stored（是否被储存），multiValued（是否存储多个值）等属性。

如下：

&lt;field name="name" type="text\_general" indexed="true" stored="true"/&gt;

&lt;field name="features" type="text\_general" indexed="true" stored="true" multiValued="true"/&gt;

multiValued：该Field如果要存储多个值时设置为true，solr允许一个Field存储多个值，比如存储一个用户的好友id（多个），商品的图片（多个，大图和小图），通过使用solr查询要看出返回给客户端是数组：

![](../../.gitbook/assets/image%20%28198%29.png)

**1.1.1.3 uniqueKey**

Solr中默认定义唯一主键key为id域，如下：

![](file:////Users/wupan/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image017.png)

Solr在删除、更新索引时使用id域进行判断，也可以自定义唯一主键。

注意在创建索引时必须指定唯一约束。

**1.1.1.4 copyField复制域**

copyField复制域，可以将多个Field复制到一个Field中，以便进行统一的检索：

比如，输入关键字搜索title标题内容content，

定义title、content、text的域：

![](../../.gitbook/assets/image%20%28144%29.png)

![](../../.gitbook/assets/image%20%28249%29.png)

![](../../.gitbook/assets/image%20%2897%29.png)

根据关键字只搜索text域的内容就相当于搜索title和content，将title和content复制到text中，如下：

![](../../.gitbook/assets/image%20%28131%29.png)

**1.1.1.5 dynamicField（动态字段）**

动态字段就是不用指定具体的名称，只要定义字段名称的规则，例如定义一个 dynamicField，name 为\*\_i，定义它的type为text，那么在使用这个字段的时候，任何以\_i结尾的字段都被认为是符合这个定义的，例如：name\_i，gender\_i，school\_i等。

自定义Field名为：product\_title\_t，“product\_title\_t”和scheam.xml中的dynamicField规则匹配成功，如下：

![](../../.gitbook/assets/image%20%2886%29.png)

“product\_title\_t”是以“\_t”结尾。

创建索引：

![](../../.gitbook/assets/image%20%283%29.png)

搜索索引：

![](../../.gitbook/assets/image%20%2879%29.png)

#### 1.6.2.   安装中文分词器

使用IKAnalyzer中文分析器。

第一步：把IKAnalyzer2012FF\_u1.jar添加到solr/WEB-INF/lib目录下。

第二步：复制IKAnalyzer的配置文件和自定义词典和停用词词典到solr的classpath下。

第三步：在schema.xml中添加一个自定义的fieldType，使用中文分析器。

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!-- IKAnalyzer-->
        </p>
        <p>
          <fieldType name="text_ik" class="solr.TextField">
        </p>
        <p>
          <analyzer class="org.wltea.analyzer.lucene.IKAnalyzer" />
        </p>
        <p>
          </fieldType>
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第四步：定义field，指定field的type属性为text\_ik

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>
          <!--IKAnalyzer Field-->
        </p>
        <p>
          <field name="title_ik" type="text_ik" indexed="true" stored="true" />
        </p>
        <p>
          <field name="content_ik" type="text_ik" indexed="true" stored="false"
          multiValued="true" />
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>第四步：重启tomcat

测试：

![](../../.gitbook/assets/image%20%28203%29.png)

### 1.7. 设置业务系统Field

如果不使用Solr提供的Field可以针对具体的业务需要自定义一套Field，如下是商品信息Field：

&lt;!--product--&gt;

   &lt;field name="product\_name" type="text\_ik" indexed="true" stored="true"/&gt;

   &lt;field name="product\_price"  type="float" indexed="true" stored="true"/&gt;

   &lt;field name="product\_description" type="text\_ik" indexed="true" stored="false" /&gt;

   &lt;field name="product\_picture" type="string" indexed="false" stored="true" /&gt;

   &lt;field name="product\_catalog\_name" type="string" indexed="true" stored="true" /&gt;

   &lt;field name="product\_keywords" type="text\_ik" indexed="true" stored="false" multiValued="true"/&gt;

   &lt;copyField source="product\_name" dest="product\_keywords"/&gt;

   &lt;copyField source="product\_description" dest="product\_keywords"/&gt;


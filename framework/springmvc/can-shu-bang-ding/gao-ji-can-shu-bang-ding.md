# 高级参数绑定

### 1.1. 绑定数组

#### 1.1.1. 需求

在商品列表页面选中多个商品，然后删除。

#### 1.1.2. 需求分析

功能要求商品列表页面中的每个商品前有一个checkbok，选中多个商品后点击删除按钮把商品id传递给Controller，根据商品id删除商品信息。

我们演示可以获取id的数组即可

#### 1.1.3. Jsp修改

修改itemList.jsp页面,增加多选框，提交url是queryItem.action

&lt;form action="${pageContext.request.contextPath }/queryItem.action" method="post"&gt;

查询条件：

&lt;table width="100%" border=1&gt;

&lt;tr&gt;

&lt;td&gt;商品id&lt;input type="text" name="item.id" /&gt;&lt;/td&gt;

&lt;td&gt;商品名称&lt;input type="text" name="item.name" /&gt;&lt;/td&gt;

&lt;td&gt;&lt;input type="submit" value="查询"/&gt;&lt;/td&gt;

&lt;/tr&gt;

&lt;/table&gt;

商品列表：

&lt;table width="100%" border=1&gt;

&lt;tr&gt;

      &lt;td&gt;选择&lt;/td&gt;

      &lt;td&gt;商品名称&lt;/td&gt;

      &lt;td&gt;商品价格&lt;/td&gt;

      &lt;td&gt;生产日期&lt;/td&gt;

      &lt;td&gt;商品描述&lt;/td&gt;

      &lt;td&gt;操作&lt;/td&gt;

&lt;/tr&gt;

&lt;c:forEach items="${itemList }" var="item"&gt;

&lt;tr&gt;

      &lt;td&gt;&lt;input type="checkbox" name="ids" value="${item.id}"/&gt;&lt;/td&gt;

      &lt;td&gt;${item.name }&lt;/td&gt;

      &lt;td&gt;${item.price }&lt;/td&gt;

      &lt;td&gt;&lt;fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/&gt;&lt;/td&gt;

      &lt;td&gt;${item.detail }&lt;/td&gt;

      &lt;td&gt;&lt;a href="${pageContext.request.contextPath }/itemEdit.action?id=${item.id}"&gt;修改&lt;/a&gt;&lt;/td&gt;

&lt;/tr&gt;

&lt;/c:forEach&gt;

&lt;/table&gt;

&lt;/form&gt;

页面选中多个checkbox向controller方法传递

本身属于一个form表单，提交url是queryItem.action

#### 1.1.4. Controller

Controller方法中可以用String\[\]接收，或者pojo的String\[\]属性接收。两种方式任选其一即可。

定义QueryVo，如下图：

![](../../../.gitbook/assets/image%20%2823%29.png)

ItemController修改queryItem方法：

/\*\*

 \* 包装类型 绑定数组类型，可以使用两种方式，pojo的属性接收，和直接接收

 \*

 \* **@param** queryVo

 \* **@return**

 \*/

@RequestMapping\("queryItem"\)

**public** String queryItem\(QueryVo queryVo, Integer\[\] ids\) {

      System.**out**.println\(queryVo.getItem\(\).getId\(\)\);

      System.**out**.println\(queryVo.getItem\(\).getName\(\)\);

      System.**out**.println\(queryVo.getIds\(\).length\);

      System.**out**.println\(ids.length\);

      **return** "success";

}

效果，如下图：

### 1.2. 将表单的数据绑定到List

#### 1.2.1. 需求

实现商品数据的批量修改。

#### 1.2.2. 开发分析

开发分析

1. 在商品列表页面中可以对商品信息进行修改。

2. 可以批量提交修改后的商品数据。

#### 1.2.3. 定义pojo

List中存放对象，并将定义的List放在包装类QueryVo中

使用包装pojo对象接收，如下图：

![](../../../.gitbook/assets/image%20%2816%29.png)

#### 1.2.4. Jsp改造

前端页面应该显示的html代码，如下图：

![](../../../.gitbook/assets/image%20%2858%29.png)

分析发现：name属性必须是list属性名+下标+元素属性。

Jsp做如下改造：

&lt;c:forEach items="${itemList }" var="item" varStatus="s"&gt;

&lt;tr&gt;

      &lt;td&gt;&lt;input type="checkbox" name="ids" value="${item.id}"/&gt;&lt;/td&gt;

      &lt;td&gt;

            &lt;input type="hidden" name="itemList\[${s.index}\].id" value="${item.id }"/&gt;

            &lt;input type="text" name="itemList\[${s.index}\].name" value="${item.name }"/&gt;

      &lt;/td&gt;

      &lt;td&gt;&lt;input type="text" name="itemList\[${s.index}\].price" value="${item.price }"/&gt;&lt;/td&gt;

      &lt;td&gt;&lt;input type="text" name="itemList\[${s.index}\].createtime" value="&lt;fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/&gt;"/&gt;&lt;/td&gt;

      &lt;td&gt;&lt;input type="text" name="itemList\[${s.index}\].detail" value="${item.detail }"/&gt;&lt;/td&gt;

      &lt;td&gt;&lt;a href="${pageContext.request.contextPath }/itemEdit.action?id=${item.id}"&gt;修改&lt;/a&gt;&lt;/td&gt;

&lt;/tr&gt;

&lt;/c:forEach&gt;

${current}  当前这次迭代的（集合中的）项

${status.first}   判断当前项是否为集合中的第一项，返回值为true或false

${status.last}    判断当前项是否为集合中的最

varStatus属性常用参数总结下：

${status.index}   输出行号，从0开始。

${status.count}   输出行号，从1开始。

${status.后一项，返回值为true或false

begin、end、step分别表示：起始序号，结束序号，跳跃步伐。


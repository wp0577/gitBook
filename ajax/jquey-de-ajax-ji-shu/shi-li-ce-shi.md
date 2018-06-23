# 实例测试

```java
@WebServlet( name="NotificationServlet",urlPatterns = {"/notification"})
```

该段代码出现在idea中的servlet类里，通过注释的方法直接映射，不用再去xml文件中配置。

### 具体代码如下：

{% code-tabs %}
{% code-tabs-item title="jsp" %}
```java
<script type="text/javascript" src="jquery-1.8.3.js"></script>
  <script type="text/javascript">
    function fn1() {
        alert("process");
        $.get(
            "ajaxServlet",
            {"name":"panwu","age":20},
            function(data) {
                alert(data)
            }
            //最后一个参数是返回的文本类型
            //比如json
        );
    }
  </script>
  
  //get提交和post提交方式基本类似
```
{% endcode-tabs-item %}

{% code-tabs-item title=undefined %}
```

```
{% endcode-tabs-item %}

{% code-tabs-item title=undefined %}
```

```
{% endcode-tabs-item %}
{% endcode-tabs %}


# 实例测试

```java
@WebServlet( name="NotificationServlet",urlPatterns = {"/notification"})
```

该段代码出现在idea中的servlet类里，通过注释的方法直接映射，不用再去xml文件中配置。

## $.get\({}\); and $.post\({}\);

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

## $.ajax\({}\);

{% hint style="warning" %}
常用的option有如下： 

async：是否异步，默认是true代表异步 

data：发送到服务器的参数，建议使用json格式 

dataType：服务器端返回的数据类型，常用text和json 

success：成功响应执行的函数，对应的类型是function类型 

type：请求方式，POST/GET 

url：请求服务器端地址
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="java" %}
```java
function fn1() {
        alert("process");

        $.ajax({
            url: "ajaxServlet",
            async: true,
            type: "POST",
            data:{"name":"panwu","age":20},
            success:function (data) {
                alert("success_ajax");
                alert(data);
            },
            dataType: "text"

        });
    }
```
{% endcode-tabs-item %}

{% code-tabs-item title=undefined %}
```
常用的option有如下：
async：是否异步，默认是true代表异步
data：发送到服务器的参数，建议使用json格式
dataType：服务器端返回的数据类型，常用text和json
success：成功响应执行的函数，对应的类型是function类型
type：请求方式，POST/GET
url：请求服务器端地址

```
{% endcode-tabs-item %}
{% endcode-tabs %}


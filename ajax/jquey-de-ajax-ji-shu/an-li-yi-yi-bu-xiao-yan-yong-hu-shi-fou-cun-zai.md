# 案例一：异步校验用户是否存在

![](../../.gitbook/assets/image%20%2857%29.png)

## 案例分析：

```javascript
$(function(){
});
//以上代码为jQuery的页面加载程序，即放进该内容里的程序都会在你点开网页的同时就会在载
```

> keyup\(\): press keyboard trigger.
>
> val\(\): the value of html elements.  $\("\#username"\).val\(\) == $\(this\).val\(\) == this.value;
>
> input等元素的内容是value值

{% page-ref page="../../front-end/jqeury/" %}

## 案例实现

简易案例实现，略去了跟数据库交互，而是直接在servlet中进行简单校验。

{% code-tabs %}
{% code-tabs-item title="jsp" %}
```java
$(function () {
      $("#username").keyup(function () {
          var text = $(this).val();
          $.ajax({
              url: "ajaxServlet",
              async: true,
              data:{"name":text},
              success:function (data) {
                  if(data == "success") {
                      text = "username exisit";
                  }
                  else {
                      text = "username doesn't exisit";
                  }
                  //alert(text);
                  $("#span").html(text);
              }

          });
      });
  })

```
{% endcode-tabs-item %}

{% code-tabs-item title="servlet" %}
```java
String name = request.getParameter("name");
        System.out.println(name);
        String res = "";

        if(name.equals("wp")) res = "success";
        else res ="false";
        response.getWriter().write(res);
        System.out.println(res);
```
{% endcode-tabs-item %}
{% endcode-tabs %}


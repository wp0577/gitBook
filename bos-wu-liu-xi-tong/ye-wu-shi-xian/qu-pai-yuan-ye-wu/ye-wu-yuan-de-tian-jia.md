# 业务员的添加

## 前端实现

```text
<%--为手机添加校验规则,必须为10位--%>
<script type="text/javascript">
        $(function () {
            $('#save').click(function () {
                var e = $('#staffAddForm').form("validate");
                if(e){
                /*$('#staffAddForm').form("submit");
                * 使用上述方法会跳用ajax请求，页面不会刷新*/
                    $('#staffAddForm').submit();
                }
            });

            $.extend($.fn.validatebox.defaults.rules, {
                telephone: {
                    validator: function(value,param){
                        return value.match(/\d/g).length===10;
                    },
                    message: 'password formate is wrong.'
                }
            });
        })
</script>
```

## 后端实现

因为页面未选择pda时，haspad的值将不存在而不是0。

```text
private String haspda = "0";//是否有PDA，1：有 0：无
```

 创建取派员对应的Action、Service、Dao（dao层已由basedao实现）


# 前端配置

## easyUI中validatebox使用

### 创建validatebox从标记。

&lt;input id="vv" class="easyui-validatebox" data-options="required:true" /&gt;

### 验证密码是否相等

```text
/*检查密码是否相等*/
      $.extend($.fn.validatebox.defaults.rules, {
          equals: {
              validator: function(value,param){
                  return value == $(param[0]).val();
          },
          message: 'Field do not match.'
      }
  });
```

```text
validType="equals['#txtNewPass']"
```

 提交表单并再次校验

## 通过ajax提交密码并得到回复信息

```text
$("#btnCancel").click(function(){
			$('#editPwdWindow').window('close');
		});
		
		$("#btnEp").click(function(){
        var e = $("#passwordForm").form("validate");
       // var password = ${"#txtRePass"}.val();
        if(e) {
            $.post("${pageContext.request.contextPath}/userAction_editPassword", { "password": $("#txtRePass").val() },
                function(data){
                    if(data == 1) {
                        $('#editPwdWindow').window('close');
                    }
                    else {
                        $.messager.alert("warning","edit password failed","warning")
                    }
            })
        }
		});
```

{% hint style="info" %}
记得

```text
$("#txtRePass").val()
```
{% endhint %}



